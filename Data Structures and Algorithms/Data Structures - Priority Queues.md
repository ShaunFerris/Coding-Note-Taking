#dsa #datastructures #algorithms #algorithmoptimization #queues

Priority queues are very similar to the standard queue data structure, with the major difference being that each element in the queue has an associated priority, and this dictates the order of operations rather than the sequence in which elements were added to the queue.

There are many different ways to implement priority queues, with various pros and cons, but here we will be looking at a simple, basic implementation just to get the idea - the unsorted priority queue.

## Unsorted priority queue
In this implementation of the priority queue, we don't bother sorting the elements by their priority, instead we search for the element with the highest priority every time we need to remove an element.

This implementation takes advantage of the fact that python allows for comparison between tuples, and does so by comparing the first two elements, then if they are the same type and value, it compares the second two. This implementation is designed to be given tuples where the first element is an integer representing the priority, while the second element is the payload.
```python
class PriorityQueue:
	def __init__(self):
		self.queue = []

	def add(self, item):
		self.queue.append(item)

	def is_empty(self):
		return len(self.queue) == 0

	def remove(self):
		max = 0
		for i in range(len(self.queue)):
			if self.queue[i] > self.queue[max]:
				max = i
		item = self.queue[max]
		del self.queue[max]
		return item
```
Now we can instantiate the queue and add items to it in a random order, then remove them in order or priority:
```python
pq = PriorityQueue()
pq.add((4, "Hello"))
pq.add((3, "this"))
pq.add((8, "is"))
pq.add((7, "a"))
pq.add((2, "priority queue"))

while not pq.is_empty():
	item = pq.remove()
	print(item)

#returns:
(8, "is")
(7, "a")
(4, "Hello")
(3, "this")
(2, "priority queue")
```

Note that if we compare two tuples with the same priority, the comparison will move on to the second elements of the tuples, in which case we need to be aware of how the comparison will play out. For example, if the payloads are user defined classes that do not overload the comparison operator, then the can't be compared and an error will be thrown. In this case it is helpful to add a third parameter to the tuples using the `id()` built-in function so that there can always be a differntiating factor.