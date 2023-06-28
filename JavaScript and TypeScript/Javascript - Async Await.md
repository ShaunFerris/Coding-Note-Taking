#javascript #async

Here we will go over the mechanics of using async await sytnax in js. Async await is essentially just syntactic sugart that makes your asynchronous code read the same way as synchronous code. It allows you to avoid the long chains of `.then()` methods with callbacks that you see when writing promises.

## Async keyword to return a promise
If we define a function that does a thing, but we use the async keyword in that functions definition, then instead of returning the thing it will now return a promise to do the thing:
```js
const getFruit = async (name) => {
	const fruits = {
		banana: '🍌',
		watermelon: '🍉',
		grapes: '🍇'
	}
	return fruits[name];
}

getFruit('banana').then(console.log)
```
The `getFruit()` function will return a promise that will resolve to the value of the emoji that corresponds to the name of a fruit passed into the function. To make this a little more clear see the below for a second version of the function which achieves the exact same thing without async sytax, by returning a promise specifically.
```js
const getFruit = (name) => {
	const fruits = {
		banana: '🍌',
		watermelon: '🍉',
		grapes: '🍇'
	}
	return Promise.resolve(fruits[name]);
}

getFruit('banana').then(console.log)
```

So the async keyword takes the functions return value and has it resolve as a promise, but it also sets up a context for you to use the await keyword.

## Await to pause while we wait for a promise
The real power of an async functions comes when it is combined with the await keyword to pause the execution of a function. This allows you to do something inside the function, after the promise as resolved, rather than calling the `.then()` method on the returned value outside the function.
```js
const makeSmoothie = async () => {
	const a = await getFruit('banana');
	const b = await getFruit('watermelon');
	return [a, b];
}

makeSmoothie().then(console.log);
```
In the above example we are saying "Pause the execution of the makeSmoothie function until the promise returned by get fruit has resolved, then continue executing the makeSmootie function."

One of the most annoying things about promises is that it is quite difficult to share resolved values between multiple steps in the promise chain. Async await syntax solves this problem nicely, as you can see by comparing the above function to how it would be written just using promise and not async syntax:
```js
const makeSmoothiePromise = () => {
	let a;
	return getFruit('banana')
		.then(v => {
			a = v;
			return getFruit('watermelon')
		})
		.then(v => [v, a])
}
```
You can see that this takes a lot more code to achieve, and is much harder to read. 

## Concurrency
Now if we go back to the async version of the `makeSmoothie()` function, we can see that it is actually making a big mistake, and a mistake that is very common for people new to async await syntax. The mistake is NOT using concurrency. It is waiting for the first promise to resolve, then only after it has it is waiting for the next promise to resolve. This IS sometimes neccessary, like when you need to await a fetch to a server, then await mapping that response to json, but in the example function there is no reason we cannot await both promises at the same time:
```js
const makeSmoothie = async () => {
	const a = getFruit('banana');
	const b = getFruit('watermelon');
	const smoothie = await Promise.all([a, b]);
	return smoothie;
}
```

## Error catching
The other nice thing about async syntax is handling errors. Instead of chaining a `.catch()` call to the promise chain, you can simply write async await code inside a try/catch block. This offers much greater flexibility when handling errors that might occur accross multiple promises.