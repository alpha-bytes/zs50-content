## Week 3: Memory

### CS50 Week 3
Use the links below to catchup on CS50's week3 content: 

- [Lecture](https://www.youtube.com/watch?v=cC9I3XxkZXw)
- [Notes](https://cs50.harvard.edu/college/weeks/3/notes/)

### ZS50 Week 3

CS50 week3 took a deep-dive into memory, the place in a computer's hardware architecture where the ephemeral magic of our programs live. This week we'll explore how many of those same concepts apply to developing on the Salesforce platform. 

### Memory in Salesforce

When writing Apex, we don't need to worry about low-level memory interactions like we do via `C`'s `malloc` (reserve memory) or `free` (give memory back) functions, but we are absolutely still bound by the same fundamental maxim: computing resources are finite. 

CS50's discussion covered the concepts of memory `stack` and `heap` and - no surprise - those are at play in the Salesforce runtime as well. In fact, although we have much more limited access to the actual memory running our Salesforce code, there are a couple places where we can see the artifacts of their presence at play.

#### The `Stack` in Apex

> Quick aside: 
> 
> This section covers concepts that are a bit abstract in nature. It's highly recommended you first check out the CS50 video short on the [Call Stack](https://www.youtube.com/embed/aCPkszeKRa4). 

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

**The important point** for us is that memory and, here specifically, `call stack` memory, is still a factor in Apex development. As it turns out, Salesforce imposes a stack limit of **1,000** `frames` before throwing an exception. A frame is pushed onto the "top" of the stack (hence the term) whenever a new function is called, and removed from the stack when it finishes execution. If the function returns a value, that value is returned to the frame below it. 

*Right, okay, but the example above is intentionally contrived to result in an infinite recursion since the `isTrue` argument will never be true. I would never write anything so heinous.*

Fair point. The example is intentionally simple. However, there are instances where you may *unintentionally* create code that results in an infinite call stack. Consider another example below: 

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

The example above actually demonstrates a couple concepts for us, including how non-primitive variables in Apex are `passed-by-reference` (more below). Have you spotted the issue above? If not, take a few minutes to walk your way through what the code is doing.

...

Okay, so you probably spotted our bug: with each iteration of the `while` loop we're incrementing `howManyAccounts`. The outcome of which is that the size of the `accounts` list will **always** be smaller than `howManyAccounts`. This example, too, may seem a bit contrived. It's something you'd more likely find "in the wild", however, since incrementing a counter inside a loop is a fairly common pattern, and one you might find yourself doing reflexively. 

### Passing Variables in Apex

#### Pass by Copy

#### Pass by Reference (sort of)

- reference 'swap' from cs50
- [Dev Blog](https://developer.salesforce.com/blogs/developer-relations/2012/05/passing-parameters-by-reference-and-by-value-in-apex.html)

> Aside: 
> `Side Effect`. Certain programming paradigms - Functional programming - aim to avoid. Vars `immutable` after assignment. OOP embraces.

## Related Content

### Read

- [Apex Transaction Limits](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_gov_limits.htm)

### Watch

#### CS50 Shorts

- [Call Stacks](https://www.youtube.com/embed/aCPkszeKRa4?autoplay=1&rel=0)
- [Dynamic Memory Allocation](https://www.youtube.com/embed/xa4ugmMDhiE?autoplay=1&rel=0)
- [Pointers](https://www.youtube.com/embed/XISnO2YhnsY?autoplay=1&rel=0)
- [Recursion](https://www.youtube.com/embed/mz6tAJMVmfM?autoplay=1&rel=0)

### Code