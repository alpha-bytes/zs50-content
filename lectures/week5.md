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

CS50 touched on these lightly and, really, they're so low-level that we don't need to know too much about them. Suffice to say that IP dictates the route by which a client and server find each other as well as the means by which the data they exchange gets "chunked" into smaller `packets`. TCP, on the other hand, dictates where those packets get sent once they are on the requested machine. An apt analogy is to think of IP as a carrier service like UPS that is delivering a package to a large apartment complex. UPS (IP) delivers the package to the front desk, but it's the concierge (TCP) that ferries it to its final destination *inside* the building (it's a fancy apartment complex 😉).

So, if `TCP/IP` handles the bundling and delivery of data on the web, what does HTTP do? It defines the rules around how clients *request* resources and the *response* format that the server must implement. These rules cover a number of things, but we'll focus on two: `request methods`, and request and response `bodies`. 

#### Request Methods

Requests on the web can be made by a client via a limited amount of methods. Method in this context isn't the same thing as a method defined in an Apex class - it simply refers to the type of request being made (sometimes referred to as the request "verb"). Some of the most common are: 

- `GET`: request a resource from server with no other effects
- `POST`: create a new instance of the given resource type on the server
- `PUT`: update an existing resource on the server
- `DELETE`: remove a resource from the server

You'll notice all the methods above revolve around `resources`. That is because every - *every* - URL on the web represents a resource. That fact is actually made plain in the acronym itself - [Uniform Resource Locator](https://en.wikipedia.org/wiki/URL#Syntax). Whether you're browsing a website or interacting with an API from the command line, if you're using HTTP, you're acting on resources.

#### Bodies

Like an HTML page, HTTP messages (requests and responses) consist of a [`header`](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields), representing the metadata of the message and, optionally, a `body` containing message data. 

So what's in a body, and what's it used for? Let's start with the second part of the question. Remembering that we work on resources over HTTP, message bodies are most often used to represent a resource that needs to be created or updated (`POST`, `PUT`), or that is returned from the server (`GET`). 

The `content-type`, the data that makes up a body, is dependent on the server resource being acted upon. There are many valid types and Mozilla Developer Network (MDN) lists some of the more common ones [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Complete_list_of_MIME_types). 

The one that you really need to know, though, and which is used in the vast majority of web services built today, is `JSON` (Javascript Object Notation). Despite its name, you don't need to learn Javascript to work with JSON. It is a language-independent data exchange format. 

[JSON](https://www.w3schools.com/js/js_json_intro.asp) is a lightweight format that is simply a collectin of name / value pairs. Names must be enclosed in double quotes (`"`), and values must be one of a handful of valid "primitive" types, including `string`, `number`, and `boolean`, as well as compound types `object` or `array`. Let's take a look at a simple example that might be used to represent the ZS50 class: 

```json
{
    "locale": "english",
    "primaryLanguage": "Apex", 
    "additionalLanguages": ["C", "Java", "Javascript"],
    "exuberantUseOfGifs": true, 
    "cornyPuns": true,
    "urls": [
        { "baseUri": "https://alpha-bytes.github.io/zs50-content" },
        { "challenges": "/psets/list.html" },
        { "lectures": "/lectures/list.html" }
    ]
}
```

#### Time for a `REST`

Let's review what we've covered so far: 
- every URL on the web represents a resource
- those resources are of some specific data type
- many APIs today utilize the JSON data type to represent resources



- A ton of additional functionality
- `Representational State Transfer` 
    - popular web services paradigm
    - has largely replaced older and heavier SOAP protocol (though not completely) and the less standardized Remote Process Invocation (RPI) paradigm
    - each endpoint is a `resource`
    - each resource can support any of the HTTP `verbs`
- Experience REST (Workbench)

#### Outbound Web Services in Apex

#### Inbound Web Services in Apex

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

## Related Content

### Read
- [HTTP Messages - Mozilla Developer Network (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages)
- [Introduction to JSON - W3 Schools](https://www.w3schools.com/js/js_json_intro.asp)
- [JSON.org](https://www.json.org/)
- [Codecademy: What is REST](https://www.codecademy.com/articles/what-is-rest)

### Watch

- CS50 Shorts
    - [Internet Protocol (IP)](https://www.youtube.com/embed/A1g9SokDJSU?autoplay=1&rel=0)
    - [Transmission Control Protocol (TCP)](https://www.youtube.com/embed/GP7uvI_6uas?autoplay=1&rel=0)
    - [Hyper Text Transfer Protocol](https://www.youtube.com/embed/4axL8Gfw2nI?autoplay=1&rel=0)
    - [JavaScript](https://www.youtube.com/embed/Z93IaNfavZw?autoplay=1&rel=0)
