#dsa #trees #graphs 

## What is a graph
A graph models a set of connections between entities, where entities are represented by nodes, and the connections between nodes are represented by edges. For example, if you had been playing poker with friends for a while and wanted to model who owes money to whom, you could model it with a graph like this (illustration from "Grokking Algorithms"):
![[Pasted image 20240306120512.png]]
In the above image, the peopel involved are the nodes, represented as circles. The lines between them are the edges and represent the relationship of money owed. A node can be connected to many other nodes, and two nodes that are directly connected by an edge are called neighbours.

## Implementing a graph
Because a graph is just a representation of the edges that connect nodes to make them neighbours, we can model this by applying a hashmap. A hashmap maps keys to values, and in this appication we can use it to map a given node to all of it's neighbour nodes. You could implement this very simply by mapping nodes keys to array values, where the array is populated with the keys neighbours:
```python
graph = {}
graph["pointA"] = ["pointB", "pointC", "pointD"]
```

## Directed and undirected graphs
In the above illustration of a graph for poker debts, the edges are represented as arrows. This is a directed graph, in which the edges are one way relationships. Adit has no neighbours, because he has an edge pointing to him but none pointing away from him. In an undirected graph, the edges are all two way relationships.

## Applications
There are a tonne of applications for graphs, because they can map so many different kinds of data. A good place to start is with the breadth first search algorithm, which is a search algo specifically for graphs. See here: [[Algorithms - Breadth First Search]]