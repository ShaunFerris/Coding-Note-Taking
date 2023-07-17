#dsa #datastructures #memory #optimization #highlevelconcepts #arrays #linkedlists

Here I will be going over a detailed look at the implemenation of a singly linked list in Python.

Here is a diagram of a singly linked list:
![[Pasted image 20230717105002.png]]
The elements of the data structure are:
- Head: This is the first element in the list
- Tail: The last element in the list, can be id'd as the only element with a null pointer
- Node: Each element in the list is represented as a node. Each node has a value and a pointer.

We can break down the implementation of this data structure down into two parts: the node and the list itself, with the list being made of nodes that point to each other.

## Basic classes for node and list
The node class is very simple as it only needs two things, a value for the element and a value for the next pointer:
```python
class Node:
	def __init__(self):
		self.elem = elem
		self.next = None
```
Note that we have the next value default to None, because we will be adding nodes to the linked list at the end position, which is identified by being the only node that points to None.

The LinkedList class itself has a few more requirements. It needs to initialize empty, so it should init with a `head` attribute set to none. Then it will need a method to add nodes to the list, and one to delete them. The init is implemented like this:
```python
class LinkedList:
	def __inti__(self):
		self.head = None
```

### Adding to a linked list
We want to add new elements to the end of the list, and the LinkedList class itself only really tracks the value of the head node. This means that if the list is empty, we add an elements by simply instantiating a new Node and assigning it to the head, or if the head is not empty, we follow the nodes until we find a node with a null pointer, instantiate a new node with our value, and change the null pointer to lead here.
```python
class LinkedList:
	def __init__(self):
		self.head = None

	def add(self, elem):
		if self.head == None:
			self.head = Node(elem)
		else:
			curr = self.head
			while curr.next != None:
				curr = curr.next
			curr.next = Node(elem)
```

### Deleting from a linked list
