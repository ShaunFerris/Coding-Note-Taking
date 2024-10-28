#webdevSpecific 

Users must be able to log into our application, and when a user is logged in, their user information must automatically be attached to any new notes they create.

In this section we're gonna look at implementing support for [token-based authentication](https://www.digitalocean.com/community/tutorials/the-ins-and-outs-of-token-based-authentication#how-token-based-works) to the backend.

The principle of token based auth can be dipicted like this:
![[Pasted image 20241028105653.png]]
To get started on this we should first implement logic for authenticating the user. Start by installing the jsonwebtoken library, which will allows us to generate [JSON web tokens](https://jwt.io/):
```bash
npm install jsonwebtoken
```

The code for login functionality could be something like the following, in a `controllers/login.js` file:
```js
const jwt = require('jsonwebtoken')
const bcrypt = require('bcrypt')
const loginRouter = require('express').Router()
const User = require('../models/user')

loginRouter.post('/', async (request, response) => {
  const { username, password } = request.body

  const user = await User.findOne({ username })
  const passwordCorrect = user === null
    ? false
    : await bcrypt.compare(password, user.passwordHash)

  if (!(user && passwordCorrect)) {
    return response.status(401).json({
      error: 'invalid username or password'
    })
  }

  const userForToken = {
    username: user.username,
    id: user._id,
  }

  const token = jwt.sign(userForToken, process.env.SECRET)

  response
    .status(200)
    .send({ token, username: user.username, name: user.name })
})

module.exports = loginRouter
```
First, we take the username from the request body and find the matching record in the users collection:
```js
const { username, password } = request.body
const user = await User.findOne({username})
```

Next, it checks the password against the hash in the database using bcrypts `compare` method:
```js
const passwordCorrect = user === null
  ? false
  : await bcrypt.compare(password, user.passwordHash)
```

If the user has no matching record, or if the password provided does not hash to the right value, we respond with a 401 unauthorized:
```js
if (!(user && passwordCorrect)) {
  return response.status(401).json({
    error: 'invalid username or password'
  })
}
```

If the password is correct, a token is created with the method `jwt.sign`. The token contains the username and the user id in a digitally signed form:
```js
const userForToken = {
  username: user.username,
  id: user._id,
}

const token = jwt.sign(userForToken, process.env.SECRET)
```

The token has been digitally signed using a string from the env variable SECRET as the secret. The digital signature ensures that only parties who know the secrete can generate a valid token. The value of this variable should be defined in the `.env` file. We respond to successful requests with a 200 OK. The generated token and the username of the user are sent back in the response body:
```js
response
  .status(200)
  .send({ token, username: user.username, name: user.name })
```

Now the code for login just has to be added to the application by adding the new router to `app.js`:
```js
const loginRouter = require('./controllers/login')

//...

app.use('/api/login', loginRouter)
```
If we tried sending a POST to the login route at this point it would fail. The command _jwt.sign(userForToken, process.env.SECRET)_ fails. We forgot to set a value to the environment variable _SECRET_. It can be any string. When we set the value in file _.env_ (and restart the server), the login works.

A successful login returns the user details and the token:
![vs code rest response showing details and token](https://fullstackopen.com/static/2e2ddac76483e17fded8f6fcc43fd7d4/5a190/18ea.png)

A wrong username or password returns an error message and the proper status code:
![vs code rest response for incorrect login details](https://fullstackopen.com/static/49fe09c494b9e591fa8811b1772404d5/5a190/19ea.png)

## Limiting the creation of notes to authenticated users
Lets make it so that new notes can only be created by logged in users, ie: posts without a valid JWT will be bounced.  The note should then be saved to the user identified in the JWT.

There are a few ways to send the token from the browser to the server. We will use the Authorization header. The header also tells which authentication scheme is used. This can be necessary if the server offers multiple ways to authenticate. Identifying the scheme tells the server how the attached credentials should be interpreted. 

The _Bearer_ scheme is suitable for our needs.

In practice this means that if the token is, for example the string _eyJhbGciOiJIUzI1NiIsInR5c2VybmFtZSI6Im1sdXVra2FpIiwiaW_, the Authorization header will have the value:

Bearer eyJhbGciOiJIUzI1NiIsInR5c2VybmFtZSI6Im1sdXVra2FpIiwiaW

Creating new notes will change like so (`controllers/notes.js`):
```js
const jwt = require('jsonwebtoken')

// ...

const getTokenFrom = request => {
  const authorization = request.get('authorization')
  if (authorization && authorization.startsWith('Bearer ')) {
    return authorization.replace('Bearer ', '')
  }
  return null
}

notesRouter.post('/', async (request, response) => {
  const body = request.body
  const decodedToken = jwt.verify(getTokenFrom(request), process.env.SECRET)
  if (!decodedToken.id) {
    return response.status(401).json({ error: 'token invalid' })
  }
  const user = await User.findById(decodedToken.id)

  const note = new Note({
    content: body.content,
    important: body.important === undefined ? false : body.important,
    user: user._id
  })

  const savedNote = await note.save()
  user.notes = user.notes.concat(savedNote._id)
  await user.save()

  response.json(savedNote)
})
```
The helper function _getTokenFrom_ isolates the token from the _authorization_ header. The validity of the token is checked with _jwt.verify_. The method also decodes the token, or returns the Object which the token was based on.
```js
const decodedToken = jwt.verify(token, process.env.SECRET)
```

If the token is missing or it is invalid, the exception _JsonWebTokenError_ is raised. We need to extend the error handling middleware to take care of this particular case:
```js
const errorHandler = (error, request, response, next) => {
  if (error.name === 'CastError') {
    return response.status(400).send({ error: 'malformatted id' })
  } else if (error.name === 'ValidationError') {
    return response.status(400).json({ error: error.message })
  } else if (error.name === 'MongoServerError' && error.message.includes('E11000 duplicate key error')) {
    return response.status(400).json({ error: 'expected `username` to be unique' })

  } else if (error.name ===  'JsonWebTokenError') {
    return response.status(401).json({ error: 'token invalid' })
  }

  next(error)
}
```
The object decoded from the token contains the _username_ and _id_ fields, which tell the server who made the request.

If the object decoded from the token does not contain the user's identity (_decodedToken.id_ is undefined), error status code [401 unauthorized](https://www.rfc-editor.org/rfc/rfc9110.html#name-401-unauthorized) is returned and the reason for the failure is explained in the response body.
```js
if (!decodedToken.id) {
  return response.status(401).json({
    error: 'token invalid'
  })
}
```

When the identity of the maker of the request is resolved, the execution continues as before.

A new note can now be created using Postman if the _authorization_ header is given the correct value, the string _Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ_, where the second value is the token returned by the _login_ operation.

Using Postman this looks as follows:
![postman adding bearer token](https://fullstackopen.com/static/cda2d24404da6db8b2ff17ee522e5f4e/5a190/20new.png)

and with Visual Studio Code REST client
![vscode adding bearer token example](https://fullstackopen.com/static/9bfbed9aace2b65be18a5699c7150a16/5a190/21new.png)

Current application code can be found on [GitHub](https://github.com/fullstack-hy2020/part3-notes-backend/tree/part4-9), branch _part4-9_.

If the application has multiple interfaces requiring identification, JWT's validation should be separated into its own middleware. An existing library like [express-jwt](https://www.npmjs.com/package/express-jwt) could also be used.

## Problems of Token-based authentication
Token auth is pretty easy to implement but it does have problems. Once the API user, for example a react client app, gets a token, the API has a blind trust to the token holder. What if the access rights fo the token holder need to be revoked? There are two solutions to the problem. The easier one is to limit the validity period of a token before issuing it:
```js
loginRouter.post('/', async (request, response) => {
  const { username, password } = request.body

  const user = await User.findOne({ username })
  const passwordCorrect = user === null
    ? false
    : await bcrypt.compare(password, user.passwordHash)

  if (!(user && passwordCorrect)) {
    return response.status(401).json({
      error: 'invalid username or password'
    })
  }

  const userForToken = {
    username: user.username,
    id: user._id,
  }

  // token expires in 60*60 seconds, that is, in one hour
  const token = jwt.sign(
    userForToken, 
    process.env.SECRET,
    { expiresIn: 60*60 }
  )

  response
    .status(200)
    .send({ token, username: user.username, name: user.name })
})
```
Once the token expires, the client app needs to get a new token. Usually this happens by forcing the user to re-login to the app. 

The error handling middleware should be extended to give a proper error in the case of an expired token:
```js
const errorHandler = (error, request, response, next) => {
  logger.error(error.message)

  if (error.name === 'CastError') {
    return response.status(400).send({ error: 'malformatted id' })
  } else if (error.name === 'ValidationError') {
    return response.status(400).json({ error: error.message })
  } else if (error.name === 'MongoServerError' && error.message.includes('E11000 duplicate key error')) {
    return response.status(400).json({
      error: 'expected `username` to be unique'
    })
  } else if (error.name === 'JsonWebTokenError') {
    return response.status(401).json({
      error: 'invalid token'
    })
  } else if (error.name === 'TokenExpiredError') {
    return response.status(401).json({
      error: 'token expired'
    })
  }

  next(error)
}
```

The shorter the expiration time, the more safe the solution is. So if the token gets into the wrong hands or user access to the system needs to be revoked, the token is only usable for a limited amount of time. On the other hand, a short expiration time forces a potential pain to a user, one must login to the system more frequently.

The other solution is to save info about each token to the backend database and to check for each API request if the access rights corresponding to the tokens are still valid. With this scheme, access rights can be revoked at any time. This kind of solution is often called a _server-side session_.

The negative aspect of server-side sessions is the increased complexity in the backend and also the effect on performance since the token validity needs to be checked for each API request to the database. Database access is considerably slower compared to checking the validity of the token itself. That is why it is quite common to save the session corresponding to a token to a _key-value database_ such as [Redis](https://redis.io/), that is limited in functionality compared to eg. MongoDB or a relational database, but extremely fast in some usage scenarios.

When server-side sessions are used, the token is quite often just a random string, that does not include any information about the user as it is quite often the case when jwt-tokens are used. For each API request, the server fetches the relevant information about the identity of the user from the database. It is also quite usual that instead of using Authorization-header, _cookies_ are used as the mechanism for transferring the token between the client and the server.

## End notes
There have been many changes to the code which have caused a typical problem for a fast-paced software project: most of the tests have broken. Because this part of the course is already jammed with new information, we will leave fixing the tests to a non-compulsory exercise.

Usernames, passwords and applications using token authentication must always be used over [HTTPS](https://en.wikipedia.org/wiki/HTTPS). We could use a Node [HTTPS](https://nodejs.org/docs/latest-v18.x/api/https.html) server in our application instead of the [HTTP](https://nodejs.org/docs/latest-v18.x/api/http.html) server (it requires more configuration). On the other hand, the production version of our application is in Fly.io, so our application stays secure: Fly.io routes all traffic between a browser and the Fly.io server over HTTPS.

We will implement login to the frontend in the [next part](https://fullstackopen.com/en/part5).

## Next
The next section of FSO content covers testing the front-end of full stack apps. It starts with: [[Webdev - FSO - Implementing auth on the frontend]]