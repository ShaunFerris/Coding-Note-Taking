#javascript #python #datastructures #mutabletypes #dictionaries #objects 

These notes will cover the fundamentals of python dictionaries and JS objects. See also: [[JavaScript - Objects]].

## Basics
At their most basic, objects in JS (dictionaries in python) are just collections of key-value pairs. In other words, they are pieces of data (values) mapped to unique identifiers called properties (keys). Take a look at an example (but note that in python dict keys must be hashable objects, so to replicate this object in python, the keys would be strings and thus would require quotes.):
```js
const tekkenCharacter = {
  player: 'Hwoarang',
  fightingStyle: 'Tae Kwon Doe',
  human: true
};
```
Another important note is that dot notation works for accessing object properties in JS, but not python, python requires bracket notation. This is because JS objects also fill the role of classes, but in python this functionality is more clearly separated.

## Reassigning values in objects and dicts
Use bracket notation, it works the same in both JS and python. In JS you can use dot notation too, but only when there aren't spaces in the name of the key you are accessing.