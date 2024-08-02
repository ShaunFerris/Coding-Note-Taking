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
