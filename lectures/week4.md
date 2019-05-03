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
> 3. Classes are only comprised of two things: 
>       - Variables (characteristics of an object)
>       - Methods (things the object can *do*)

Remembering from last week that objects live on the `heap`, and primitives (including object pointers, or references) live on the `stack`. Did we miss anything? We did not. So, with this mental model in mind, we can confidently state that **the totallity of any Apex process is the interplay between objects and primitives**. 

![furSure](https://media.giphy.com/media/HFHovXXltzS7u/giphy.gif)

Okay, that may be obvious but it's worth hammering home the point that there's no magic going on in any code (Apex or otherwise) ever written. And that's great news for us as developers because it means the classes and objects we create can leverage a ton of functionality provided by the Apex language "out of the box".  

### Visiting the (Standard) Library 📖

Most every programming language has a standard library - a bundle of functionality that comes with the language for performing common logic. C has [libc](https://en.wikipedia.org/wiki/C_standard_library); the [Python Standard Library](https://docs.python.org/3/library/) is called just that, as is [Java's](https://docs.oracle.com/javase/7/docs/api/index.html); Javascript has a relatively small [library](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects) but there is a [TC39 Proposal](https://github.com/tc39/proposal-javascript-standard-library) to expand it (see *https://github.com/stdlib-js/stdlib*). 

Apex is no different, and its standard library is located under [Apex Language Reference](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_reference.htm) in the Apex Developer Guide. A couple things to note here: 

1. Classes are organized into logical `namespaces` based on what they do. You can think of a namespace like a folder on your computer, where the classes it contains are the individual files in that folder.
2. In some cases, if there is a naming conflict between a class in one namespace and the class in another, you'll need to write your code to reference the "fully qualified" class name (e.g. *\<namespace\>.\<className\>*), but this doesn't come up all that often. 

Let's take a look at some of the common namespaces and classes you'll use in your daily life as a Salesforce developer. 

#### System Namespace
The System namespace contains some core functionality for working in Apex, including many of the "primitive" data types we've used in previous weeks such as String. 

> Aside: 



### SOLID

# Related Content

## Read

## Watch

### CS50 Shorts

- [Data Structures](https://www.youtube.com/embed/3uGchQbk7g8?autoplay=1&rel=0)
- [Hash Tables](https://www.youtube.com/embed/nvzVHwrrub0?autoplay=1&rel=0)