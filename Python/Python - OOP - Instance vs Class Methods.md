#python #paradigms #objects #oop 

There are two types of methods for classes in python, instance and class methods. Instance methods are the most common type of methods in classes, and the ones that youshould be familiar with already. They are called instance methods as they act on specific instances of a class. They can access the data attributes that are unique to that class.

Class methods are methods that know about their class, are permanently bound to the class and allow us to modify the class's state in a way that will apply to all instances of that class. They cannot access data that is specific to an instance, but they can call other methods, and can modify class specific details such as <span style="color: cyan;">class variables</span>.

Unlike instance methods, that take self as their first parameter, class methods take the class itself, usually called `cls`. We use the built-in decorator `@classmethod` to mark a method as a class method.