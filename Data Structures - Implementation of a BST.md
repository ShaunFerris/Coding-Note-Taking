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

