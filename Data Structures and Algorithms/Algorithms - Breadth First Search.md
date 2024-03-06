#dsa #trees #graphs 

Breadth first search (BFS) is a search algorithm specifically for operating on graph structures. It can help answer two distinct questions about a graph:
1. Is there a path from node A to node B?
2. If there is, what is the shortest path?

Say you are searching through your network of friends for someone who can sell you drugs. The first step is to check all of the friends you are connected to directly. If none of them can sell you drugs, your next step might be to expand your search to their friends, so every time you check with a friend, if they say no, you then add their friends to the search list, so that once you have asked all of your friends, you then start asking all the friends of friends you added. This is traversing the graph by **breadth first**. By adding all of your neighbours neighbours to the search list, you will search the whole network and eventually answer the question of if there is a path to the target node, in this case someone who has drugs to sell.

## Finding the shortest path
We have already covered how bfs answers the first question above, it checks all neighbours of the start node, and if they are not hits adds their neighbours to the "to search" list, after the neighbours of the start node. But how do we find the shortest path once we have confirmed that there is a path from start to target? <span style="color: cyan; font-weight: bold;">Bfs checks all first degree connections of the starting node before checking the second or third or so on connections for exactly this reason, it is prioritising the shortest path. It will always find the shortest path before any of the longer paths.</span>

This of course only works if you search nodes in the order they are added to the search list, so that a second order connection node is never searched before a first order node. This is achieved using a queue, because it is FIFO by nature. See: [[Data Structures - Queues]]

## Implementing BFS
To recap, here is how the algorithm will work, using the example from Grokking Algorithms of searching through a network of friends for a mango seller (I changed this to drugs in my note taking from the book above):
![[Pasted image 20240306125536.png]]
If the friend network we are searching is represented by this graph:
![[Pasted image 20240306130033.png]]
We can represent that in code by setting up a dictionary like this:
```python
graph = {}
graph[“you”] = [“alice”, “bob”, “claire”]
graph[“bob”] = [“anuj”, “peggy”]
graph[“alice”] = [“peggy”]
graph[“claire”] = [“thom”, “jonny”]
graph[“anuj”] = []
graph[“peggy”] = []
graph[“thom”] = []
graph[“jonny”] = []

print(graph) #{'you': ['alice', 'bob', 'claire'], 'bob': ['anuj', 'peggy'], 'alice': ['peggy'], 'claire': ['thom', 'jonny'], 'anuj': [], 'peggy': [], 'thom': [], 'jonny': []}
```

The first step for our BFS implementation is to setup a queue. You can use the dequeue class from the collections module in the standard library to achieve this:
```python
from collections import deque

search_queue = deque()
search_queue += graph["you"]
```
 This will add your list of first order connections, `['alice', 'bob', 'claire']` to the queue.

Next we can add the main loop to our algo, I've also included a function to check if a node is a mango seller:
```python
def is_seller(name):
    return name[-1] == "m"


def bfs(graph, start):
    search_queue = deque()
    search_queue += graph[start]

    while search_queue:
        current = search_queue.popleft()
        if is_seller(current):
            print(f"{current} is the target")
            return True
        else:
            search_queue += graph[current]
    return False


print(bfs(graph, "you"))
```
This works, and finds the mango seller Thom, but there is a small problem with the implementation. Alice and Bob share a friend, Peggy, which means that Peggy will be added to the queue twice and you will be doing repeated work by checking her twice. This can also lead to an infinite loop, so to avoid this, you need to add nodes to a list of already searched nodes and check this before checking any given node or adding its neighbours to the search queue.

The fixed function looks like this:
```python
def bfs(graph, start):
    search_queue = deque()
    search_queue += graph[start]
    checked = []
    while search_queue:
        current = search_queue.popleft()
        if not current in checked:
            if is_seller(current):
                print(f"{current} is the target")
                return True
            else:
                search_queue += graph[current]
                checked.append(current)
    return False
```

## Runtime
If you search the entire network for the target vertex (nodes are also called vertices), that means that you will follow every edge in the network, so the running time will be at least O(n) where n is the number of edges. You also add every person to the queue. Enqueueing a person takes constant time, but since you're doing it for every node it will be O(n) where n is the number of nodes. <span style="color: cyan; font-weight: bold;">Thus, the total runtime will be O(V + E) where V is vertices and E is edges.</span>

