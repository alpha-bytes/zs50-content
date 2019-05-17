# Week 4: Apex Challenge

ZS50 week4 was all about writing efficient code, whether by harnessing existing functionality available in the `Apex Standard Library` or implementing our own code that leverages `inheritance`.

In this week's challenge we'll tap into both concepts by implementing an interface from the standard library, while calling back to a few concepts we explored in prior weeks. Let's jump in!

## comparable
In week2 we touched on the ability to tell the Salesforce runtime, via the `equals()` and `hashcode()` methods, how to determine the uniqueness of objects constructed from our custom types. In this challenge, we'll tell Salesforce how to *sort* our custom types. How? By implementing the [Comparable](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_comparable.htm#apex_comparable) interface.

> **Scenario**
>
> Remember the `LeadEnrichmentService` class we implemented last week? Further review of the documentation for the `HttpEnrichmentService` class we call from it has revealed that the `Map<String, String>` (companies -> industry) it returns is first sorted alphabetically by company name. 
>
> Lead volume is increasing and we want to make sure our algorithms are operating at maximum efficiency 🚗. To that end, we'd like to also sort our `List<Lead>` by company before retrieving industry from the returned `Map`. 
> 
> Of course, we can't edit the `Lead` class itself since it's part of the Standard Library. What we *can* do is implement a wrapper class around our Leads, and implement **Comparable** on that...

To successfully complete this challenge: 

1. If you didn't successfully complete the [week3](week3.md) challenge, do that first! You'll need the code written there to get a `success` on this challenge. 
2. Create a class `LeadWrapper` that `implements` the `Comparable` interface. The class should have a single-argument constructor which accepts an argument of type Lead, and sets the following instance variables:
    * `public Lead theLead` (set to the passed-in Lead)
    * `public String company` (set to the passed in Lead's Company value)
2. Note that the `compareTo` method takes a single argument of type `Object` (the superclass of all complex data types in Apex). To implement this logic, you'll need to `cast` the **compareTo** argument to `LeadWrapper` in your implementation.
3. Comparison should be based on the **first character** (good enough for our purposes) of the `LeadWrapper.company` values, without respect to case, such that:   
    * Return `-1` if `this` instance's first char comes **before compareTo**'s first char in the alphabet, 
    * Return `1` if `this` instance's first char comes **after compareTo**'s first char in the alphabet,
    * In all other cases, return `0` (first chars must be the same)
4. Refactor your `LeadEnrichmentService.enrich()` method as follows: 
    * Instantiate a variable of type `List<LeadWrapper>` named **wrappers**.  
    * For each Lead passed to your method, construct a `LeadWrapper` instance and add it to **wrappers**
    * Call the `wrappers.sort()`
    * Iterate through **wrappers**, getting the industry from the Map returned by `HttpEnrichmentService` based on company, and setting each Lead's Industry variable to it (e.g. `LeadWrapper.theLead`). 
5. This challenge will validate that logic implemented last week still returns that same result, plus the additional pre-sorting implemented through `LeadWrapper`. Remember, we're `refactoring`, which changes only the implementation details, not the outcome.  

**Hint: Remember back to week1 when we discovered how characters correspond to integers in encoding schemas like `ascii` and `utf-8`.** Make sure to check the [String Class](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_methods_system_string.htm) documentation to see if there's an easy way to convert that first char into a more useful Integer for comparison. 

If you need a little extra help on this challenge, make sure to check out the [Comparable Example Implementation](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_comparable.htm?search_text=sortable#apex_comparable_example) in the Apex Developer Guide. And, as always, take advantage of the `scaf` command in the `zs50` CLI to get up-and-running with less overhead.




