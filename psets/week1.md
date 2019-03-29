## Week 1: Apex Challenges
In week1 we'll start in the shallow end of the Apex pool, with some basic method construction, intake of arguments, and String manipulation. Strings are one of the [primitive data types](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_primitives.htm) available in Apex. 

Once you're ready to check an element of this challenge, just enter the following command into the terminal at your project directory's location (e.g. if you're using a [ZS50 Sandbox](../setup/sandbox.md) the terminal will launch in the correct location): 

```
zs50 check sayHello
```

Remember, you can also "scaffold" a blueprint class into your terminal window to help you get started via the command: 

```
zs50 scaf sayHello
```

## SayHello class
Create a public class `SayHello` and, inside it...

### sayHi
Implement a method `sayHi` that: 
- Initialize a String type variable to 'Hello, World'
- write it to the Apex debug logs (hint: see the Apex [System.debug()](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_methods_system_system.htm))
- return the string

### sayMyName
Implement a method `sayMyName` that: 
- returns a String set to your name. 

### speak
Implement a method `speak` that: 
- takes a single String as an argument
- checks to make sure the argument is not `null`
    - if it is, return a String 'Invalid argument`. 
    - else: 
        - return the String, AFTER converting it to all lowercase (hint: see Apex [String](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_methods_system_string.htm) class)


Once your class and methods pass all validations you'll see a nice, if anti-climactic, little `Success!` message. 
