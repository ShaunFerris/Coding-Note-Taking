#javascript #objects #oop #datastructures #dictionaries

Objects in JS allow for complex and flexible data structures. They allow arbitrary combinations of strings, numbers, booleans, arrays, functions and objects.

Objects are decalred as constants and contain properties. Properties is the name used for the variable data stored in an object. The data stored as properties in a JS object is assigned by a colon instead of an assignment operator, and functionally JS objects are very similar to python dictionaries. The below is an example of an object constructor statement:
```js
const cat {
	name : "Whiskers",
	legs : 4,
	enemies : ["water", "dogs"]
};
```
Dot notations is used to access properties in objects just like accessing a class instance property in python, eg `cat.name`. Bracket notation can also be used, eg `cat[name]`. If an there is a space in a property name for an object then that property must be accessed with bracket notation, dot notation will not work. Bracket notation also must be used if you want to access an object property through a variable.

The `.hasOwnProperty(propName)` method is built in to JS objects and returns True if the arg is a property name in the object for which the method is called. 

## Object instancing
The ability to create new instances of an object was added in ES6 with the new keyword.
```js
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}

const car1 = new Car('Eagle', 'Talon TSi', 1993);

console.log(car1.make);
// Expected output: "Eagle"
```

Other advanced objects topics added in ES6 include [[JavaScript - Object Destructuring]] and [[JavaScript - ES6 Objects and the Class Keyword]].

## Declaring functions inside objects
When defining functions within objects in ES5, we have to use the keyword `function` as follows:

```js
const person = {
  name: "Taylor",
  sayHello: function() {
    return `Hello! My name is ${this.name}.`;
  }
};
```

With ES6, you can remove the `function` keyword and colon altogether when defining functions in objects. Here's an example of this syntax:

```js
const person = {
  name: "Taylor",
  sayHello() {
    return `Hello! My name is ${this.name}.`;
  }
};
```

## Object methods in JS
Objects in JS can have methods like classes in python. Even objects created without the class keyword in JS can have methods.

Methods are properties that are functions. This adds different behavior to an object. Here is the `duck` example with a method:
```js
let duck = {
  name: "Aflac",
  numLegs: 2,
  sayName: function() {return "The name of this duck is " + duck.name + ".";}
};
duck.sayName();
```
This method could also be created with an anonymous function [[General - Annonymous Functions]]:
```js
let duck = {
  name: "Aflac",
  numLegs: 2,
  sayName: () => "The name of this duck is " + duck.name + "."
};
duck.sayName();
```

## Objects and the `this` keyword
When you write object methods like in the above example, you often refernce a property of the object. This can be done directly as above, but would mean that the definitions would all need to be changed if the objects name ever changes. This can be avoided with the `this` keyword, exactly like the `self` keyword in python.

`this` is a deep topic, and the above example is only one way to use it. In the current context, `this` refers to the object that the method is associated with: `duck`. If the object's name is changed to `mallard`, it is not necessary to find all the references to `duck` in the code. It makes the code reusable and easier to read.

## Constructor functions
These are mostly irrelvant now that the class keyword has been introduced. See [[JavaScript - ES6 Objects and the Class Keyword]].

Constructors are functions that create new objects. They define properties and behaviors that will belong to the new object. Think of them as a blueprint for the creation of new objects.

Here is an example of a constructor:
```js
function Bird() {
  this.name = "Albert";
  this.color = "blue";
  this.numLegs = 2;
}
```
This constructor defines a `Bird` object with properties `name`, `color`, and `numLegs` set to Albert, blue, and 2, respectively. Constructors follow a few conventions:

-   Constructors are defined with a capitalized name to distinguish them from other functions that are not `constructors`.
-   Constructors use the keyword `this` to set properties of the object they will create. Inside the constructor, `this` refers to the new object it will create.
-   Constructors define properties and behaviors instead of returning a value as other functions might.

A new instance of an object is made with the `this` keyword.
```js
function Bird() {
  this.name = "Albert";
  this.color  = "blue";
  this.numLegs = 2;
}

let blueBird = new Bird();
```

## Extending constructors
Suppose you were writing a program to keep track of hundreds or even thousands of different birds in an aviary. It would take a lot of time to create all the birds, then change the properties to different values for every one. To more easily create different `Bird` objects, you can design your Bird constructor to accept parameters:
```js
function Bird(name, color) {
  this.name = name;
  this.color = color;
  this.numLegs = 2;
}
```

Then pass in the values as arguments to define each unique bird into the `Bird` constructor: `let cardinal = new Bird("Bruce", "red");` This gives a new instance of `Bird` with `name` and `color` properties set to `Bruce` and `red`, respectively. The `numLegs` property is still set to 2. The `cardinal` has these properties:
```js
cardinal.name
cardinal.color
cardinal.numLegs
```

The constructor is more flexible. It's now possible to define the properties for each `Bird` at the time it is created, which is one way that JavaScript constructors are so useful. They group objects together based on shared characteristics and behavior and define a blueprint that automates their creation.

Anytime a constructor function creates a new object, that object is said to be an instance of its constructor. JavaScript gives a convenient way to verify this with the `instanceof` operator. `instanceof` allows you to compare an object to a constructor, returning `true` or `false` based on whether or not that object was created with the constructor. Here's an example:
```js
let Bird = function(name, color) {
  this.name = name;
  this.color = color;
  this.numLegs = 2;
}

let crow = new Bird("Alexis", "black");

crow instanceof Bird;
```
This `instanceof` method would return `true`.

## Own properties
In the following example, the `Bird` constructor defines two properties: `name` and `numLegs`:
```js
function Bird(name) {
  this.name = name;
  this.numLegs = 2;
}

let duck = new Bird("Donald");
let canary = new Bird("Tweety");
```
`name` and `numLegs` are called own properties, because they are defined directly on the instance object. That means that `duck` and `canary` each has its own separate copy of these properties. In fact every instance of `Bird` will have its own copy of these properties. The following code adds all of the own properties of `duck` to the array `ownProps`:
```js
let ownProps = [];

for (let property in duck) {
  if(duck.hasOwnProperty(property)) {
    ownProps.push(property);
  }
}

console.log(ownProps);
```
The console would display the value `["name", "numLegs"]`.

