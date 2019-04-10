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

In fact, there are several means by which Salesforce forces us to implement bulkified code. One of those approaches is through Apex Triggers and the [context variables](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_context_variables.htm) Salesforce provides to us within a given trigger. 



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
    - library reference (architecture)
```

### Sorting
```
- implement in Apex?
    - no, but can make more efficient 
    - hashcode(), equals() [refdoc](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_collections_maps_keys_userdefined.htm)
```

### `Static` Apex Methods


## Related Content

### Read

### Watch

#### CS50 Shorts
- [Arrays](https://www.youtube.com/embed/K1yC1xshF40?autoplay=1&rel=0)
- [Algorithms Summary](https://www.youtube.com/embed/ktWL3nN38ZA?autoplay=1&rel=0)
- [Merge Sort](https://www.youtube.com/embed/Ns7tGNbtvV4)
