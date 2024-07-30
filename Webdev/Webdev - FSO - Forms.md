#webdevSpecific 

This section of FSO course content will carry on from the section on using data collections, and will use the same example notes application. [[Webdev - FSO - Rendering collections]]

To get our page to update when new notes are added it's best to store the notes in the _App_ component's state. Let's import the [useState](https://react.dev/reference/react/useState) function and use it to define a piece of state that gets initialized with the initial notes array passed in the props.
```jsx
import { useState } from 'react'
import Note from './components/Note'


const App = (props) => {
  const [notes, setNotes] = useState(props.notes)

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
      </ul>
    </div>
  )
}

export default App 
```
 The above example initialises the statefull notes value to the array of notes passed as props. Next, lets add an HTML form for adding new notes:
 ```jsx
 const App = (props) => {
  const [notes, setNotes] = useState(props.notes)


  const addNote = (event) => {
    event.preventDefault()
    console.log('button clicked', event.target)
  }

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
      </ul>

      <form onSubmit={addNote}>
        <input />
        <button type="submit">save</button>
      </form>   
    </div>
  )
}
```
Here we have added a form with an even handler that fires on button click event, prevents the default behaviour of the form submission which would normally cause a page reload, and then logs the event target. <span style="color: cyan; font-weight: bold;">In this case, the target of the form submission event is the form itself. The html form element is logged, but how do we access the data?</span>

## Controlled components
There are multiple ways to extract the data from a submitted form, the first that we will look at is the controlled component strategy.

The first step to this is to store newly added notes in state, and connect this state to the value prop of the forms input element:
```jsx 
const App = (props) => {
  const [notes, setNotes] = useState(props.notes)

  const [newNote, setNewNote] = useState(
    'a new note...'
  ) 

  const addNote = (event) => {
    event.preventDefault()
    console.log('button clicked', event.target)
  }

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
      </ul>
      <form onSubmit={addNote}>

        <input value={newNote} />
        <button type="submit">save</button>
      </form>   
    </div>
  )
}
```

The initial value given to the `newNote` state will appear in the input now, but the input value can't be changed, because we haver given the input a value, but not given it an `onChange` function. This will be reflected by an error in the console. To fix this you would need to register an event handler to the on change event of the input, like this:
```jsx
const App = (props) => {
  const [notes, setNotes] = useState(props.notes)
  const [newNote, setNewNote] = useState(
    'a new note...'
  ) 

  // ...


  const handleNoteChange = (event) => {
    console.log(event.target.value)
    setNewNote(event.target.value)
  }

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
      </ul>
      <form onSubmit={addNote}>
        <input
          value={newNote}
          onChange={handleNoteChange}
        />
        <button type="submit">save</button>
      </form>   
    </div>
  )
}
```
The on change handler will be called whenever the inputs value changes, and will keep the newNote state value sync'd with it. <span style="color: cyan; font-weight: bold;">This is essentially what is meant by a controlled input</span>. Note that there is no need to prevent default behaviour in the onChange method as there is no default behaviour for an input change, unlike a form submission.

Now that the `newNote` state reflects the current value of the input at any given time, we can complete the `addNote` event handler like this:
```jsx
const addNote = (event) => {
  event.preventDefault()
  const noteObject = {
    content: newNote,
    important: Math.random() < 0.5,
    id: String(notes.length + 1),
  }

  setNotes(notes.concat(noteObject))
  setNewNote('')
}
```

Note that some better logic for the `important` property of the notes should be added later. For now we have made it such that any note added has a 50% chance to be important.

Also note that we are adding new notes to state using a call to the concat method, which as previously mentioned does NOT mutate the array, it returns a new copy with the new value added. <span style="color: cyan; font-weight: bold;">NEVER mutate state directly in React!</span>

## Filtering displayed elements
Next we will add functionality to the example app to allow only viewing the notes marked as important. We start this by adding a state value for a `showAll` flag, and then set the `notesToShow` variable to reflect the state this flag is set to:
```jsx
import { useState } from 'react'
import Note from './components/Note'

const App = (props) => {
  const [notes, setNotes] = useState(props.notes)
  const [newNote, setNewNote] = useState('') 
  const [showAll, setShowAll] = useState(true)

  // ...

  const notesToShow = showAll
    ? notes
    : notes.filter(note => note.important === true)

  return (
    <div>
      <h1>Notes</h1>
      <ul>

        {notesToShow.map(note =>
          <Note key={note.id} note={note} />
        )}
      </ul>
      // ...
    </div>
  )
}
```
Now if `showAll` evaluates to true, then `notesToShow` is all the notes, otherwise it is only notes marked important. Note the use of the ternary operator for conditional statements to set the `notesToShow` variable.

The comparison operator is redundant, since the value of _note.important_ is either _true_ or _false_, which means that we can simply write:
```jsx
notes.filter(note => note.important)
```
The filter function will include in the output any items for which the callback functions body evaluates to true.

Next, let's add functionality that enables users to toggle the _showAll_ state of the application from the user interface.
```jsx
import { useState } from 'react' 
import Note from './components/Note'

const App = (props) => {
  const [notes, setNotes] = useState(props.notes) 
  const [newNote, setNewNote] = useState('')
  const [showAll, setShowAll] = useState(true)

  // ...

  return (
    <div>
      <h1>Notes</h1>

      <div>
        <button onClick={() => setShowAll(!showAll)}>
          show {showAll ? 'important' : 'all' }
        </button>
      </div>
      <ul>
        {notesToShow.map(note =>
          <Note key={note.id} note={note} />
        )}
      </ul>
      // ...    
    </div>
  )
}
```

## Next
The next part of the FSO course content is on getting data from the server: [[Webdev - FSO - Getting data from the server]]

