#webdevSpecific 

This note marks the beginning of part three of the FSO course content, where the focus will shift from front-end engineering to back-end engineering.

We will be working through the building of backend systems using NodeJS, a javascript runtime based on google's V8 engine.

The course material is written with v20.11.0 of Node, don't attempt to follow examples here with an older version, and bear in mind that future versions _may_ also break it. At time of writing of these notes, I am using v20.17.0, so hopefully there haven't been any breaking changes lol.

## Hello world node app
Here we will be looking at building a simple back end for the notes application used as an example in part two (or the phonebook app used in the exercises). Before getting into that properly though, let's go through a quick "hello world".

As this back end app is not a react project, we will not use vite for tooling, and will instead use node. In an appropriate directory, running `npm init` and following the setup will generate a `package.json` file at the project root with infor about the project, like this:
```json
{
  "name": "backend",
  "version": "0.0.1",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Matti Luukkainen",
  "license": "MIT"
}
```

The file defines a few things, like the project name and version, but also functional aspects, like the entry-point for the app being the `index.js` file. We can modify the `package.json` by adding a script to start the app like this:
```json
{
  // ...
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  // ...
}
```

Now we can complete the "hello world" app by adding a creating an `index.js` file at the project root with the contents `console.log("hello world")`. You can then run the app by either running the npm script we defined, or simply by running `node index.js` from the root. It is customary to use the scritpt though.

## Building a simple web server
Lets change our node app into a webserver by editing the `index.js` file like this:
```js
const http = require('http')

const app = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'text/plain' })
  response.end('Hello World')
})

const PORT = 3001
app.listen(PORT)
console.log(`Server running on port ${PORT}`)
```

Once the application is running, it will log to console to say so, at which point you can visit the listening port (3001) and be served the hello world message.

The server defined here will work the same way no matter what you add to the later part of the URL, eg: "http://localhost:3001/foo/bar" will still display the "hello world" message.

Let's step throught the code a bit. The first line, `const http = require('http')` is an import statement, but using different syntax to what we used on the client side. It imports Nodes built-in web server module. On the client side we have been using ES6 import statements like `import http from 'http'`. Node does support ES6 imports now but the FSO content says the support is not perfect yet so the course content will only use CommonJS module import statements.

The next code chunk in the simple server is the app definition:
```js
const app = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'text/plain' })
  response.end('Hello World')
})
```
This code uses the `createServer` method of the imported `http` module to create a webserver. An event handler is registered to the server which is called every time an HTTP request is made to the servers address, localhost port 3000 in this case.

The server responds to all requests with status code 200, the content type header set to text/plain and the site content set to "hello world".

The last rows of the example server code bind the http server in the `app` variable to listen on port 3001.

The primary purpose of the backend server in this course (and kind of in general lol) is to offer raw data in JSON format to the front-end application. For this reason, lets change our server to return a hardcoded list of notes in JSON format, for the example notes app:
```js
const http = require('http')

let notes = [
  {
    id: "1",
    content: "HTML is easy",
    important: true
  },
  {
    id: "2",
    content: "Browser can execute only JavaScript",
    important: false
  },
  {
    id: "3",
    content: "GET and POST are the most important methods of HTTP protocol",
    important: true
  }
]
const app = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'application/json' })
  response.end(JSON.stringify(notes))
})

const PORT = 3001
app.listen(PORT)
console.log(`Server running on port ${PORT}`)
```

Restarting the app here you can see the data is served as json in the browser. We have changed the `Content-Type` header to reflect that the data is json now, and we have turned the `notes` array into json with the `JSON.stringify()` method call. Thisis necessary, because the `response.end` method from the `http` module expects a string or a buffer to send as the response body.

## Express
Implementing server code directly with the built-in `http` module of node is possible, but will become difficult and annoying as the app grow bigger.

Express is one of many libraries for server-side dev on top of node that have aimed to improve the developer experience. Such libs aim to provide a better abstraction for general use cases, and Express is by far the most popular.

To get started with it, you need to install it as a dependancy:
```bash
npm install express
```

Which will automatically cause it to be added to your projects `package.json` file. The source code for express and all it's child dependancies will be installed in the `node_modules` dir at the root of the project.

Note that Node uses semantic versioning, so you might see something like this in your `package.json`:
```json
"express": "^4.18.2"
```
where the caret means that the project requires a version of express that is at least 4.18.2, but will accept versions with higher patch or minor release numbers. 4 is the major release number, 18 is the minor and 2 is the patch number.

We can update project dependancies by running `npm update`. Likewise, if we start working on the project on another computer, we can run `npm install` to install all up to date dependancies of the project.

### Implementing a basic server in express
Getting back to our example notes backend, lets use express like this:
```js
const express = require('express')
const app = express()

let notes = [
  ...
]

app.get('/', (request, response) => {
  response.send('<h1>Hello World!</h1>')
})

app.get('/api/notes', (request, response) => {
  response.json(notes)
})

const PORT = 3001
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

The code has not changed a whole lot. We have first imported express and then  called it as a function, to create an express app which we assing to the variable `app`.

Next we start defining "routes" for the express app. The first defines an event handler for HTTP GET requests made to the applications root "/". The second also defines a GET handler, but this one is for GET requests made to the "baseURL/api/notes" route. The event handler functions accept two parameters, a request and a response. The first is the contents of the HTTP request object, and the second is used to define how the server should respond.

In the handler for the root route, we use the `response.send` method to send an HTTP  response object with the hello world string as the body. Express recognizes that we have called the send method with a string and automatically sets the content type header for us. The status code defaults to 200.

The second handler we have defined above uses the `.json` method of the `response` object. Calling this method will send the `notes` array it was passed as a JSON formatted string. Again, Express will automatically handle the content type header, and in this case it will also handle stringifying the JSON object for us.

## nodemon
If we make changes to the application code in our express back-end, we need to restart the server for them to be reflected. In the front end apps from part 2 we did not do this, as the vite development server includes functionality for hot re-loading. The solution for this in node is a library called nodemon, which will watch files in the directory it is started, and automatically restart the node app if any files change.

Install nodemon as a dev dep:
```bash
npm install --save-dev nodemon
```
Then we can start our node app using nodemon to manage the process like this:
```bash
node_modules/.bin/nodemon index.js
```
where `index.js` is the entry point for our app. Now the node server will auto re-start on saving changes to a relevant file. You can also hand off the command that starts the app under nodemon to a dev script in `package.json` like this:
```json
{
  // ..
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  // ..
}
```

Note that "start" and "test" are native scripts to node, so can be run with `npm start`, but "dev" is non native so must be invoked like this: `npm run dev`.

## REST with Express
Lets expand the notes app backend so that it provides the same RESTful HTTP api as the json-server library previously used to simulate a backend.

Quick recap: REST stands for representational state transfer, and is an architectrue pattern for building scalable web apps. The term was coined in Roy Fieldings dissertation in 2000. 

Here, we will only concern ourselves with a narrow view of REST, the way it is typically understood for web apps ignoring the full definition as an architectural model.

In restful thinking, any singular thing in an app, like a note, is called a resource. Every resource has an associated URL which is it's unique address.

A common convention for creating uniuqe addresses is to combine the name of the resource with the name of it's resource type. 

(From here to the end of the rest section is taken verbatim from the fso notes)
Let's assume that the root URL of our service is _www.example.com/api_.

If we define the resource type of note to be _notes_, then the address of a note resource with the identifier 10, has the unique address _www.example.com/api/notes/10_.

The URL for the entire collection of all note resources is _www.example.com/api/notes_.

We can execute different operations on resources. The operation to be executed is defined by the HTTP _verb_:

|URL|verb|functionality|
|---|---|---|
|notes/10|GET|fetches a single resource|
|notes|GET|fetches all resources in the collection|
|notes|POST|creates a new resource based on the request data|
|notes/10|DELETE|removes the identified resource|
|notes/10|PUT|replaces the entire identified resource with the request data|
|notes/10|PATCH|replaces a part of the identified resource with the request data|
||||

This is how we manage to roughly define what REST refers to as a [uniform interface](https://en.wikipedia.org/wiki/Representational_state_transfer#Architectural_constraints), which means a consistent way of defining interfaces that makes it possible for systems to cooperate.

This way of interpreting REST falls under the [second level of RESTful maturity](https://martinfowler.com/articles/richardsonMaturityModel.html) in the Richardson Maturity Model. According to the definition provided by Roy Fielding, we have not defined a [REST API](http://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven). In fact, a large majority of the world's purported "REST" APIs do not meet Fielding's original criteria outlined in his dissertation.

In some places (see e.g. [Richardson, Ruby: RESTful Web Services](http://shop.oreilly.com/product/9780596529260.do)) you will see our model for a straightforward [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) API, being referred to as an example of [resource-oriented architecture](https://en.wikipedia.org/wiki/Resource-oriented_architecture) instead of REST. We will avoid getting stuck arguing semantics and instead return to working on our application.

## Fetching a single resource
Let's expand our example back-end to offer a REST interface for operating on individual notes. First we need a route for fetching a single resource from it's resource type and unique identifier, eg "baseURL/api/notes/10".

We can define parameters for routes in Express by using the colon syntax:
```js
app.get('/api/notes/:id', (request, response) => {
  const id = request.params.id
  const note = notes.find(note => note.id === id)
  response.json(note)
})
```
Now any HTTP GET requests to a URL of the general form `/api/notes/{something}` where `{something}` is an arbitrary string, will be handled by this event handler. The id parameter in the route can be accessed throught the request object:
```js
const id = request.params.id
```

