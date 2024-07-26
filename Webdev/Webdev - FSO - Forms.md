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
