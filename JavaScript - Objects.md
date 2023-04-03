#javascript #objects #oop #datastructures #dictionaries

Objects in JS allow for complex and flexible data structures. They allow arbitrary combinations of strings, numbers, booleans, arrays, functions and objects.

Objects are decalred as constants and contain properties. Properties is the name used for the variable data stored in an object. The data stored as properties in a JS object is assigned by a colon instead of an assignment operator, and functionally JS objects are very similar to python dictionaries.
```
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
```
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}

const car1 = new Car('Eagle', 'Talon TSi', 1993);

console.log(car1.make);
// Expected output: "Eagle"
```

Other advanced objects topics added in ES6 include [[JavaScript - Object Destructuring]]
