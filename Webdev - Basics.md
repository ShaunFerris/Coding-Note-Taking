#webdevSpecific #HTML #CSS #frontend #frontend-libs #webdevSpecific #react 

This note serves as the hub for front-end web design related topics, and as the jumping off point for learning about HTML, CSS and eventually front-end frameworks like react.js.

Related notes:
	[[Webdev HTML and CSS Basics]]
	[[Webdev - Bootstrap - Basics]]
	[[Webdev - Font Awesome Icons]]
	[[Webdev - jQuery]]
	[[Webdev - SASS]]
	[[Webdev - React - Basics]]
	[[Webdev - Vite]]

## How websites are loaded
The term **front-end** refers simply to the parts of an applications code that encode the appearance layer of a web site or application. To get started learning about how to code this appearance layer we should first understand a little about how a website is loaded and displayed by a browser, the software for which websit code is written.

When you type an address into your browser and hit enter, the webiste is then loaded in your browser, but what is actually happening behind the scenes boils down to a series of client-server communications, the client being your browser, and the server being some machine, somewhere in the world, that is hosting the files which make up the website you want to see.

What happens first, is that the client makes a request for the `index.html` file of the target website, and the server sends it. The client browser then reads in the file, and creates something called the DOM, or Document Object Model.

### The DOM
The Document Object Model (DOM) is a data representation of all of the building blocks of the page and their structure. So paragraphs, images, divs and headings are all represented in the DOM. It is essentially a programming interface for web documents, allowing them to be interacted with via code.