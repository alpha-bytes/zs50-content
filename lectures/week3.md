## Week 3: Memory

### CS50 Week 3
Use the links below to catchup on CS50's week3 content: 

- [Lecture](https://www.youtube.com/watch?v=cC9I3XxkZXw)
- [Notes](https://cs50.harvard.edu/college/weeks/3/notes/)

### ZS50 Week 3

CS50 week3 took a deep-dive into memory, the place in a computer's hardware architecture where the magic of our programs' algorithms are stored, if only ephemerally. 

This week we'll apply many of those same concepts to developing on the Salesforce platform. 

### Memory in Salesforce

When writing Apex, we don't need to worry about low-level memory interactions like we do via `C`'s `malloc` (reserve memory) or `free` (give memory back) functions, but we are absolutely still bound by the same fundamental constraint: computing resources are finite. 

#### Limitations

In week2 we touched on some of the issues you might encounter relative to this constraint. 

- heap 
- stack (frames)
- cpu time
- link to apex governors

### Passing Variables in Apex

#### Pass by Copy

#### Pass by Reference (sort of)

- reference 'swap' from cs50
- [Dev Blog](https://developer.salesforce.com/blogs/developer-relations/2012/05/passing-parameters-by-reference-and-by-value-in-apex.html)

## Related Content

### Read

### Watch

#### CS50 Shorts

- [Call Stakcs](https://www.youtube.com/embed/aCPkszeKRa4?autoplay=1&rel=0)
- [Dynamic Memory Allocation](https://www.youtube.com/embed/xa4ugmMDhiE?autoplay=1&rel=0)
- [File Pointers](https://www.youtube.com/embed/bOF-SpEAYgk?autoplay=1&rel=0)
- [Hexadecimal](https://www.youtube.com/embed/u_atXp-NF6w?autoplay=1&rel=0)
- [Pointers](https://www.youtube.com/embed/XISnO2YhnsY?autoplay=1&rel=0)
- [Recursion](https://www.youtube.com/embed/mz6tAJMVmfM?autoplay=1&rel=0)

### Code