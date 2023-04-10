#datastructures #memory #optimization #highlevelconcepts #arrays #linkedlists

The notes in this video start with a summary of [this video](https://www.youtube.com/watch?v=34ky600VTN0) by simon dev titled **Why linked lists vs arrays is not a real choice.**

## Arrays and linked lists
Linked lists are used to hold lists of items, in a similar vein to arrays, but how do you know which to use and when? Here will go over this, and not just guidelines of when to use one or the other, but trying to gain an understanding fundamentally of the connection between these two data structures and the underlying memory.

## What are linked lists?
In the most simple terms, linked lists are way of storing several items at once, just like arrays, they let you store a collection of data that is somehow in a meta sense related, in one place rather than having say, all the names of the people on a team stored to seperate variables. So how are they different from arrays? Lets look at an example:

### Example
Say you are out for dinner with family at a restaurant. You are seated at a nice table, and later on some family friends arrive, so you need some extra seats. There are a few strategies to take here. If the table you are at has some extra room, they could just join you, if the table has no extra room, the wait staff could find you a bigger table and seat you all there, or finally, the friends could sit at  a new table, but you know where they are so you can all say hi to each other. 

Memory is not much different to this example. If we have some numbers stored in a block of memory and wish to assign another item to this list, then if there is room in that block of memory  we can just write the new number to the end. If there is no more room, then we could ask for a bigger piece, copy everything there and then write in the new thing. Or finally, we could allocate a new piece of memory, and in there we would store the new data, **and an address to the original data that it is related to. THIS is what linked lists are.**

### Linked lists in more detail
Linked lists are made up of fragments of memory that we will call nodes. Each node is structured with two sections, the first holds the data to store, and the second, holds the address of the next node in the chain.

When we start one of these lists, we put our data into the first nodes data slot, and leave the address slot empty. Then when we add a second piece of data, it's node gets the address of the first node, and so on in a chain.

## Are they better or worse than arrays?
To understand that, we need to understand how to do basic operations with them. 

### Traversing the list
When we say traversing a list, we mean simply visiting all the elements one by one. For arrays this is obviously very simple, loop over one at a time, incrementing in index each time. But linked lists are different due to their layout and the fact that the individual members of the linked list are not guaranteed to be contiguous in memory. You start at the head node, or most recent one, and then follow the pointer in it's second slot to the next node, until you reach the one with a null value in  its address slot.

Here is some sample code for traversing a list in JS:
```js
const node1 = {data: 4, next: null};
const node2 = {data: 1, next: node1};
const node3 = {data: 3, next: node2};

function traverseLinkedList(head) {
	let currNode = head;
	while (currNode) {
		console.log(currNode.data);
		currNode = currNode.next;
	}
}

traverseLinkedList(node3);
```
And the same in python:
```python
node1 = {data: 4, next: None}
node2 = {data: 1, next: node1}
node3 = {data: 3, next: node2}

def traverse_linked_list(head):
	curr_node = head
	while curr_node:
		print(curr_node['data'])
		curr_node = curr_node['next']

treverse_linked_list(node3)
```

Both of these pieces of code use a loop, where we print or otherwise access the data of the current node, before updating the current node with the pointer in the what was previously the current node. This loop continues until the current node is set to null by the base node signifying we have gone off the end of the linked list.

