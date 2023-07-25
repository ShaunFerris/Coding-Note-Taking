#dsa #trees

A binary search tree is different from the more basic data structures that you are likely already familiar with, because it is not a linear data structure. A BST is type of ordered data structure that stores items and allows for fast searching, addition and subtraction of items.

BSTs keep their items in a sorted order to keep the operations fast. <span style="color: cyan; font-weight: bold; font-style: italic"> Where a linked list has add and remove operations that go in O(n) time, a BST has them in O(log n) time, and also has search operations in O(n log) time</span>.

## Nomenclature
- <span style="color: cyan;">Tree: </span>A **tree** is an abstract data type that is widely used in computer science. The term "abstract datatype" simply means that it is on defined by it's behaviour, NOT it's implementation. The tree simulates a heierarchical tree structure like a family tree, with a root node from which emerge subtrees of child nodes.
- <span style="color: cyan;">Binary Tree: </span> The BInary Tree is a subtype of the tree data structure in which each node has **at most** two child nodes.
-  <span style="color: cyan;">Binary Search Tree: </span> The BST is a subtype of binary tree in which all the elements are ordered.

## Example BST
![[Pasted image 20230725102411.png]]
The above diagram from the book "Slither into Data Structures and Algorithms" illustrates a simple BST. You can see that:
- The root of the tree is the node labelled "10"
- The circled sub-tree is the RIGHT subtree of the ROOT node
- 10 is the PARENT node to 5 and 22
- Similarly 5 is the CHILD node of 10

The above information satisfies the requirements to be a binary tree, as no node has more than two direct child nodes, but the tree also satisfies the requirements to be a BST as it is sorted. Take the root node 10, and notice that every child in it's right subtree is larger than it, while everything in the left subtree is less than it. The same is true of every other node.
<blockquote style="color: cyan; font-weight: bold; font-style: italic">What makes searching fast is this, if we wanted to check if 2 was in the tree, we start at the
root node and check if 2 is less than the value at the root. In this case 2 is less than 10 so we
know we don't need to bother searching the root node's right sub tree. Essentially what we've
done here is cut out everything in that blue circle as the element 2 will not be in there.</blockquote>
So by virtue of the way the data is organised, we engineer a situation where every traversal of the tree cuts the data we need to consider in half.

## Implementation and applications
For implementation of a basic BST see here: [[Data Structures - Implementation of a BST]]

For Examples of applications of a BST, see here: 