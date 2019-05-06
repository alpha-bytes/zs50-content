# Week 4: Memory

## CS50 Week 4
Use the links below to catchup on CS50's week3 content: 

- [Lecture](https://www.youtube.com/watch?v=ed2lnJNf7HU)
- [Notes](https://cs50.harvard.edu/college/weeks/4/notes/)

## ZS50 Week 4

If you watched CS50's week4 lecture and thought...

> *Oh no, not another week of pointers, linked lists and malloc. More like mallYUCK!*

...well, first of all, kudos on your mad pun skills 😂. Secondly, **you're in luck**! This is the first week where we begin to take more of a departure from the CS50 content to focus on the specificities of Salesforce dev, although the former's core concepts will still inform our discussion. 

### Back to Classes

Let's take a moment to review some core concepts we've learned so far about classes. 

> 1. Classes are blueprints for constructing objects. 
> 2. We can also refer to a given object as an `instance` of the class from which it was constructed. 
> 3. Classes are comprised of only two things: 
>       - Variables (characteristics of an object)
>       - Methods (things the object can *do*)
> 4. Objects live on the `heap`, and primitives (including object pointers, or references) live on the `stack`. 

Did we miss anything? We did not. Thus, we can confidently state that **the totallity of any Apex process is the interplay between objects and primitives**. 

![furSure](https://media.giphy.com/media/HFHovXXltzS7u/giphy.gif)

Okay, that may be obvious but it's worth hammering home the point that there's no magic going on in any code (Apex or otherwise) ever written. It's all just objects, primitives and the logic working on them. That's great news for us as developers because it means the classes and objects we create can leverage a ton of functionality provided by the Apex language "out of the box".  

### Checking Out the (Standard) Library 📖

Most every programming language has a standard library - a bundle of functionality that comes with the language for performing common logic. C has [libc](https://en.wikipedia.org/wiki/C_standard_library); the [Python Standard Library](https://docs.python.org/3/library/) is called just that, as is [Java's](https://docs.oracle.com/javase/7/docs/api/index.html); Javascript has a relatively small [library](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects) but there is a [TC39 Proposal](https://github.com/tc39/proposal-javascript-standard-library) to expand it (see *https://github.com/stdlib-js/stdlib*). 

Apex is no different, and its standard library is documented under the [Apex Language Reference](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_reference.htm) heading in the Apex Developer Guide. A couple things to note here: 

1. Classes are organized into logical `namespaces` based on what they do. You can think of a namespace like a folder on your computer, where the classes it contains are the individual files in that folder.
2. In some cases, if there is a naming conflict between a class in one namespace and the class in another, you'll need to write your code to reference the "fully qualified" class name (e.g. *\<namespace\>.\<className\>*), but this doesn't come up all that often. 

Let's take a look at some of the common namespaces and classes you'll use in your daily life as a Salesforce developer. 

#### [System Namespace](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_namespace_System.htm)

The System namespace contains core functionality for working in Apex, several of which are described in more detail below. 

**`Primitive Wrapper` classes**

So, it turns out we've sort of fudged usage of the word "primitive" in prior weeks when referring to Apex primitive data types Although, the Salesforce docs use the same terminology so we get a pass 🙉. 

True primitives cannot have variables or methods - hence the term. Looking at the System namespace you'll see that every ["primitive"](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_primitives.htm) Apex type has a class in this space, with exception of `Object`, whose name betrays it as obviously non-primitive. Most likely it is also an `abstract` superclass of the other primitive wrapper classes. But more on `abstraction` later. 

We can think of and refer to these classes as `wrapper` classes, because they wrap (in some cases) true primitives like `Booleans`, providing convenience methods that make primitives easier to work with. But, sure enough, when we use them in code they get allocated on the heap, not on the stack as a true primitive would. Run the following code in Anonymous Apex in your org and inspect the logs for the evidence: 

```java
// anon apex
String s = 'hi there you'; 
Boolean b = true; 
Integer i = 42; 
```

Your logs should look similar to the following: 

![heaped](../assets/heaped.png)

Now that our heap/stack 🎈 has been sufficiently burst, let's move on to discusing some other classes you'll use frequently in Apex development. 

**`Database` class**

The Database class is used for manipulating data - i.e. inserting, updating, and deleting records. You'll also find methods for common operations such as converting leads, merging duplicates, and dynamic querying. 

For many common operations you can also use DML statements such as `insert`, `update`, `upsert`, etc. rather than Database class methods. The Apex Developer Guide has a section devoted to [deciding when to use each](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_dml_database.htm).

**`Exception` class**

It's great to be exceptional, right? Well, not so much in programming. An exception occurs when something goes wrong in your code at runtime (i.e. not a syntax or compilation problem, but something that's encountered during execution). 

Depending on the issue, the Apex runtime will throw one of the enumerable exception types described in this section. All of them share common methods such as `getMessage()`, `getStackTraceString()`, and others. Certain exception types provide additional information on what went wrong, such as the `DmlException` class's `getDmlFields()` and `getDmlType()` methods. 

It's nice that both the **Exception** class and the **Database** class live in the same namespace, because you'll often find they work well together in your programs. Wonder if that was intentional...😜


> ⏸ Aside (lengthy, but important)
> 
> This is a natural place to mention that, where your code is complex or you know that things *might* go wrong, it's a good idea (read: you absolutely should) wrap those lines in `try/catch` blocks. 
> 
> What do `try/catch` blocks do for you? They allow you to write handler logic for when things go wrong. Without them, an exception will cause your process to exit immediately with all database operations rolled back. Rather than allowing this to happen - referred to as an `uncaught exception` scenario - you should allow your code to "fail gracefully". 
> 
> Consider the following code. What happens if one of the Lead records in the list of Leads is missing a value in the `Company` field: 

```java
List<Lead> leadsToInsert = new List<Lead>(); 
// other code that populates the list of Leads for insertion
insert leadsToInsert; 
```

> Aaaarrrgh. You get an unhandled exception `System.DmlException: Insert failed. First exception on row 0; first error: REQUIRED_FIELD_MISSING, Required fields are missing: [Company]: [Company]`. So if you had 5,000 leads in that list, and only 1 was missing a value in the Company field, not only does that 1 lead not get inserted, but neither do the other 4,999. No bueno, right? 
> 
> A **good** ✅ implementation would wrap the `insert` statement in a try/catch block: 
```java
List<Lead> leadsToInsert = new List<Lead>(); 
// other code that populates the list of Leads for insertion
try{
    insert leadsToInsert; 
} catch(Exception e){
    system.debug(e.getMessage()); 
}
```
> A **better** ✅ ✅ implementation might use multiple catch blocks, knowing that the wrapped line is most likely to produce a `DmlException`, specifically: 
```java
List<Lead> leadsToInsert = new List<Lead>(); 
// other code that populates the list of Leads for insertion
try{
    insert leadsToInsert; 
} catch(DmlException de){
    system.debug(de.getMessage()); 
    Integer i = de.getDmlIndex(); 
    leadsToInsert.remove(i); 
    insert leadsToInsert; 
} catch(Exception e){
    system.debug(e.getMessage()); 
}
```
> The **best** ✅ ✅ ✅ implementation, assuming it's desirable to allow partial insertion, would utilize the `Databse.insert()` method which provides for this option, *and* wraps the line in a try/catch block. It may even use an inheritance trick to restrict the number of catch blocks to 1, allowing access to specific  `DmlException` type methods: 
```java
List<Lead> leadsToInsert = new List<Lead>(); 
// other code that populates the list of Leads for insertion
try{
    Database.insert(leadsToInsert, false); 
} catch(Exception e){
    // don't let your brain explode here 🔥
    // we'll cover what's going on in the Inheritance section below
    if(e instanceof DmlException){
        DmlException de = (DmlException) e; 
        system.debug(de.getDmlFieldNames());
    }
}
```
> To summarize, using `try/catch` blocks in your code is a really, really, really, really good idea and will save you from spending your hard-earned money on hair plugs down the line. Use them in places where code is complex, known exceptions are likely, or inputs are highly variable or unkown. As a first rule of thumb, **always wrap database operations in try/catch blocks**. 
>
> Whew. Now back to our regularly-scheduled programming.





### Inheritance


#### SOLID

# Related Content

## Read

## Watch

### CS50 Shorts

- [Data Structures](https://www.youtube.com/embed/3uGchQbk7g8?autoplay=1&rel=0)
- [Hash Tables](https://www.youtube.com/embed/nvzVHwrrub0?autoplay=1&rel=0)