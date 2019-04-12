# Week 2: Arrays

## CS50 Week 2
Use the links below to catchup on CS50's week1 content: 

- [Lecture](https://www.youtube.com/watch?v=u-kH-5JJSgU)
- [Notes](https://cs50.harvard.edu/college/weeks/2/notes/)

## ZS50 Week 2

### Compilation
CS50 kicks off week 2 with an overview of compilation, the process of converting source code like `C` or `Apex` into something a computer can execute. Compilation includes sub-steps such as: 

- preprocessing
- compiling
- assembling
- linking

If you want a little deeper-dive on how this all translates to hardware execution here's a 5 minute video from [Techquickie](https://www.youtube.com/watch?v=FkeRMQzD-0Y). 

Otherwise, the primary takeaways for this section are: 
1. When Apex is deployed to a Salesforce org a proprietary compiler performs the steps above, and
2. Your code is compiled to Java [bytecode](https://www.techopedia.com/definition/7866/java-bytecode) - remember when we said Java is Apex's closest cousin? - and stored in the deployment org as metadata for later execution.

### Memory
Next week's CS50 lecture covers memory so we'll hold off our deep-dive until then, too. For now just keep in mind that, even developing on a powerful platform like Salesforce, we are still constrained by the limits of the underlying hardware. 

These limitations are manifest in platform-enforced [Governors and Limits](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_limits_intro.htm) such as those relating to total `heap` size and `stack` depth, for example. These are two important CS terms to keep in mind for next week so, for now, just collect 'em in your back pocket. Speaking of `collections`...

Smooth segue, no?

![segwayFail](https://media.giphy.com/media/9048oxDWssE24/giphy.gif)


### Collections
Collections, such as `Arrays` and `Lists`, are hugely important to the way code is written in Apex. Anyone who has worked through a programmatic Trailhead module for more than 5 minutes has heard the terms **bulkification** or **bulkified** at least 92 times, and there's a good reason for it: Salesforce needs operations to execute efficiently in order to ensure resource availability for all tenants on their platform. 

In fact, there are several means by which Salesforce forces us to implement bulkified code. One of those approaches is through Apex Triggers and the [context variables](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_context_variables.htm) Salesforce provides to us within a given trigger. 

#### Triggers and Trigger Context Variables

We're not going to explicitly cover writing an Apex Trigger here (though you may want to bone up on the [syntax](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_syntax.htm) for the weekly Apex Challenge). Rather, what's important for our discussion is that, depending on what event context your trigger is operating in (e.g. `before insert`, `after update`, etc.), many of the context variables available to you are collections. 

Let's explore one such variable: `Trigger.new`. This variable is available to all triggers defined for *insert*, *update* or *undelete* events. So what is it? `Trigger.new` is an ordered [List](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_collections_lists.htm) of all SObjects of the specified type that *a given User* has initiated a specific dml operation against. 

*Queue crickets...* 🐛

Okay, this one is probably best illustrated with an example. Say you've defined the following Contact trigger: 
```java
trigger ContactTrigger on Contact (before insert){
    // do stuff
}
```

This trigger is going to "fire" every time a user attempts to insert one ore more Contact records. 

> Quick aside:
>
> Triggers can be written to fire either before or after records are saved (but not committed) to the database, hence the `before` and `after` contexts for most dml events. There are some important [considerations](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_context_variables_considerations.htm) when deciding which context to use. Getting familiar with Salesforce's save [order of execution](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_order_of_execution.htm) is also highly recommended. 
> 
> Onward.

Now let's say one of your org's users has just mass-inserted 500 new Contacts using the Data Import Wizard (*note to self: remove users' mass import permissions..*). Expanding on the trigger we started above, what number will be written to the debug logs in the code below?  

```java
trigger ContactTrigger on Contact (before insert){
    Integer counter = 0; 
    for(Contact c : Trigger.new){
        counter++; 
    }
    System.debug(counter); 
}
```

If you said **500**, you're today's big winner! 

![winner](https://media.giphy.com/media/3ohhwGl5urKvvygu08/giphy.gif)

#### Trigger Best Practices

Apex Triggers are a bit like `stored procedures` in many database management systems (DBMS) - they provide a means to execute a series of steps, a procedure, during the lifecycle of a database transaction. 

What they lack, however, is features that make object-oriented programming so powerful. Things like `inheritance` and `abstraction`. Those concepts can wait for another day so, for now, just remember the following: 

> Apex Triggers should delegate their logic to an Apex **Class** for processing. 

So, the implementation for our code above might look like the following: 

```java

// ContactTrigger.trigger file
trigger ContactTrigger on Contact (before insert){
    ContactDomain.doBeforeInsert(Trigger.new); 
}


// ContactDomain.cls file
public class ContactDomain{

    public static void doBeforeInsert(List<Contact> newContacts){
        Integer counter = 0; 
        for(Contact c : newContacts){
            counter++; 
        }
        System.debug(counter);
    }
}
```

Right now you may asking yourself: *"what's that new `static` word doing in there?"*, and *"how were we able to call `ContactDomain.doBeforeInsert()` without first creating a new instance of `ContactDomain`?"*. Or maybe, *"why are we creating a class to do what we could write directly in the trigger? Isn't that just an additional step?!"*. 

All good questions. Time for a ⚡️round. 

1. Declaring a method as `static` allows it to be called without the need to first create an instance of its class. 
2. See #1. 
3. The power of delegating trigger logic to a handler class really comes into play when you start to implement - like we touched on above - OOP features like `abstraction`. You'll notice we named our handler class `ContactDomain`, which is a naming pattern used in orgs following some common architectural Apex patterns.

> Quick aside...
> 
> Trailhead has two good modules on Apex Enterprise Patterns if you'd like to learn more. Links are provided in the *Related Content* section below.

For now, if you remember nothing else from this section, remember: **Trigger delegation. Just Do It.** 👍 


#### Apex Collection Types

Lists are one of the collection data types available in Apex, the others being `Set` and `Map`. Check out the [Collections](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_collections.htm) page in the Apex developer guide for more info. Here are the primary characteristics to know for each: 

|Collection Type| Primary Characteristic
|--|--
|List| Ordered collection of items. 
|Set| Unordered collection of items, each of which *must be unique*
|Map| Collection of mappings (think "rows") of unique `keys` to `values`


### Sorting
CS50 spent a lot of time this week on different sorting algorithms. While intellectually stimulating, we don't really need to worry about selecting the best sorting algorithm in Apex - the Salesforce runtime takes care of that for us. 

However, one thing you might want to take a look at - or store in your mind's back pocket for later - is that we *can* tell Salesforce *what criteria to use* when sorting our custom types (e.g. Apex Classes we create). 

This is done through the `equals()` and `hashCode()` methods, which can be implemented on any custom type. Take a look at the documentation [here](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_collections_maps_keys_userdefined.htm) to learn more. 

### Debugging
CS50 week2 covered this topic a bit and we've glossed over it here to focus on more core concepts, but it's an important aspect of developing in Apex (or any language). The term `debugging` may sound like something you do after hiking through the woods in a t-shirt and, in a way, it's similar. It's the process of locating and removing bugs - unwanted behavior - from the code you write. 

There are various methods and tools you can utilize for debugging Apex code, some of which is covered in the [Debugging Apex](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_debugging.htm) section of the Apex Developer Guide. 

For now, the tool you'll want in your toolbox is the `System.debug()` Apex method, which simply writes a provided String to the debug logs that get generated during a transaction. 

### Constructor Methods
CS50's discussion this week didn't specifically cover constructor methods or, as they're more commonly referred to, just `constructors`, but it's an important part of writing code in object-oriented languages. 

A `constructor` is a special type of method that is used to create - or *construct* - an instance of a class. Some things to know about constructors: 
- may take 0 or more arguments
- defines no return value in the method signature, because...
- implicitly returns an instance of the class where it is defined
- method signature must be the same as the class name in which it resides
- if no constructor is explicitly defined for a class, it inherits a default zero-argument constructor

Okay, that may seem like a lot at this point, but a quick example should help. Up until now in our Apex challenges we've been benefiting from default, zero-arg constructors: 

```java
public class SayHello{

    // we don't see it, but SayHello has an implicit constructor
    // that takes no arguments, and doesn't do anything, as such:
    // public SayHello(){ }

    public String sayHi(){
      String s = 'Hello, World' 
      System.debug(s); 
      return s; 
    }

    ...

}
```

Now say, for example, that every time we `instantiate` (fancy term for "creating an instance of") a `SayHello` object we want to declare a default String, we could do that via an explicit constructor: 

```java
public class SayHello{

    private String default; 

    // this time our constructor will take an argument
    public SayHello(String defaultStr){
        default = defaultStr; 
    }

    // note that the implicit zero-arg constructor is always
    // available, unless we explicitly override it as below:
    public SayHello(){
        default = 'zero arg is overwritten'; 
    }

    public String sayHi(){
      String s = 'Hello, World' 
      System.debug(s); 
      return s; 
    }

    public String getDefault(){
        return default; 
    }

    ...

}
```

Now the "caller" (any code that creates an instance of `SayHello`) can choose to provide a default String upon creation, like so:

```java
SayHello sayHello = new SayHello('My Default String');
system.debug(sayHello.getDefault()); 
// prints 'My Default String' to the Apex debug log
```

There are some other nice things you can do with constructors, such as overloading. To learn more check out the [Using Constructors](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_classes_constructors.htm) page of the Apex Developer Guide.

## Wrap up
We covered a lot this week, from fundamental CS concepts all the way to Apex trigger delegation. If your head is spinning right now - don't sweat it. Practice makes perfect, and the best way to get the concepts to "stick" is to head on over to the [Week2 Apex Challenges](../psets/week2.md) and dive in. 

But first, give yourself a big self-five for sticking it out this far. Exploring these concepts will make you a better Salesforce developer, general developer and problem-solver-in- general!  

![lizLemonade](https://media.giphy.com/media/ujGfBmVppmgEg/giphy.gif)

## Related Content

### Read
- [Trailhead: Apex Domain and Selector Layers](https://trailhead.salesforce.com/en/content/learn/modules/apex_patterns_dsl)
- [Trailhead: Apex Service Layer](https://trailhead.salesforce.com/en/content/learn/modules/apex_patterns_sl)

### Watch
- [Financial Force DF'12 Session: Enterprise Design Patterns](https://docs.google.com/file/d/0B6brfGow3cD8UHhzWDF1WENEaXc/preview?pli=1)

#### CS50 Shorts
- [Arrays](https://www.youtube.com/embed/K1yC1xshF40?autoplay=1&rel=0)
- [Algorithms Summary](https://www.youtube.com/embed/ktWL3nN38ZA?autoplay=1&rel=0)

### Code
- [Financial Force DF'12 Apex Enterprise Patterns Repo](https://github.com/financialforcedev/df12-apex-enterprise-patterns)
