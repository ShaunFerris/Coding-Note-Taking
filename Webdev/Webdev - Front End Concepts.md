#webdevSpecific #frontend 

This note covers concepts that are important background for front-end webdev topics, things that are generally good to know before diving into HTML, CSS and front-end frameworks like React.

## How websites are loaded
The term **front-end** refers simply to the parts of an applications code that encode the appearance layer of a web site or application. To get started learning about how to code this appearance layer we should first understand a little about how a website is loaded and displayed by a browser, the software for which websit code is written.

When you type an address into your browser and hit enter, the webiste is then loaded in your browser, but what is actually happening behind the scenes boils down to a series of client-server communications, the client being your browser, and the server being some machine, somewhere in the world, that is hosting the files which make up the website you want to see.

What happens first, is that the client makes a request for the `index.html` file of the target website, and the server sends it. The client browser then reads in the file, and creates something called the DOM, or Document Object Model.

### The DOM
The Document Object Model (DOM) is a data representation of all of the building blocks of the page and their structure. So paragraphs, images, divs and headings are all represented in the DOM. It is essentially a programming interface for web documents, allowing them to be interacted with via code.