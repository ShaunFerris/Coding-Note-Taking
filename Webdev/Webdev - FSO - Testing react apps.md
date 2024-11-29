#webdevSpecific 

The FSO content used to use Jest for testing React apps, but vitest is the newer generation so they migrated. Apart from the configurations, the libraries provide the same programming interface, so there is virtually no difference in the test code.

Probably also good here to link my notes on Cypress for end to end testing React apps: [[JavaScript - Testing with Cypress - Intoduction]]

## Vitest
Get started by installing vitest itself, along with the library **jsdom**, which is used to simulate a webbrowser.
```bash
npm install --save-dev vitest jsdom
```
You will also need a library to render React components for testing. The best option right now (November 2024) is [react-testing-library](https://github.com/testing-library/react-testing-library). It is also worth extending the expressive power of the tests with the library [jest-dom](https://github.com/testing-library/jest-dom).
```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```
Next, we need to do some setup. We add a script to the `package.json` for running tests:
```json
{
  "scripts": {
    // ...
    "test": "vitest run"
  }
  // ...
}
```
and we create a file called `test-setup.js` in the project root containing the following:
```js
import { afterEach } from 'vitest'
import { cleanup } from '@testing-library/react'
import '@testing-library/jest-dom/vitest'

afterEach(() => {
  cleanup()
})
```
Now after each test is run, the cleanup function is executed to reset jsdom which is simulating the browser.

Next, expand the vite config with this:
```js
export default defineConfig({
  // ...
  test: {
    environment: 'jsdom',
    globals: true,
    setupFiles: './testSetup.js', 
  }
})
```
With _globals: true_, there is no need to import keywords such as _describe_, _test_ and _expect_ into the tests.

Finally, we can start writing tests. Let's start with tests for the component that is responsible for rendering a note. As a reminder, it looks like this:
```js
const Note = ({ note, toggleImportance }) => {
  const label = note.important
    ? 'make not important'
    : 'make important'

  return (
    <li className='note'>
      {note.content}
      <button onClick={toggleImportance}>{label}</button>
    </li>
  )
}
```
Note that the `<li>` has been given a `className` attribute. This can be used to access the component from our test code.

## Rendering the component for tests
We will write our tests for the `Note` component in `src/components/Note.test.jsx`, which is colocated with the component itself.

The first test will verify that the component renders the contents of the note given to it:
```js
import { render, screen } from '@testing-library/react'
import Note from './Note'

test('renders content', () => {
  const note = {
    content: 'Component testing is done with react-testing-library',
    important: true
  }

  render(<Note note={note} />)

  const element = screen.getByText('Component testing is done with react-testing-library')
  expect(element).toBeDefined()
})
```
After the initial configuration, the test renders the component with the [render](https://testing-library.com/docs/react-testing-library/api#render) function provided by the react-testing-library:
```js
render(<Note note={note} />)
```

Normally React components are rendered to the [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model). The render method we used renders the components in a format that is suitable for tests without rendering them to the DOM. We can use the object [screen](https://testing-library.com/docs/queries/about#screen) to access the rendered component. We use screen's method [getByText](https://testing-library.com/docs/queries/bytext) to search for an element that has the note content and ensure that it exists:
```js
  const element = screen.getByText('Component testing is done with react-testing-library')
  expect(element).toBeDefined()
```

The existence of an element is checked using Vitest's [expect](https://vitest.dev/api/expect.html#expect) command. Expect generates an assertion for its argument, the validity of which can be tested using various condition functions. Now we used [toBeDefined](https://vitest.dev/api/expect.html#tobedefined) which tests whether the _element_ argument of expect exists.

Run the test with command _npm test_:
```js
$ npm test

> notes-frontend@0.0.0 test
> vitest


 DEV  v1.3.1 /Users/mluukkai/opetus/2024-fs/part3/notes-frontend

 ✓ src/components/Note.test.jsx (1)
   ✓ renders content

 Test Files  1 passed (1)
      Tests  1 passed (1)
   Start at  17:05:37
   Duration  812ms (transform 31ms, setup 220ms, collect 11ms, tests 14ms, environment 395ms, prepare 70ms)


 PASS  Waiting for file changes...
```

Eslint complains about the keywords _test_ and _expect_ in the tests. The problem can be solved by installing [eslint-plugin-vitest-globals](https://www.npmjs.com/package/eslint-plugin-vitest-globals):
```text
npm install --save-dev eslint-plugin-vitest-globals
```
and enable the plugin by editing the _.eslintrc.cjs_ file as follows:
```js
module.exports = {
  root: true,
  env: {
    browser: true,
    es2020: true,
    "vitest-globals/env": true  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'plugin:react-hooks/recommended',
    'plugin:vitest-globals/recommended',  ],
  // ...
}
```

## Test file location
In React there are (at least) [two different conventions](https://medium.com/@JeffLombardJr/organizing-tests-in-jest-17fc431ff850) for the test file's location. We created our test files according to the current standard by placing them in the same directory as the component being tested.

The other convention is to store the test files "normally" in a separate _test_ directory. Whichever convention we choose, it is almost guaranteed to be wrong according to someone's opinion.

I (both the original FSO author and me, Shaun) do not like this way of storing tests and application code in the same directory. The reason we choose to follow this convention is that it is configured by default in applications created by Vite or create-react-app.

## Searching for content in a component
The `react-testing-library` package offers multiple ways to inspect the content of a tested component. The expect statement we used above 
```js
import { render, screen } from '@testing-library/react'
import Note from './Note'

test('renders content', () => {
  const note = {
    content: 'Component testing is done with react-testing-library',
    important: true
  }

  render(<Note note={note} />)

  const element = screen.getByText('Component testing is done with react-testing-library')

  expect(element).toBeDefined()})
```

Test fails if _getByText_ does not find the element it is looking for.

We could also use [CSS-selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors) to find rendered elements by using the method [querySelector](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector) of the object [container](https://testing-library.com/docs/react-testing-library/api/#container-1) that is one of the fields returned by the render:
```js
import { render, screen } from '@testing-library/react'
import Note from './Note'

test('renders content', () => {
  const note = {
    content: 'Component testing is done with react-testing-library',
    important: true
  }

  const { container } = render(<Note note={note} />)
  const div = container.querySelector('.note')  expect(div).toHaveTextContent(    'Component testing is done with react-testing-library'  )})
```
**NB:** A more consistent way of selecting elements is using a [data attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/data-*) that is specifically defined for testing purposes. Using _react-testing-library_, we can leverage the [getByTestId](https://testing-library.com/docs/queries/bytestid/) method to select elements with a specified _data-testid_ attribute.

## Debugging tests
There are a lot of problems you can run into when testing on the front-end, it is a complicated system. The object `screen` from react testing library provides a [debug](https://testing-library.com/docs/dom-testing-library/api-debugging#screendebug) method that can be used to print the HTML of a component to the terminal. If we add it to our test like this:
```js
import { render, screen } from '@testing-library/react'
import Note from './Note'

test('renders content', () => {
  const note = {
    content: 'Component testing is done with react-testing-library',
    important: true
  }

  render(<Note note={note} />)


  screen.debug()

  // ...

})
```
the html will print to the console like this:
```html
console.log
  <body>
    <div>
      <li
        class="note"
      >
        Component testing is done with react-testing-library
        <button>
          make not important
        </button>
      </li>
    </div>
  </body>
```

It is also possible to use the same method to print a wanted element to console:
```js
import { render, screen } from '@testing-library/react'
import Note from './Note'

test('renders content', () => {
  const note = {
    content: 'Component testing is done with react-testing-library',
    important: true
  }

  render(<Note note={note} />)

  const element = screen.getByText('Component testing is done with react-testing-library')

  screen.debug(element)
  expect(element).toBeDefined()
})
```

Now the HTML of the wanted element gets printed:
```html
  <li
    class="note"
  >
    Component testing is done with react-testing-library
    <button>
      make not important
    </button>
  </li>
```

## Clicking buttons in tests
