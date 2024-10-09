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
