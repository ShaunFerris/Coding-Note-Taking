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
