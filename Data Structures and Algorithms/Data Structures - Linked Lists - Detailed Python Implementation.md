#dsa #datastructures #memory #optimization #arrays #linkedlists

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
We have a few different options when implementing a delete function for a linked list, we could delete the first element, the last element or the first occurence of an element. In this instance, lets go for that last option, deleting the first instance of a given element.

When deleting an element from a linked list we are essentially just re-routing the next pointer of the preceeding node to go around the deleted node:
![[Pasted image 20230717113141.png]]
Lets look at the implementation:
```python
class LinkedList:
	def __init__(self):
		self.head = None

	def delete(sefl, val):
		if self.head == None:
			return
		if self.head.elem == val:
			self.head = self.head.next
			return val
			
		curr = self.head
		while curr.next != None and curr.next.elem != val:
			curr = curr.next

		if curr.next == None:
			return
		else:
			curr.next = curr.next.next
			return val
```
We have to cover a couple of cases here. The first is that the list is empty in which case return nothing. The second is that the head of the linked list is the element we want to delete. If it is then we make the head of the list the second `Node` in the list (Which may be `None` but thats ok, it just means the list only had one item). Finally, if it is neither of those cases we traverse the list using linear search until we find the first occurrence of the element then we 're-route' the `next` pointer of the `Node` that points to the `Node` we want to delete, to the `Node` the `next` pointer of the `Node` we want to delete points to.

## Time complexity
So far we have looked at adding and deleting from the linked list, so lets take a second to think about the time complexity of these operations.

The run time complexity of the add method is O(n) in this case as we must iterate over each entry in the list to find the last element. The runtime complexity of the deletion method is also O(n). Although we don't always iterate over each element in the list, Big-O deals with the worst case, which is going through the entire list. There are optimizations we could make though so that the `add()` method is always O(1). In other words, it is constant. See below for the more efficient implementation of the add method:
```python
class LinkedList:
	def __init__(self):
		self.head = None

	def add(self, elem):
		if self.head == None:
			self.head = Node(elem)
		else:
			new_head = Node(elem)
			new_head.next = self.head
			self.head = new_head
```
