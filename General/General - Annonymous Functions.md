#functions #arrowfunctions #lambdafunctions #ES6 #general

Added in ES6, arrow functions are the syntax used for short, annonymous function definintions, typically intended for one use, like lambda functions in python.

Prior to ES6, anonyomous functions in JS were written like this:
```js
const myFunction = function(arg) {
	//do this;
	return;
}
```
With arrow functionality the same function can be written like this:
```js
const myFunction = (arg) => {
	//do this;
	return;
}
```
For reference, the equivalent to above written as a python lambda funtion would be:
```python
my_function = lambda arg: #do this
```

When there is no function body (`//do this` in the above example) and only a return value, JS allows you to omit the return keyword and curly braces, for example:
```js
const myFunction = (x) => x + 5
```

[[JavaScript - The Rest Function and Handling Multiple Function Args]] The rest function can also be applied to anonymous functions in js.

