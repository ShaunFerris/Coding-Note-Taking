#javascript #ES6 #webdevSpecific 

Promises in JS are exactly what they sound like, they are code blocks that promise to do something, usually in an uncertain amount of time, and the promise tracks the state of it's objective as either **pending**, **fulfilled** or **rejected**.

Promises are most useful in regards to asynchronous processes or web requests. Indicating what to do when the task is finished.

`Promise` is a constructor function so we use the new keyword to instantiate one.  As its argument, `Promise` takes an anonymous function, and that function takes as its arguments the `resolve` and `reject` methods.
```js
const myPromise = new Promise((resolve, reject) => {
  if(condition here) {
    resolve("Promise was fulfilled");
  } else {
    reject("Promise was rejected");
  }
});
```

Promises are most useful when you have a process that takes an unknown amount of time in your code (i.e. something asynchronous), often a server request. When you make a server request it takes some amount of time, and after it completes you usually want to do something with the response from the server. This can be achieved by using the `then` method. The `then` method is executed immediately after your promise is fulfilled with `resolve`. Here’s an example:
```js
myPromise.then(result => {
  
});
```
`result` comes from the argument given to the `resolve` method.

`catch` is the method used when your promise has been rejected. It is executed immediately after a promise's `reject` method is called. Here’s the syntax:

```js
myPromise.catch(error => {
  
});
```

`error` is the argument passed in to the `reject` method.