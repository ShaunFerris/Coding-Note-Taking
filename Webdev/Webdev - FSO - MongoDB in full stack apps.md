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
