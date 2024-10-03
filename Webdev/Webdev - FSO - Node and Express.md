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
Then the `.find()` array method is used to find the note with a matching id and return it to the request sender in the response object.

Going to `localhost:3001/api/notes/1` will now render the content of the first note as a json formatted string.

The problem with this version of the api is that if we search for a note that does not exist, the server still responds with a 200 HTTP status code. There will be no data sent back with the response, as the `note` variable is `undefined` if no matching note is found. We should handle this better, and send a 404 if the resource cannot be found. Lets do that like this:
```js
app.get('/api/notes/:id', (request, response) => {
  const id = request.params.id
  const note = notes.find(note => note.id === id)
  
  if (note) {
    response.json(note)
  } else {
    response.status(404).end()
  }
})
```
The `if (note)` condition here leverages the fact that JS objects are truthy, meaning that the evealuate to true in comparison operations, while `undefined` and `null` are falsy.

In this state, the app works, and sends a 404 on missing resource. It does not however, display anything is the browser in the case of a 404. This is fine as this is an API, and is an interface intended for programmatic use. The front end can handle serving the user an appropriate view in the case of a 404.

## Deleting resources
We can implement a route for deleting resources in Express like this:
```js
app.delete('/api/notes/:id', (request, response) => {
  const id = request.params.id
  notes = notes.filter(note => note.id !== id)
  
  response.status(204).end()
})
```
If deleting the resource is successful, meaning that the note exists and is removed, we respond to the request with the status code [204 no content](https://www.rfc-editor.org/rfc/rfc9110.html#name-204-no-content) and return no data with the response.

There's no consensus on what status code should be returned to a DELETE request if the resource does not exist. The only two options are 204 and 404. For the sake of simplicity, our application will respond with 204 in both cases.

## Postman
How can we test that our delete operation worked successfully? HTTP GET requests are easy to make from the browser console. We could write some js for testing deletion, but writing test code is not always the best solution. 

Postman is a tool that exists to make testing backends easier. Curl is useful in a similar way, but for the FSO content we will be looking at postman.

You can install the Postman desktop client [from here](https://www.postman.com/downloads/) if you want to try it out.

Using it is easy, just define the URL and then select the request type, in this case DELETE. You will see the backend example server respond correctly with 204:
![[Pasted image 20241003111459.png]]You will also see that the note with ID 2 is no longer in the list.

**Personally I use curl for sending test requests at the moment. I may look into postman if it is an important part of course materials here, but if I can just use curl I might.**

## The vscode REST client
There is also a rest client extension for vs code that you can use instead of postman. Once installed, make a directory at the root of the app named "requests". We save all the REST client requests in the directory as files ending with a .rest extension. In the files you can define a request like this:
```bash
GET http://localhost:3001/api/notes/

###
POST http://localhost:3001/api/notes/ HTTP/1.1
content-type: application/json

{
    "name": "sample",
    "time": "Wed, 21 Oct 2015 18:27:50 GMT"
}
```
The extension will create text saying "send request" that sends on click, and the response will open in a new editor tab.
## Receiving data
Next, let's make it possible to add a new note to the notes server. This is done by sending a POST request to the address "http://localhost:3001/api/notes", which makes intuitive sense from a restful thinking perspective. The info for the new note in the request body should be sent in JSON format.

To access the sent data, we will use the Express built-in json parser with the `.use()` method of the express app object. An initial handler for POST requests using the json parser might look like this:
```js
const express = require('express')
const app = express()

app.use(express.json())
//...

app.post('/api/notes', (request, response) => {
  const note = request.body
  console.log(note)
  response.json(note)
})
```

The POST event handler can access the sent data from the body property of the `request` object. Without the json parser, the body prop would be undefined. The parser takes the JSON data, turns it into a JavaScript object and reattaches it to the body property of the request object before the route handler is called.

For the time being, the handler only logs the note content to console. We can test it works at this point by sending a post request with postman and verifying the log.

Note that you could use curl for this test instead of postman. You would however have to make sure that you set the headers for the curl request properly. It would look something like this:
```bash
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"username":"xyz","password":"xyz"}' \
  http://localhost:3000/api/login
```
In postman you can also have problems if the body type and content-type header don't match. The server will not even try and parse the body if it is not the correct type for the header.

The vscode rest client extension is probably the most useful tool for this kind of endpoint testing as the test files are included at the project root and can be easily shared over version control software.

Returning to the example notes backend, it's time to finalize the handling of the POST request for adding a note:
```js
app.post('/api/notes', (request, response) => {
  const maxId = notes.length > 0
    ? Math.max(...notes.map(n => Number(n.id))) 
    : 0

  const note = request.body
  note.id = String(maxId + 1)

  notes = notes.concat(note)

  response.json(note)
})
```
We need a unique id for the note. First, we find out the largest id number in the current list and assign it to the _maxId_ variable. The id of the new note is then defined as _maxId + 1_ as a string. This method is not recommended, but we will live with it for now as we will replace it soon enough.

The current version still has the problem that the HTTP POST request can be used to add objects with arbitrary properties. Let's improve the application by defining that the _content_ property may not be empty. The _important_ property will be given a default value of false. All other properties are discarded:
```js
const generateId = () => {
  const maxId = notes.length > 0
    ? Math.max(...notes.map(n => Number(n.id)))
    : 0
  return String(maxId + 1)
}

app.post('/api/notes', (request, response) => {
  const body = request.body

  if (!body.content) {
    return response.status(400).json({ 
      error: 'content missing' 
    })
  }

  const note = {
    content: body.content,
    important: Boolean(body.important) || false,
    id: generateId(),
  }

  notes = notes.concat(note)

  response.json(note)
})
```
The logic for generating the new id number for notes has been extracted into a separate _generateId_ function.

If the received data is missing a value for the _content_ property, the server will respond to the request with the status code [400 bad request](https://www.rfc-editor.org/rfc/rfc9110.html#name-400-bad-request).

## Next
After the notes here, I did exercises 3.1 to 3.6. The notes following those exercises are about HTTP request types and middleware, and I have split them from the notes on express and node. Those notes are here: [[Webdev - FSO - Http request types and middlewares]]