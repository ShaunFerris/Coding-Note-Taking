#datastructures #mutabletypes #dictionaries #objects 

These notes will cover the fundamentals of python dictionaries and JS objects. The terms dictionary, or dict, and object will be used interchagably here, despite the fact that in JS, objects can also refer to class definitions. See also: [[JavaScript - Objects]], [[JavaScript - ES6 Objects and the Class Keyword]].

## Basics
At their most basic, objects in JS (dictionaries in python) are just collections of key-value pairs. In other words, they are pieces of data (values) mapped to unique identifiers called properties (keys). Take a look at an example (but note that in python dict keys must be hashable objects, so to replicate this object in python, the keys would be strings and thus would require quotes.):
```js
const tekkenCharacter = {
  player: 'Hwoarang',
  fightingStyle: 'Tae Kwon Doe',
  human: true
};
```

The most important difference between python dictionaries and JS objects is that from python 3.7 onwards, dicts are ordered by insertion order. In JS objects do not maintain ordering and a values position in the object is only tied to it's key.

Another important note is that dot notation works for accessing object properties in JS, but not python, python requires bracket notation. This is because JS objects also fill the role of classes, but in python this functionality is more clearly separated.

## Reassigning values in objects and dicts
Use bracket notation, it works the same in both JS and python. In JS you can use dot notation too, but only when there aren't spaces in the name of the key you are accessing. The `delete` keyword(JS) or the `del` keyword (python) can be used to remove entries from dicts.

## Test the existence of a key
To check if a property is in a JS object, use the `object.hasOwnProperty()` method. This will return true or false. You can also use the `in` keyword.

The same can be achieved in python using the in keyword: `print(key in my_dict)`.

To get a keys value from a python dict you can use the `my_dict.get(key)` method. This method is also very useful for counting when supplied with a default value. 

If you want to check if a key is in a dict, then add it if it's not and add one to it's value use `my_dict[key] = my_dict.get(key, 0) + 1`

## Listing object keys
### Js
We can also generate an array which contains all the keys stored in an object with the `Object.keys()` method. This method takes an object as the argument and returns an array of strings representing each property in the object. Again, there will be no specific order to the entries in the array.

### Python
The `keys()` method returns a view object. The view object contains the keys of the dictionary, as a list.
```python
car = {  
  "brand": "Ford",  
  "model": "Mustang",  
  "year": 1964  
}  
  
x = car.keys()  
  
print(x)
```
The above will return an object containing the keys as a list:
```python
dict_keys(['brand', 'model', 'year'])
```
