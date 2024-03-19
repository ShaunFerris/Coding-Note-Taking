#dsa #trees #graphs 

As briefly covered in the note introducing the concept of weighted graphs, here: [[Data Structures - Weighted Graphs]], Dijkstra's algo is used to find the shortest path from one node to another, factoring is the weights of the edges.

There are four steps to the algo:
1. Find the cheapest node that you can go directly to from the starting point
2. Update the weight costs of the neighbours of this node
3. Repeat these steps until you have touched every node in the graph
4. Calculate the final path

## Trading example
This example is from "Grokking Algorithms". 

You are trying to trade a book for a piano. The below graph represents trades that have been offered in your friend network, the weights represent how much additional money  needs to be added to the item trade to make it agreed upon as fair. <span style="color: cyan; font-weight: bold;">How are you going to figure out how to trade your book for the piano whilst adding the smalles amount of additional cash?</span>
![[Pasted image 20240310100722.png]]
Using the four steps to Dijkstra's algo, we start by setting up a table to store the cost of each node. The cost is how expensive it is to get to, and the nodes we have not reached yet are marked as infinitely expensive, until we reach them and update. The table will be updated as we go, and should also have a parent column to calculate the final path.
![[Pasted image 20240310100626.png]]
![[Pasted image 20240310100637.png]]
**Step one:** Find the cheapest node. Given that the start node for this example is the book, the poster is the cheapest node because it costs $0. The key here is that the cheapest node we can reach from the start cannot be reached for cheaper by any other path.

**Step two:** Figure out the cost to get to the neighbours of the cheapest node ( the poster).
![[Pasted image 20240314095537.png]]
Now you have the costs to get to the guitar and drums from the start through the cheapest node, the poster. You set poster as their parents and set the costs. 

NOW YOU REPEAT THE STEPS. Step one is find the cheapest node, the next cheapest node from the start is the LP. Step two is to find the cost of the cheapest nodes neighbours, in this case bass guitar and drums are the neighbours again. Update their costs from the start through the LP and they are cheaper, so update their parents in the table too. 

This is the general progression for how the algorithm works. I am not going to explain any further cos this is getting tedious.

## Negative weight edges
In the above trading example, the edge weights represented how much money you would have to add to a given trade to make the person accept it. What if they offered to give YOU money as part of the trade? We could represent this with a negative weighting value. 

<span style="color: cyan; font-weight: bold;">You cannot use Dijkstras algorithm on graphs with negative weights, they break the algo.</span>

The idea is that the algo **processes** nodes in order of cheapest to most costly and **looks** at the costs of children to the node it is processing. When you process a node, the algo relies on there being no cheaper way to get to it, because you process in cheapest order. But negative weights can break this and casuse you to find a cheaper route to the cheapest node. 

To find the shortest path through a weighted graph that includes negative weights you need to use the Bellman-Ford algorithm, which I have not covered yet (if you're reading this maybe search the repo incase I have now covered it and not updated here.)

## Implementation
