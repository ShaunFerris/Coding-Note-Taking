#webdevSpecific 

Thinking again about the notes app that has been used as an example in the previous parts of FSO part 2 content, the next thing we need to figure out is storing newly added notes data in our json database. 

The json-server package that we have been using as a stand-in for a real database claims to be a RESTful API. It is not an exact match of the definition of a REST API, but neither are most things that cliam to be restful.

We will cover REST as a concept in greater depth in the part 3 FSO content, but here will cover some basics, in particular the conventional use of routes (aka URLs) and HTTP request types.

## REST
NOTE: It should be capitalised but I am not going to do so every time cos I can't be arsed.

In rest terminology, individual data objects, like the notes in the example app or the persons in the phonebook exercise are referred to as "resources". Every resource has a unique address associated with it - a URL. according to the general convention used by json-server, we can locate an individual note at the URL `notes/3` where 3 is the id of the resource. The `notes` URL on the other hand will point to a collection containing all of the notes. For reference, the notes object I am referring to looks like this:
```json
{
  "notes": [
    {
      "id": "1",
      "content": "HTML is easy",
      "important": true
    },
    {
      "id": "2",
      "content": "Browser can execute only JavaScript",
      "important": false
    },
    {
      "id": "3",
      "content": "GET and POST are the most important methods of HTTP protocol",
      "important": true
    }
  ]
}
```

Resources are fetched from the server using an HTTP GET request, ie a request to `http://localhost:3001/notes/3` will return the note with id == 3. 

Creating a new resource for storing a new note is done with an HTTP POST request to the `notes` URL. The data for the new resource is sent in the `body` of the request. 

The json-server package requires that all data be sent in JSON format. In practice this means that the data must be a correctly formatted string and that the request must contain the `Content-Type` request header with the value `application/json`.

## Sending data to the server
Lets make the following changes to the event handler for adding a note in the example app:
```jsx
addNote = event => {
  event.preventDefault()
  const noteObject = {
    content: newNote,
    important: Math.random() < 0.5,
  }

  axios
    .post('http://localhost:3001/notes', noteObject)
    .then(response => {
      console.log(response)
    })
}
```

We create a new object for the new note, but leave out the id, as it is better practice to let the server create id's for new resources. 

The object is sent to the server using the axios POST method. The registered event handler logs the response that it gets back from the server. 

Because we used the POST method in axios, the library knows to add the appropriate headers to the request (content-type in this case).

Using the above, we have sent the new note to the json-server, but it is not reflected in the UI yet. To achieve this we need to update the notes stateas part of the operation, like this:
```jsx
addNote = event => {
  event.preventDefault()
  const noteObject = {
    content: newNote,
    important: Math.random() > 0.5,
  }

  axios
    .post('http://localhost:3001/notes', noteObject)
    .then(response => {
      setNotes(notes.concat(response.data))
      setNewNote('')
    })
}
```

Now the new note returned in the response from the backend server is added to the list of notes in the applications state using the useState setter. 

Once the data returned by the server starts to have an effect on the behaviour of our web apps we are faced with new challenges arising from, for instance, the async nature of the communication between server and client. This necessitates new debugging strategies, console logging and other means of debugging become increasingly more important. We must also develop a sufficient understanding of the principles of both the JavaScript runtime and React components. Guessing won't be enough.

It's beneficial to inspect the state of the backend server, e.g. through the browser by visiting `https://localhost:3001/notes`.

## Updating the importance of notes
In the notes example, we can add a button to toggle the importance of the individual notes, by modifying the `Note` components like this:
```jsx
const Note = ({ note, toggleImportance }) => {
  const label = note.important
    ? 'make not important' : 'make important'

  return (
    <li>
      {note.content} 
      <button onClick={toggleImportance}>{label}</button>
    </li>
  )
}
```

We now have a button that switches its label depending on if the note is currently marked important or not. It also has a bound `onClick` event handler passed as props. This handler should be defined in the `App` component and passed to every `Note`.
```jsx
const App = () => {
  const [notes, setNotes] = useState([]) 
  const [newNote, setNewNote] = useState('')
  const [showAll, setShowAll] = useState(true)

  // ...


  const toggleImportanceOf = (id) => {
    console.log('importance of ' + id + ' needs to be toggled')
  }

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
          <Note
            key={note.id}
            note={note} 

            toggleImportance={() => toggleImportanceOf(note.id)}
          />
        )}
      </ul>
      // ...
    </div>
  )
}
```

Notice that each individual note recieves it's own version of the `toggleImportance` handler, because they each have a differen id. E.g., if _note.id_ is 3, the event handler function returned by _toggleImportance(note.id)_ will be:
```js
() => { console.log('importance of 3 needs to be toggled') }
```

When modifying data on the server, like changing the importance of a note, we can do it using either a PATCH or a PUT request. The PUT request will replace the entire note, whereas the PATCH request will directly mutate some properties of the existing note object. 

The final form of the `toggleImportanceOf` handler would look something like this:
```jsx
const toggleImportanceOf = id => {
  const url = `http://localhost:3001/notes/${id}`
  const note = notes.find(n => n.id === id)
  const changedNote = { ...note, important: !note.important }

  axios.put(url, changedNote).then(response => {
    setNotes(notes.map(n => n.id !== id ? n : response.data))
  })
}
```

The unique URL of the resource to modify is defined using a template string, then we save the note to modify into a variable with the `Array.find()` method. Next the note is updated by spreading a copy  into a new variable with updated `important` property. Finally, we use a PUT request to replace the old object with the modified one, and then update the state.

Why did we make a copy of the note object we wanted to modify when the following code also appears to work?
```js
const note = notes.find(n => n.id === id)
note.important = !note.important

axios.put(url, note).then(response => {
  // ...
```
This is not recommended because the variable _note_ is a reference to an item in the _notes_ array in the component's state, and as we recall we must [never mutate state directly](https://react.dev/learn/updating-objects-in-state#why-is-mutating-state-not-recommended-in-react) in React.

It's also worth noting that the new object _changedNote_ is only a so-called [shallow copy](https://en.wikipedia.org/wiki/Object_copying#Shallow_copy), meaning that the values of the new object are the same as the values of the old object. If the values of the old object were objects themselves, then the copied values in the new object would reference the same objects that were in the old object.

## Extracting backend communication into a seperate module
