#webdevSpecific 

Java script is a single threaded programming language, and a lot of actions performed on the web with it are blocking or time consuming. This makes asynchronous programming essential for working on the web. 

To understand async programming we first need to understand the javascript runtime event loop, callbacks and promises. Before ES6 added promises, callbacks were the only real way to handle asynchronous events, then later ES7 made promises more elegant with async await syntax.

Previous notes on callbacks are here: [[Javascript - Callback functions]]
Notes on promises in js are here: [[JavaScript - Promises]]
and notes on the actual use of async await are here: [[Javascript - Async Await]]. This note will primarily focus on the theory of the event loop in a webbrowser or node.js runtime.

## JS runtimes and the event loop
A very in depth explanation of the event loop can be found here: ![](https://www.youtube.com/watch?v=cCOL7MC4Pl0&t=0s)

But to give a simple explanation, both the browser and node.js runtimes run your code with a single threaded event loop. On the first go around it will run all of your synchronous code, and it will also queue up async code on a seperate thread pool. Essentially you can have a piece of code that says "I need to do this, but first I need this data from the network", and because you don't know how long it will take to get that data, you make it async. Then the loop can continue, executing all your synchronous code, and checking in on the status of the async function on it's next loop around.