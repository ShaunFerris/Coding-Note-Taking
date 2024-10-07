#webdevSpecific 

## About HTTP request types
The HTTP standard outlines two properties for the request types reprented by the HTTP verbs, **safety** and **idempotency**. 

The HTTP GET request should be safe, meaning it should never take and action other than retrieval of a resource. Executing a GET request must never cause any side-effects on the server and must only return data that already exists on the server.

Nothing can actually gurantee that a GET request is safe, or prevent you from making your GETs unsafe, but the standard outlines that this is how it should be done, and if we are adhering to REST principles then it is how we should do it.

The HTTP standard also defines the request type HEAD, which should be safe in the same way as the GET request. A HEAD should do the same as a GET, but return only the HTTP staus code and the headers, without including a body.

All HTTP request types except for POST should be **idempotent**. Below is a quote from the standard itself:

_Methods can also have the property of "idempotence" in that (aside from error or expiration issues) the side-effects of N > 0 identical requests is the same as for a single request. The methods GET, HEAD, PUT and DELETE share this property_

This means that if a request does not generate side effects, then the result should not change no matter how many times you send the same request.

Like _safety_ for the GET request, _idempotence_ is also just a recommendation in the HTTP standard and not something that can be guaranteed simply based on the request type. However, when our API adheres to RESTful principles, then GET, HEAD, PUT, and DELETE requests are used in such a way that they are idempotent.

POST is the only HTTP request type that is neither _safe_ nor _idempotent_. If we send 5 different HTTP POST requests to _/api/notes_ with a body of _{content: "many same", important: true}_, the resulting 5 notes on the server will all have the same content.

## Middleware
The json parser built-in from express used earlier in the notes app example is an example of what is called a middleware. Middlewares are functions that can be used for handling `request` and `response` objects. The json parser used earlier takes the raw data from the requests object coming in, parses it into javascript and assigns the result to the request object as a new property `body`.

In practice, you can use multiple middlewares at the same time. When you have more than one, they're executed one by one in th eorder that they were invoked in the source code.

We can implement a simple example middleware of our own that logs information about incoming requests to the server. A simple middleware can be made from a function that takes three args, the `request` the `response` and `next`:
```js
const requestLogger = (request, response, next) => {
  console.log('Method:', request.method)
  console.log('Path:  ', request.path)
  console.log('Body:  ', request.body)
  console.log('---')
  next()
}
```
At the end of the function body, the `next` function that was passed to the logging middleware is called, yielding control to the next middleware in the sequence. `next` is an express.js builtin (I think?? The notes on FSO are not super clear about this actually).

Middlewares are invoked in express.js in the same way we invoked the json parser in previous examples, like this:
```js
app.use(requestLogger)
```
Where the `app` object is the express instance representing your server application.

Remember that middlewares are ivoked in order, so you must invoke the json parser middleware **before** invoking our custom request logger, or else the `request.body` parameter will not exist on the request object for the logger to read.

Middleware functions also must be invoked before routes in our express server files, if we want them to be executed by the route handlers. Sometimes you might want a middleware after the routes, for example if we only want the middleware called on requests that did not get processed by a route handler.

An example would be something like this:
```js
const unknownEndpoint = (request, response) => {
  response.status(404).send({ error: 'unknown endpoint' })
}

app.use(unknownEndpoint)
```

## Next
The next section of FSO course content continues the full-stack theme for part three, and focuses on combining a backend and front end for a project, and hosting them on the internet. See here: [[Webdev - FSO - Connecting backend to front and deploying an App]]
