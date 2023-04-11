#javascript #objects #oop #datastructures #dictionaries

Objects in JS allow for complex and flexible data structures. They allow arbitrary combinations of strings, numbers, booleans, arrays, functions and objects. Objects are used as the JS implementation of dictionaries (Hash tables, see [[Data Structures - Hash Tables]]). Object constructors, discussed later in this note, are also the equivalent of class definitions, although class definitions using the class keyword were also added in ES6, see [[JavaScript - ES6 Objects and the Class Keyword]].

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

## Prototype properties
Since `numLegs` will probably have the same value for all instances of `Bird`, you essentially have a duplicated variable `numLegs` inside each `Bird` instance.

This may not be an issue when there are only two instances, but imagine if there are millions of instances. That would be a lot of duplicated variables.

A better way is to use the `prototype` of `Bird`. Properties in the `prototype` are shared among ALL instances of `Bird`. Here's how to add `numLegs` to the `Bird prototype`:
```js
Bird.prototype.numLegs = 2;
```
Now all instances of `Bird` have the `numLegs` property.
```js
console.log(duck.numLegs);
console.log(canary.numLegs);
```
Since all instances automatically have the properties on the `prototype`, think of a `prototype` as a "recipe" for creating objects. Note that the `prototype` for `duck` and `canary` is part of the `Bird` constructor as `Bird.prototype`.

Adding the prototype properties for an object individually is slow and annoying when there are lots of prototype props to add.

A more efficient way is to set the `prototype` to a new object that already contains the properties. This way, the properties are added all at once:
```js
Bird.prototype = {
  numLegs: 2, 
  eat: function() {
    console.log("nom nom nom");
  },
  describe: function() {
    console.log("My name is " + this.name);
  }
};
```

## Object constructor property
There is a special `constructor` property located on the object instances `duck` and `beagle` that were created in the previous challenges:
```js
let duck = new Bird();
let beagle = new Dog();

console.log(duck.constructor === Bird); 
console.log(beagle.constructor === Dog);
```
Both of these `console.log` calls would display `true` in the console.

Note that the `constructor` property is a reference to the constructor function that created the instance. The advantage of the `constructor` property is that it's possible to check for this property to find out what kind of object it is. Here's an example of how this could be used:
```js
function joinBirdFraternity(candidate) {
  if (candidate.constructor === Bird) {
    return true;
  } else {
    return false;
  }
}
```
**Note:** Since the `constructor` property can be overwritten (which will be covered in the next two challenges) it’s generally better to use the `instanceof` method to check the type of an object.

## Objects and prototypes
Just like people inherit genes from their parents, an object inherits its `prototype` directly from the constructor function that created it. For example, here the `Bird` constructor creates the `duck` object:
```js
function Bird(name) {
  this.name = name;
}

let duck = new Bird("Donald");
```
`duck` inherits its `prototype` from the `Bird` constructor function. You can show this relationship with the `isPrototypeOf` method:
```js
Bird.prototype.isPrototypeOf(duck);
```
This would return `true`.

All objects in JavaScript (with a few exceptions) have a `prototype`. Also, an object’s `prototype` itself is an object.
```js
function Bird(name) {
  this.name = name;
}

typeof Bird.prototype;
```
Because a `prototype` is an object, a `prototype` can have its own `prototype`! In this case, the `prototype` of `Bird.prototype` is `Object.prototype`:
```js
Object.prototype.isPrototypeOf(Bird.prototype);
```
How is this useful? You may recall the `hasOwnProperty` method from a previous challenge:
```js
let duck = new Bird("Donald");
duck.hasOwnProperty("name");
```
The `hasOwnProperty` method is defined in `Object.prototype`, which can be accessed by `Bird.prototype`, which can then be accessed by `duck`. This is an example of the `prototype` chain. In this `prototype` chain, `Bird` is the `supertype` for `duck`, while `duck` is the `subtype`. `Object` is a `supertype` for both `Bird` and `duck`. `Object` is a `supertype` for all objects in JavaScript. Therefore, any object can use the `hasOwnProperty` method.

## Object inheritance
There's a principle in programming called Don't Repeat Yourself (DRY). The reason repeated code is a problem is because any change requires fixing code in multiple places. This usually means more work for programmers and more room for errors.

Notice in the example below that the `describe` method is shared by `Bird` and `Dog`:
```js
Bird.prototype = {
  constructor: Bird,
  describe: function() {
    console.log("My name is " + this.name);
  }
};

Dog.prototype = {
  constructor: Dog,
  describe: function() {
    console.log("My name is " + this.name);
  }
};
```
The `describe` method is repeated in two places. The code can be edited to follow the DRY principle by creating a `supertype` (or parent) called `Animal`:
```js
function Animal() { };

Animal.prototype = {
  constructor: Animal, 
  describe: function() {
    console.log("My name is " + this.name);
  }
};
```
Since `Animal` includes the `describe` method, you can remove it from `Bird` and `Dog`:
```js
Bird.prototype = {
  constructor: Bird
};

Dog.prototype = {
  constructor: Dog
};
```
 To actually give the animal objects describe method to instances of Dog or bird you need to let them inherit the method from the animal prototype.
```js
Bird.prototype = Object.create(Animal.prototype);
```
Remember that the `prototype` is like the "recipe" for creating an object. In a way, the recipe for `Bird` now includes all the key "ingredients" from `Animal`.
```js
let duck = new Bird("Donald");
duck.eat();
```
`duck` inherits all of `Animal`'s properties, including the `eat` method.

## Override inherited methods
In previous lessons, you learned that an object can inherit its behavior (methods) from another object by referencing its `prototype` object:
```js
ChildObject.prototype = Object.create(ParentObject.prototype);
```
Then the `ChildObject` received its own methods by chaining them onto its `prototype`:
```js
ChildObject.prototype.methodName = function() {...};
```
It's possible to override an inherited method. It's done the same way - by adding a method to `ChildObject.prototype` using the same method name as the one to override. Here's an example of `Bird` overriding the `eat()` method inherited from `Animal`:
```js
function Animal() { }
Animal.prototype.eat = function() {
  return "nom nom nom";
};
function Bird() { }

Bird.prototype = Object.create(Animal.prototype);

Bird.prototype.eat = function() {
  return "peck peck peck";
};
```
If you have an instance `let duck = new Bird();` and you call `duck.eat()`, this is how JavaScript looks for the method on the `prototype` chain of `duck`:
1.  `duck` => Is `eat()` defined here? No.
2.  `Bird` => Is `eat()` defined here? => Yes. Execute it and stop searching.
3.  `Animal` => `eat()` is also defined, but JavaScript stopped searching before reaching this level.
4.  Object => JavaScript stopped searching before reaching this level.

## Mixins
As you have seen, behavior is shared through inheritance. However, there are cases when inheritance is not the best solution. Inheritance does not work well for unrelated objects like `Bird` and `Airplane`. They can both fly, but a `Bird` is not a type of `Airplane` and vice versa.

For unrelated objects, it's better to use mixins. A mixin allows other objects to use a collection of functions.
```js
let flyMixin = function(obj) {
  obj.fly = function() {
    console.log("Flying, wooosh!");
  }
};
```
The `flyMixin` takes any object and gives it the `fly` method.

## Closure
In the previous challenge, `bird` had a public property `name`. It is considered public because it can be accessed and changed outside of `bird`'s definition.
```js
bird.name = "Duffy";
```
Therefore, any part of your code can easily change the name of `bird` to any value. Think about things like passwords and bank accounts being easily changeable by any part of your codebase. That could cause a lot of issues.

The simplest way to make this public property private is by creating a variable within the constructor function. This changes the scope of that variable to be within the constructor function versus available globally. This way, the variable can only be accessed and changed by methods also within the constructor function.
```js
function Bird() {
  let hatchedEgg = 10;

  this.getHatchedEggCount = function() { 
    return hatchedEgg;
  };
}
let ducky = new Bird();
ducky.getHatchedEggCount();
```
Here `getHatchedEggCount` is a privileged method, because it has access to the private variable `hatchedEgg`. This is possible because `hatchedEgg` is declared in the same context as `getHatchedEggCount`. In JavaScript, a function always has access to the context in which it was created. This is called `closure`.