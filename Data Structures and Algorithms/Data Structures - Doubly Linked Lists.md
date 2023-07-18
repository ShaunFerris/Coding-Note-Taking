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

