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
The `App` component in the notes example is becoming bloated. In line with the "single responsibility" principle, we should consider moving the communication with the server out of the `App` component and into it's own module.

Lets create a `src/services` directory and add a file there called `notes.js`:
```js
import axios from 'axios'
const baseUrl = 'http://localhost:3001/notes'

const getAll = () => {
  return axios.get(baseUrl)
}

const create = newObject => {
  return axios.post(baseUrl, newObject)
}

const update = (id, newObject) => {
  return axios.put(`${baseUrl}/${id}`, newObject)
}

export default { 
  getAll: getAll, 
  create: create, 
  update: update 
}
```

This module returns an object containing three functions as it's properties which all deal with notes operations. The functions directly return the promises returned by the axios methods.

This module is then imported into the `App` component:
```jsx
import noteService from './services/notes'

const App = () => {
```
And now the functions returned by it can be used from the imported module variable like this:
```jsx
const App = () => {
  // ...

  useEffect(() => {

    noteService
      .getAll()
      .then(response => {
        setNotes(response.data)
      })
  }, [])

  const toggleImportanceOf = id => {
    const note = notes.find(n => n.id === id)
    const changedNote = { ...note, important: !note.important }

    noteService
      .update(id, changedNote)
      .then(response => {
        setNotes(notes.map(note => note.id !== id ? note : response.data))
      })
  }

  const addNote = (event) => {
    event.preventDefault()
    const noteObject = {
      content: newNote,
      important: Math.random() > 0.5
    }

    noteService
      .create(noteObject)
      .then(response => {
        setNotes(notes.concat(response.data))
        setNewNote('')
      })
  }

  // ...
}

export default App
```
Notice the calls to `noteService.getAll()` etc.

We could take our implementation a step further. When the _App_ component uses the functions, it receives an object that contains the entire response for the HTTP request:
```js
noteService
  .getAll()
  .then(response => {
    setNotes(response.data)
  })
```

The _App_ component only uses the _response.data_ property of the response object.

The module would be much nicer to use if, instead of the entire HTTP response, we would only get the response data. Using the module would then look like this:
```js
noteService
  .getAll()
  .then(initialNotes => {
    setNotes(initialNotes)
  })
```

We can achieve this by changing the code in the module as follows (the current code contains some copy-paste, but we will tolerate that for now):
```js
import axios from 'axios'
const baseUrl = 'http://localhost:3001/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

const create = newObject => {
  const request = axios.post(baseUrl, newObject)
  return request.then(response => response.data)
}

const update = (id, newObject) => {
  const request = axios.put(`${baseUrl}/${id}`, newObject)
  return request.then(response => response.data)
}

export default { 
  getAll: getAll, 
  create: create, 
  update: update 
}
```

We no longer return the promise returned by axios directly. Instead, we assign the promise to the _request_ variable and call its _then_ method:
```js
const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}
```

The last row in our function is simply a more compact expression of the same code as shown below:
```js
const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => {
	  return response.data  
  })
}
```

The modified _getAll_ function still returns a promise, as the _then_ method of a promise also [returns a promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then).

After defining the parameter of the _then_ method to directly return _response.data_, we have gotten the _getAll_ function to work like we wanted it to. When the HTTP request is successful, the promise returns the data sent back in the response from the backend.

We have to update the _App_ component to work with the changes made to our module. We have to fix the callback functions given as parameters to the _noteService_ object's methods so that they use the directly returned response data:
```js
const App = () => {
  // ...

  useEffect(() => {
    noteService
      .getAll()
      .then(initialNotes => {setNotes(initialNotes)})
  }, [])

  const toggleImportanceOf = id => {
    const note = notes.find(n => n.id === id)
    const changedNote = { ...note, important: !note.important }

    noteService
      .update(id, changedNote)
      .then(returnedNote => {
	    setNotes(notes.map(note => note.id !== id ? note : returnedNote))
	})
  }

  const addNote = (event) => {
    event.preventDefault()
    const noteObject = {
      content: newNote,
      important: Math.random() > 0.5
    }

    noteService
      .create(noteObject)
      .then(returnedNote => {
	      setNotes(notes.concat(returnedNote))
	      setNewNote('')
      })
  }

  // ...
}
```

## Cleaner syntax for defining object literals
The module defining note-related services currently exports and object with the functions to make note changes in an object like this:
```js
export default { 
  getAll: getAll, 
  create: create, 
  update: update 
}
```

We can use a feature of ES6 to instead define it like this:
```js
{ 
  getAll, 
  create, 
  update 
}
```

## Promises and errors
If our application had implemented functionality to allow users to delete notes, then we could run into issues where a user tries to update the importance of a note that no longer exists. In this situation we would get a  404 response to the PUT request to replace a note that does not exist. 

The application should be able to handle these error situations gracefully, as users don't usually have the console open to see the error and will not know what happened. 

We had previously mentioned that promises can have three states, pending, fulfilled or rejected. Our example notes app does not handle rejected promises in any way. 

We can handle rejections by chaining the `.then()` method to a `catch()` method with a callback for an error state:
```jsx
axios
  .get('http://example.com/probably_will_fail')
  .then(response => {
    console.log('success!')
  })
  .catch(error => {
    console.log('fail')
  })
```

If the request fails, the event handler registered to the `catch` method will be called, and in the body of this callback we can add code to notify the user of what happened. 

We often add the catch method to the end of promise chains, and define it's callback to handle the situation where any of the promises in that chain fail and become rejected.
```jsx
axios
  .put(`${baseUrl}/${id}`, newObject)
  .then(response => response.data)
  .then(changedNote => {
    // ...
  })
  .catch(error => {
    console.log('fail')
  })
```

# Next
The next part of the FSO content can be found here: [[Webdev - FSO - Adding styles to react apps]]