#javascript #ES6 #objects 

Object destructuring is used when you want to assign the values of an objects properties to named variables outside that object. The basic way to do this is just have a series of variable assignments like this:
```js
//Non-destructuring approach
const user {
	name: "Jim"
	age: 1000
};

const userName = user.name;
const userAge = user.age;
```
But with destructuring, we can insted write this:
```js
const {name: userName, age: userAge} = user;
```
If you instead write it like this:
```js
const {name, age} = user;
```
Then the values for name and age from the user object will be assigned to the variables name and age outside the object, where the first example assigns those same values to userName and userAge.

## Nested object destructuring
Nested objects can have their property values destructured in a similar fashion:
```js
const users {
	johnDoe {
		age: 34
		email: 'test@gmail.com'	
	};
};

const {johnDoe: {age: userAge, email: userEmail}} = users;
```

## Array destructuring
Similar to the spread operator, this is a method of unpacking values from an array and is useful for assigning them to variables.
While the spread operator unpacks every member of the array into a comma separated list, this means you can't pick and choose a subset of the arrays values to assign to variables. With array destructuring you can do just that:
```js
const [a, b] = [1, 2, 3, 4, 5, 7];
console.log(a, b);
//The console would display the values 1 and 2.
const [a, b, , , c] = [1, 2, 3, 4, 5, 7];
console.log(a, b, c);
//The console would display the values 1, 2 and 5
```

### Array destructuring via rest elements
[[JavaScript - The Rest Function and Handling Multiple Function Args]]
In some situations involving array destructuring, we might want to collect the rest of the elements into a separate array.

The result is similar to `Array.prototype.slice()`, as shown below:
```js
const [a, b, ...arr] = [1, 2, 3, 4, 5, 7];
console.log(a, b);
console.log(arr);
```
