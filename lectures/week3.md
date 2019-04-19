## Week 3: Memory

### CS50 Week 3
Use the links below to catchup on CS50's week3 content: 

- [Lecture](https://www.youtube.com/watch?v=cC9I3XxkZXw)
- [Notes](https://cs50.harvard.edu/college/weeks/3/notes/)

### ZS50 Week 3

CS50 week3 took a deep-dive into memory, the place in a computer's hardware architecture where the ephemeral magic of our programs live. This week we'll explore how many of those same concepts apply to developing on the Salesforce platform. 

### Memory in Salesforce

When writing Apex, we don't need to worry about low-level memory interactions like we do in `C` via `malloc()` (reserve memory) or `free()` (give memory back). We are bound, however, by the same maxim: the computing resources available to us are finite. 

CS50's discussion covered the concepts of memory `stack` and `heap` and - no surprise - those are at play in the Salesforce runtime as well. In fact, although we have much more limited access to the actual memory running our Salesforce code, there are places where we can see the artifacts of their presence at play.

#### The `Stack` in Apex

> Quick aside: 
> 
> It's highly recommended you check out the CS50 video short on the [Call Stack](https://www.youtube.com/embed/aCPkszeKRa4) before continuing. 

Here's an interesting example of some Apex code that will run into trouble: 

```java
public Boolean returnWhenTrue(Boolean isTrue){
    if(isTrue)
        return true; 
    
    return returnWhenTrue(false); 
}

returnWhenTrue(false);
// uh-oh! Running this code throws exception: 'System.LimitException: Maximum stack depth reached: 1001'
```

So what's going on here? Well, `returnWhenTrue()` is an example of a `recursive` function. That is - its outcome depends on calling itself. 

![wtf](https://media.giphy.com/media/114U9jSRDkSWSk/giphy.gif)

You're probably saying right now, *why in the name of Ned Stark would I put myself through the mental anguish of using a recursive function?!* Well, we're not going to do a deep dive of recursion here, but it can be a more efficient and elegant way of implementing logic that has to make calculations over many iterations. If you'd like to learn more check out the link to CS50's recursion short in the *Related Content* section below.

**The important point** for us is that memory and, here specifically, `call stack` memory, is still a factor in Apex development. As it turns out, Salesforce imposes a stack limit of **1,000** `frames` before throwing an exception. A frame is created and pushed onto the "top" of the stack (hence the phrase) whenever a new function is called, and popped (removed) from the stack when it finishes execution. If the function returns a value, that value is returned to the frame below it. 

*Right, okay, but the example above is intentionally contrived to result in an infinite recursion since the `isTrue` argument will never be true. I would never write anything so heinous.*

Fair point. The example is intentionally simplistic. However, there are instances where you may *unintentionally* create code that results in an infinite call stack. Consider another example below: 

```java
public class TestDataFactory(){

    public static List<Account> initTestAccounts(Integer howManyAccounts){
        List<Account> accounts = new List<Account>(); 
        while(accounts.size() < howManyAccounts){
            addTestAccount(accounts); 
            howManyAccounts++;  
        }
    }

    public static void addTestAccount(List<Account> accounts){
        accounts.add(new Account()); 
    }

}

// calling code...
List<Account> testAccounts = TestDataFactory.initTestAccounts(100); 
// uh-oh! that pesky stack frame exception is back

```

Have you spotted the bug 🐛? If not, take a few minutes to step your way through the code.

...

🎵 🎵

...

Okay, so you probably spotted our issue. With each iteration of the `while` loop we're incrementing `howManyAccounts`. The outcome of which is that the size of the `accounts` list will **always** be smaller than `howManyAccounts`. This example, too, may seem a bit contrived, but it's something you'd more likely find "in the wild" since incrementing a counter inside a loop is a fairly common pattern, and one you might find yourself doing reflexively. 

> Quick aside: 
> 
> This is a good time to mention the importance of writing unit tests for your program logic to make sure bugs like this don't make it to production (or at least that fewer do). In Apex, unit test classes are denoted using the `@isTest` decorator as are their test methods. The `system.assert()` family of methods are used to validate that your code is working as expected. In fact, they're how the `zs50` CLI validates your Apex challenge code! 

Last point before we move onto `heap` memory. So what exactly gets allocated to `stack` memory? Anything that doesn't get allocated to the `heap`, of course.

![hardeeHar](https://media.giphy.com/media/M7t5GIszd4Nc4/giphy.gif)

Knee-slapper, right?! 🙄 Actually, the rules for where memory is allocated can be summed up simply: 

> In Apex: 
> - [**primitive data types**](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_primitives.htm) and **pointers** live on the `stack`
> - complex data types (objects) live on the `heap`

#### The `Heap` in Apex
*So objects are stored on the heap, you say? That's fine-and-dandy, but what's it mean for me as a developer?* 

Glad you asked and, actually, quite a lot! An object stored in the heap is accessible to any function (and, technically, `thread`, but we don't need to worry too much about those in Apex) which knows its memory address. And how might we pass a memory address around, again? 

`Pointers`, of course. 

Luckily for us, working with pointers in Apex isn't nearly as painful as it is in C. In fact, from a syntax perspective, we don't need to do anything special to harness them. In the following section, we'll look at some simple examples that demonstrate the power of pointers in our code. 

### Passing Variables in Apex
Like Java, all Apex variables are passed between functions `by copy of value`. This is true for both primitives and objects.

#### Passing Primitives

Before jumping into pointers, let's first convince ourselves that Apex indeed passes *copies* of variables between functions. Consider the following:

```java
public class IntPrinter{

    Integer instanceInt = 3; 

    public void printInt(){
        Incrementer.increment(instanceInt); 
        system.debug(instanceInt);
    }

}

public class Incrementer{

    public static void increment(Integer x){
        x++; 
    }
}
```

Given that Apex passes variables by copy of value, what will calling the following code print to the debug logs?

```java
IntPrinter ip = new IntPrinter(); 
ip.printInt(); 
```

Hopefully you said **3**. Why does this happen? Because when you pass `instanceInt` from the `printInt()` method to the `Incrementer.increment()` method, the latter is receiving a *copy* of `instanceInt`'s value, an integer equal to **3**. It then increments the copy but, since the method returns no value, the changes are erased from memory as soon as the method completes. 

#### Passing Objects

Okay so, really, everything we've covered so far has been leading up to this. 

![anticipation](https://media.giphy.com/media/KekSpdgEffjvW/giphy.gif)

Let's look at an example where we pass a complex data type, an Account [`SObject`](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_methods_system_sobject.htm): 

```java
public class AccountDomain{

    public static void enrichAccount(Account acct){
        // let's say this code reaches out to a 3rd party web service
        // to get industry data about this account, based on name. The 
        // web service returns a Map of standardized attributes about the
        // account, including its 'naicsCode' 
        Map<String, String> accountAttributes = SomeWebService.getAttributes(acct); 
        // now that we've got the data, we'll update what we're interested in
        acct.NaicsCode = accountAttributes.get('naicsCode'); 
    }

}
```

Now let's examine what will write to the debug logs after executing the following code. Let's assume Apple's naics code is 35719904. 

```java
Account a = new Account(name='Apple Inc.'); 
system.debug(a.NaicsCode); // prints null
AccountDomain.enrichAccount(a); 
system.debug(a.NaicsCode); // prints 35719904
```

*But WHY did that work? Didn't you just say that all variables are passed by **copy** in Apex?? Shouldn't `a` be unaffected by what happens in `AccountDomain.enrichAccount()`, because it only receives a copy of `a`?* 

Rest assured there's no slight-of-hand here. The code above is still passing variables by copy. But *what* is being passed is just less obvious because it's happening behind-the-scenes without the need for any special syntax. What's actually being passed to `AccountDomain.enrichAccount()` is the **pointer** to the Account `a`. 

Let's take this line-by-line: 
```java
Account a = new Account(name='Apple Inc.');
// to the RIGHT side of =, we provision memory for a new Account on the heap
// to the LEFT side of =, we declare a variable `a`, to which the address in heap where the Account object is stored gets assigned
```

So, "under the hood", what *actually* gets assigned to the variable `a` is a memory address, such as '`0x64521e08`'. In fact, this is the exact value where our Account was stored in memory when running this code at the time of writing. 

> Quick Aside
> 
> If that memory syntax feels familiar, it should. It's in hexadecimal format, like we saw in the CS50 lecture.

So, in line 2 when we run...

```java
system.debug(a.NaicsCode); 
```

What we're actually telling the computer is: `go to the location stored in a, and from the object stored there, get the value of its 'NaicsCode' field`. 

You probably see where we're going from here, huh? So in line 3 we pass `a` - and remember, Apex is passing a **copy**, literally the value '`0x64521e08`' - to `AccountDomain.enrichAccount()`, which is then able to access that same object in the heap and make changes to it. 

Thusly, an object initialized in one method (stack frame) can be acted upon in a subsequent method (another frame), and the changes will be reflected anywhere that object can be accessed. 

If this is all starting to make your head spin right now - that's okay! These are some heavy concepts. But rest-assured that, once you've wrapped your mind around them, the knowledge will make you a much stronger developer and your code more efficient and scalable. 

## Wrap Up
Whew. If last week felt like a lot, it probably feels like week3 dumped a HEAP of new information on you. 😜

So, for the sake of our sanity, let's boil this week's concepts down to some digestible takeaways:

> 1. **Primitives** and **pointers** live on the `stack`
> 2. **Objects** live on the `heap`
> 3. Any code with access to an object's `address` can access and alter it. 
> 4. All variables are `passed by copy`



## Related Content

### Read

- [Apex Transaction Limits](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_gov_limits.htm)
- [Testing in Apex](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_testing.htm)
- [Java Heap vs Stack Memory Allocation](https://www.journaldev.com/4098/java-heap-space-vs-stack-memory)
- [Salesforce Help: 'Heap Size Too Large' Example](https://help.salesforce.com/articleView?id=000004186&type=1)

### Watch

#### CS50 Shorts

- [Call Stacks](https://www.youtube.com/embed/aCPkszeKRa4?autoplay=1&rel=0)
- [Pointers](https://www.youtube.com/embed/XISnO2YhnsY?autoplay=1&rel=0)
- [Dynamic Memory Allocation](https://www.youtube.com/embed/xa4ugmMDhiE?autoplay=1&rel=0)
- [Recursion](https://www.youtube.com/embed/mz6tAJMVmfM?autoplay=1&rel=0)