#dsa #algorithms #datastructures #problemsolutions

A very common application of this data structure is in internet browsers, to implement the ability to go back and forwards to previously visited sites or pages.

To begin implementing the algorithm for this we will need to instantiate a class that inherits from the doubly linked list class, and give it a position variable to track the browsers current position. I'm going to include the full class for the doubly linked list just for reference:
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

class Browser_Navigation(Doubly_Linked_List):
	def __init__(self):
		super(Browser_Navigation, self).__init__()
		self.position = self.tail
```
We start by implementing the position variable on top of the DLL, and now we need to implement methods for the forwards and backwards functionality. To do this we need to think about what it means to navigate in a browser. As we navigate around the web in the forwards direction, we are inserting into the DLL, and staying at the tail. When we go back, we move backwards toward the head of the list. If we then go forwards again we move back toward the tail, BUT if we go back and then navigate to a different site, we need to clear the list from our position forwards, and add the destination page as the new tail node.

This means that forwards and backwards movement are relatively easy to do, but we also need a method for navigation. Here is how you would implement the three methods:
```python
class Browser_Navigation(Doubly_Linked_List):
	def __init__(self):
		super(Browser_Navigation, self).__init__()
		self.position = self.tail

	def back(self):
		if self.position.prev:
			self.position = self.position.prev
			print(f'visiting {self.position.elem}')
		else:
			print("No page in history")

	def forward(self):
		if self.position.next:
			self.position = self.position.next
			print(f'visiting {self.position.elem}')
		else:
			print("No page ahead")

	def navigate(self, site):
		if self.position is None:
			self.insert(site)
			self.position = self.tail
			print(f`Visiting {self.position.elem}`)
		elif:
			self.position.next is None:
				self.insert(site)
				self.position = self.tail
				print(f`Visiting {self.position.elem}`)
		else:
			#void everything after the current position
			self.position.next = None
			#set tail to where we are
			self.tail = self.position
			#add new site
			self.insert(site)
			#move the position forward to new site
			self.position = self.tail
			print(f`Visiting {self.position.elem}`)
```
