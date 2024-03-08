#dsa #trees #graphs 

Weighted graphs are exactly what they sound like. They are just like un-weighted ones except every edge between nodes has an associated weight that means something about the relationship between the two nodes. The classic example would be to for mapping routes around a city, where the shortest journey between two points is not neccessarily the one that takes the fewest steps along the way, because the time to complete 1 step between nodes is not necessarily the same.

Where the breadth first search algo (see: [[Algorithms - Breadth First Search]]) can find the path that takes the fewest steps between points, Dijkstra's algorithm can find the shortest distance betweent two points when considering the weights of the different paths. See: [[Algorithms - Dijkstra's Algorithm]].

The notes on weighted graphs here are heavily linked with the notes on Dijkstra's algo. If you are reading this you should also read that.
## Terminology
A graph with weights is a weighted graph. A weight is a number associated with an edge between nodes. The shortest path through a weighted graph is the one with the lowest sum of weights, in an un weighted graph it is the one with the fewest steps. 

Graphs can have **cycles**. A cycle is when it is possible to leave node a, and arrive back at node a in a circular fashion.
![[Pasted image 20240308111851.png]]
Following a cycle will never be part of the shortest route through a weighted graph, for obvious reasons. In undirected graphs, every edge is a cycle, because you can go back the way you came.
![[Pasted image 20240308112129.png]]
<span style="color: cyan; font-weight: bold;">Dijkstra's algo for the shortest path in a weighted graph only works for Directed Acyclic Graphs, or DAGs. This is because cycles always add unnecessary total weight to a path and undirected graphs are full of cycles.</span>

## Implementation
When working with un-weighted graphs, both directed and undirected, we have previously implemented them as hash tables with node_id: neighbours_array as the key value pairs. With weighted graphs we need to store not only the neighbours but the weight of the edge that goes to that neighbour. We can do this with nested hashtables. For example the below graph:
![[Pasted image 20240308112815.png]]
Could be implemented like this:
```python
graph = {
	"start": {"a": 6, "b": 2},
	"a": {"fin": 1},
	"b": {"a": 3, "fin": 5},
	"fin": {}
}
```
Similar to what I noted in my notes on unweighted graphs, you could also use a class structure to implement this with methods for adding nodes and edges. 

## Next
That is it for the basics of what a weighted graph is and how to implement it, but next you really should read about Dijkstra's algo here: [[Algorithms - Dijkstra's Algorithm]].

