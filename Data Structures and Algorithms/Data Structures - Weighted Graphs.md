#dsa #trees #graphs 

Weighted graphs are exactly what they sound like. They are just like un-weighted ones except every edge between nodes has an associated weight that means something about the relationship between the two nodes. The classic example would be to for mapping routes around a city, where the shortest journey between two points is not neccessarily the one that takes the fewest steps along the way, because the time to complete 1 step between nodes is not necessarily the same.

Where the breadth first search algo (see: [[Algorithms - Breadth First Search]]) can find the path that takes the fewest steps between points, Dijkstra's algorithm can find the shortest distance betweent two points when considering the weights of the different paths.