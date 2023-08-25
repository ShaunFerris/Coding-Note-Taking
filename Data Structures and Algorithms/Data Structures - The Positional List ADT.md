#dsa #linkedlists #datastructures 

## What it is
**A variation of the linked list ADT that allows for arbitrary insertion, deletion and location of elements.**

Simple abstract data types covered already in this repo like stacks, queues and dequeues only allow update operations that occur at one end or the other of the data structure. The poisitional list is a more general abstraction with wider application. For example while the FIFO behaviour of the queue can model a real world situation like an actual queue at a restaurant, what if a person wants to leave before reaching the front of the queue, or a person holds a space in line for a friend to later join them? <span style="color: cyan; font-weight: bold;">The positional list allows us to model situations like this and many others, by giving us a way to refer to elements that are anywhere in the sequence, and to perfrom arbitrary insertions and deletions.</span>

When working with array based sequences like python lists, we already have indices to describe the position of elements, but this is not a great way for describing the position of an element in a linked list, because we can't locate an element with just an index, we need to follow the pointers instead. 

Furthermore, when we are allowing arbitrary insertions and deletions into the list, the index of a given element will change, so we need an abstraction where  some other mechanism allows us to describe an elements position.