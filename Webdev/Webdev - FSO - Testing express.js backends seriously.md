#webdevSpecific 

In the last block of FSO course content, we looked at writing basic tests, but only for utility functions that we wrote in preparation for actual tests. 

Moving on to test the actual backend, we should consider that the backend is currently so simple that it doesn't make sense to test it. It does not contain any complex logic or transformations of data. The only thing we could really unit test at the moment is the `toJSON` method used for formatting the notes. 

In some situations, it can be beneficial to implement some of the backend tests by mocking the database instead of using the real db. One libraray that could be used for this is [mongodb-memory-server](https://github.com/nodkz/mongodb-memory-server).

Since our apps backend is still relatively simple, we will decide to test the entire app through it's REST API, so that the database is also included. The kind of testing where multiple components of the system are being tested in a group is called [integration testing](https://en.wikipedia.org/wiki/Integration_testing). Unit testing would require isolation from the database and thus would involve mocking.

## Test environment
It is a good practice to get into the habit of maintaining seperate environments for production and development. In node this is done using the `NODE_ENV` environment variable. In our current apps we only load the env variables defined in the `.env` file if the app is not in production mode. 

It is common practice to define seperate modes for testing and devlopment, as well as prod. We can change the value of `NODE_ENV` by altering our scripts like this:
```json
{
  // ...
  "scripts": {
    "start": "NODE_ENV=production node index.js",
    "dev": "NODE_ENV=development nodemon index.js",
    "test": "NODE_ENV=test node --test",
    "build:ui": "rm -rf build && cd ../frontend/ && npm run build && cp -r build ../backend",
    "deploy": "fly deploy",
    "deploy:full": "npm run build:ui && npm run deploy",
    "logs:prod": "fly logs",
    "lint": "eslint .",
  },
  // ...
}
```
Specifying the environments like this works great, except it does not work on Windows machines. If you want it to work on windows, you need to install the `cross-env` package and preface the above commands with `cross-env`. Install it as a dev dep for local use or a regular dep if you are deploying on windows.

Now with different environments for test, dev and prod, we can start to modify the way that our app runs in the testing mode. An example of this would be having the app use a  different database for testing. 

We can create our separate test database in mongo atlas. This is not an optimal solution in situations where many people are devloping the same applications. Test execution in particular typically requires a single database instance that is not used by tests that are running concurrently.

**It would be better to run our tests using a database that is installed and running locally on the dev machine**. The solution would be to have every test execution use a seperate database. This is relatively simple to achieve by running Mongo0in-memory, or using Docker. **We will not complicate things with this right now and will continue using Atlas.**

Let's make some changes to the module that defines the application's configuration in `utils/config.js`:
```js
require('dotenv').config()

const PORT = process.env.PORT

const MONGODB_URI = process.env.NODE_ENV === 'test'   ? process.env.TEST_MONGODB_URI  : process.env.MONGODB_URI
module.exports = {
  MONGODB_URI,
  PORT
}
```

The _.env_ file has _separate variables_ for the database addresses of the development and test databases:
```bash
MONGODB_URI=mongodb+srv://fullstack:thepasswordishere@cluster0.o1opl.mongodb.net/noteApp?retryWrites=true&w=majority
PORT=3001

TEST_MONGODB_URI=mongodb+srv://fullstack:thepasswordishere@cluster0.o1opl.mongodb.net/testNoteApp?retryWrites=true&w=majority
```
The _config_ module that we have implemented slightly resembles the [node-config](https://github.com/lorenwest/node-config) package. Writing our implementation is justified since our application is simple, and also because it teaches us valuable lessons.

## Supertest
We are going to use supertest to help test the API. Install as normal:
```bash
npm install --save-dev supertest
```

Now we can write our first API test in the `tests/note_api.test.js` file:
```js
const { test, after } = require('node:test')
const mongoose = require('mongoose')
const supertest = require('supertest')
const app = require('../app')

const api = supertest(app)

test('notes are returned as json', async () => {
  await api
    .get('/api/notes')
    .expect(200)
    .expect('Content-Type', /application\/json/)
})

after(async () => {
  await mongoose.connection.close()
})
```
The test imports the Express application from the _app.js_ module and wraps it with the _supertest_ function into a so-called [superagent](https://github.com/visionmedia/superagent) object. This object is assigned to the _api_ variable and tests can use it for making HTTP requests to the backend.

Our test makes an HTTP GET request to the _api/notes_ url and verifies that the request is responded to with the status code 200. The test also verifies that the _Content-Type_ header is set to _application/json_, indicating that the data is in the desired format.

Checking the value of the header uses a bit strange looking syntax:
```js
.expect('Content-Type', /application\/json/)
```

The desired value is now defined as [regular expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) or in short regex. The regex starts and ends with a slash /, because the desired string _application/json_ also contains the same slash, it is preceded by a \ so that it is not interpreted as a regex termination character.

In principle, the test could also have been defined as a string:
```js
.expect('Content-Type', 'application/json')
```

The problem here, however, is that when using a string, the value of the header must be exactly the same. For the regex we defined, it is acceptable that the header _contains_ the string in question. The actual value of the header is _application/json; charset=utf-8_, i.e. it also contains information about character encoding. However, our test is not interested in this and therefore it is better to define the test as a regex instead of an exact string.

The test contains some details that we will explore [a bit later on](https://fullstackopen.com/en/part4/testing_the_backend#async-await). The arrow function that defines the test is preceded by the _async_ keyword and the method call for the _api_ object is preceded by the _await_ keyword. We will write a few tests and then take a closer look at this async/await magic. Do not concern yourself with them for now, just be assured that the example tests work correctly. The async/await syntax is related to the fact that making a request to the API is an _asynchronous_ operation. The async/await syntax can be used for writing asynchronous code with the appearance of synchronous code.

Once all the tests (there is currently only one) have finished running we have to close the database connection used by Mongoose. This can be easily achieved with the [after](https://nodejs.org/api/test.html#afterfn-options) method:
```js
after(async () => {
  await mongoose.connection.close()
})
```

One tiny but important detail: at the [beginning](https://fullstackopen.com/en/part4/structure_of_backend_application_introduction_to_testing#project-structure) of this part we extracted the Express application into the _app.js_ file, and the role of the _index.js_ file was changed to launch the application at the specified port via _app.listen_:
```js
const app = require('./app') // the actual Express app
const config = require('./utils/config')
const logger = require('./utils/logger')

app.listen(config.PORT, () => {
  logger.info(`Server running on port ${config.PORT}`)
})
```

The tests only use the Express application defined in the _app.js_ file, which does not listen to any ports:
```js
const mongoose = require('mongoose')
const supertest = require('supertest')
const app = require('../app')
const api = supertest(app)
// ...
```
The documentation for supertest says the following:

> _if the server is not already listening for connections then it is bound to an ephemeral port for you so there is no need to keep track of ports._

In other words, supertest takes care that the application being tested is started at the port that it uses internally.

Let's add two notes to the test database using the _mongo.js_ program (here we must remember to switch to the correct database url).

Let's write a few more tests:
```js
test('there are two notes', async () => {
  const response = await api.get('/api/notes')

  assert.strictEqual(response.body.length, 2)
})

test('the first note is about HTTP methods', async () => {
  const response = await api.get('/api/notes')

  const contents = response.body.map(e => e.content)
  assert.strictEqual(contents.includes('HTML is easy'), true)
})
```

Both tests store the response of the request to the _response_ variable, and unlike the previous test that used the methods provided by _supertest_ for verifying the status code and headers, this time we are inspecting the response data stored in _response.body_ property. Our tests verify the format and content of the response data with the method [strictEqual](https://nodejs.org/docs/latest/api/assert.html#assertstrictequalactual-expected-message) of the assert-library.

We could simplify the second test a bit, and use the [assert](https://nodejs.org/docs/latest/api/assert.html#assertokvalue-message) itself to verify that the note is among the returned ones:
```js
test('the first note is about HTTP methods', async () => {
  const response = await api.get('/api/notes')

  const contents = response.body.map(e => e.content)
  // is the argument truthy
  assert(contents.includes('HTML is easy'))
})
```

The benefit of using the async/await syntax is starting to become evident. Normally we would have to use callback functions to access the data returned by promises, but with the new syntax things are a lot more comfortable:
```js
const response = await api.get('/api/notes')

// execution gets here only after the HTTP request is complete
// the result of HTTP request is saved in variable response
assert.strictEqual(response.body.length, 2)
```

The middleware that outputs information about the HTTP requests is obstructing the test execution output. Let us modify the logger so that it does not print to the console in test mode:
```js
const info = (...params) => {
  if (process.env.NODE_ENV !== 'test') {     console.log(...params)  }}

const error = (...params) => {
  if (process.env.NODE_ENV !== 'test') {     console.error(...params)  }}

module.exports = {
  info, error
}
```

## Initializing the database before tests
