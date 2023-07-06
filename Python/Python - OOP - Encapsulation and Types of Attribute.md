#python #paradigms #objects #oop 

## Encapsulation
Encapsulation in the context of software systems is the key principle of Object Oriented Programming, and refers to the <span style="color: cyan">bundling of data with the mechanisms or methods that operate on that data.</span> It may also refer to the limiting of direct access to some of that data, such as an object's components. Encapsulation allows developers to present a consistent and usable interface which is independent of how a system is implemented internally.

## Public private and protected attributes
An important concept in OOP that allows us to properly implement encapsulation is the idea of access modifiers. These tell compilers or the runtime which other classes should have access to data attributes and methods that are defined within a given class, helping to prevent unintended modification of class data.

Access modifiers are used to make code more robust and secure by limiting access to variables that don't need to be accessed by other parts of the code (or maybe they do in which case they're public). 

Attributes and classes in python are public by default, in fact python does not actually have access modifiers built in, and instead access control is handled by convention.
```python
#Public attributes example
def __init__(self, hours, minutes, seconds):
	self.hours = hours
	self.minutes = minutes
	self.seconds = seconds

#Private attributes example
def __init__(self, hours, minutes, seconds):
	self.__hours = hours
	self.__minutes = minutes
	self.__seconds = seconds

#Protected attributes example
def __init__(self, hours, minutes, seconds):
	self._hours = hours
	self._minutes = minutes
	self._seconds = seconds
```
To "make" variables private we add a double underscore in-front of the variable name which means that only the class they are contained in can call and  access them. Nothing on the outside. In the Java language, if you had two classes and tried to call a private method from another class you would get an error. This convention is defined in PEP 8. **This isn't enforced by Python**. Python does not have strong distinctions between “private” and “public” variables like Java does.

The single underscore denotes a protected variable, which is the same as a private one except that subclasses are allowed to access it.