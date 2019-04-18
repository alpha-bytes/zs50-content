## Week 3: Memory

### CS50 Week 3
Use the links below to catchup on CS50's week3 content: 

- [Lecture](https://www.youtube.com/watch?v=cC9I3XxkZXw)
- [Notes](https://cs50.harvard.edu/college/weeks/3/notes/)

### ZS50 Week 3

CS50 week3 took a deep-dive into memory, the place in a computer's hardware architecture where the ephemeral magic of our programs live. This week we'll explore how many of those same concepts apply to developing on the Salesforce platform. 

### Memory in Salesforce

When writing Apex, we don't need to worry about low-level memory interactions like we do via `C`'s `malloc` (reserve memory) or `free` (give memory back) functions, but we are absolutely still bound by the same fundamental maxim: computing resources are finite. 

#### Limitations

In week2 we alluded to some of the issues you might encounter relative to this constraint, including those imposed by the [Apex Transaction Governors](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_gov_limits.htm). Here's an interesting example of some code that would run into trouble if you run it in your org: 

```java
public Boolean returnWhenTrue(Boolean isTrue){
    if(isTrue)
        return true; 
    
    return returnWhenTrue(false); 
}

returnWhenTrue(false);
// uh-oh! Running this code results in 'System.LimitException: Maximum stack depth reached: 1001'
```

So what's going on here? Well, `returnWhenTrue()` above is an example of a `recursive` function. That is - its outcome depends on calling itself. 

![wtf](https://media.giphy.com/media/114U9jSRDkSWSk/giphy.gif)

You're probably asking *why in the name of Ned Stark would I put myself through the mental anguish of using a recursive function*. Well, we're not going to do a deep dive of recursion here, but they can be a more elegant and readable way of implementing logic that has to make calculations over many iterations (sound like a loop? I like the way you think). If you *are* interested in learning more, however, check out the link to CS50's recursion short in the *Resources* section below.

**The more important point** this illustrates for us is that the `stack` is still a factor in our Apex development. As it turns out, Salesforce imposes a stack *limit* of 1,000 frames before throwing an exception and exiting the process. 

*Right, okay, but I can see from the example above that `returnWhenTrue()` is an intentionally infinite recursion since the `isTrue` argument will never be true. I would never write anything so heinous.*

You're right! The example is intentionally simple. However, there are instances where you may *unintentionally* create code that is infinitely recursive, so it's always good to be armed with knowledge. That way you can code defensively. 

CS50's discussion covered the concepts of `stack` and `heap` and - no surprise - those are at play in the Salesforce runtime as well. In fact, although we have much more limited access to the actual memory running our Salesforce code, there are a couple places where the 


### Passing Variables in Apex

#### Pass by Copy

#### Pass by Reference (sort of)

- reference 'swap' from cs50
- [Dev Blog](https://developer.salesforce.com/blogs/developer-relations/2012/05/passing-parameters-by-reference-and-by-value-in-apex.html)

> Aside: 
> `Side Effect`. Certain programming paradigms - Functional programming - aim to avoid. Vars `immutable` after assignment. OOP embraces.

## Related Content

### Read

### Watch

#### CS50 Shorts

- [Call Stacks](https://www.youtube.com/embed/aCPkszeKRa4?autoplay=1&rel=0)
- [Dynamic Memory Allocation](https://www.youtube.com/embed/xa4ugmMDhiE?autoplay=1&rel=0)
- [File Pointers](https://www.youtube.com/embed/bOF-SpEAYgk?autoplay=1&rel=0)
- [Hexadecimal](https://www.youtube.com/embed/u_atXp-NF6w?autoplay=1&rel=0)
- [Pointers](https://www.youtube.com/embed/XISnO2YhnsY?autoplay=1&rel=0)
- [Recursion](https://www.youtube.com/embed/mz6tAJMVmfM?autoplay=1&rel=0)

### Code