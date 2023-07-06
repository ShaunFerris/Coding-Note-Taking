#python #paradigms #objects #oop 

Given that one of the defining characteristics of OOP is the ability to model data or objects in an encapsulated way, a common situation to encounter is one where we want to <span style="color: cyan;">model an object that is an example or subset of one that we have already modeled.</span>

For example we may have a vehicle class for modeling vehicles, of which both motorbikes and planes are examples. The bikes and the plane both share characteristics that are shared by all vehicles, but also have associated data that is not shared. In this case we could define subclasses that inherit from the vehicle class but allow more accurate modeling of a given sub-class of vehicle. In this case the vehicle class is the <span style="color: cyan;">parent or base class</span> and the plane and bike classes are the <span style="color: cyan;">child, derived or sub classes</span>.

## Parent and child classes
We can create a parent which will act as a base from which subclasses can be derived. This allows us to create child classes through inheritance without having to rewrite the same code repeatedly.

Let’s say we have Rectangle and Square classes. Many of the methods and attributes between a Rectangle and Square will be similar. For example both a rectangle and a square have an area() and a perimeter() that is calculated in the same way. The only difference is that squares are initialized with a single length value, while rectangles have a length and a width. We can avoid repeating ourselves by defining the square to inherit it's methods from the Rectangle class, and the overwrite the `__init__` method like so:
```python
class Rectangle:
	def __init__(self, length, width):
		self.length = length
		self.width = width

	def area(self):
		return self.length * self.width

	def perimeter(self):
		return 2 * self.length + 2 * self.width

class Square(Rectangle):
	def __init__(self, length):
		super().__init__(length, length)
```
The `super()` method in Python returns a proxy object that delegates method calls to a parent or sibling of type. That definition may seem a little confusing but what that means in this case is that we have used `super()` to call the `__init__()` method of the Rectangle class, which allows us to use it without having to re-implement it in the Square class. Now, a squares length is length and its width is also length as we passed length in twice to the call to the `super()`’s `__init__()` method.

With inheritance we can also override methods in our child classes. Let’s add a new class called a Cube. The cube will inherit from Square (which inherits from Rectangle). The Cube class will
extend the functionality of the area method to calculate the surface area of a cube. It will also have a new method called volume to calculate the cubes volume. We can make good use of
`super()` here to help us reduce the amount of code we’ll need.
```python
class Square(Rectangle):
	def __init__(self, length):
		super().__init__(length, length)

class Cube(Square):
	def area(self):
		side_area = super().area()
		return 6 * side_area

	def volume(self):
		side_area = super().area()
		return side_area * self.length
```
As you can see, we haven’t had to override the `__init__()` method as it’s inherited from Square. We have however overridden the `area()` method from the parent class (Square) on the Cube to reflect how the area of a cube is calculated. We have also used the `super()` method to help us with this.

## Multiple inheritance
So far, we have looked a single inheritance. That is, child classes that inherit from a single parent class. Multiple inheritance is when a class can inherit from more than one parent class. 

This allows programs to reduce redundancy, but it can also introduce a certain amount of complexity as well as ambiguity. If you plan on doing multiple inheritance in your programs, it should be done with care. Give thought to your overall program and how this might affect it. It may also affect the readability of your programs. For example:
```python
class A:
	def method():
		pass

class B:
	def method():
		pass

class C(A, B):
	pass

C.method()
```
Which method is the C class referring to? The method from A or the method from B? This may make your code harder to read. In Python, when calling method() on C, Python first checks if C has an implementation of method then it checks if A has implemented method. It has in this case so it uses that, but you can see how this can get confusing as shit really fast.