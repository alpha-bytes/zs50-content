# Week 5: Apex Challenge

In week5 we learned about the protocols underpinning the modern web including `http`. We also saw features of the Apex language that make it possible to interact with, and provide, RESTful APIs, specifically:
* outbound, by way of the `Http`, `HttpRequest` and `HttpResponse` classes
* inbound, via Apex REST

In this week's challenge, we'll dust off our refactoring skills to interact with some external APIs. 

## timeToREST

In last week's challenge we updated our `LeadDomain` class to operate more efficiently via implementation of the `Comparable` interface. This was done because the service class it called, `HttpEnrichmentService`, first sorted its results alphabetically before returning a response from its `enrich()` method. 

Well, the times they are a-changin'. Turns out that class calls a version of an external API that is now deprecated, and all callouts are now resulting in `404` responses. We don't like [400 responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) here - we're all about the golden `200`s.

So, your task is to refactor the `HttpEnrichmentService` itself, using a new API we have in hand. Ready to go? Hit up that `scaf` command from the zs50 cli to get started. To successfully complete this challenge:

1. Internally, the `enrich` method calls the private `callout` method and returns the industry from its result. Modify the internal implementation in this method to utilize the new-and-improved **v2** endpoint, passing in as a query parameter the name of each company (e.g. `https://cocky-bartik-095df3.netlify.com/.netlify/functions/v2company?company=<companyName>`). Any requests without a company parameter will return `404`. 
2. The new endpoint returns additional company info (`catchPhrase`, `latitude`, `longitude`) in addition to `industry`. Add these fields to the `ResultWrapper` class and make sure they get initialized during construction (i.e. when the `new` keyword is called) inside this method.  
3. Business has requested that we leverage these new fields by adding them to the Lead record (`catchPhrase` => Lead.Description, address fields => Lead.<addressField>). Rather than modifying the current `enrich` method - we're keeping our code [SOLID](https://en.wikipedia.org/wiki/SOLID), after all - add a new `enrichFull` method to the `HttpEnrichmentService` that returns `Map<String, ResultWrapper>`, and update your `LeadEnrichmentService` class from last week to call this new method and persist the fields to each Lead record appropriately.