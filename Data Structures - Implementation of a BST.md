#dsa #trees

Implementing the BST starts with defining a node, similar to a linked list, but the node for a BST will be slightly different:
```python
class Tree_Node:
	def __init__(self, elem):
		self.elem = elem
		self.left = None
		self.right = None
```
Each node contains an element (the data) and a pointer to the left and right child nodes, each of which is initialized to None but will later point to another node.

The very basics of the BST class will just initialize with root pointer that points to the root node. That's all you need for the very basic tree, and you can move through the tree following left or right pointers till you find what you are looking for, then add or remove it.

In terms of improving this class, we will look first at implementing searching as that is the main thing BSTs are good for. To implement this searching functionality into the class, we will use recursion and return None if the target is not found, or return the Node if it is. This will look something like this:
```python
class BST:
	def __init__(self):
		self.root = None

	def search(self, target):
		return self.recursive_search(self.root, value)

	def recursive_search(self, node, value):
		if node is None or node.elem == value:
			return node
		elif value < node.value:
			return recursive_search(node.left, value)
		else:
			return recursive_search(node.right, value)
```
This code looks very strange at first, but it is done like this for a good reason. <span style="color: cyan; font-weight: bold;">Why do we need two different search functions where one is just a wrapper for the other?</span> The `search()` method allows us to call the `recursive_search()` method and pass in the root node, which means that the function will be working on a copy of the root node and not the node itself. This prevents the function from entering a recursive infinite loop.