#dsa 

Circular linked lists are exactly like singly linked lists, except that the tail node links back to the head node.

In code the implementation has some differences to SLL's because there are a couple of special cases to pay attention to; most importantly around searching/traversing the list because this can go infinite in a CLL.

The node class for the CLL is the same as it is for SLL, so that will be omitted here, but we can start our CLL by defining the init and travers methods:
```python
class Circular_Linked:
	def __init__(self, head=None, tail=None):
		self.head = head
		self.tail = tail

	def traverse(self):
		curr = self.head
		while curr.next:
			print(curr.elem)
			curr = curr.next
			if curr == self.head:
				break
```
The traverse method sets the current node to the head node and then traverses infinitely through the list, but breaks when it gets to the head nond again.

## Inserting into CLLs
We will be inserting new items into the end of the list, which is still O(1) because we have a tail reference and don't have to traverse from the head to the tail. Implementing and insertion method for the head of the list would also be trivial and very similar in terms of code.
```python
class Circular_Linked:
	def __init__(self, head=None, tail=None):
		self.head = head
		self.tail = tail

	def insert(self, elem):
		new_node = Node(elem)
		if self.head == None:
			self.head = new_node
			self.head.next = new_node
			self.tail = new_node
		if self.tail != None
			self.tail.next = new_node
			new_node.next = self.head
			self.tail = new_node
```
The first if statement here handles the case of inserting into an empty list, in which case the head and tail will both be the same new node. The second statement handles adding the new node to the tail position.

## Deleting from CLLs


## Applications of CLLs
Without looking at actual code, some examples of use cases for CLLs are:
- Games: Handling the game cycle in turn based games and triggering round start effects when we get back to the first player in turn order
- Operating system process scheduling
- Media players for repeat functionality of music queues.