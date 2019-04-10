## Week 2: Arrays

### CS50 Week 2
Use the links below to catchup on CS50's week1 content: 

- [Lecture](https://www.youtube.com/watch?v=u-kH-5JJSgU)
- [Notes](https://cs50.harvard.edu/college/weeks/2/notes/)

### ZS50 Week 2

#### Compilation
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

#### Triggers and Trigger Context Variables
In fact, there are several means by which Salesforce forces us to implement bulkified code. One of those approaches is through Apex Triggers and the [context variables](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_context_variables.htm) Salesforce provides to us within a given trigger. 

We're not going to explicitly cover writing an Apex Trigger here (though you may want to bone up on the [syntax](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_syntax.htm) for the weekly Apex Challenge). Rather, what's important to discuss is that, depending on what event context your trigger is operating in (e.g. `before insert`, `after update`, etc.), many of the context variables available to you are collection types. 

Let's explore one such variable: `Trigger.new`. This variable is available to all triggers defined for *insert*, *update* or *undelete* events. So what is it? `Trigger.new` is an ordered [List](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_collections_lists.htm) of all SObjects of the specified type that *a given User* has initiated a specific dml operation against. 

*Queue crickets...*

Okay, this one is probably best illustrated with an example. Say you've defined the following Contact trigger: 
```java
trigger ContactTrigger on Contact (before insert){
    // do stuff
}
```

This trigger is going to "fire" every time a user attempts to insert one ore more Contact records. 

> Quick aside:
>
> Triggers can be defined to fire either before or after records are saved (but not committed) to the database, hence the `before` and `after` contexts for most dml events. There are some important [considerations](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_context_variables_considerations.htm) when deciding which context to use. Getting familiar with Salesforce's save [order of execution](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_order_of_execution.htm) is also highly recommended. 
> 
> Onward.

Now let's say one of your org's users has just mass-inserted 500 new Contacts using the Data Import Wizard (*note to self: remove user mass import permissions..*). Expanding the trigger we started above, what number is going to get written to the debug logs on line 6 below?  

```java
trigger ContactTrigger on Contact (before insert){
    Integer counter = 0; 
    for(Contact c : Trigger.new){
        counter++; 
    }
    System.debug(counter); 
}
```

If you said `500`, you're today's big winner! 

![winner](https://media.giphy.com/media/3ohhwGl5urKvvygu08/giphy.gif)






```
- types of collections (list, set, map)
    - hashcode
- implicit, or requisite, collection contexts
    - triggers (link to context vars)
    - declarative automations (link to automation order)
    - batch apex
- triggers
    - issues with triggers
    - handler, best practice
    - library reference (architecture) (sforce)
```

### Sorting
```
- implement in Apex?
    - no, but can make more efficient 
    - hashcode(), equals() [refdoc](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_collections_maps_keys_userdefined.htm)
```

### Debugging
CS50 week2 covered this topic a bit and we've glossed over it here to focus on more core concepts, but it's an important aspect of developing in Apex (or any language). The term `debugging` may sound like something you do after a jungle safari and, in a way, it's similar. It's the process of locating and removing bugs - unwanted behavior - from the code you write. 

There are various methods and tools you can utilize for debugging Apex code, some of which is covered in the [Debugging Apex](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_debugging.htm) section of the Apex Developer Guide. 

For now, the tool you'll want in your toolbox is the `System.debug(String s)` method, which simply writes a provided String to the debug logs that get generated during a transaction. 

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

Now, say every time we `instantiate` (fancy term for "creating an instance of") a `SayHello` object we want to let the caller declare a default String, we could do that via an explicit constructor: 

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
SayHello sh = new SayHello('My Default String');
system.debug(sh.getDefault()); 
// prints 'My Default String' to the Apex debug log
```

There are some other nice things you can do with constructors, such as overloading

## Related Content

### Read
- [Apex Collection Types](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_collections.htm)

### Watch

#### CS50 Shorts
- [Arrays](https://www.youtube.com/embed/K1yC1xshF40?autoplay=1&rel=0)
- [Algorithms Summary](https://www.youtube.com/embed/ktWL3nN38ZA?autoplay=1&rel=0)
- [Merge Sort](https://www.youtube.com/embed/Ns7tGNbtvV4)
