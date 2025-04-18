#webdevSpecific 

To store the notes/persons data permanently in the example apps, we need to incorporate a database into our stack. Currently this data is stored in a variable on the Express back end, so any changes made by sending POST or Delete requests will revert if the server restarts. By moving the data to a db we avoid this.

In part 13 of FSO, we will go over using relational databases, which are probably the most common for webapps, but for most parts of the course outside part 13 we will use MongoDB, which is a document or "noSQL" database.

Doc databases differ from relational ones in how they organize the data, and what query languages they support (SQL vs noSQL). If you want to dive into the nitty gritty of how document databases work, read the chapters on [collections](https://docs.mongodb.com/manual/core/databases-and-collections/) and [documents](https://docs.mongodb.com/manual/core/document/) from the MongoDB manual.

You could install and run MongoDB locally, or dockerize and host your own instance, but there are also lots of Mongo providers out there. For the course work we will use MongoDBAtlas to host our instance. 

I have used Atlas before, so I will skip over this a little, but basically make and account, log into it and make a cluster on the "shared" tier, as these are free to use. Under the security tab make a username and password for connecting to the cluster (note that this is different to your credentials for the Atlas account). Under network access allow access from anywhere, later you can go here to restrict the ips that can access the db. Note that adding 0.0.0.0 as an IP will also allow access from anywhere. Now click on connect and grab the connection string for your Atlas cluster.

The connection string or mongoDB URI is the address of the database that we will give to our mongoDB client library in our application code. It looks like this:
```bash
mongodb+srv://<username>:<password>@cluster0.o1opl.mongodb.net/?retryWrites=true&w=majority
```
You can have multiple databases in a cluster on Atlas, if you want to connect to a specific database, add it's name to the URI before the query string starts like this:
```bash
mongodb+srv://<username>:<password>@cluster0.o1opl.mongodb.net/<databaseName>?retryWrites=true&w=majority
```

## Mongoose
With the database URI in hand we can now connect to the database and start using it. We could use the database directly in our javascript code with the official MongoDB Node.js driver library, but it can be a little cumbersome. **We will instead use Mongoose, a library that provides a higher level API with a greater level of abstraction.**

Mongoose could be described as an _object document mapper_ (ODM), and saving JavaScript objects as Mongo documents is straightforward with this library.

Install Mongoose in the backend of the project (notes or phonebook or whatever) the normal way:
```bash
npm install mongoose
```

Before adding any code to the back end that deals with the database, lets make a practice script that will allow us to play around. In a new file called mongo.js we could write this:
```js
const mongoose = require('mongoose')

if (process.argv.length<3) {
  console.log('give password as argument')
  process.exit(1)
}

const password = process.argv[2]

const url =
  `mongodb+srv://fullstack:${password}@cluster0.o1opl.mongodb.net/?retryWrites=true&w=majority`

mongoose.set('strictQuery',false)

mongoose.connect(url)

const noteSchema = new mongoose.Schema({
  content: String,
  important: Boolean,
})

const Note = mongoose.model('Note', noteSchema)

const note = new Note({
  content: 'HTML is easy',
  important: true,
})

note.save().then(result => {
  console.log('note saved!')
  mongoose.connection.close()
})
```
Reading through this code, we can see that the script is meant to be launched from the terminal, and given the database password as an argument like this `node mongo.js <yourpassword>`. It will then create a note and save it to the database. Also note that if the password contains special characters it will need to be URL encoded, [see here.](https://docs.atlas.mongodb.com/troubleshoot-connection/#special-characters-in-connection-string-password).

If you now go to your mongo atlas dashboard, you will see that under collections the note has been added to a `notes` collection in the `test` database on your cluster. If we delete this database and change the connection string in our script to reference a database called `noteApp` like this:
```js
const url =
  `mongodb+srv://fullstack:${password}@cluster0.o1opl.mongodb.net/noteApp?retryWrites=true&w=majority`
```
and run the script again, we will see that we now have a database named `noteApp` on our cluster, and the note has been added to it.

You can also create databases directly on the Atlas dashboard, but this is not necessary as a new database will be auto created when a non-existent db is referenced in the connection string as above.

## Schema
After establishing the connection to the database, we define the schema for a note and the matching Model:
```js
const noteSchema = new mongoose.Schema({
  content: String,
  important: Boolean,
})

const Note = mongoose.model('Note', noteSchema)
```
The schema tells Mongoose how the note objects are to be stored in the database. In the `Note` model definition, the first argument is the singular name for the model. The name of hte collection will be the lowercase plural `notes`. This is by Mongoose convention, that Upercase singular is models have lower case plural collections.

Document databases like Mongo are _schemaless_, meaning that the database itself does not care about the structure of the data that is stored in the database. It is possible to store documents with completely different fields in the same collection.

The idea behind Mongoose is that the data stored in the database is given a _schema at the level of the application_ that defines the shape of the documents stored in any given collection.

## Creating and saving objects to Mongo
After defining the schema and model for notes, the above code creates a new note object using the `Note` model:
```js
const note = new Note({
  content: 'HTML is Easy',
  important: false,
})
```
Models are constructor functions that create new JS objects based on the provided parameters. Since the objects are created with the models constructor function, they have all the properties of the model, which include methods for saving the object to the database.

Saving the object to the db happens when you call the `save` method. This method returns a promise you can give it a callback by chaining it to the `then` method, like this:
```js
note.save().then(result => {
  console.log('note saved!')
  mongoose.connection.close()
})
```
Doing it like this ensures that the connection to the db will only be closed after the save operation is finished. If you try to close the connection outside of a callback, the you may close the connection before the save operation has finished.

The result of the save operation is in the _result_ parameter of the event handler. The result is not that interesting when we're storing one object in the database. You can print the object to the console if you want to take a closer look at it while implementing your application or during debugging.

Let's also save a few more notes by modifying the data in the code and by executing the program again.

**NB:** Unfortunately the Mongoose documentation is not very consistent, with parts of it using callbacks in its examples and other parts, other styles, so it is not recommended to copy and paste code directly from there. Mixing promises with old-school callbacks in the same code is not recommended.

## Fetching objects from the db
Let's comment out the code for generating a new note and replace it with this:
```js
Note.find({}).then(result => {
  result.forEach(note => {
    console.log(note)
  })
  mongoose.connection.close()
})
```
Now when the program is executed, it will print all the notes stored in the db.

The objects are retrieved from the db using the `find` method of the `Note` model. The parameter of the `find` method is an object expressing search conditions, so when we give it an empty object like we have done, it will match every note. The search conditions for this param follow the Mongo search query syntax, [read about it here](https://www.mongodb.com/docs/manual/tutorial/query-documents/). A quick example would be if you wanted to find only notes with the content "HTML is easy", you would do it like this:
```js
Note.find({content: "HTML is easy"}).then(result => {
  result.forEach(note => {
    console.log(note)
  })
  mongoose.connection.close()
})
```
Or if you wanted to get every note marked important:
```js
Note.find({important: true}).then(result => {
  result.forEach(note => {
    console.log(note)
  })
  mongoose.connection.close()
})
```

## Connecting the app backend to a mongoDB
Now that we have covered the basics of querying and posting to our mongoDB instance from node.js code, lets start looking at how to intgrate the db into our notes/phonebook apps.

We could start doing this by pasting the definitions for our connection string, the mongoose connection, and our schemas and models into the `index.js` of the notes app:
```js
const mongoose = require('mongoose')

const password = process.argv[2]

// DO NOT SAVE YOUR PASSWORD TO GITHUB!!
const url =
  `mongodb+srv://fullstack:${password}@cluster0.o1opl.mongodb.net/?retryWrites=true&w=majority`

mongoose.set('strictQuery',false)
mongoose.connect(url)

const noteSchema = new mongoose.Schema({
  content: String,
  important: Boolean,
})

const Note = mongoose.model('Note', noteSchema)
```
However this code is still trying to take the password from command line variables. To avoid issue with auth we need to instead take the password from an environment variable that we will not commit to source control. What we can do is install the `dotenv` module from npm in our express app, then create a `.env` file at the root of the project and place the full connection string including the password into it, like this:
```js
MONGODB_URI="mongodb+srv://fullstack:password@db.gwcmebp.mongodb.net/?retryWrites=true&w=majority&appName=db"
```
Next, add `.env` to the projects `.gitignore` to prevent it from being leaked.

Then in `index.js` you can remove the line trying to take the password from `argv` and replace the url definition like this:
```js
const url = process.env.MONGODB_URI;
```
Now that we have a way to connect to the database, we can change the route handler for fetching all the notes to this:
```js
app.get('/api/notes', (request, response) => {
  Note.find({}).then(notes => {
    response.json(notes)
  })
})
```
At this point we can verify that the backend works post these changes by going to the `/api/notes/` route in the browser, where you would see something like this:
![[Pasted image 20241009141432.png]]This is almost perfect, but the front-end is expecting each note to have a unique id under an `id` field, not an `_id` field, and we also do not want to include the `__v` field which is the mongo versioning field.

One way to format the objects returned by Mongoose is to modify the `toJSON` method of the schema, which is used on all instances of the models produced with that schema.

To modify the method we need to change the configurable options of the schema, options can be changed using the set method of the schema, see here for more info on this method: [https://mongoosejs.com/docs/guide.html#options](https://mongoosejs.com/docs/guide.html#options). See [https://mongoosejs.com/docs/guide.html#toJSON](https://mongoosejs.com/docs/guide.html#toJSON) and [https://mongoosejs.com/docs/api.html#document_Document-toObject](https://mongoosejs.com/docs/api.html#document_Document-toObject) for more info on the _toJSON_ option. Also see [https://mongoosejs.com/docs/api/document.html#transform](https://mongoosejs.com/docs/api/document.html#transform) for more info on the _transform_ function.

A call to the set method to modify the `toJSON` method might look like this:
```js
noteSchema.set('toJSON', {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id.toString()
    delete returnedObject._id
    delete returnedObject.__v
  }
})
```
Note that we are editing the `transform` property of the `toJSON` option object.
Even though the `_id` property of Mongoose objects looks like a string, it is in fact an object. The _toJSON_ method we defined transforms it into a string just to be safe. If we didn't make this change, it would cause more harm to us in the future once we start writing tests.

No changes need to be made to the handler above, the code will automatically used the re-defined `toJSON` method when formatting notes for the response. This shit can get confusing so remember to refer to the docs if you need to.

## Moving db config to it's own module
Keeping all this database specific code in `index.js` is annoying and will get messy fast, especially if the app gets bigger and we add a large number of route handlers. So, before refactoring the other route handlers we already have, let's extract the mongoose related code into a module. Let's create a `models` directory in the project root and add a `note.js` file to it, containing the relevant code:
```js
const mongoose = require('mongoose')

mongoose.set('strictQuery', false)

const url = process.env.MONGODB_URI

console.log('connecting to', url)

mongoose.connect(url)

  .then(result => {
    console.log('connected to MongoDB')
  })
  .catch(error => {
    console.log('error connecting to MongoDB:', error.message)
  })

const noteSchema = new mongoose.Schema({
  content: String,
  important: Boolean,
})

noteSchema.set('toJSON', {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id.toString()
    delete returnedObject._id
    delete returnedObject.__v
  }
})

module.exports = mongoose.model('Note', noteSchema)
```
Note that we have defined this module using the common.js standard, which is different to what we did when working with React apps where we used ES6 module definitions. New versions of Node do support ES6 modules, but when the FSO course content was written that support was not mature yet so the course uses common modules.

Using common.js module definitions, the public interface of the module is defined by setting a value to the _module.exports_ variable. We will set the value to be the _Note_ model. The other things defined inside of the module, like the variables _mongoose_ and _url_ will not be accessible or visible to users of the module.

Importing the module happens by adding the following line to _index.js_:
```js
const Note = require('./models.note')
```
And now the `Note` variable in `index.js` will correspond to the `Note` model object defined in the module file.

Note that we are getting the connection string from an ENV variable. This can either be from a .env file using the `dotenv` npm package discussed earlier, or it can be provided at run time by starting the program like this:
```bash
MONGODB_URI=address_here npm run dev
```

We should go with the `dotenv` method as it is more elegant. Note that you must also add `require('dotenv').config()` to the `index.js` file to use the variables. **Also worth noting that node versions past 20.6.0** have support for .env files without needing the `dotenv` package. You can read about that [here](https://nodejs.org/en/blog/release/v20.6.0).

It is important that the dotenv require statement is imported before the `note` model is imported to ensure that the variables from the `.env` file are globally available as early as possible.

## Updating the other route handlers
Now let's change the rest of the backend functionality to use the database instead of the `notes` variable. Creating a new note is handled like this:
```js
app.post('/api/notes', (request, response) => {
  const body = request.body

  if (body.content === undefined) {
    return response.status(400).json({ error: 'content missing' })
  }

  const note = new Note({
    content: body.content,
    important: body.important || false,
  })

  note.save().then(savedNote => {
    response.json(savedNote)
  })
})
```
The note objects are created with the `Note` constructor function that we imported from our module. The reponse is sent inside the callback function for the `save` method call, ensuring the response is only sent if the operation is successful. We will discuss error handling later on. 

The `savedNote` parameter in the callback function is the saved and newly created note. The data sent back in the response is the formatted version created automaticaally with the `toJSON` method, which we have earlier modified. 

Using mongooses `findBy` method which is inherited by our `Note` model, fetching an individual note can be done with a handler like this:
```js
app.get('/api/notes/:id', (request, response) => {
  Note.findById(request.params.id).then(note => {
    response.json(note)
  })
})
```

## Verifying the integration
Don't forget to test that newly re-written endpoints. Using the vsCode rest client or postman is a good way to do this. 

Only once everything has been verified to work in the backend, is it a good idea to test that the frontend works with the backend. It is highly inefficient to test things exclusively through the frontend.

It's probably a good idea to integrate the frontend and backend one functionality at a time. First, we could implement fetching all of the notes from the database and test it through the backend endpoint in the browser. After this, we could verify that the frontend works with the new backend. Once everything seems to be working, we would move on to the next feature.

Once we introduce a database into the mix, it is useful to inspect the state persisted in the database, e.g. from the control panel in MongoDB Atlas. Quite often little Node helper programs like the _mongo.js_ program we wrote earlier can be very helpful during development.

## Error handling for backends with a database
If we try to visit the URL for a specific note in the current version of the notes app, and the id we use in the URL does not match a note in the database, then the response to our request will be null, and the browser will display a blank page.

Let's change this behavior so that if a note with a given id is non-existant, the server will respond with a 404. On top of this we should also add a catch block to handle the case where the promise returned by the `findById` call is rejected:
```js
app.get('/api/notes/:id', (request, response) => {
  Note.findById(request.params.id)
    .then(note => {
      if (note) {
        response.json(note)
      } else {
        response.status(404).end()
      }
    })
    .catch(error => {
      console.log(error)
      response.status(500).end()
    })
})
```
If no matching object is found in the db, the value of `note` will be null and the `else` statement will fire, sending the 404 response. If the promise is rejected, the `.then` callback will *not* fire, and the `.catch` block will, resulting in a 500 "internal server error" response.

There is one more error case that we should handle for the single note route, which is the case of a user supplying an invalid id. All the entires in our db are keyed with an id string in the Mongo identifier format, and if you try to use a URL with an id that is not in the same format, you will get this error:
![[Pasted image 20241015115815.png]]
Given an improper id as an argument, the `findById` method will throw an error causing the returned promise to be rejected. This will trigger the catch blocks callback to trigger.

Let's make some small adjustments to the response in the _catch_ block:
```js
app.get('/api/notes/:id', (request, response) => {
  Note.findById(request.params.id)
    .then(note => {
      if (note) {
        response.json(note)
      } else {
        response.status(404).end() 
      }
    })
    .catch(error => {
      console.log(error)
      response.status(400).send({ error: 'malformatted id' })
    })
})
```
Now, if the format for the id is incorrect, we will trigger the catch block and a 400 error will be returned. **400 is the error code for "bad request".**

We have also added and error message to the reponse to provide the user with info about why the request failed. **It is always a good idea to implement some error and exception handling when you are dealing with promises**.

It is also, often a good idea to print the object that caused the exception to the console, to provided you with info while debugging:
```js
.catch(error => {

  console.log(error)
  response.status(400).send({ error: 'malformatted id' })
})
```
The error handler in the catch block above might still get called for reasons completely different from what you anticipated though, so logging it can save you some stress. 

## Moving error handling to a middleware
So far, all the error handling we have implemented has been mingled with the route handler code. This is an approach that works, but there are cases where it is better to implement all the error handling in one place. This can be particularly useful when we want to report data related to errors to an external error-tracking system like Sentry later on.

Let's change  the handler for the `/api/notes/:id` route handler so that it passes the error forward with the `next` function. The `next` funciton is passed to the handler as the third param:
```js
app.get('/api/notes/:id', (request, response, next) => {
  Note.findById(request.params.id)
    .then(note => {
      if (note) {
        response.json(note)
      } else {
        response.status(404).end()
      }
    })
    .catch(error => next(error))
})
```
The error that is passed forward is given to the `next` function as an arg. If `next` was called with an argument, the execution will continue to the error handler middleware.

**Express error handlers are defined using a function that takes four args, error, request, response and next. An error handler middleware might look something like this:**
```js
const errorHandler = (error, request, response, next) => {
  console.error(error.message)

  if (error.name === 'CastError') {
    return response.status(400).send({ error: 'malformatted id' })
  } 

  next(error)
}

// this has to be the last loaded middleware, also all the routes should be registered before this!
app.use(errorHandler)
```
The error handler checks if the error is a _CastError_ exception, in which case we know that the error was caused by an invalid object id for Mongo. In this situation, the error handler will send a response to the browser with the response object passed as a parameter. In all other error situations, the middleware passes the error forward to the default Express error handler.

Note that the error-handling middleware has to be the last loaded middleware, also all the routes should be registered before the error-handler!

## The order of middleware loading
The execution order of middleware is the same as the order that they are loaded into Express with the _app.use_ function. For this reason, it is important to be careful when defining middleware.

The correct order is the following, note that all the middleware except the error handler are loaded up top, the error handler is saved for last.
```js
app.use(express.static('dist'))
app.use(express.json())
app.use(requestLogger)

app.post('/api/notes', (request, response) => {
  const body = request.body
  // ...
})

const unknownEndpoint = (request, response) => {
  response.status(404).send({ error: 'unknown endpoint' })
}

// handler of requests with unknown endpoint
app.use(unknownEndpoint)

const errorHandler = (error, request, response, next) => {
  // ...
}

// handler of requests with result to errors
app.use(errorHandler)
```
The json-parser middleware should be among the very first middleware loaded into Express. If the order was the following:
```js
app.use(requestLogger) // request.body is undefined!

app.post('/api/notes', (request, response) => {
  // request.body is undefined!
  const body = request.body
  // ...
})

app.use(express.json())
```
Then the JSON data sent with the HTTP requests would not be available for the logger middleware or the POST route handler, since the _request.body_ would be _undefined_ at that point.

It's also important that the middleware for handling unsupported routes is next to the last middleware that is loaded into Express, just before the error handler. For example, the following loading order would cause an issue:
```js
const unknownEndpoint = (request, response) => {
  response.status(404).send({ error: 'unknown endpoint' })
}

// handler of requests with unknown endpoint
app.use(unknownEndpoint)

app.get('/api/notes', (request, response) => {
  // ...
})
```
Now the handling of unknown endpoints is ordered _before the HTTP request handler_. Since the unknown endpoint handler responds to all requests with _404 unknown endpoint_, no routes or middleware will be called after the response has been sent by unknown endpoint middleware. The only exception to this is the error handler which needs to come at the very end, after the unknown endpoints handler.

## Other operations

Let's add some missing functionality to our application, including deleting and updating an individual note. The easiest way to delete a note from the database is with the [findByIdAndDelete](https://mongoosejs.com/docs/api/model.html#Model.findByIdAndDelete()) method:
```js
app.delete('/api/notes/:id', (request, response, next) => {
  Note.findByIdAndDelete(request.params.id)
    .then(result => {
      response.status(204).end()
    })
    .catch(error => next(error))
})
```

In both of the "successful" cases of deleting a resource, the backend responds with the status code _204 no content_. The two different cases are deleting a note that exists, and deleting a note that does not exist in the database. The _result_ callback parameter could be used for checking if a resource was actually deleted, and we could use that information for returning different status codes for the two cases if we deem it necessary. Any exception that occurs is passed onto the error handler.

The toggling of the importance of a note can be easily accomplished with the [findByIdAndUpdate](https://mongoosejs.com/docs/api/model.html#model_Model-findByIdAndUpdate) method.
```js
app.put('/api/notes/:id', (request, response, next) => {
  const body = request.body

  const note = {
    content: body.content,
    important: body.important,
  }

  Note.findByIdAndUpdate(request.params.id, note, { new: true })
    .then(updatedNote => {
      response.json(updatedNote)
    })
    .catch(error => next(error))
})
```
In the code above, we also allow the content of the note to be edited.

Notice that the _findByIdAndUpdate_ method receives a regular JavaScript object as its argument, and not a new note object created with the _Note_ constructor function.

There is one important detail regarding the use of the _findByIdAndUpdate_ method. By default, the _updatedNote_ parameter of the event handler receives the original document [without the modifications](https://mongoosejs.com/docs/api/model.html#model_Model-findByIdAndUpdate). We added the optional `{ new: true }` parameter, which will cause our event handler to be called with the new modified document instead of the original.

After testing the backend directly with Postman or the VS Code REST client, we can verify that it seems to work. The frontend also appears to work with the backend using the database.

## Next
The next section of FSO content covers input validation and ESLint. This is the last section of FSO part three before we move into section four on testing express apps. [[Webdev - FSO - Validation and ESLint]]