#python #paradigms #objects #oop 

These notes will skip over some of the very basic concepts that I already know very well, but are based on chapter 2 of the book "Slither into Data Structures and Algorithms". If you are reading these notes and want a more thorough exploration, find that book for free online.

## Classes and objects
Python has many bulit-in types such as booleans, integers, dictionaries etc. These are al class types, meaning that any particular instance of a string, list, float etc is an object of the class string, list or float. Every string is an <span style="color: cyan;">instance</span> of the string <span style="color: cyan;">class</span> of <span style="color: cyan;">objects.</span> An object is an implementation of a type, and a class is the bluprint for that type.

OOP is a paradigm, like functional or imperitive programming. It is based on the concept of objects, which contain data in the form of <span style="color: cyan;">attrributes</span> and functions called <span style="color: cyan;">methods</span>. Objects are entities that model some real world entity or conceptualized data structure.

In OOP, programs are built by defining object classes and instantiating objects that will interact with each other to perform the actions of the program. Objects use their methods to access and modify their attributes (data), for example a person class may have an age attribute and a change_age() method to increment it as the person ages. the general python syntax for invoking a method is `object_name.method_name(args)`.

<blockquote style="color: cyan; font-weight: bold; font-style: italic;">Most of the time, Python’s built-in types are not enough to model data we want, so we built
our own types (or classes) which model the data exactly how we need.</blockquote>

## Defining new types - classes
Classes are defined using the `class` keyword, and it is convention to name classes with capital letters. An instance of a class is then created by calling the name of the class as a function. In JS you would use the `new` keyword, but that is not neccessary in python.

Imagine we wanted to model time. This is difficult with pythons built in data types, so we will make our own class for the purpose. We want to model the time in hours, minutes and secons, so we define a method that initializes the time of our time objects instance. We also want to define a display() method to print the time:
```python
class Time:
	def __init__(self, hours, mins, secs):
		self.hours = hours
		self.mins = mins
		self.secs = secs

	def display(self):
		print(f'The time is {self.hours} : {self.mins} : {self.secs}')
```

Notice the use of the init method and the self keyword. The init method is an example of a special method or dunder method.

### The str() method
The `__str()__` method is another special function that dictates what is returned when the print() function is called on an object instance. When we call print on a built-in like string or list, we get a nice, human readable representation of the object. Under the hood, the list built in type implements the `__str()__` method to achieve this.

We can override this method to allow us to format and print our objects as we see fit: without implementing this method on our custom classes, we would see something like `<time.Time object at 0x000001A7C6902F60>` when we try to print. This allows us to replace the display() method from the above example time class, and then simply call print on the object rather thatn invoking the display method.

### The len() method
This dunder method allows us to override the default implementation of the len() functionto define what the length of our custom data type should be. For example we might implement a Point class for modeling a location in two dimensional space, and we want the length to represent the distance between the point and the origin (0,0). We will get an error otherwise. So, we’ll have to round either up or down. Python does this in order to make the len() function predictable. In this example, we’ll just cast our return value to an integer:
```python
class Point:
	def __init__(self, x=0, y=0):
		self.x = x
		self.y = y

	def __str__(self):
		return f"X coordinate: {self.x}\nY coordinate: {self.y}"

	def __len__(self):
		distance_from_origin = ((self.x) ** 2 + (self.y) ** 2) ** 0.5
		return int(distance_from_origin)
```
Now if we call the built-in len() function on an instantiated point it will tell us the distance to the center from that point rounded to an int. We could also implement another class method that does not cast to int if we wanted an accurate measure.

There are a tonne of special methods in python, and I won't cover them all in these notes, but the last aspect of special methods that will be useful for making our own data structures is operator overloading. See [[Python - OOP - Operator Overloading]].

Or see other topics in Python OOP:
[[Python - OOP - Instance vs Class Mehods]]
