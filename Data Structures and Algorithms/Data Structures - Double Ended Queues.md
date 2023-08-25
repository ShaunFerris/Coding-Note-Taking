#dsa #queues 

The double ended queue, or dequeue (pronounced **_deck_**) is an abstract data structure that is essentially just like the queue, except it supports insertion and deletion at both ends of the structure. This means it is essentially capable of both last in first out operations like a stack, or first in first out operations like a queue.

The dequeue ADT is more general than both the standard stack and queue structures, having more flexibility. The dequeue is a symmetrical abstraction, so it needs methods for identical operations at both ends of the list. It needs to be able to add and remove from either end, and it will need accessor methods for returning the current first element and the current last element, the length and and isEmpty boolean.

## Python implementation
This ADT can be implemented in python using the built-in array data type, much the same way as a regular queue or stack. A very simple implementation would be something like this:
```python
class Dequeue:
	def __init__(self):
		self.data = []
		
	def isEmpty(self):
		return len(self.data) == 0
		
	def addFront(self, item):
		self.data.append(item)
		
	def addBack(self, item):
		self.data.insert(0, item)
		
	def removeFront(self):
		return self.data.pop()
		
	def removeBack(self):
		return self.data.pop(0)
		
	def size(self):
		return len(self.data)
```

## Premade dequeue class
The collections module in the python standard library already has an implementation of this ADT. Look up the docs for the names of it's implementation of all the necessary methods for the data structure. 