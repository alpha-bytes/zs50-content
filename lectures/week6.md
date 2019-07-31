# Week 6: ~~Python~~ Design Patterns

## CS50 Week 6
Use the links below to catchup on CS50's week6 content: 

- [Lecture](https://www.youtube.com/watch?v=mvlTSMUNQN4)
- [Notes](https://cs50.harvard.edu/college/weeks/6/notes/)

## ZS50 Week 2

CS50 week6 covered Python, a widely used programming language which has grown increasingly popular. While it's a great general purpose programming language to have in your toolbelt, there's not much new fodder in this week's CS50 content to translate over to Apex.

So, this week we'll blaze our own trail and talk about how we go about organizing our Apex codebase(s) in a logical, structured manner. That is, how we can leverage something called `design patterns` to write code that is scalable, flexible and maintainable. 

Note: there won't be any Apex challenges this week, so take the opportunity to catch up on prior weeks if need be. 

### Design Patterns - An Introduction

When we say the term `design pattern`, you may be picturing this: 

![bobRoss](https://media.giphy.com/media/AbPNdmgk6TJK/giphy.gif)

In a way, actually, it's not that far off! Just like a painting needs layers to make a cohesive whole, so too can your codebase benefit from the same type of "layering" mindset. 

Think about it this way. When you create an Apex class, you're doing so to achieve a certain goal. That's the class's purpose, and you probably give it a descriptive name to make that purpose clear. Over time, you begin to accumulate many classes that solve the same types of problems. Said another way, they share the same ["areas of concern"](https://en.wikipedia.org/wiki/Separation_of_concerns). 

This is where design patterns come into play. They *emerge* as the outcomes of tried-and-true approaches for solving the same types of problems in software development. This is an important point to remember, because the whole topic can seem academic, so we'll reiterate: 

> Design patterns are not *created*. They *emerge* as the result of practical approaches to solving similar problems. 

So really, like programming languages themselves, design patterns are a type of abstraction that we have inherited from our programming forebearers. They make life easier by providing us blueprints of sorts for solving common problems in software development. 

### Design Patterns in Apex

Salesforce development is particularly well-suited to design pattern implementation, in any org, because the underlying structure is the same everywhere. This is a huge windfall for us as Salesforce developers because we don't have to reinvent the wheel. There is a ton of code organization and layering baked right into the platform.

Let's have an example. Remember the SObject class we reviewed in [week4](./week4.md)? Each of its subclasses (`Account`, `Case`, `My_Custom_Object__c`, etc.) are an implementation of the [Object Relational Mapper](https://en.wikipedia.org/wiki/Object-relational_mapping) pattern. Their area of concern is data and, more specifically, providing a progamming-friendly means of accessing and modifying data stored in the underlying database tables. 

You may be thinking, *"Okay, okay, I see the benefits of the organization that Salesforce implements, but is there really a need for me to do the same with my custom code?"*. The answer, quite simply, is **yes**! That is, if you want to write scalable, flexible and maintainable code 😉

### Design Pattern All The Things!

So, before you sit down and write any Apex code, you should find an appropriate pattern to implement first, right? No. Like abstractions in the code itself, design patterns are generally best left for **refactor** time, when a solution to a common concern has *emerged* as a need. 

#### ...Well, At Least *Some* of the Things

We touched above on the fact that all Salesforce orgs share the same underlying platform and code organization (standard library). More specifically, almost everything we do *in any org* revolves around the manipulation of SObjects, either directly or indirectly. 

This SObject centricity sets us up nicely to implement a common set of architectural layers known as the `Service`, `Domain` and `Selector` layers. These layers provide a separation of concerns that make our Apex code more modular and durable. 

We could devote paragraphs (and paragraphs, and paragraphs...) to the discussion of these layers, and understanding them is important. Luckily for this writer, however, there are already a couple of great Trailhead modules covering [Separation of Concerns and the Service Layer](https://trailhead.salesforce.com/en/content/learn/modules/apex_patterns_sl) and the [Domain and Selector Layers](https://trailhead.salesforce.com/en/content/learn/modules/apex_patterns_dsl). 

Consider these as "imports" into this week's notes. They're required reading to grasp the concepts, so head on over to Trailhead to complete them. We can wait...

![cIsForCodeAndCookies](https://media.giphy.com/media/o5oLImoQgGsKY/giphy.gif)

Back already! You're fast 😉. 

Okay, so now that you have a good understanding of each of the layers, here's the exception to the **refactor** rule mentioned above: 

> I recommend you implement the Service / Domain / Selector pattern in every single org in which you deploy code bound for production. 

Even if the Salesforce org you're working in is small, with relatively little code, you should still implement at least the `Domain` and `Selector` layers. Why? Because they are so universal and applicable to the nature of how Salesforce's *core* is organized (SObject-centric) that they will set you up for faster code development that more dependably stands the test of time. 

Additionally, if you plan to implement code modularization through packaging ([ref1](https://trailhead.salesforce.com/en/content/learn/modules/unlocked-packages-for-customers), [ref2](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_dev2gp.htm)) - an advanced topic beyond the scope of this class, but a **highly recommended** approach - then these layers will form the backbone of your code organization and separation of dependencies.

## Wrap Up

If this week's content seemed brief for such a large topic, that brevity was intentional. Design patterns, while a powerful tool to put in your dev toolbelt, can be overwhelming if you're just starting out. 

So, to condense this week's lesson into some TL;DR takeaways: 
1. Implement the `Domain / Selector` layers in **every** org in which you write production-bound code; *otherwise*
3. Always seek the simplest solution first - the need for more advanced solutions or abstractions will **emerge** over time; *and*
2. Generally, leave design pattern implementation for **refactor** time. 

Happy coding!

## Related Resources

- [Enterprise Layering](https://martinfowler.com/bliki/PresentationDomainDataLayering.html) 
- [*Patterns of Enterprise Architecture* by Martin Fowler (Amazon)](https://www.amazon.com/gp/product/0321127420)
- [Trailhead: Separation of Concerns and the Service Layer](https://trailhead.salesforce.com/en/content/learn/modules/apex_patterns_sl)
- [Trailhead: Domain and Selector Layers](https://trailhead.salesforce.com/en/content/learn/modules/apex_patterns_dsl)
