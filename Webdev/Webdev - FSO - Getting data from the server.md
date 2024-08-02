#webdevSpecific 

Through all of the FSO course content covered so far, it has only touched on front-end development. Back end content will begin in part three ([[Webdev - FSO - Node.js and Express]]), but before getting into that we need to look at how the front-end of an application consumes data that it gets from the back end.

To start this section we will be using a tool meant for use in development and testing, called JSON server as a stand in for a proper back end server. This tool is available over npm, and can serve a json file over HTTP, using the local host ports. 

You can either install it globally with:
```bash
npm install -g json-server
```
and then serve your desired JSON with:
```bash
json-server --port 3001 --watch db.json
```
In the above example the json file we want to serve is called db.json, and should be in the directory from which you run the command. The watch flag automatically updates the served file if there are changes, and the port flag changes from the default port of 3000 to a specified one. 

You can also run it from the directory of the json file you want to serve with npx to avoid installing globally:
```bash
npx json-server --port 3001 --watch db.json
```

For the purposes of this example, the contents of the `db.json` file should be this:
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
and once served by the server, you can see the contents by navigating to `localhost:3001/notes` in the browser. Note the /notes in the url comes from the name of the json object. Also note that you can install a json view plugin for your browser to get a prettier view of the data.

Going forward in this exercise, the idea will be to save notes added in our example notes app to the server, which in this case means adding them to the json file being served. 

json-server stores all the data in the _db.json_ file, which resides on the server. In the real world, data would be stored in some kind of database. However, json-server is a handy tool that enables the use of server-side functionality in the development phase without the need to program any of it.

## The browser as a runtime environment
The first task for the example notes app in this section is to fetch the data from the JSON server so that the notes can be displayed on the front-end. In the first block of content from FSO ([[Webdev - FSO - fundamentals of web apps]]), we already looked at a way to fetch data using Javascript, with the XMLHttpRequest (XHR) constructor. You can read the above note and also check the mozilla docs for more info on it [here](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest).

XHR has been supported since 1999 but is not longer recommended now, instead you should use the fetch method. Fetch is based on promises while XHR is event driven. Read more about fetch on the mozilla docs [here](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

As a reminder for how to use XHR for fetching (WHICH YOU SHOULDN'T DO WITHOUT GOOD REASON), it looks like this:
```js
const xhttp = new XMLHttpRequest()

xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    const data = JSON.parse(this.responseText)
    // handle the response that is saved in variable data
  }
}

xhttp.open('GET', '/data.json', true)
xhttp.send()
```

We initialise a new XHR object and then register to it an event handler that will fire whenever the request object changes. If the change in state means that the response to the request has arrived then the data can be handled accordingly. 

Note that the code in the event handler is defined before the request is sent to the server, the even handler will fire at some later point in time, so it is asynchronous. A synchronous version of this code in Java would look something like this:
```java
HTTPRequest request = new HTTPRequest();

String url = "https://studies.cs.helsinki.fi/exampleapp/data.json";
List<Note> notes = request.get(url);

notes.forEach(m => {
  System.out.println(m.content);
});
```
The above would execute line by line, and at the line that makes the request to the url, the program would pause execution until the reponse is recieved and saved into the variable. Again, this is SYNCHRONOUS execution.

In contrast, JS engines or runtime envs follow an ASYNCHRONOUS model, meaning that all IO operations (with some exceptions) are required to be non-blocking. Simply put this means that the code continues to execute after and IO call is made, without waiting for it to return.

When an async operation is finished, or at some point after it is finished, the JS engine calls any event handlers registered to the operation.

JS engines are single-threaded, meaning code cannot execute in parallel. This is why IO operations must be non-blocking, otherwise the UI would freeze up any time data was fetched.

For the browser to remain _responsive_, i.e., to be able to continuously react to user operations with sufficient speed, the code logic needs to be such that no single computation can take too long.

A great source of more info on this topic is [this](https://www.youtube.com/watch?v=8aGhZQkoFbQ)by Phillip Roberts at JSConf.

## NPM
Going back to fetching data from the server, we could do it using the promise based fetch api, supported by all browsers. We will not however be doing that, and will instead use a library called axios. It functions very much like fetch, but is more user friendly and fills the double role of getting us more comfortable with using NPM to add packages to React projects. 

Nowadays, almost all JS projects are defined using NPM (the node package manager). Projects using Vite also follow the npm format and use a `package.json` file at the project root to track settings and dependancies. 

NPM commands should always be run in the project root. To intall axios into a project run:
```bash
npm install axios
```
Axios will now be included in the projects `package.json`. 

The libraries code will be downloaded and added to the `node_modules` directory at the project root. 

For the notes app example we have been talking about we will also want the json server package.  This package however will only be used in development, not in the final build, so we will include it as a development only dependancy like this:
```bash
npm install json-server --save-dev
```

You could then add a script to your `package.json` like this:
```json
{
  // ... 
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint . --ext js,jsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview",

    "server": "json-server -p3001 --watch db.json"
  },
}
```
which allows you to start the json server with:
```bash
npm run server
```

## Axios and promises
Going forward with the notes app example, it is assumed that the json-server is running on port 3000, serving the json notes data above. 

The axios library can be included the same way that React has been included in previous examples, by using an es6 import statement. 

If we add the following to the `main.jsx` file of the example notes app:
```jsx
import axios from 'axios'

const promise = axios.get('http://localhost:3001/notes')
console.log(promise)

const promise2 = axios.get('http://localhost:3001/foobar')
console.log(promise2)
```
we will see two entries in the console similar to this:
![[Pasted image 20240802113442.png]]
The `.get()` method of axios returns a promise. The definition of a promise taken from the mozilla docs is:

<div style="color: cyan; font-weight: bold; display: flex; justify-content: center; text-align: center">A Promise is an object representing the eventual completion of failure of an asynchronous operation.</div>

A promise can have three states, corresponding to the state of the request that it represents: Pending, fulfilled or rejected. 

The first promise in the above console screenshot is fulfilled, because the request succeeded and returned the json object. The second was rejected, because the `/foobar` endpoint does not exist on the json file that we are serving. 

If we want to access the data returned by the successful request to the `notes` endpoint on the json server, we should use the `.then()` method on promises to define a callback which will fire when the promise is successfully resolved:
```jsx
const promise = axios.get('http://localhost:3001/notes')

promise.then(response => {
  console.log(response)
})
```
The JavaScript runtime environment calls the callback function registered by the _then_ method providing it with a _response_ object as a parameter. The _response_ object contains all the essential data related to the response of an HTTP GET request, which would include the returned _data_, _status code_, and _headers_. It looks something like this when printed to the console:
![[Pasted image 20240802114330.png]]

Rather than storing the promise object in a variable, we can just chain the `.then()` method directly onto the get request like this:
```jsx
axios.get('http://localhost:3001/notes').then(response => {
  const notes = response.data
  console.log(notes)
})
```
Now the resulting response object is stored in a variable rather than the promise object.

The data returned by the server is plain text, basically just one long string. The axios library is still able to parse the data into a JavaScript array, since the server has specified that the data format is _application/json; charset=utf-8_ (see the previous image) using the _content-type_ header.

We can finally begin using the data fetched from the server.

Let's try and request the notes from our local server and render them, initially as the App component. Please note that this approach has many issues, as we're rendering the entire _App_ component only when we successfully retrieve a response:
```jsx
import ReactDOM from 'react-dom/client'
import axios from 'axios'
import App from './App'

axios.get('http://localhost:3001/notes').then(response => {
  const notes = response.data
  
ReactDOM.createRoot(document.getElementById('root')).render(<App notes={notes} />)
})
```
Instead of doing this, we should move the data fetching to within the `App` component.

## Effect hooks
The FSO course content so far has used a lot of state hooks (useState), which provide state to React components defined as functions. There is also an effect hook for use in function components, useEffect. From the React docs:

<div style="color: cyan; font-weight: bold; display: flex; justify-content: center; text-align: center">> Effects let a component connect to and synchronize with external systems. This includes dealing with network, browser DOM, animations, widgets written using a different UI library, and other non-React code.</div>

As such, effect hooks are a good tool to use when fetching data from a server. (This is becoming less of a standard these days I think, with fetching in server components and things like SWR. SWR in particular appeals to me a lot, it's docs are [here](https://swr.vercel.app/))

Removing the axios calls from the `main.jsx` file, and moving data fetching to `App.jsx`, the files will now look like this:
```jsx
//main.jsx
import ReactDOM from "react-dom/client";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")).render(<App />);
```

```jsx
//App.jsx
import { useState, useEffect } from 'react'
import axios from 'axios'
import Note from './components/Note'

const App = () => {
  const [notes, setNotes] = useState([])
  const [newNote, setNewNote] = useState('')
  const [showAll, setShowAll] = useState(true)

  useEffect(() => {
    console.log('effect')
    axios
      .get('http://localhost:3001/notes')
      .then(response => {
        console.log('promise fulfilled')
        setNotes(response.data)
      })
  }, [])
  console.log('render', notes.length, 'notes')

  // ...
}
```
The console output upon loading the webpage will look like this:
```bash
render 0 notes
effect
promise fulfilled
render 3 notes
```

First, the body of the function defining the component executes and the component is rendered for the first time. At this point, "render 0 notes" is printed ( see the last line). Next, because the render is finished, the effect fires, first printing "effect", then fetching the data, then printing "Promise fulfilled." Because the `.then()` method updates the state, the component re-renders, and the "notes" variable is now populated so this time "render 3 notes" is printed. 

If we re-write the use-effect call like this:
```jsx
const hook = () => {
  console.log('effect')
  axios
    .get('http://localhost:3001/notes')
    .then(response => {
      console.log('promise fulfilled')
      setNotes(response.data)
    })
}

useEffect(hook, [])
```
we can now more clearly see that the `useEffect()` hook takes two args, the callback function to fire on render, and a dependancy array. 

<div style="color: cyan; font-weight: bold; display: flex; justify-content: center; text-align: center">By default, effects run after every completed render, but you can choose to fire it only when certain values have changed.</div>

The second parameter of _useEffect_ is used to [specify how often the effect is run](https://react.dev/reference/react/useEffect#parameters). If the second parameter is an empty array _[]_, then the effect is only run along with the first render of the component.

Note that the use-effect could also be re-written like this:
```jsx
useEffect(() => {
  console.log('effect')

  const eventHandler = response => {
    console.log('promise fulfilled')
    setNotes(response.data)
  }

  const promise = axios.get('http://localhost:3001/notes')
  promise.then(eventHandler)
}, [])
```
A reference to an event handler function is assigned to the variable _eventHandler_. The promise returned by the _get_ method of Axios is stored in the variable _promise_. The registration of the callback happens by giving the _eventHandler_ variable, referring to the event-handler function, as a parameter to the _then_ method of the promise. It isn't usually necessary to assign functions and promises to variables, and a more compact way of representing things, as seen below, is sufficient.
```jsx
useEffect(() => {
  console.log('effect')
  axios
    .get('http://localhost:3001/notes')
    .then(response => {
      console.log('promise fulfilled')
      setNotes(response.data)
    })
}, [])
```

At this point we have got to the point of getting the notes data that is already saved into the front-end app, but we still need to work out how to save new notes added by users into the json-db.

## The development runtime env
The below diagram describes the makeup of the example notes app so far:
![[Pasted image 20240802120741.png]]
The JavaScript code making up our React application is run in the browser. The browser gets the JavaScript from the _React dev server_, which is the application that runs after running the command _npm run dev_. The dev-server transforms the JavaScript into a format understood by the browser. Among other things, it stitches together JavaScript from different files into one file. We'll discuss the dev-server in more detail in part 7 of the course.

The React application running in the browser fetches the JSON formatted data from _json-server_ running on port 3001 on the machine. The server we query the data from - _json-server_ - gets its data from the file _db.json_.

At this point in development, all the parts of the application happen to reside on the software developer's machine, otherwise known as localhost. The situation changes when the application is deployed to the internet. We will do this in part 3.

# Next
The next part of the FSO content can be found here: [[Webdev - FSO - Altering data on the server]]
