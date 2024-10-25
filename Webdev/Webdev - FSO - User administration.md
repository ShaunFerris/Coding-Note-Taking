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
        content: 'The most important operations of HTTP protocol are GET and POST',
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