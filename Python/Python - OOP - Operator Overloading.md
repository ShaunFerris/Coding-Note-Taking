#python #paradigms #objects #oop 

When using classes to model our own data structures in python, it can be useful to have the built-in operators like `==` or `*` interact with our custom objects in a way that we define. We can do this by using certain dunder methods in the class definition. For example when we use the `==` operator on two integers, the `__eq__` method is called. 

When method overriding is done on methods that define the behavior of operators we call that <span style="color: cyan;">operator overloading</span>. We can overload the default behaviour of the equivalence method for a class that models points in 2d space as follows:
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

	def __eq__(self, other):
		return ((self.x, self.y) == (other.x, other.y))
```
In this case, we overload the `__eq__()` method by comparing two tuples. If the two tuples, each containing the x and y coords of points are equal, then our two points are equal:
```python
p1 = Point(2,2)
p2 = Point(2.2)
p3 = Point(2,3)
p1 = p2 #true
p1 = p3 #false
```

We could also add two points together to get a new point. In this case we would overload the __add()__ method which dictates the functionality of the `+` operator. Note that it does **NOT** affect the `+=` method for in place addition, so we must return a new object. We can however also use the `__iadd__()` method to overwrite the in place addition operator:
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

	def __eq__(self, other):
		return ((self.x, self.y) == (other.x, other.y))

	def __add__(self, other):
		new_x = self.x + other.x
		new_y = self.y + other.y
		return Point(new_x, new_y)

	def __iadd__(self, other):
		temp_point = self + other
		self.x, self.y = temp_point.x, temp_point.y
		return self
```
Be careful when overloading in-place operators. In this case we use create a temporary point and then update the self instance using multiple assignment. Remember when we swapped the value in one variable for another and we had to create a temp variable? We can do that much faster by doing x, y = y, x. In this case x, y and y, x are tuples! Anyway, after we update the attributes for self, we must return self.

Below are a list of other operators you can overload:
![[Pasted image 20230630152157.png]]

