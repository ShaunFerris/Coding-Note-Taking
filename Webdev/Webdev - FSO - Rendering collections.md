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

