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
We have already established that when a page is loaded, the browser fetches the HTML with a GET request to the server. Then when the HTML is loaded, the script tag causes another GET for the js script. Similarly, the head element also contains a link tag that tells the browser to fetch a CSS stylesheet from the address `main.css`. CSS is the style sheet language used to determine the appearance of webpages.

The fetched CSS for the example page is this:
```css
.container {
  padding: 10px;
  border: 1px solid;
}

.notes {
  color: blue;
}
```
It defines two class selectors that define styles for any element with `class="container"` or `class="notes"` in their attributes.\

Examining the HTML in the console, you will see that the pages outermost div element has the container class, and it should have 10px padding and a 1px solid border. The div element also has and id attribute, and the js code uses this id attribute to target it. You can edit the css in console just like you could run js code or edit the html tree.

## Review: loading a page that contains JS
Here is a diagram recapping the sequence of events:
![[Pasted image 20240716121307.png]]

## Froms and HTTP POST
Now, we will examine how the operation of adding a new note is handled by the example site. The notes page contains a form element:
![[Pasted image 20240716121426.png]]![[Pasted image 20240716121505.png]]
When the button is clicked, the form is submitted and a POST request is sent. In this case the page is also reloaded, so the original 4 GET reqeusts also reoccur. Lets look closer at the POST request:
![[Pasted image 20240716121832.png]]

You can see under the POST section that the request was sent to the `/new-note` endpoint, and under the response headers section that the request originated from the `/notes` page. You can also see that the response status code was 302, which is a URL redirect which asks the browser to perform a new GET request to the address defined in the headers location, this is why the page reloads and the HTML, CSS, JS and other resources are all refetched.

Looking back at the html, you can see that the form attribute has action and method attributes that define that a POST request should be sent, and to which server address.

On the server, at the `exampleapp/new_note` endpoint, this code handles the POST request:
```javascript
app.post('/new_note', (req, res) => {
  notes.push({
    content: req.body.note,
    date: new Date(),
  })

  return res.redirect('/notes')
})
```
It takes the note data from the body field of the request object and pushes it onto the notes array, then returns a redirect response. Note that the server does not save new notes to a database or append them to the json data file, so they will disappear on a refresh when json notes data is reloaded.

## AJAX
The notes page of the example app follows an early 90s dev style using AJAX, an acronym for "asynchronous javascript and xml". The term was coined in 2005 after browser tech had advanced to the point that fetching content for webpages could be done with js instead of in the html. The home page of the example app uses the older style where the html does the data fetching. The notes page uses the traditional style for the form POST request too, and only uses ajax for the notes data GET request.

The application URLs reflect the old, carefree times. JSON data is fetched from the URL [https://studies.cs.helsinki.fi/exampleapp/data.json](https://studies.cs.helsinki.fi/exampleapp/data.json) and new notes are sent to the URL [https://studies.cs.helsinki.fi/exampleapp/new_note](https://studies.cs.helsinki.fi/exampleapp/new_note). Nowadays URLs like these would not be considered acceptable, as they don't follow the generally acknowledged conventions of [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer#Applied_to_web_services) APIs.

The thing termed AJAX is now so commonplace that it's taken for granted. The term has faded into oblivion, and the new generation has not even heard of it.

## Single page apps
The example app **homepage** works like a very traditional webpage, with all the logic on the server, and the browser only rendering HTML as instructed. The notes page gives the task of generating HTML for the existing notes data to the browser, which it does by executing js code that is sent to it by the server.

In recent years, the [Single-page application](https://en.wikipedia.org/wiki/Single-page_application) (SPA) style of creating web applications has emerged. SPA-style websites don't fetch all of their pages separately from the server like our sample application does, but instead comprise only one HTML page fetched from the server, the contents of which are manipulated with JavaScript that executes in the browser.

The Notes page of our application bears some resemblance to SPA-style apps, but it's not quite there yet. Even though the logic for rendering the notes is run on the browser, the page still uses the traditional way of adding new notes. The data is sent to the server via the form's submit, and the server instructs the browser to reload the Notes page with a _redirect_.

A single-page app version of our example application can be found at [https://studies.cs.helsinki.fi/exampleapp/spa](https://studies.cs.helsinki.fi/exampleapp/spa). At first glance, the application looks exactly the same as the previous one. The HTML code is almost identical, but the JavaScript file is different (_spa.js_) and there is a small change in how the form-tag is defined:![[Pasted image 20240716123813.png]]
The form no longer has method or action attributes, as the handling of the form is now done in js. Now, when the form is submitted, the page does not reload, so only the post request is sent and the GETs do not need to be redone. Lets look at the POST:
![[Pasted image 20240716124138.png]]You can see that the `content-type` header on the request tells the server that the payload will be json, so the server knows how to parse it, and in the POST section you can see that the server has responded with 201 created, to confirm that the entry was created. 

The spa does not use the form attributes to send the data, it instead uses js code fetched from the server:
```javascript
var form = document.getElementById('notes_form')
form.onsubmit = function(e) {
  e.preventDefault()

  var note = {
    content: e.target.elements[0].value,
    date: new Date(),
  }

  notes.push(note)
  e.target.elements[0].value = ''
  redrawNotes()
  sendToServer(note)
}
```
It gets the form element by it's id using the DOM api and then attaches an event handler for the submit event. This event handler overrides the default behaviour of a form submission, which would send the data to the server and cause a new GET request. It then creates a new json object for the note and pushes it to the notes array, and calls `redrawNotes()` to re-render the notes list on the page. Finally it sends the note to the server. The code for sending the note to the server looks like this:
```javascript
var sendToServer = function(note) {
  var xhttpForPost = new XMLHttpRequest()
  // ...

  xhttpForPost.open('POST', '/new_note_spa', true)
  xhttpForPost.setRequestHeader('Content-type', 'application/json')
  xhttpForPost.send(JSON.stringify(note))
}
```
The code determines that the data is to be sent with an HTTP POST request and the data type is to be JSON. The data type is determined with a _Content-type_ header. Then the data is sent as JSON string.

The application code is available at [https://github.com/mluukkai/example_app](https://github.com/mluukkai/example_app). It's worth remembering that the application is only meant to demonstrate the concepts of the course. The code follows a poor style of development in some measures, and should not be used as an example when creating your applications. The same is true for the URLs used. The URL _new_note_spa_ that new notes are sent to, does not adhere to current best practices.

## JavaScript-libraries
The sample app is done with so-called [vanilla JavaScript](https://www.freecodecamp.org/news/is-vanilla-javascript-worth-learning-absolutely-c2c67140ac34/), using only the DOM-API and JavaScript to manipulate the structure of the pages.

Instead of using JavaScript and the DOM-API only, different libraries containing tools that are easier to work with compared to the DOM-API are often used to manipulate pages. One of these libraries is the ever-so-popular [jQuery](https://jquery.com/).

jQuery was developed back when web applications mainly followed the traditional style of the server generating HTML pages, the functionality of which was enhanced on the browser side using JavaScript written with jQuery. One of the reasons for the success of jQuery was its so-called cross-browser compatibility. The library worked regardless of the browser or the company that made it, so there was no need for browser-specific solutions. Nowadays using jQuery is not as justified given the advancement of JavaScript, and the most popular browsers generally support basic functionalities well.

The rise of the single-page app brought several more "modern" ways of web development than jQuery. The favorite of the first wave of developers was [BackboneJS](http://backbonejs.org/). After its [launch](https://github.com/angular/angular.js/blob/master/CHANGELOG.md#100rc1-moir%C3%A9-vision-2012-03-13) in 2012, Google's [AngularJS](https://angularjs.org/) quickly became almost the de facto standard of modern web development.

However, the popularity of Angular plummeted in October 2014 after the [Angular team announced that support for version 1 will end](https://web.archive.org/web/20151208002550/https://jaxenter.com/angular-2-0-announcement-backfires-112127.html), and that Angular 2 will not be backwards compatible with the first version. Angular 2 and the newer versions have not received too warm of a welcome.

Currently, the most popular tool for implementing the browser-side logic of web applications is Facebook's [React](https://react.dev/) library. During this course, we will get familiar with React and the [Redux](https://github.com/reactjs/redux) library, which are frequently used together.

The status of React seems strong, but the world of JavaScript is ever-changing. For example, recently a newcomer - [VueJS](https://vuejs.org/) - has been capturing some interest.

## Full-stack web development
What does the name of the course, _Full stack web development_, mean? Full stack is a buzzword that everyone talks about, but no one knows what it means. Or at least, there is no agreed-upon definition for the term.

Practically all web applications have (at least) two "layers": the browser, being closer to the end-user, is the top layer, and the server the bottom one. There is often also a database layer below the server. We can therefore think of the _architecture_ of a web application as a _stack_ of layers.

Often, we also talk about the [frontend and the backend](https://en.wikipedia.org/wiki/Front_and_back_ends). The browser is the frontend, and the JavaScript that runs on the browser is the frontend code. The server on the other hand is the backend.

In the context of this course, full-stack web development means that we focus on all parts of the application: the frontend, the backend, and the database. Sometimes the software on the server and its operating system are seen as parts of the stack, but we won't go into those.

We will code the backend with JavaScript, using the [Node.js](https://nodejs.org/en/) runtime environment. Using the same programming language on multiple layers of the stack gives full-stack web development a whole new dimension. However, it's not a requirement of full-stack web development to use the same programming language (JavaScript) for all layers of the stack.

It used to be more common for developers to specialize in one layer of the stack, for example, the backend. Technologies on the backend and the frontend were quite different. With the Full stack trend, it has become common for developers to be proficient in all layers of the application and the database. Oftentimes, full-stack developers must also have enough configuration and administration skills to operate their applications, for example, in the cloud.

Thats the end of the fundamentals section, the next section is: [[Webdev - FSO - Intro to React]].

