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
