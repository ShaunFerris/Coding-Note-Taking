#webdevSpecific 

For a note taking app or a blogging app or many others, one might need to add user authentication and authorization protocols, so that only users who can proved certain facts about their identity can access certain routes or functions of the api.

We could start implementing this in the notes example app by adding information about users to the database. **There is a one to many relationship between users and notes in our data**:
![[Pasted image 20241025143254.png]]
If we were working with a relational database the implementation would be straightforward. Both resources would have their seperate database tables, and the id of the user who created a note would be stored in the notes table as a foreign key.

When working with a document database though the situation is a bit different, as there are many different ways we could choose to model the situation.

The existing solution saves every note in the `notes` collection in the database. If we do not want to change this existing collection, then the natural choice is to save users in their own collection, `users` for example.

Like with all document databases, we can use object ID's in Mongo to reference documents in  other collections. This is similar to using foreign keys in relational databases.

Traditionally doc dbs like Mongo do not support "join queries" that are available in trad relational dbs. **Join queries are used for aggregating data from multiple tables.** Starting from version 3.2 of Mongo db, it has supported [lookup aggregation queries](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/). We will not be taking a look at this functionality in this course.

**If we need functionality similar to join queries, we will implement it in our application code by making multiple queries. In certain situations, Mongoose can take care of joining and aggregating data, which gives the appearance of a join query. However, even in these situations, Mongoose makes multiple queries to the database in the background.**

## References across collections
In a relational db, the notes would all contain a _reference key_ to the user who created it. In document databases, we can do the same thing.

Let's assume that the _users_ collection contains two users:
```js
[
  {
    username: 'mluukkai',
    _id: 123456,
  },
  {
    username: 'hellas',
    _id: 141414,
  },
]
```

The _notes_ collection contains three notes that all have a _user_ field that references a user in the _users_ collection:
```json
[
  {
    content: 'HTML is easy',
    important: false,
    _id: 221212,
    user: 123456,
  },
  {
    content: 'The most important operations of HTTP protocol are GET and POST',
    important: true,
    _id: 221255,
    user: 123456,
  },
  {
    content: 'A proper dinosaur codes with Java',
    important: false,
    _id: 221244,
    user: 141414,
  },
]
```
Document databases do not demand the foreign key to be stored in the note resources, it could _also_ be stored in the users collection, or even both:
```js
[
  {
    username: 'mluukkai',
    _id: 123456,
    notes: [221212, 221255],
  },
  {
    username: 'hellas',
    _id: 141414,
    notes: [221244],
  },
]
```

Since users can have many notes, the related ids are stored in an array in the _notes_ field.

Document databases also offer a radically different way of organizing the data: In some situations, it might be beneficial to nest the entire notes array as a part of the documents in the users collection:
```js
[
  {
    username: 'mluukkai',
    _id: 123456,
    notes: [
      {
        content: 'HTML is easy',
        important: false,
      },
      {
        content: 'The most important operations of HTTP are GET and POST',
        important: true,
      },
    ],
  },
  {
    username: 'hellas',
    _id: 141414,
    notes: [
      {
        content:
          'A proper dinosaur codes with Java',
        important: false,
      },
    ],
  },
]
```

In this schema, notes would be tightly nested under users and the database would not generate ids for them.

The structure and schema of the database are not as self-evident as it was with relational databases. The chosen schema must support the use cases of the application the best. This is not a simple design decision to make, as all use cases of the applications are not known when the design decision is made.

Paradoxically, schema-less databases like Mongo require developers to make far more radical design decisions about data organization at the beginning of the project than relational databases with schemas. On average, relational databases offer a more or less suitable way of organizing data for many applications.

## Mongoose schema for users
In this case, we decide to store the ids of the notes created by the user in the user document. Let's define the model for representing a user in the `models.user.js` file:
```js
const mongoose = require('mongoose')

const userSchema = new mongoose.Schema({
  username: String,
  name: String,
  passwordHash: String,
  notes: [
    {
      type: mongoose.Schema.Types.ObjectId,
      ref: 'Note'
    }
  ],
})

userSchema.set('toJSON', {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id.toString()
    delete returnedObject._id
    delete returnedObject.__v
    // the passwordHash should not be revealed
    delete returnedObject.passwordHash
  }
})

const User = mongoose.model('User', userSchema)

module.exports = User
```
The ids of the notes are stored within the user document as an array of Mongo id's, with a string as a reference.  The definition is as follows:
```json
{
  type: mongoose.Schema.Types.ObjectId,
  ref: 'Note'
}
```
The type of the field is `ObjectId` that references note-style documents. Mongo does not inherently know that this is a field that references notes, the syntax is purely related to and defined by Mongoose.

Lest's expand the schema of the note defined in the `models/note.js` file so that the note contains information about who created it:
```js
const noteSchema = new mongoose.Schema({
  content: {
    type: String,
    required: true,
    minlength: 5
  },
  important: Boolean,
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User'
  }
})
```
In stark contrast to the conventions of relational dbs, **references are now stored in both documents**. The note references the user who created it, and the user has an array of references to all the notes created by them.

## Creating users
Let's implement a route for creating new users. Users have a unique _username_, a _name_ and something called a _passwordHash_. The password hash is the output of a [one-way hash function](https://en.wikipedia.org/wiki/Cryptographic_hash_function) applied to the user's password. It is never wise to store unencrypted plain text passwords in the database!

To hash passwords, we will install bcrypt:
```
npm install bcrypt
```
Creating a new user in compliance with REST conventions happens by sending a POST request to the `/api/users` route. We should define a new router, seperate from the `notesRouter` object, to contain the route handlers for the users route:
```js
// app.js
const usersRouter = require("./controllers/users')
// ...
app.use("/api/users", usersRouter)

```
The contents of the `./controllers/users` file can then be someting like this:
```js
const bcrypt = require('bcrypt')
const usersRouter = require('express').Router()
const User = require('../models/user')

usersRouter.post('/', async (request, response) => {
  const { username, name, password } = request.body

  const saltRounds = 10
  const passwordHash = await bcrypt.hash(password, saltRounds)

  const user = new User({
    username,
    name,
    passwordHash,
  })

  const savedUser = await user.save()

  response.status(201).json(savedUser)
})

module.exports = usersRouter`
```
You can see that the user provides a password to create their user record with, but this is **NEVER** saved to our database. Instead we immediately hash it, and save the hash to the db. The fundamentals of [storing passwords](https://codahale.com/how-to-safely-store-a-password/) are outside the scope of this course material. We will not discuss what the magic number 10 assigned to the [saltRounds](https://github.com/kelektiv/node.bcrypt.js/#a-note-on-rounds) variable means, but you can read more about it in the linked material.

Our current code does not contain any error handling or input validation for verifying that the username and password are in the desired format.

The new feature can and should initially be tested manually with a tool like Postman. However testing things manually will quickly become too cumbersome, especially once we implement functionality that enforces usernames to be unique.

It takes much less effort to write automated tests, and it will make the development of our application much easier.

Our initial tests could look like this:
```js
const bcrypt = require('bcrypt')
const User = require('../models/user')

//...

describe('when there is initially one user in db', () => {
  beforeEach(async () => {
    await User.deleteMany({})

    const passwordHash = await bcrypt.hash('sekret', 10)
    const user = new User({ username: 'root', passwordHash })

    await user.save()
  })

  test('creation succeeds with a fresh username', async () => {
    const usersAtStart = await helper.usersInDb()

    const newUser = {
      username: 'mluukkai',
      name: 'Matti Luukkainen',
      password: 'salainen',
    }

    await api
      .post('/api/users')
      .send(newUser)
      .expect(201)
      .expect('Content-Type', /application\/json/)

    const usersAtEnd = await helper.usersInDb()
    assert.strictEqual(usersAtEnd.length, usersAtStart.length + 1)

    const usernames = usersAtEnd.map(u => u.username)
    assert(usernames.includes(newUser.username))
  })
})
```
The tests use the `usersInDB()` helper function to help verify the state of the database after a user is created.
```js
const User = require('../models/user')

// ...

const usersInDb = async () => {
  const users = await User.find({})
  return users.map(u => u.toJSON())
}

module.exports = {
  initialNotes,
  nonExistingId,
  notesInDb,
  usersInDb,
}
```
The _beforeEach_ block adds a user with the username _root_ to the database. We can write a new test that verifies that a new user with the same username can not be created:
```js
describe('when there is initially one user in db', () => {
  // ...

  test('creation fails with proper statuscode and message if username already taken', async () => {
    const usersAtStart = await helper.usersInDb()

    const newUser = {
      username: 'root',
      name: 'Superuser',
      password: 'salainen',
    }

    const result = await api
      .post('/api/users')
      .send(newUser)
      .expect(400)
      .expect('Content-Type', /application\/json/)

    const usersAtEnd = await helper.usersInDb()
    assert(result.body.error.includes('expected `username` to be unique'))

    assert.strictEqual(usersAtEnd.length, usersAtStart.length)
  })
})
```
The test case obviously will not pass at this point. We are essentially practicing [test-driven development (TDD)](https://en.wikipedia.org/wiki/Test-driven_development), where tests for new functionality are written before the functionality is implemented.

Mongoose validations do not provide a direct way to check the uniqueness of a field value. However, it is possible to achieve uniqueness by defining [uniqueness index](https://mongoosejs.com/docs/schematypes.html) for a field. The definition is done as follows:
```js
const mongoose = require('mongoose')

const userSchema = mongoose.Schema({
  username: {
    type: String,
    required: true,
    unique: true // this ensures the uniqueness of username
  },
  name: String,
  passwordHash: String,
  notes: [
    {
      type: mongoose.Schema.Types.ObjectId,
      ref: 'Note'
    }
  ],
})

// ...
```
However, we want to be careful when using the uniqueness index. If there are already documents in the database that violate the uniqueness condition, [no index will be created](https://dev.to/akshatsinghania/mongoose-unique-not-working-16bf). So when adding a uniqueness index, make sure that the database is in a healthy state! The test above added the user with username _root_ to the database twice, and these must be removed for the index to be formed and the code to work.

Mongoose validations do not detect the index violation, and instead of _ValidationError_ they return an error of type _MongoServerError_. We therefore need to extend the error handler for that case:
```js
const errorHandler = (error, request, response, next) => {
  if (error.name === 'CastError') {
    return response.status(400).send({ error: 'malformatted id' })
  } else if (error.name === 'ValidationError') {
    return response.status(400).json({ error: error.message })

  } else if (error.name === 'MongoServerError' && error.message.includes('E11000 duplicate key error')) {
    return response.status(400).json({ error: 'expected `username` to be unique' })
  }

  next(error)
}
```

After these changes, the tests will pass.

We could also implement other validations into the user creation. We could check that the username is long enough, that the username only consists of permitted characters, or that the password is strong enough. Implementing these functionalities is left as an optional exercise.

Before we move onward, let's add an initial implementation of a route handler that returns all of the users in the database:
```js
usersRouter.get('/', async (request, response) => {
  const users = await User.find({})
  response.json(users)
})
```

For making new users in a production or development environment, you may send a POST request to `/api/users/` via Postman or REST Client in the following format:
```js
{
    "username": "root",
    "name": "Superuser",
    "password": "salainen"
}
```

The list looks like this:
![browser api/users shows JSON data with notes array](https://fullstackopen.com/static/485f61a7db35371fea0db42b2bcc1cda/5a190/9.png)


## Creating a new note
The code as it stands right now in the example app needs to be updated so that new notes are assigned to the user who created them. Let's expand the implementation in `controllers/notes.js` file to add the id of the user to the note and add the id of the note to the users note id array:
```js
const User = require('../models/user')

//...

notesRouter.post('/', async (request, response) => {
  const body = request.body

  const user = await User.findById(body.userId)

  const note = new Note({
    content: body.content,
    important: body.important === undefined ? false : body.important,

    user: user.id
  })

  const savedNote = await note.save()
  user.notes = user.notes.concat(savedNote._id)
  await user.save()
  
  response.status(201).json(savedNote)
})
```

Let's try to create a new note
![Postman creating a new note](https://fullstackopen.com/static/a562436685978cc842c89a5d157a2938/5a190/10e.png)

The operation appears to work. Let's add one more note and then visit the route for fetching all users:
![api/users returns JSON with users and their array of notes](https://fullstackopen.com/static/c60c79686add78ebf57de1711c47fb47/5a190/11e.png)

We can see that the user has two notes.

Likewise, the ids of the users who created the notes can be seen when we visit the route for fetching all notes:
![api/notes shows ids of users in JSON](https://fullstackopen.com/static/6798064b6620269dd272d7ee515aa4a9/5a190/12e.png)

## Populate
We would like our API to work in such a way, that when an HTTP GET request is made to the `/api/users` route, the user objects would also contain the contents of the user's notes and not just their id. In a relational database, this functionality would be implemented with a _join query_.

As previously mentioned, document databses do not properly support join queries between collecitons, but the Mongoose library can do some of these joins for us. Mongoose acomplishes the join by doing multiple queries, which is different from join queries in relational dbs which are transactional, meaning that the state of the database does not change during the time that the query is made. **With join queries in Mongoose, nothing can guarantee that the state between collections being joined is consistent, meaning that if we make a query that joins the user and notes collections, the state of the collections may change during the query.**

The mongoos join is done with the [populate](http://mongoosejs.com/docs/populate.html) method. Let's update the route that returns all users first in `controllers/users.js` file:
```js
usersRouter.get('/', async (request, response) => {

  const users = await User
    .find({}).populate('notes')

  response.json(users)
})
```
The [populate](http://mongoosejs.com/docs/populate.html) method is chained after the _find_ method making the initial query. The argument given to the populate method defines that the _ids_ referencing _note_ objects in the _notes_ field of the _user_ document will be replaced by the referenced _note_ documents.

The result is almost exactly what we wanted:
![JSON data showing populated notes and users data with repetition](https://fullstackopen.com/static/ca7c3b6c859225179712112462a7c2b7/5a190/13new.png)
We can use the populate method for choosing the fields we want to include from the documents. In addition to the field _id_ we are now only interested in _content_ and _important_.

The selection of fields is done with the Mongo [syntax](https://www.mongodb.com/docs/manual/tutorial/project-fields-from-query-results/#return-the-specified-fields-and-the-_id-field-only):
```js
usersRouter.get('/', async (request, response) => {
  const users = await User
    .find({}).populate('notes', { content: 1, important: 1 })

  response.json(users)
})
```

The result is now exactly like we want it to be:
![combined data showing no repetition](https://fullstackopen.com/static/e526a27a1953221b1f00ef911436eb8b/5a190/14new.png)

Let's also add a suitable population of user information to notes in the _controllers/notes.js_ file:
```js
notesRouter.get('/', async (request, response) => {
  const notes = await Note
    .find({}).populate('user', { username: 1, name: 1 })

  response.json(notes)
})
```

Now the user's information is added to the _user_ field of note objects.
![notes JSON now has user info embedded too](https://fullstackopen.com/static/6f9441d387d1a6e6e90ec2c42aa88813/5a190/15new.png)

It's important to understand that the database does not know that the ids stored in the _user_ field of the notes collection reference documents in the user collection.

The functionality of the _populate_ method of Mongoose is based on the fact that we have defined "types" to the references in the Mongoose schema with the _ref_ option:
```js
const noteSchema = new mongoose.Schema({
  content: {
    type: String,
    required: true,
    minlength: 5
  },
  important: Boolean,
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User'
  }
})
```

## Next
The next section of FSO covers Token Auth: [[Webdev - FSO - Token Authentication]]