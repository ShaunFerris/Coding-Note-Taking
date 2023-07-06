#python #paradigms #objects #oop 

There are two main types of methods for classes in python, instance and class methods. Instance methods are the most common type of methods in classes, and the ones that youshould be familiar with already. They are called instance methods as they act on specific instances of a class. They can access the data attributes that are unique to that class.

## Class methods
Class methods are methods that know about their class, are permanently bound to the class and allow us to modify the class's state in a way that will apply to all instances of that class. They cannot access data that is specific to an instance, but they can call other methods, and can modify class specific details such as <span style="color: cyan;">class variables</span>.

Unlike instance methods, that take self as their first parameter, class methods take the class itself, usually called `cls`. We use the built-in decorator `@classmethod` to mark a method as a class method.

<blockquote style="color: cyan; text-align: center; font-weight: bold; font-style: italic;">Essentially, class methods are methods that can be called from the class itself, not only from an instance of the class.</blockquote>

For an example, here is a class constructor to model time:
```python
class Time:
	def __init__(self, hours, mins, secs):
		self.hours = hours
		self.mins = mins
		self.secs = secs

	def __str__(self):
		print(f'The time is {self.hours} : {self.mins} : {self.secs}')

	@classmethod
	def seconds_to_time(cls, s):
		minutes, seconds = divmod(s, 60)
		hours, minutes = divmod(minutes, 60)
		extra, hours = divmod(hours, 24)
		return cls(hours, minutes, seconds)
```
With the classmethod added to this class, now we can instatiate a Time object with hours minutes and seconds from only a number of seconds by calling the seconds to time method.
```python
from myTime import Time
t = Time.seconds_to_time(11982)
print(t)
#'The time is 03:19:42'
```

We can also use classmethods to access class variables. Muc like class methods theses are bound to the class itself, not a give instance. By convention they are defined in all caps. As an example we can use a class variable to keep track of how many times the class has been instantiated:
```python
class Time:
	COUNT = 0

	def __init__(self, hours, mins, secs):
		self.hours = hours
		self.mins = mins
		self.secs = secs
		COUNT += 1

	def __str__(self):
		print(f'The time is {self.hours} : {self.mins} : {self.secs}')

	@classmethod
	def seconds_to_time(cls, s):
		minutes, seconds = divmod(s, 60)
		hours, minutes = divmod(minutes, 60)
		extra, hours = divmod(hours, 24)
		return cls(hours, minutes, seconds)

from myTime import Time
t1 = Time(23, 11, 37)
t2 = Time()
Print(Time.COUNT)
#2
```

## Static methods
Static methods can be thought of as ordinary functions, and unlike instance and class methods, they don't need special args like `self` or `class`.

Static  methods are functions that are in some way related to the class, but don't need to actually access or interact with any of the class specific data, or need an instance to be invoked. 

We use the `@staticmethod` decorator to specify a static method, and this ensures that the method in question will not require a special parameter and will not be allowed to access the class or instance attributes.