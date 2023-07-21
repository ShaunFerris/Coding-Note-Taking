#dsa #datastructures #memory #optimization #arrays #linkedlists

The doubly linked list is a very similar data structure to the singly linked list, the only difference being that each node also back-links to the previous node in the chain. See the illustration below:
![[Pasted image 20230718173834.png]]

In terms of implementing the data structure this essentially just means that where the nodes we used for singly linked lists had a payload and a next pointer, <span style="color: cyan;">the nodes for a doubly linked list will have a payload, a next pointer and a previous pointer.</span> In the same way that the tail node of a linked list always has a null pointer for it's next attribute, the head of a doubly linked list will always have a null pointer for it's previous attribute.

## Classes for a doubly linked list data structure
We start with the node class, constructed as discussed above:
```python
class Node:
	def __init__(self, elem, next=None, prev=None):
		self.elem = elem
		self.next = next
		self.prev = prev
```

When constructing the class for the doubly linked list itself, we need to keep track of both the head and tail of the list, and we will also track the length of the list. This will be useful for making traversal of the data more efficient later:
```python
class Doubly_Linked_List:
	def __init__(self):
		self.head = None
		self.tail = None
		self.length = 0
```

### Inserting into the D-linked list
Appending to the D-linked list is straightforward but is done with a different method than we use for single linked lists.

There are two cases that we need to handle:
1. When the list is empty, in which case the head and tail will be pointing to the same node object
2. When the list is not empty and we insert into the end of the list. In this case we will need to update the tail pointer to point at the new element.

Here is the example D-linked list with an insert method:
```python
class Doubly_Linked_List:
	def __init__(self):
		self.head = None
		self.tail = None
		self.length = 0

	def insert(self, elem):
		new_node = Node(elem, None, None)
		if self.head is None:
			self.head = new_node
			self.tail = self.head
		else:
			new_node.prev = self.tail
			self.tail.next = new_node
			self.tail = new_node
		self.length += 1
```

### Deleting from a D-linked list
When we handled deleting from a singly linked list we had to keep track of the previously visited node, but with a doubly linked list we avoid the need to do this due to the `prev` pointer on each node.

The node we want to delete is identified when its data instance variable matches the `elem` variable. We will also return false if the item to be deleted doesn't exist in the list and true if it did. Here's the code:
```python
class Doubly_Linked_List:
	def __init__(self):
		self.head = None
		self.tail = None
		self.length = 0

	def delete(self, val):
		curr = self.head
		if curr is None:
			return False
		elif curr.elem == val:
			self.head = curr.next
			self.head.prev = None
			self.length -= 1
			return true
		elif self.tail.elem == val:
			self.tail = self.tail.prev
			self.tail.next = None
			self.length -= 1
			return True
		else:
			while curr:
				if curr.elem == val:
					curr.prev.next = curr.next
					curr.next.prev = curr.prev
					self.length -= 1
					return True
				curr = curr.next
```

That pretty much wraps up the implementation of a D-linked list, for applications of the data structure, see: [[Algorithms - Doubly Linked Lists for Browser Navigation]]