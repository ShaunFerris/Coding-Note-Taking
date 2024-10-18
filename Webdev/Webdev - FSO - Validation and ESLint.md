#webdevSpecific 

When working on POST routes that receive input data from the client, we often want to validate that data in some way before commiting it to the database. The notes app for example, should not accept any new notes that have a missing or emtpy `content` prop. The validity of a new note can be checked against this rule in the relevant route handler like this:
```js
app.post('/api/notes', (request, response) => {
  const body = request.body
  if (body.content === undefined) {
    return response.status(400).json({ error: 'content missing' })
  }
  // ...
})
```
If the `content` prop is missing, we just respond with a 400, bad request.

Alternatively, we could validate the data prior to db storage by simply using the [validation](https://mongoosejs.com/docs/validation.html) functionality available in Mongoose. We can define specific validation rules for each field of a valid notes object in the schma for that object like this:
```js
const noteSchema = new mongoose.Schema({
  content: {
    type: String,
    minLength: 5,
    required: true
  },
  important: Boolean
})
```
The `content` field is now reuqired to be at least five characters long and it is not allowed to be empty (required). The `minLength` and `required` validators are built-in to the Mongoose library, but we can also use Mongoose's [custom validator](https://mongoosejs.com/docs/validation.html#custom-validators) functionality to crate new validators for our needs.

If we try to store an object in the db that breaks one of the validation constraints in the schema definition, the operation will throw an exception. Let's change our handler for the PUT route so that it passses any potential exceptions to an error handler middleware:
```js
app.post('/api/notes', (request, response, next) => {
  const body = request.body

  const note = new Note({
    content: body.content,
    important: body.important || false,
  })

  note.save()
    .then(savedNote => {
      response.json(savedNote)
    })
    .catch(error => next(error))
})
```
And now expand the error handler to deal with validation errors:
```js
const errorHandler = (error, request, response, next) => {
  console.error(error.message)

  if (error.name === 'CastError') {
    return response.status(400).send({ error: 'malformatted id' })
  } else if (error.name === 'ValidationError') {
    return response.status(400).json({ error: error.message })
  }

  next(error)
}
```
When validation for an object fails, we return the following default message from Mongoose:
![[Pasted image 20241018135852.png]]The back end will now have a problem though: validations are not done when editing a note. The Mongoose [documentation](https://mongoosejs.com/docs/validation.html#update-validators) addressest this issue by explaining that validations are not run be default when the `findOneAndUpdate` and related methods are called. **The fix for this is easy though, we can just include the "run validators" option in the options object for the method call:**
```js
app.put('/api/notes/:id', (request, response, next) => {
  const { content, important } = request.body

  Note.findByIdAndUpdate(
    request.params.id, 
    { content, important },
    { new: true, runValidators: true, context: 'query' }
  ) 
    .then(updatedNote => {
      response.json(updatedNote)
    })
    .catch(error => next(error))
})
```

## Deploying the database to prod
Going to mostly skip this section, as it is really only concerned with adding an env variable to a fly.io or render app, which is baby town easy and I already did without thinking as soon as I added mongo to the phonebook app.

