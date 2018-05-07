# Introduction
The aim of this task is to get you started with developing applications for Node.js, teaching you everything you need to know about “advanced” JavaScript along the way. It goes way beyond your typical “Hello World” tutorial. Upon finishing this task, you will have created a complete web application which allows the users of this application to view web pages and upload files.
Which, of course, is not exactly world-changing, but we will go some extra miles and not only create the code that is “just enough” to make these use cases possible, but create a simple, yet complete framework to cleanly separate the different aspects of our application. You will see what I mean in a minute.
We will start with looking at how JavaScript development in Node.js is different from JavaScript development in a browser.
Next, we will stay with the good old tradition of writing a “Hello World” application, which is a most basic Node.js application that “does” something.
Then, we will discuss what kind of “real” application we want to build, dissect the different parts which need to be implemented to assemble this application, and start working on each of these parts step-by-step.
As promised, along the way we will learn about some of the more advanced concepts of JavaScript, how to make use of them, and look at why it makes sense to use these concepts instead of those we know from other programming languages.
Node.js really is just another context: it allows you to run JavaScript code in the backend, outside a browser.
In order to execute the JavaScript you intend to run in the backend, it needs to be interpreted and, well, executed. This is what Node.js does, by making use of Google’s V8 VM, the same runtime environment for JavaScript that Google Chrome uses.
Plus, Node.js ships with a lot of useful modules, so you don’t have to write everything from scratch, like for example something that outputs a string on the console.
Thus, Node.js is really two things: a runtime environment and a library.
## Installation
In order to make use of these, you need to install Node.js. Instead of repeating the process here[https://nodejs.org/en/download/package-manager/]

## "Hello World!"
Ok, let’s just jump in the cold water and write our first Node.js application: “Hello World”.
Open your favorite editor and create a file called helloworld.js. We want it to write “Hello World” to STDOUT, and here is the code needed to do that:
console.log("Hello World");
Save the file, and execute it through Node.js: node helloworld.js
This should output Hello World on your terminal.
Ok, this stuff is boring, right? Let’s write some real stuff.
# Let's Start
## The use Cases
Let’s keep it simple, but realistic:
* The user should be able to use our application with a web browser
* The user should see a welcome page when requesting http://domain/start which displays a file upload form
* By choosing an image file to upload and submitting the
form, this image should then be uploaded to http://domain/upload, where it is displayed once the upload is finished
Fair enough. Now, you could achieve this goal by googling and hacking together something. But that’s not what we want to do here.
Furthermore, we don’t want to write only the most basic code to achieve the goal, however elegant and correct this code might be. We will intentionally add more abstraction than necessary in order to get a feeling for building more complex Node.js applications.

## The application stack
Let’s dissect our application. Which parts need to be imple- mented in order to fulfill the use cases?
* We want to serve web pages, therefore we need an HTTP server
* Our server will need to answer differently to requests, depending on which URL the request was asking for, thus we need some kind of router in order to map requests to request handlers
* To fullfill the requests that arrived at the server and have been routed using the router, we need actual request handlers
* TherouterprobablyshouldalsotreatanyincomingPOST data and give it to the request handlers in a convenient form, thus we need request data handling
* We not only want to handle requests for URLs, we also want to display content when these URLs are requested, which means we need some kind of view logic the request handlers can use in order to send content to the user’s browser
* Last but not least, the user will be able to upload images, so we are going to need some kind of upload handling which takes care of the details
Let’s think a moment about how we would build this stack with PHP. It’s not exactly a secret that the typical setup would be an Apache HTTP server with mod_php5 installed.
Which in turn means that the whole “we need to be able to serve web pages and receive HTTP requests” stuff doesn’t happen within PHP itself.
Well, with node, things are a bit different. Because with Node.js, we not only implement our application, we also implement the whole HTTP server. In fact, our web application and its web server are basically the same.
This might sound like a lot of work, but we will see in a moment that with Node.js, it’s not.
Let’s just start at the beginning and implement the first part of our stack, the HTTP server

# Let's Code

### A basic HTTP server
When I arrived at the point where I wanted to start with my first “real” Node.js application, I wondered not only how to actually code it, but also how to organize my code.
Do I need to have everything in one file? Most tutorials on the web that teach you how to write a basic HTTP server in Node.js have all the logic in one place. What if I want to make sure that my code stays readable the more stuff I implement?
Turns out, it’s relatively easy to keep the different concerns of your code separated, by putting them in modules.
This allows you to have a clean main file, which you execute with Node.js, and clean modules that can be used by the main file and among each other.
So, let’s create a main file which we use to start our application, and a module file where our HTTP server code lives.
My impression is that it’s more or less a standard to name your main file index.js. It makes sense to put our server module into a file named server.js.
Let’s start with the server module. Create the file server.js in the root directory of your project, and fill it with the following code:
```
var http = require("http");
http.createServer(function(request, response) { response.writeHead(200, {"Content-Type": "text/plain"}); response.write("Hello World");
response.end();
}).listen(8888);
```
That’s it! You just wrote a working HTTP server. Let’s prove it
by running and testing it. First, execute your script with Node.js:
```node server.js```
Now, open your browser and point it at http://localhost:8888/2. This should display a web page that says “Hello World”.
That’s quite interesting, isn’t it. How about talking about what’s going on here and leaving the question of how to organize our project for later? I promise we’ll get back to it.

## Analyzing our HTTP server
Well, then, let’s analyze what’s actually going on here.
The first line requires the http module that ships with Node.js
and makes it accessible through the variable http.
We then call one of the functions the http module offers: cre-
ateServer. This function returns an object, and this object has a 2http://localhost:8888/
 
Building the application stack 12
method named listen, and takes a numeric value which indicates the port number our HTTP server is going to listen on.
Please ignore for a second the function definition that follows the opening bracket of http.createServer.
We could have written the code that starts our server and makes it listen at port 8888 like this:
```
var http = require("http");
var server = http.createServer();
server.listen(8888);
```
That would start an HTTP server listening at port 8888 and doing
nothing else (not even answering any incoming requests).
The really interesting (and, if your background is a more con- servative language like PHP, odd looking) part is the function definition right there where you would expect the first parameter of the *createServer()* call.
Turns out, this function definition IS the first (and only) parame- ter we are giving to the *createServer()* call. Because in JavaScript, functions can be passed around like any other value.









