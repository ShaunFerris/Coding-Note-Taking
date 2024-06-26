#javascript #objects #classes #ES6 

Prior to ES6 an object was created with a constructor statement, and a new instance was created by invoking the `new` operator. See [[JavaScript - Objects]].

In ES6, the class keyword was added and has a constructor method that is invoked with the new operator. If a constructor is not defined in a class definition then that class will use an implicit constructor. 

Terminology wise, a constructor method is the same as an `__init__` method in python.
In place of the `self` keyword used in python, JS uses the `this` keyword.
```js
// Explicit constructor
class SpaceShuttle {
  constructor(targetPlanet) {
    this.targetPlanet = targetPlanet;
  }
  takeOff() {
    console.log("To " + this.targetPlanet + "!");
  }
}

// Implicit constructor 
class Rocket {
  launch() {
    console.log("To the moon!");
  }
}

const zeus = new SpaceShuttle('Jupiter');
// prints To Jupiter! in console
zeus.takeOff();

const atlas = new Rocket();
// prints To the moon! in console
atlas.launch();
```

## Getters and Setters
In JS you can get values and set the values of properties within an object. When an object property is named with an underscore at the beginning, it is private, meaning the usual syntax for getting it's value or changing it from outside its constructor or methods will not work.

To allow the getting and setting of private object variables, you must write getter and setter methods into the objects class definition:
```js
class Book {
  constructor(author) {
    this._author = author;
  }
  // getter
  get writer() {
    return this._author;
  }
  // setter
  set writer(updatedAuthor) {
    this._author = updatedAuthor;
  }
}
const novel = new Book('anonymous');
console.log(novel.writer);
novel.writer = 'newAuthor';
console.log(novel.writer);
```
Notice the syntax used to invoke the getter and setter. They do not even look like functions. Getters and setters are important because they hide internal implementation details.

**Note:** It is convention to precede the name of a private variable with an underscore (`_`). However, the practice itself does not make a variable private.

## Constructors and instancing
When a class is instantiated using a constructor, anything that is declared in the constructor and assigned to the this object is considered to belong to that instance of the class. If you were to define a method as part of the constructor method, then for every instance of the class that you spin up, you will have one distinct version of that function assigned to memory. 

By contrast, any methods or properties defined in **outside** the constructor method are handed down to the prototype object of the class, and thus only exist once  in memory no matter how many instances of the class have been instantiated.

You can get the prototype of a given object with:
```javascript
Object.getPrototypeOf(classInstance)
```

For two instance of the Dog class:
```javascript
class Dog {
	constructor(name) {
		this.username = username;
		this.wagTail = () => {
			return "Wagging tail"
		}
	}
	bark() {
		return "Woof"
	}
}

const dog1 = new Dog("Max", "Labrodor")
const dog2 = new Dog("Spot", "Dalmatian")
```
All of these statements are true:
```javascript
dog1.wagTail() === dog2.wagTail()

dog1.bark === dog2.bark

Object.getPrototypeOf(dog1) === Object.getPrototypeOf(dog2)

dog1.constructor === dog2.constructor
```
And this statement is not true:
```javascript
dog1.wagTail === dog2.wagTail
```