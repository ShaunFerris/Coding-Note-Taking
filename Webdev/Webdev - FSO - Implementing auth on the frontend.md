#webdevSpecific 

Part three and four of the FSO content have focused heavily on the back end. The example front-ends from part 2 do not yet support the user management and auth funcitonalities we have practiced implementing on the back end. 

Thinking about the example notes app, the front-end can currently show existing notes and let users change the notes importance property. New notes cannot be added anymore because they are not submitted with a token, and the backend now expects a token to identify the user before it will complete the POST operation. 

We will now look at implementing user management and auth in the front end of the notes app example, with exercises doing it in the blog app example for real. Lets start with handling logins on the front end.

## Handling login
The first step would be to add a simple login form to the notes app. We can do this quick and dirty by editing the `App` component like this:
```jsx
const App = () => {
  const [notes, setNotes] = useState([]) 
  const [newNote, setNewNote] = useState('')
  const [showAll, setShowAll] = useState(true)
  const [errorMessage, setErrorMessage] = useState(null)
  const [username, setUsername] = useState('') 
  const [password, setPassword] = useState('') 

  useEffect(() => {
    noteService
      .getAll().then(initialNotes => {
        setNotes(initialNotes)
      })
  }, [])

  // ...

  const handleLogin = (event) => {
    event.preventDefault()
    console.log('logging in with', username, password)
  }

  return (
    <div>
      <h1>Notes</h1>

      <Notification message={errorMessage} />

      <form onSubmit={handleLogin}>
        <div>
          username
            <input
            type="text"
            value={username}
            name="Username"
            onChange={({ target }) => setUsername(target.value)}
          />
        </div>
        <div>
          password
            <input
            type="password"
            value={password}
            name="Password"
            onChange={({ target }) => setPassword(target.value)}
          />
        </div>
        <button type="submit">login</button>
      </form>

      // ...
    </div>
  )
}

export default App
```
Remember that the front-end app will not display any notes if the backend is not also running. You will need to run servers for both the back and front-ends in seperate terminal sessions, and then the notes from the database of the part-4 backend will show in the front-end.

The login form we hacked together in the `App` component is handled in the same way that we handled forms in part 2 of FSO. The form fields each have an event handler, which syncs the changes in the field to the state of the `App` component. The event handlers are simple, an object (the event object??) is passed as a param and they destructure the field `target` from the object and save it's value to state. 
```js
({ target }) => setUsername(target.value)
```
The method `handleLogin`, which is responsible for handling the data in the form,  is yet to be implemented in the above code, and currently only logs. 

Logging in is done by sending an HTTP POST request to the endpoint `/api/login` on the server. Let's separate the code that sends this request into it's own module in the file `services/login.js`. We will write this service with `async/await` syntax instead of promise chaining:
```js
import axios from 'axios'
const baseUrl = '/api/login'

const login = async credentials => {
  const response = await axios.post(baseUrl, credentials)
  return response.data
}

export default { login }
```
Notice we are still using axios for data fetching on the front end, but we could just as easily use the native `fetch` api.

Now we can write a method for handling the login from the login form like this:
```jsx
import loginService from './services/login'

const App = () => {
  // ...
  const [username, setUsername] = useState('') 
  const [password, setPassword] = useState('') 
  const [user, setUser] = useState(null)

  const handleLogin = async (event) => {
    event.preventDefault()
    
    try {
      const user = await loginService.login({
        username, password,
      })
      setUser(user)
      setUsername('')
      setPassword('')
    } catch (exception) {
      setErrorMessage('Wrong credentials')
      setTimeout(() => {
        setErrorMessage(null)
      }, 5000)
    }
    

  // ...
}
```
If the login is successful, the form fields are reset using the state values that are pinned to the field values. The response from the server which contains the users details and the auth token are returned and saved as state. If the login request fails or the `loginService.login` function throws an error, we catch the error and show the user an error notification. 

The user is not currently notified in any way if the login succeeds. Lets change that by showing the login form only in the user is not logged in. We can tell if a user is not logged in because the `user` state will be null. We should also make the new note form conditionally rendered, so that it is not shown when the user is not logged in. First we can move the forms jsx into helper functions that generate them:
```jsx
const App = () => {
  // ...

  const loginForm = () => (
    <form onSubmit={handleLogin}>
      <div>
        username
          <input
          type="text"
          value={username}
          name="Username"
          onChange={({ target }) => setUsername(target.value)}
        />
      </div>
      <div>
        password
          <input
          type="password"
          value={password}
          name="Password"
          onChange={({ target }) => setPassword(target.value)}
        />
      </div>
      <button type="submit">login</button>
    </form>      
  )

  const noteForm = () => (
    <form onSubmit={addNote}>
      <input
        value={newNote}
        onChange={handleNoteChange}
      />
      <button type="submit">save</button>
    </form>  
  )

  return (
    // ...
  )
}
```
Then we can conditionally render the forms like this:
```jsx
const App = () => {
  // ...

  const loginForm = () => (
    // ...
  )

  const noteForm = () => (
    // ...
  )

  return (
    <div>
      <h1>Notes</h1>

      <Notification message={errorMessage} />

      {user === null && loginForm()}
      {user !== null && noteForm()}

      <div>
        <button onClick={() => setShowAll(!showAll)}>
          show {showAll ? 'important' : 'all'}
        </button>
      </div>
      <ul>
        {notesToShow.map((note, i) => 
          <Note
            key={i}
            note={note} 
            toggleImportance={() => toggleImportanceOf(note.id)}
          />
        )}
      </ul>

      <Footer />
    </div>
  )
}
```
A slightly odd looking, but commonly used [React trick](https://react.dev/learn/conditional-rendering#logical-and-operator-) is used to render the forms conditionally:
```js
{
  user === null && loginForm()
}
```
If the first statement evaluates to false or is [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy), the second statement (generating the form) is not executed at all.

We can make this even more straightforward by using the [conditional operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator):
```jsx
return (
  <div>
    <h1>Notes</h1>

    <Notification message={errorMessage}/>

    {user === null ?
      loginForm() :
      noteForm()
    }

    <h2>Notes</h2>

    // ...

  </div>
)
```
Let's do one more modification. If the user is logged in, their name is shown on the screen:
```js
return (
  <div>
    <h1>Notes</h1>

    <Notification message={errorMessage} />

    {user === null ?
      loginForm() :
      <div>
        <p>{user.name} logged-in</p>
        {noteForm()}
      </div>
    }

    <h2>Notes</h2>

    // ...

  </div>
)
```

The solution isn't perfect, but we'll leave it like this for now.

Our main component _App_ is at the moment way too large. The changes we did now are a clear sign that the forms should be refactored into their own components. However, we will leave that for an optional exercise.

## Creating new notes
