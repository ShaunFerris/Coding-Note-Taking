#webdevSpecific 

So far in the fso content, with all the example apps we have looked at, we have completely ignored automated testing. Automated testing is an integral and essentially non-optional part of moden software development. 

Let's start getting ready to test our example application by creating some utility functions to use in the tests. We will create a `utils/for_testing.js` file and define these utility functions:
```js
const reverse = (string) => {
  return string
    .split('')
    .reverse()
    .join('')
}

const average = (array) => {
  const reducer = (sum, item) => {
    return sum + item
  }
  return array.reduce(reducer, 0) / array.length
}

module.exports = {
  reverse,
  average,
}
```

## Test libs
There are a lot of options for testing libraries out there in the js ecosystem. Mocha was on top for a very long time, unitl it was replaced with jest. Currently jest is losing favor somewhat to [Vitest](https://vitest.dev/), which bills itself as a new generation of test libraries. **Vitest is my current favourite personally.** 

The point being that the test library in vogue may change over time but the concepts discssed here will mostly translate to all of them (for unit and integration tests, not end to end tests).

Nowadays, Node has it's own built in test library, [node:test](https://nodejs.org/docs/latest/api/test.html), which is well suited to the needs of the course.

Let's define the an npm script to run our tests:
```json
{
  //...
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "build:ui": "rm -rf build && cd ../frontend/ && npm run build && cp -r build ../backend",
    "deploy": "fly deploy",
    "deploy:full": "npm run build:ui && npm run deploy",
    "logs:prod": "fly logs",
    "lint": "eslint .",

    "test": "node --test"
  },
  //...
}
```
Now let's create some initial tests in `tests/reverse.test.js`:
```js
const { test } = require('node:test')
const assert = require('node:assert')

const reverse = require('../utils/for_testing').reverse

test('reverse of a', () => {
  const result = reverse('a')

  assert.strictEqual(result, 'a')
})

test('reverse of react', () => {
  const result = reverse('react')

  assert.strictEqual(result, 'tcaer')
})

test('reverse of saippuakauppias', () => {
  const result = reverse('saippuakauppias')

  assert.strictEqual(result, 'saippuakauppias')
})
```
The `test` defines the keyword test and the library assert, which is used by the tests to check the results of the functions under the test.

Individual cases are defined with the `test` function. The first arg of the function is the test description as a string. The second  is a function that defines the functionality of the test case. The content of the second test is:
```js
() => {
  const result = reverse('react')

  assert.strictEqual(result, 'tcaer')
}
```
First, we reverse the string "react", and then we assert that it should be equal to "tcaer" using the `strictEqual` method of the `assert` lib.

All the above tests pass as expected. Note that the node test runner looks for files containing `.test.js` in their name. 

We could break the test like this:
```js
test('reverse of react', () => {
  const result = reverse('react')

  assert.strictEqual(result, 'tkaer')
})
```
At which point we would get an error.

Let's put the tests for the _average_ function, into a new file called _tests/average.test.js_.
```js
const { test, describe } = require('node:test')

// ...

const average = require('../utils/for_testing').average

describe('average', () => {
  test('of one value is the value itself', () => {
    assert.strictEqual(average([1]), 1)
  })

  test('of many is calculated right', () => {
    assert.strictEqual(average([1, 2, 3, 4, 5, 6]), 3.5)
  })

  test('of empty array is zero', () => {
    assert.strictEqual(average([]), 0)
  })
})
```
The test reveals that the function does not work correctly with an empty array (this is because in JavaScript dividing by zero results in _NaN_).

Fixing the function is easy:
```js
const average = array => {
  const reducer = (sum, item) => {
    return sum + item
  }

  return array.length === 0
    ? 0
    : array.reduce(reducer, 0) / array.length
}
```
If the length of the array is 0 then we return 0, and in all other cases, we use the `reduce` method to calc the average.

There are a few things to notice about the tests that we just wrote. We defined a  describe block around the tests that were given the name `average`:
```js
describe('average', () => {
//tests go here
})
```
Describe blocks can be used for grouping tests into logical collections. The test output also uses the name of the describe block.

As we will see later on _describe_ blocks are necessary when we want to run some shared setup or teardown operations for a group of tests.

Another thing to notice is that we wrote the tests in quite a compact way, without assigning the output of the function being tested to a variable:
```js
test('of empty array is zero', () => {
  assert.strictEqual(average([]), 0)
})
```