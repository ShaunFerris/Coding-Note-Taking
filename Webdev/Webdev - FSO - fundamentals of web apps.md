#webdevSpecific 

This section is supposed to prepare students for the fso course by examining an example webapp hosted [here](https://studies.cs.helsinki.fi/exampleapp/). The app is supposed to demonstrate basic concepts, and should NOT be used an example of how to build a modern app, it uses bad practices in it's implementation.

## Rule one of web dev
The first rule of web development is to always keep the browser console open when developing. Open the console in chrome or firefox by hitting f12 (or fn + f12 depending on your system setup). 

A good place to start when examining an app through the devtools is the network tab. Make sure to check the "disable cache" option so that you see all the requests every time, then reload the page and watch the network requests go in and out.

## HTTP GET
The server and client communicate over the HTTP protocol. The network tab of the devtools display these communications.

When you reload the page on the example webapp, you will see two events:
1. the browser has fetched the page contents
2. and it has downloaded the image "kuva.png"

Note that when I did it, there was also a third entry in the network tab, a 404 on a GET request for the favicon.

Clicking on the first GET for the page content allows you to inspect the request headers:
![[Pasted image 20240715133638.png]]Note, that screenshot is from chrome, but I use firefox, which looks like this:
![[Pasted image 20240715133817.png]]
In the general part of the headers you can see the URL and resource that the request was sent to, and the 200 response code indicating that it was successful. If you move over from the headers tab to the response tab, you can see the server rendered HTML that was sent as the response payload.

In the response headers part, we can see more info, like the size in bytes of the response and the exact time of the response. 

An important header is the content-type header, which tells us that the reponse is a text file in utf-8 format, and the content is html. This lets the browser know what the response is and how it should render it, in this case, as a normal webpage. 

Going back to the response tab, we can see the server rendered html, or we can click the "raw" tab to see the html itself unrendered. 
![[Pasted image 20240715135138.png]]
Note: the raw/rendered thing might be firefox only, it is not in the course provided screenshot which is from chrome. 

This diagram shows the chain of events that is triggered by opening the app:
![[Pasted image 20240715135241.png]]

## Traditional web applications
The homepage of example app operates in what you could call the **traditional** way. Visiting the page triggers a GET request to the server for the HTML document which details the structure and content of the page. This document is either made by the server, or statically saved on the servers file system. This is essentially dynamic vs static. Dynamic generation of the served HTML document could be necessary if you for example need values from a database to determine what to render. The example app shows a number of "notes" created, and this come from the database, so in this case the HTML is generated dynamically. 

The example app generates the served HTML like this:
```javascript
const getFrontPageHtml = noteCount => {
  return `
    <!DOCTYPE html>
    <html>
      <head>
      </head>
      <body>
        <div class='container'>
          <h1>Full stack example app</h1>
          <p>number of notes created ${noteCount}</p>
          <a href='/notes'>notes</a>
          <img src='kuva.png' width='200' />
        </div>
      </body>
    </html>
`
}

app.get('/', (req, res) => {
  const page = getFrontPageHtml(notes.length)
  res.send(page)
})
```
The html for the page is saved as a template string, with noteCount as a variable, which is returned from a function. 

Writing HTML amid the javascript code like this is not generally a good idea (unless using something like jsx) but it was common practice among old school php devs. 

<div style="font-weight: bold; color: cyan;">In traditional web apps, the browser is "dumb" and only renders HTML sent by the server. A server can be created with Python flask, ruby on rails or java spring (among many others). The example app uses Express and Node. </div>

## Running application logic in the browser
When you go the /notes route on the example app, and refresh with the network tab open, you see four GET requests going out from the browser:
![[Pasted image 20240716112248.png]]You can see in the screenshot that each GET request is retrieving a different document, and they all have different types. The first request is of type "document" and is the html for the page:
![[Pasted image 20240716112443.png]]
This time the html does not contain the list of notes. Instead the head section includes a script, which does the GET request for the notes list in the last two lines. Note that scripts included in html this way are executed immediately after the browser fetches the script. Here is the js code, examined from the browser devtools:
![[Pasted image 20240716112707.png]]
You can see that the last two lines use the XMLHttpRequest() method to send a GET request to the data.json resource on the server. If you were to navigate directly to this address in the browser you would see the raw json data. Alternatively, you could use curl to fetch this resource in the terminal, and if you were feeling spicy could pipe the output into jq or bat to make it all pretty.

Looking at the `.onreadystatechange` methods callback function above, you can see that if the response code for the GET request is a 200 (200 means success), then the script will parse the JSON response and create an unordered list from the returned JSON data, then get the notes element from the base HTML and append the list to it. This is how the list of notes gets rendered from the json data after the page loads.

## Event handlers and callbacks
Looking again at the above code, the structure is a little strange, with the request being sent to the server in the last lines, but the handling of the response being described above that line. This is because the `onreadystatechange` is an <span style='font-weight: bold; color: cyan'>event handler.</span> It is defined on the object `xhttp`, so whenever this object changes ready state, the code in the handlers callback function will run. Note that ready state 4 indicates that the operation is complete. The callback function is called that because the application code does not execute the function itself, but rather the browser does at an approprite time when the event to which it is a callback has happened.

## Document object model (DOM)
We can think of HTML pages as trees:
```html
html
  head
    link
    script
  body
    div
      h1
      div
        ul
          li
          li
          li
      form
        input
        input
```
Browsers are based on the idea of depicting html elements as a tree. The DOM is an API that enables programmatic modification of the <span style="font-weight: bold; color: cyan">element trees</span> corresponding to a web page.

The call to `document.createElement()` in the above js script is an example of using the DOM api to add content to the html tree.

## Manipulating the document object from the console
The topmost node of the DOM tree of an HTML document is called the `document`. You can access it by typing `document` into the console.
![[Pasted image 20240716114740.png]]
You can run js code in real time in the console, so we can add a new item to the list by running the following:
```javascript
list = document.getElementsByTagName('ul')[0]
newElement = document.createElement('li')
newElement.textContent = 'Page manipulation from console is easy'
list.appendChild(newElement)
```
This obviously only updates the page for the current session, the changes will disappear when you refresh because nothing on the server has changed.

## CSS
