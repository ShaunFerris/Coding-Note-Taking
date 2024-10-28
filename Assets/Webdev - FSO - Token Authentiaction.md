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
