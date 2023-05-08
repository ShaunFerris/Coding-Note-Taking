#webdevSpecific #HTML #CSS #frontend

This note serves as the hub for front-end web design related topics, and as the jumping off point for learning about HTML, CSS and eventually front-end frameworks like react.js.

Related notes:
	[[Web Front-End HTML and CSS Basics]]
	[[Web Front-End - Bootstrap - Basics]]
	[[Web Front-End - Font Awesome Icons]]
	[[Web Front-End - jQuery]]
	[[Web Front-End - SASS]]
You can also target elements based on their positions using `:odd` or `:even` selectors.

Note that jQuery is zero-indexed which means the first element in a selection has a position of 0. This can be a little confusing as, counter-intuitively, `:odd` selects the second element (position 1), fourth element (position 3), and so on.

Here's how you would target all the odd elements with class `target` and give them classes:

```js
$(".target:odd").addClass("animated shake");
```
## How websites are loaded
The term **front-end** refers simply to the parts of an applications code that encode the appearance layer of a web site or application. To get started learning about how to code this appearance layer we should first understand a little about how a website is loaded and displayed by a browser, the software for which websit code is written.

When you type an address into your browser and hit enter, the webiste is then loaded in your browser, but what is actually happening behind the scenes boils down to a series of client-server communications, the client being your browser, and the server being some machine, somewhere in the world, that is hosting the files which make up the website you want to see.

What happens first, is that the client makes a request for the `index.html` file of the target website, and the server sends it. The client browser then reads in the file, and creates something called the DOM, or Document Object Model.

### The DOM
The Document Object Model (DOM) is a data representation of all of the building blocks of the page and their structure. So paragraphs, images, divs and headings are all represented in the DOM. It is essentially a programming interface for web documents, allowing them to be interacted with via code.