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

## Searching in a BST
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

The recursive search function itself is pretty straightforward. The base case returns out if we have either found the target value or if the current node is None, which signifys we have reached the end of the tree. The other two cases recur the function on either the left or right branch of the current node depending on if the target is smaller or larger than the current nodes data.

**NOTE**: I am not actually sure that the two search methods are necessary. This came from the "Slither into DSA with python" book. I think it's possible that it needs to be done this way if you want to return the actual node object, but may be unnecessary if you only want to return True when found and false when not found.

## Inserting into a BST
Next we will look at inserting new data into the tree structure. This method will traverse the tree in very much the same way as the search method, except when we reach a leaf node we set the left or right pointer to a new node depending on it's value compared to the child node. 

The base case: If the node we are at is `None` we insert here.
The recursive case: If the insert value is lower than current go down the left tree, insert if none or call insert again. Do the same but down the right tree if insert is larger than current.

Here is the method written out in python:
```python
class BST:
	def __init__(self):
		self.root = None

	def insert(self, val):
		if self.root is None:
			self.root = Node.val
		else:
			self.recursive_insert(self.root, val)

	def recursive_insert(self, node, val):
		if val < node.elem:
			if node.left is None:
				node.left = Node(val)
			else:
				self.recursive_insert(node.left, val)

		else:
			if node.right is None:
				node.right = Node(val)
			else:
				self.recursive_insert(node.right, val)
```

## Deleting from a BST
This is a bit more difficult than inserting into the tree. When we insert we are always just inserting into a leaf on the tree (a terminal node) but with deletions there are three distinct cases we need to cover:

1. The node we are removing has no child nodes, ie it is a **leaf node**. This is the simplest case and we can just remove it. ![[Pasted image 20230820143215.png]]
2. The node has one child. This is the next simplest case as we can just remove the node, and then patch it's parent node to point to it's child node. ![[Pasted image 20230820143242.png]]
3. Finally there is the case where the node to be removed has two children. In this case we can consider that one set of values can be represented as a BST in more than one configuration. Fore example the set `{9, 11, 23, 30}` can be represented as the following two trees: ![[Pasted image 20230820143504.png]]
The two trees are different, but both valid and  represent the same set. The transformation was achieved by starting from the root and searching the right subtree for the minimum element (11) replacing the root with that element and appending the old root to the left tree. This idea can be applied to a deletion method as follows:
1. Find the node to delete
2. Find the min of the BSTs right sub-tree
3. Replace the element of the node to be deleted with the min we just found
4. Recur this removal to the right subtree to remove the now duplicated element
<span style="color: cyan; font-weight: bold; font-style: italic;">When we apply the recursion to remove the duplicate we can be sure that it will fall into case 1 or 2, not case three because it was the minimum of the right tree and thus cannot have two children.</span>

To apply this in code we need two functions, a removal method and a method to find the min. Here it is in code:
```python
class BST:
	def __init__(self):
		self.root = None

	def remove(self, val):
		return self.recursive_remove(self.root, None, val)

	def recursive_remove(self, node, parent, val):
		#helper function
		def min_node(node):
			curr = node
			while curr.left != None:
				curr = curr.left
			return curr
		#base case
		if node == None:
			return node
		#left tree recursion case
		if val < node.elem:
			node.left = self.recursive_remove(node.left, node, val)
		#right tree case
		elif val > node.elem:
			node.right = self.recursive_remove(node.right, node, val)
		#case where node is the target
		else:
			#Node is not the root node
			if not parent is None:
				if node.left is None and node.right is None:
					#node has no children, case 1
					if parent.left == node:
						parent.left = None
					else:
						parent.right = None
				elif node.left != None or node.right != None:
					#Node has one child, case 2
					if node.left != None:
						child = node.left
					else:
						child = node.right
					#point the parent node to the child node of the one being removed
					if parent.left == node:
						parent.left = child
					else:
						parent.right = child
				#Node has two childre, case 3
				elif node.left != None and node.right != None:
					smallest_node = min_node(node.right)
					node.elem = smallest_node.elem
					self.recursive_remove(node, parent, val)
			#Node to delete is root node
			else:
				if node.left is None and node.right is None:
					node = None
				elif node.left != None or node.right != None
```