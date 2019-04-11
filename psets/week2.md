# Week 2: Apex Challenges
In the week 2 challenges, we'll tie together our general knowledge of collections like `arrays` and Apex `lists` with specifics of developing for the Salesforce platform, such as Trigger delegation and handling. 

Remember, make use of the `zs50 scaf <psetName>` command to help you get started!

## loopy
Many languages have an operator for the arithmetic Modulus[https://en.wikipedia.org/wiki/Modulo_operation] operation. Apex? Not so much. 

But that doesn't mean we can't implement it! For this challenge, you'll create a public class `Loopy` with a method `modulo` that...
1. accepts as arguments two Integers (name them a and b, for simplicity)
2. using a loop, recursive function, or otherwise, return the modulo of a divided by b (e.g. modulo(412, 17) returns 4)

## triggerHappy
Nowwwww we're getting somewhere. In this challenge, create a class `LeadDomain` that acts as a trigger handler for Lead records. 

Right now we'll pass on writing the trigger itself. But, if you decided to, it would look like this: 
```java
trigger LeadTrigger on Lead (before insert){
    LeadDomain ld = new LeadDomain(Trigger.new); 
    ld.doBeforeInsert();
}
```

To pass this challenge, your `LeadDomain` class will need to: 

1. Implement a constructor that...
    - Takes a List<Lead> as an argument
    - Sets the 'leads' instance variable to the list passed into the constructor

2. Implement a method doBeforeInsert() that...
    - Loops through each Lead in 'leads' and, for each
        - Checks if the 'Company' field is blank and, if so, set the field to the Lead's firstName + ' ' + lastName
        - For this challenge, a blank field is considered one that is not just null, but an empty String. 
        - (Hint) Check out the String class in the apex developer guide. 
        - Example - A Lead with firstName='John' and lastName='Smith', and a blank company field value, should have the
       company field set to 'John Smith'. 
    - Does not return a value (e.g. `void`) 

Happy Apex-ing! Yep, it's a word, now. 

![isItThough](https://media.giphy.com/media/Bdws8eJtJMziE/giphy.gif)