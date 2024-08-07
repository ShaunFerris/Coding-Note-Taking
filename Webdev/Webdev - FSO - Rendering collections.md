#webdevSpecific 

## Javascript arrays
Data collections often come from the backend in the form of objects or arrays. The course content from FSO up to this point has avoided overcomplicating things for the beginner, but from this point on will be encouraging the use of the functional methods for manipulating arrays, meaning `map(), filter(), reduce()` etc. If you need to brush up on these check the mozillla docs.

## Rendering collections
For this section of the course content we will be looking at rendering collections of data on the front end of an app. In practice the data will ususally come from a backend service/database of some kind, but here we will start with rendering a hardcoded data collection. Lets start with an example app that looks like this and renders some notes (note the filenames are in comments at the top):

```jsx
//App.jsx
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        <li>{notes[0].content}</li>
        <li>{notes[1].content}</li>
        <li>{notes[2].content}</li>
      </ul>
    </div>
  )
}

export default App
```

```jsx
///main.jsx
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

const notes = [
  {
    id: 1,
    content: 'HTML is easy',
    important: true
  },
  {
    id: 2,
    content: 'Browser can execute only JavaScript',
    important: false
  },
  {
    id: 3,
    content: 'GET and POST are the most important methods of HTTP protocol',
    important: true
  }
]

ReactDOM.createRoot(document.getElementById('root')).render(
  <App notes={notes} />
)
```

Every note now contains it's text content, a boolean from importance, and a unique id. 

The above example is shit though. It only works when there are exactly three elements in the `notes` array - array elements are accessed from the `App` component using hard coded indices, which is dumb.

The obvious way to make this scale to any size of `notes` array is to use map:
```jsx
//DO NOT USE THIS:
<li>{notes[1].content}</li>

//USE THIS INSTEAD
notes.map(note => <li>{note.content}</li>)
```

In practice, you need the map to be in curly braces, to be evaluated in the jsx, and within a `<ul>` tag to be valid html:
```jsx
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>

      <ul>
        {notes.map(note => <li>{note.content}</li>)}
      </ul>
    </div>
  )
}
```

## Key attributes
The code above as written will work but will generate a warning in the console (or in your linter).  You need to give each `<li>` element that is generated by the map statement a unique key prop. 

React uses the key attributes of objects in an array to determine how to update the view generated by a component when the component is re-rendered. More about this is in the [React documentation](https://react.dev/learn/preserving-and-resetting-state#option-2-resetting-state-with-a-key).

## Map
The FSO course information takes a little detour here to cover the `.map` array method in JS. Search through the other notes in this repo or check the mozilla docs if you need a recap, I do not at this time so I am skipping it.

## Anti-pattern: Array indexes as keys
We could have made the error message on our console disappear by using the array indexes as keys. The indexes can be retrieved by passing a second parameter to the callback function of the _map_ method:
```jsx
notes.map((note, i) => ...)
```

When called like this, _i_ is assigned the value of the index of the position in the array where the note resides.

As such, one way to define the row generation without getting errors is:
```jsx
<ul>
  {notes.map((note, i) => 
    <li key={i}>
      {note.content}
    </li>
  )}
</ul>
```

This is, however, **not recommended** and can create undesired problems even if it seems to be working just fine.

Read more about this in [this article](https://robinpokorny.com/blog/index-as-a-key-is-an-anti-pattern/).

## Recapping things
At this point the FSO course covers refactoring the example to have components in seperate files, exporting them as modules and impoting the es6 modules where needed. 

Then they do a little recapping on debugging and using the console, I'm skipping these for note taking purposes as I already have a pretty good handle on it all.

## Next
The next part of the FSO course content is on forms: [[Webdev - FSO - Forms]]