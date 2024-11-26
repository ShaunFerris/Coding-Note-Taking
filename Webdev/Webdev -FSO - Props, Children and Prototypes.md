#webdevSpecific 

## Displaying the login form conditionally
Going back to the theroetical example notes app, lets modify it so that the form for user login is not shown all the time, but only when the user wants to login in. We will make it so there is a button to display the login form, and if it is displayed, another button to hide it. Before implementing this behaviour we should start by extracting the login form into it's own component:
```jsx
const LoginForm = ({
   handleSubmit,
   handleUsernameChange,
   handlePasswordChange,
   username,
   password
  }) => {
  return (
    <div>
      <h2>Login</h2>
      <form onSubmit={handleSubmit}>
        <div>
          username
          <input
            value={username}
            onChange={handleUsernameChange}
          />
        </div>
        <div>
          password
          <input
            type="password"
            value={password}
            onChange={handlePasswordChange}
          />
      </div>
        <button type="submit">login</button>
      </form>
    </div>
  )
}

export default LoginForm
```

In the above implementation, all the state and functions that the component needs are passed as props from the parent component, in this case the `App` component in the `App.jsx` file. Note that the props are destructured into variables straight out of the props object.

One way to quickly implement the desired functionality for the login form is to edit the `App` component like this:
```jsx
const App = () => {
  const [loginVisible, setLoginVisible] = useState(false)

  // ...

  const loginForm = () => {
    const hideWhenVisible = { display: loginVisible ? 'none' : '' }
    const showWhenVisible = { display: loginVisible ? '' : 'none' }

    return (
      <div>
        <div style={hideWhenVisible}>
          <button onClick={() => setLoginVisible(true)}>log in</button>
        </div>
        <div style={showWhenVisible}>
          <LoginForm
            username={username}
            password={password}
            handleUsernameChange={({ target }) => setUsername(target.value)}
            handlePasswordChange={({ target }) => setPassword(target.value)}
            handleSubmit={handleLogin}
          />
          <button onClick={() => setLoginVisible(false)}>cancel</button>
        </div>
      </div>
    )
  }

  // ...
}
```
**Personally I hate this, becuase why would you need a loginForm function in the app component when you have extracted this, I think this is dumb as fuck**

The _App_ component state now contains the boolean _loginVisible_, which defines if the login form should be shown to the user or not.

The value of _loginVisible_ is toggled with two buttons. Both buttons have their event handlers defined directly in the component:
```js
<button onClick={() => setLoginVisible(true)}>log in</button>

<button onClick={() => setLoginVisible(false)}>cancel</button>
```

The visibility of the component is defined by giving the component an [inline](https://fullstackopen.com/en/part2/adding_styles_to_react_app#inline-styles) style rule, where the value of the [display](https://developer.mozilla.org/en-US/docs/Web/CSS/display) property is _none_ if we do not want the component to be displayed:
```js
const hideWhenVisible = { display: loginVisible ? 'none' : '' }
const showWhenVisible = { display: loginVisible ? '' : 'none' }

<div style={hideWhenVisible}>
  // button
</div>

<div style={showWhenVisible}>
  // button
</div>
```

We are once again using the "question mark" ternary operator. If _loginVisible_ is _true_, then the CSS rule of the component will be:
```css
display: 'none';
```
If _loginVisible_ is _false_, then _display_ will not receive any value related to the visibility of the component.

## Components and their children, aka props.children
The code related to managing the visibility of the login form could be considered to be its own logical entity, and for this reason, it would be good to extract it from the `App` component into a separate component. The goal here is to implement a new togglable component that can be used in the following way:
```jsx
<Togglable buttonLabel='login'>
  <LoginForm
    username={username}
    password={password}
    handleUsernameChange={({ target }) => setUsername(target.value)}
    handlePasswordChange={({ target }) => setPassword(target.value)}
    handleSubmit={handleLogin}
  />
</Togglable>
```

The way that the component is used is slightly different from our previous components. It has both opening and closing tags and is given the `LoginForm` component as a child. Any components could be given to the `Togglable` component as children, like this:
```jsx
<Togglable buttonLabel="reveal">
  <p>this line is at start hidden</p>
  <p>also this is hidden</p>
</Togglable>
```

The `Togglable` component could be implemented like this:
```jsx
import { useState } from 'react'

const Togglable = (props) => {
  const [visible, setVisible] = useState(false)

  const hideWhenVisible = { display: visible ? 'none' : '' }
  const showWhenVisible = { display: visible ? '' : 'none' }

  const toggleVisibility = () => {
    setVisible(!visible)
  }

  return (
    <div>
      <div style={hideWhenVisible}>
        <button onClick={toggleVisibility}>{props.buttonLabel}</button>
      </div>
      <div style={showWhenVisible}>
        {props.children}
        <button onClick={toggleVisibility}>cancel</button>
      </div>
    </div>
  )
}

export default Togglable
```

The new and interesting part of the code here is the use of `props.children`, which gives us access tot he child components of the current component. The child components are the React elements that we define between the opening and closing taghs of a component.

This time the children are rendered in the code that is used for rendering the component itself:
```jsx
<div style={showWhenVisible}>
  {props.children}
  <button onClick={toggleVisibility}>cancel</button>
</div>
```

Unlike the standard use of props that we have seen before, the `children` prop is added by React and always exists on every component. If a componnent is defined with an auto closing tag, `<compName />`, then `props.children` is an empty array. 

The `Togglable` component is reusable and we can use it to add similar visibility toggling functionality to the form that is used for creating new notes.

Before we do that, let's extract the form for creating notes into a component:
```js
const NoteForm = ({ onSubmit, handleChange, value}) => {
  return (
    <div>
      <h2>Create a new note</h2>

      <form onSubmit={onSubmit}>
        <input
          value={value}
          onChange={handleChange}
        />
        <button type="submit">save</button>
      </form>
    </div>
  )
}
```

Next let's define the form component inside of a `Togglable` component:
```js
<Togglable buttonLabel="new note">
  <NoteForm
    onSubmit={addNote}
    value={newNote}
    handleChange={handleNoteChange}
  />
</Togglable>
```


## State of the forms
Currently, in the example notes app, all of the state for the application is in the `App` component. The React docs give the following guidance about where to locate stateful values in a React app:

_Sometimes, you want the state of two components to always change together. To do it, remove state from both of them, move it to their closest common parent, and then pass it down to them via props. This is known as lifting state up, and it’s one of the most common things you will do writing React code._

If we think about the state of the forms, so for example the contents of a new note before it has been created, the `App` component does not need it for anything. We could just as well move the state of the forms to the corresponding components. **Note: in my work on the exercises for this section I have already done this, because I already learned this before the course**.

The component for creating a new note changes like so:
```jsx
import { useState } from 'react'

const NoteForm = ({ createNote }) => {
  const [newNote, setNewNote] = useState('')

  const addNote = (event) => {
    event.preventDefault()
    createNote({
      content: newNote,
      important: true
    })

    setNewNote('')
  }

  return (
    <div>
      <h2>Create a new note</h2>

      <form onSubmit={addNote}>
        <input
          value={newNote}
          onChange={event => setNewNote(event.target.value)}
        />
        <button type="submit">save</button>
      </form>
    </div>
  )
}

export default NoteForm
```
**NOTE** At the same time, we changed the behavior of the application so that new notes are important by default, i.e. the field _important_ gets the value _true_.

The `newNote` state and the event handler for changing it have been moved from the `App` component to the note form one. There is only one prop left, the `createNote` function, which the form calls when a new note is created. 

The `App` component becomes simpler now that we have got rid of the `newNote` state and its event handler. The `addNote` function for creating new notes receives a new note as a parameter, and the function is the only prop we send to the form:
```jsx
const App = () => {
  // ...

  const addNote = (noteObject) => {
    noteService
      .create(noteObject)
      .then(returnedNote => {
        setNotes(notes.concat(returnedNote))
      })
  }
  // ...
  const noteForm = () => (
    <Togglable buttonLabel='new note'>
      <NoteForm createNote={addNote} />
    </Togglable>
  )

  // ...
}
```
We could do the same for the log in form, but we'll leave that for an optional exercise.

## References to components with 'ref'
The current implementation is good, but it could be improved.

After a new note is created, it would make sense to hide the new note form. Currently, the form stays visible. There is a slight problem with hiding it, the visibility is controlled with the `visible` state variable inside the `Togglable` component. One solution to this would be to move control of the `Togglable` components state outside of the component itself. However, we won't do that here, because we want the component to be responsible for it's own state.
So we have to find another solution, and find a mechanism to change the state of the component externally.

There are several different ways to implement access to a component's functions from outside the component, but let's use the [ref](https://react.dev/learn/referencing-values-with-refs) mechanism of React, which offers a reference to the component.

Let's make the following changes to the _App_ component:
```js
import { useState, useEffect, useRef } from 'react'
const App = () => {
  // ...
  const noteFormRef = useRef()
  const noteForm = () => (
    <Togglable buttonLabel='new note' ref={noteFormRef}>      
	  <NoteForm createNote={addNote} />
    </Togglable>
  )

  // ...
}
```

The [useRef](https://react.dev/reference/react/useRef) hook is used to create a _noteFormRef_ reference, that is assigned to the _Togglable_ component containing the creation note form. The _noteFormRef_ variable acts as a reference to the component. This hook ensures the same reference (ref) that is kept throughout re-renders of the component.

We also make the following changes to the _Togglable_ component:
```js
import { useState, forwardRef, useImperativeHandle } from 'react'

const Togglable = forwardRef((props, refs) => {  
  const [visible, setVisible] = useState(false)

  const hideWhenVisible = { display: visible ? 'none' : '' }
  const showWhenVisible = { display: visible ? '' : 'none' }

  const toggleVisibility = () => {
    setVisible(!visible)
  }

  useImperativeHandle(refs, () => {    
    return { 
      toggleVisibility
    }  
  })
  
  return (
    <div>
      <div style={hideWhenVisible}>
        <button onClick={toggleVisibility}>{props.buttonLabel}</button>
      </div>
      <div style={showWhenVisible}>
        {props.children}
        <button onClick={toggleVisibility}>cancel</button>
      </div>
    </div>
  )
})
export default Togglable
```

The function that creates the component is wrapped inside of a [forwardRef](https://react.dev/reference/react/forwardRef) function call. This way the component can access the ref that is assigned to it.

The component uses the [useImperativeHandle](https://react.dev/reference/react/useImperativeHandle) hook to make its _toggleVisibility_ function available outside of the component.

We can now hide the form by calling _noteFormRef.current.toggleVisibility()_ after a new note has been created:
```js
const App = () => {
  // ...
  const addNote = (noteObject) => {
    noteFormRef.current.toggleVisibility()    
    noteService
      .create(noteObject)
      .then(returnedNote => {     
        setNotes(notes.concat(returnedNote))
      })
  }
  // ...
}
```

To recap, the [useImperativeHandle](https://react.dev/reference/react/useImperativeHandle) function is a React hook, that is used for defining functions in a component, which can be invoked from outside of the component.

This trick works for changing the state of a component, but it looks a bit unpleasant. We could have accomplished the same functionality with slightly cleaner code using "old React" class-based components. We will take a look at these class components during part 7 of the course material. So far this is the only situation where using React hooks leads to code that is not cleaner than with class components.

There are also [other use cases](https://react.dev/learn/manipulating-the-dom-with-refs) for refs than accessing React components.

## One point about components
When we define a component in React:
```js
const Togglable = () => ...
  // ...
}
```

And use it like this:
```js
<div>
  <Togglable buttonLabel="1" ref={togglable1}>
    first
  </Togglable>

  <Togglable buttonLabel="2" ref={togglable2}>
    second
  </Togglable>

  <Togglable buttonLabel="3" ref={togglable3}>
    third
  </Togglable>
</div>
```

We create _three separate instances of the component_ that all have their separate state:
![browser of three togglable components](https://fullstackopen.com/static/c7355696281ca0c4d8d1e734a1d81a26/5a190/12e.png)
The _ref_ attribute is used for assigning a reference to each of the components in the variables _togglable1_, _togglable2_ and _togglable3_.

## Prop types
The _Togglable_ component assumes that it is given the text for the button via the _buttonLabel_ prop. If we forget to define it to the component:
```js
<Togglable> buttonLabel forgotten... </Togglable>
```

The application works, but the browser renders a button that has no label text. We would like to enforce that when the _Togglable_ component is used, the button label text prop must be given a value. **NOTE: this obviously wouldn't be a problem if you just used typescript**.

The expected and required props of a component can be defined with the [prop-types](https://github.com/facebook/prop-types) package. Let's install the package:
```shell
npm install prop-types
```

We can define the _buttonLabel_ prop as a mandatory or _required_ string-type prop as shown below:
```js
import PropTypes from 'prop-types'

const Togglable = React.forwardRef((props, ref) => {
  // ..
})

Togglable.propTypes = {
  buttonLabel: PropTypes.string.isRequired
}
```

The console will display the following error message if the prop is left undefined:
![console error stating buttonLabel is undefined](https://fullstackopen.com/static/7a239ed6d3ad6721a65ae3ac24eb29b5/5a190/15.png)

The application still works and nothing forces us to define props despite the PropTypes definitions. Mind you, it is extremely unprofessional to leave _any_ red output in the browser console.

Let's also define PropTypes to the _LoginForm_ component:
```js
import PropTypes from 'prop-types'

const LoginForm = ({
   handleSubmit,
   handleUsernameChange,
   handlePasswordChange,
   username,
   password
  }) => {
    // ...
  }

LoginForm.propTypes = {
  handleSubmit: PropTypes.func.isRequired,
  handleUsernameChange: PropTypes.func.isRequired,
  handlePasswordChange: PropTypes.func.isRequired,
  username: PropTypes.string.isRequired,
  password: PropTypes.string.isRequired
}
```

If the type of a passed prop is wrong, e.g. if we try to define the _handleSubmit_ prop as a string, then this will result in the following warning:

![console error saying handleSubmit expected a function](https://fullstackopen.com/static/ec732518823c5e2921d46285e5549bf3/5a190/16.png)

## ESlint
In part 3 we configured the [ESlint](https://fullstackopen.com/en/part3/validation_and_es_lint#lint) code style tool to the backend. Let's take ESlint to use in the frontend as well.

Vite has installed ESlint to the project by default, so all that's left for us to do is define our desired configuration in the _.eslintrc.cjs_ file.

Let's create a _.eslintrc.cjs_ file with the following contents:
```js
module.exports = {
  root: true,
  env: {
    browser: true,
    es2020: true,
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'plugin:react-hooks/recommended',
  ],
  ignorePatterns: ['dist', '.eslintrc.cjs'],
  parserOptions: { ecmaVersion: 'latest', sourceType: 'module' },
  settings: { react: { version: '18.2' } },
  plugins: ['react-refresh'],
  rules: {
    "indent": [
        "error",
        2  
    ],
    "linebreak-style": [
        "error",
        "unix"
    ],
    "quotes": [
        "error",
        "single"
    ],
    "semi": [
        "error",
        "never"
    ],
    "eqeqeq": "error",
    "no-trailing-spaces": "error",
    "object-curly-spacing": [
        "error", "always"
    ],
    "arrow-spacing": [
        "error", { "before": true, "after": true }
    ],
    "no-console": 0,
    "react/react-in-jsx-scope": "off",
    "react/prop-types": 0,
    "no-unused-vars": 0    
  },
}
```
NOTE: If you are using Visual Studio Code together with ESLint plugin, you might need to add a workspace setting for it to work. If you are seeing `Failed to load plugin react: Cannot find module 'eslint-plugin-react'` additional configuration is needed. Adding the line `"eslint.workingDirectories": [{ "mode": "auto" }]` to settings.json in the workspace seems to work. See [here](https://github.com/microsoft/vscode-eslint/issues/880#issuecomment-578052807) for more information.

Let's create [.eslintignore](https://eslint.org/docs/latest/use/configure/ignore#the-eslintignore-file) file with the following contents to the repository root

```bash
node_modules
dist
.eslintrc.cjs
vite.config.js
```

Now the directories _dist_ and _node_modules_ will be skipped when linting.

As usual, you can perform the linting either from the command line with the command
```bash
npm run lint
```
or using the editor's Eslint plugin.

Component _Togglable_ causes a nasty-looking warning _Component definition is missing display name_:
![vscode showing component definition error](https://fullstackopen.com/static/f61843245205294dd4fbf50d8b864dd7/5a190/25x.png)

The react-devtools also reveals that the component does not have a name:

![react devtools showing forwardRef as anonymous](https://fullstackopen.com/static/1fc750ed2c0c78b8736615837a6be1a0/5a190/26ea.png)

Fortunately, this is easy to fix
```js
import { useState, useImperativeHandle } from 'react'
import PropTypes from 'prop-types'

const Togglable = React.forwardRef((props, ref) => {
  // ...
})

Togglable.displayName = 'Togglable'
export default Togglable
```

## Next
The next section, 5c covers testing frontend apps: [[Webdev - FSO - Testing react apps]]