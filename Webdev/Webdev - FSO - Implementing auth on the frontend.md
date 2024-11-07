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
The token returned with a successful login is saved to application state as the token field in the `user` object:
```jsx
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
    // ...
  }
}
```
Now let's fix the operation for creating a new note so that it works with the backend. We need to do this because the backend expects the token to be sent along with the POST request. 

We should edit the `noteService` like this:
```js
import axios from 'axios'
const baseUrl = '/api/notes'

let token = null

const setToken = newToken => {
  token = `Bearer ${newToken}`
}

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

const create = async newObject => {
  const config = {
    headers: { Authorization: token },
  }

  const response = await axios.post(baseUrl, newObject, config)
  return response.data
}

const update = (id, newObject) => {
  const request = axios.put(`${ baseUrl }/${id}`, newObject)
  return request.then(response => response.data)
}

export default { getAll, create, update, setToken }
```
The `noteService` module contains a private variable called `token` declared up the top. It's value can be changed with the `setToken` function which is exposed by the module as an export. `create`, now re-written with `async/await` syntax, sets the token to the Authorization header. The header is given to axios as part of a config object, as the third parameter of the POST method. 

The event handler responsible for login must be changed to call the method `noteService.setToken(user.token)` with a successful login:
```js
const handleLogin = async (event) => {
  event.preventDefault()
  try {
    const user = await loginService.login({
      username, password,
    })

    noteService.setToken(user.token)
    setUser(user)
    setUsername('')
    setPassword('')
  } catch (exception) {
    // ...
  }
}
```
And now adding new notes works again!

## Saving the token to local storage
Currently our app has a problem, in that the user will not be logged in after a refresh. **The token is saved to application state so it does not persist.**

This can be easily fixed by savign the login details to the browsers [local storage](https://developer.mozilla.org/en-US/docs/Web/API/Storage). Local Storage is a [key-value](https://en.wikipedia.org/wiki/Key-value_database) database in the browser. It is very easy to use. A _value_ corresponding to a certain _key_ is saved to the database with the method [setItem](https://developer.mozilla.org/en-US/docs/Web/API/Storage/setItem). For example:
```js
window.localStorage.setItem('name', 'juha tauriainen')
```
saves the string given as the second parameter as the value of the key _name_.

The value of a key can be found with the method [getItem](https://developer.mozilla.org/en-US/docs/Web/API/Storage/getItem):
```js
window.localStorage.getItem('name')
```
while [removeItem](https://developer.mozilla.org/en-US/docs/Web/API/Storage/removeItem) removes a key.

Values in the local storage are persisted even when the page is re-rendered. The storage is [origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin)-specific so each web application has its own storage.

Let's extend our application so that it saves the details of a logged-in user to the local storage.

Values saved to the storage are [DOMstrings](https://docs.w3cub.com/dom/domstring), so we cannot save a JavaScript object as it is. The object has to be parsed to JSON first, with the method _JSON.stringify_. Correspondingly, when a JSON object is read from the local storage, it has to be parsed back to JavaScript with _JSON.parse_.

Changes to the login method are as follows:
```js
  const handleLogin = async (event) => {
    event.preventDefault()
    try {
      const user = await loginService.login({
        username, password,
      })

      window.localStorage.setItem(        
	    'loggedNoteappUser', JSON.stringify(user)
	  )       
	  noteService.setToken(user.token)
      setUser(user)
      setUsername('')
      setPassword('')
    } catch (exception) {
      // ...
    }
  }
```
The details of a logged-in user are now saved to the local storage, and they can be viewed on the console (by typing _window.localStorage_ in it).

You can also inspect the local storage using the developer tools. On Chrome, go to the _Application_ tab and select _Local Storage_ (more details [here](https://developer.chrome.com/docs/devtools/storage/localstorage)). On Firefox go to the _Storage_ tab and select _Local Storage_ (details [here](https://firefox-source-docs.mozilla.org/devtools-user/storage_inspector/index.html)).

We still have to modify our application so that when we enter the page, the application checks if user details of a logged-in user can already be found on the local storage. If they are there, the details are saved to the state of the application and to _noteService_.

The right way to do this is with an [effect hook](https://react.dev/reference/react/useEffect): a mechanism we first encountered in [part 2](https://fullstackopen.com/en/part2/getting_data_from_server#effect-hooks), and used to fetch notes from the server.

We can have multiple effect hooks, so let's create a second one to handle the first loading of the page:
```js
const App = () => {
  const [notes, setNotes] = useState([]) 
  const [newNote, setNewNote] = useState('')
  const [showAll, setShowAll] = useState(true)
  const [errorMessage, setErrorMessage] = useState(null)
  const [username, setUsername] = useState('') 
  const [password, setPassword] = useState('') 
  const [user, setUser] = useState(null) 

  useEffect(() => {
    noteService
      .getAll().then(initialNotes => {
        setNotes(initialNotes)
      })
  }, [])

  useEffect(() => {    
    const loggedUserJSON = window.localStorage.getItem('loggedNoteappUser')            if (loggedUserJSON) {      
        const user = JSON.parse(loggedUserJSON)      
        setUser(user)      
        noteService.setToken(user.token)    
      }  
   }, [])
  // ...
}
```
The empty array as the parameter of the effect ensures that the effect is executed only when the component is rendered [for the first time](https://react.dev/reference/react/useEffect#parameters).

Now a user stays logged in to the application forever. We should probably add a _logout_ functionality, which removes the login details from the local storage. We will however leave it as an exercise.

It's possible to log out a user using the console, and that is enough for now. You can log out with the command:
```js
window.localStorage.removeItem('loggedNoteappUser')
```
or with the command which empties _localstorage_ completely:
```js
window.localStorage.clear()
```

## A note on using local storage
At the [end](https://fullstackopen.com/en/part4/token_authentication#problems-of-token-based-authentication) of the last part, we mentioned that the challenge of token-based authentication is how to cope with the situation when the API access of the token holder to the API needs to be revoked.

There are two solutions to the problem. The first one is to limit the validity period of a token. This forces the user to re-login to the app once the token has expired. The other approach is to save the validity information of each token to the backend database. This solution is often called a _server-side session_.

No matter how the validity of tokens is checked and ensured, saving a token in the local storage might contain a security risk if the application has a security vulnerability that allows [Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/) attacks. An XSS attack is possible if the application would allow a user to inject arbitrary JavaScript code (e.g. using a form) that the app would then execute. When using React sensibly it should not be possible since [React sanitizes](https://legacy.reactjs.org/docs/introducing-jsx.html#jsx-prevents-injection-attacks) all text that it renders, meaning that it is not executing the rendered content as JavaScript.

If one wants to play safe, the best option is to not store a token in local storage. This might be an option in situations where leaking a token might have tragic consequences.

It has been suggested that the identity of a signed-in user should be saved as [httpOnly cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#restrict_access_to_cookies), so that JavaScript code could not have any access to the token. The drawback of this solution is that it would make implementing SPA applications a bit more complex. One would need at least to implement a separate page for logging in.

However, it is good to notice that even the use of httpOnly cookies does not guarantee anything. It has even been suggested that httpOnly cookies are [not any safer than](https://academind.com/tutorials/localstorage-vs-cookies-xss/) the use of local storage.

So no matter the used solution the most important thing is to [minimize the risk](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html) of XSS attacks altogether.