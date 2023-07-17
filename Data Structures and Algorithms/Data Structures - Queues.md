#dsa #datastructures #algorithms #algorithmoptimization #queues

Queues are very similar to stacks, see: [[Data Structures - Stacks]].

Where stacks are a linear data structure with a **last in first out** operation order, <span style="color: cyan; font-weight: bold;">Queues are linear data structures with a first in first out operation order.</span> They behave exactly like any queue in real life at the supermarket or a theme park.

Queues are commonly useful in computing for coordinating or schedueling processes on the CPU.
In computers, there are many things happening at what seems to be, the same time. For example, you may have music playing as you read an article. There are many things happening here. Your music is being played, your monitor is being updated (refreshed) as you scroll through your article and you are using your mouse wheel to indicate when to scroll. In a simple sense, your CPU can only deal with one thing at a time. Your music application needs time on the CPU to output the next bit of music, as you scroll while this is going on, your mouse driver needs CPU time to tell the computer what to do, then your monitor drivers need CPU time to refresh the pixels on your screen. This all seems to be happening at the same time, except it is not (computers are just so fast it appears it is). What is actually happening in this situation is each program is quickly using the CPU, then jumping back in the queue. This is happening over and over again. A CPU scheduling algorithm deals with which program gets to go next on the CPU and may implement some variation of a queue to handle this. (A priority queue for example). 

They have similar methods for adding and removing except they are `enqueue()` and `dequeue()` respectively.

A simple implementation of the queue data structure in python would be someting like this:
```python
class Queue:
	def __init__(self):
		self.q = []

	def enqueue(self, elem):
		self.q.append(elem)

	def dequeue(self):
		if len(self.q) != 0:
			returnself.q.pop(0)

	def isEmpty(self):
		if len(self.q) > 0:
			return False
		return True
```
Notice how we use the optional argument for the built-in `pop()` method so that it removes the first element instead of the default which removes the last ( which is what stacks do).

For a common application of the queue data structure, see [[Algorithms - Music Player Queue]] or for further reading on queues, see [[Data Structures - Priority Queues]]