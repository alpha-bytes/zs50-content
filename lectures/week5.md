# Week 5: HTML, HTTP, CSS

## CS50 Week 5
Use the links below to catchup on CS50's week3 content: 

- [Lecture](https://www.youtube.com/watch?v=uEmF74eHRO8)
- [Notes](https://cs50.harvard.edu/college/weeks/5/notes/)

## ZS50 Week 5

CS50 did a good job of describing the basic circuitry of the internet in their week5 discussion, so we'll focus on filling out the picture with a bit more depth and Salesforce-specific context. 

### HTTP

HTTP, the `Hypertext Transfer Protocol`, is something we utilize literally every day in modern life. As enterprises move their mission-critical applications off-premise and into the cloud, it's also become an increasingly important piece of technology for the global economy. Of course, as Salesforce developers we already know this: 

![neilTyson](https://media.giphy.com/media/zKsfeEYZI4Ku4/giphy.gif)

So HTTP is kind of a big deal. But it's just one of many protocols that lives atop two other technologies which, due to their ubiquity in networking applications, are almost always discussed together: `IP` (Internet Protocol) and `TCP` (Transmission Control Protocol). 

CS50 touched on these lightly and, really, they're low-level enough that we don't need to know too much about them. Suffice to say that IP dictates the route by which a client and server find each other as well as the means by which the data they exchange gets "chunked" into smaller `packets`. TCP, on the other hand, dictates where those packets get sent once they are on the requested machine. An easy analogy might be to think of IP as a large carrier service like UPS or Fedex delivering a package to a large apartment complex. They deliver the package to the front desk, but it's the concierge (this is a fancy apartment 😉) - TCP - takes that package the remainder of the journey to its final destination in the building. 

So, if `TCP/IP` handles the bundling and delivery of data on the web, what does HTTP do? It defines the rules around how clients *request* resources, and the *response* fromat that the receiving server should send. 

- Decoding Hypertext Transfer Protocol (HTTP)
    - computers only understand bits, and can only send them
    - HTTP is a protocol that interprets those bits *only* as text
    - the `hyper` simply means the transmitted text represents some higher-order concept
- What about `HTTPS`? 
    - still just HTTP, with `SSL` atop it
- HTTP Status Codes

### Document Object Model (DOM)

- How related to HTML
    - hierarchical tree (remember data structs) created from the current HTML page, nested according to parent (e.g. `<body>` tag) and child (e.g. `<div>`, `<h1>`, etc.) `nodes`
- Lightning Components
    - implement security by "sandboxing" components
    - means they do not have direct access to the DOM, but only a representation of it called a "virtualized DOM" 

### JavaScript (cntd...)

- Asynchronous execution (event loop)
    - Apex, generally, executes `synchronously`
        - certain cases where async execution makes more sense
        - tools to do so `batch`, `scheduled`, `queueable` [matrix]
- Object Oriented?
    - Yes, but on steroids
    - Less prescriptive than Apex (or Java)
    - Functions themselves are objects (enables passing functions as arguments, anonymous functions, etc.)
- Much more than a "front end" (i.e. browser based) language these days
    - since emergence of [Node.js](https://nodejs.org/en/), runtime engine for JS
    - now used frequently as central "back end" (i.e. server-side) application lang

### Salesforce Rediscovered

- Can of course be experienced through the UI

#### Take a `REST`

- A ton of additional functionality
- `Representational State Transfer` 
    - popular web services paradigm
    - has largely replaced older and heavier SOAP protocol (though not completely) and the less standardized Remote Process Invocation (RPI) paradigm
    - each endpoint is a `resource`
    - each resource can support any of the HTTP `verbs`
- Experience REST (Workbench)

#### Outbound Web Services in Apex

#### Inbound Web Services in Apex


## Related Content

### Read
- [The Virtual DOM](https://bitsofco.de/understanding-the-virtual-dom/)
- [Codecademy: What is REST](https://www.codecademy.com/articles/what-is-rest)

### Watch

- CS50 Shorts
    - [Internet Protocol (IP)](https://www.youtube.com/embed/A1g9SokDJSU?autoplay=1&rel=0)
    - [Transmission Control Protocol (TCP)](https://www.youtube.com/embed/GP7uvI_6uas?autoplay=1&rel=0)
    - [Hyper Text Transfer Protocol](https://www.youtube.com/embed/4axL8Gfw2nI?autoplay=1&rel=0)
    - [JavaScript](https://www.youtube.com/embed/Z93IaNfavZw?autoplay=1&rel=0)
