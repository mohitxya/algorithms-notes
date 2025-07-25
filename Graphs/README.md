## Table of Contents
- [Introduction](#introduction)

#### Introduction
- Directed and Undirected graphs.
- Circular things: nodes or vertex. Represented by numbers.
- Line: Edge, un-directed edge, directed edge.
- Directed: Can go in one direction only.
**Cycles in a Graph:**
- Binary Tree is a graph.
- Doesn't have to be enclosed.
- cycle: start and end at same node.
- Acyclic: No cycles.
- Degree of node: number of edges attached to it.
- Total degree= sum of all degrees= 2 x edges
**Directed Graphs:**
- Indegree (edges coming in) and Outdegree(edges going out).
**Edge Weight:**
- weight associated with an edge.
- If not specified assume 1.
**Graph Representation:**
- How do we represent graphs in `c++`?
- ![](attachments/Pasted%20image%2020250725222448.png)
- given: n-nodes, m-edges
```
- first line contains n,m.
- m lines representing edges.
- e.g. edge 1 is conncected to 2 and 1.
2 1
1 3
2 4
3 4
2 5
4 5
```
- Two ways to store 1. Matrix 2. List
- Adjacency Matrix : `N x M` space, which is costly.
- ![](attachments/Pasted%20image%2020250725224907.png)
- Adjacency List: takes less space
```
vector<vector<int>> adj[n+1]
- store neighbours of ith node at ith position.
1: [2, 3] 
2: [1, 4, 5] 
3: [1, 4] 
4: [2, 3, 5] 
5: [2, 4]
- The space you take is O(2E)
- If directed u-->v only store v at adj[u].
- directed: O(E) 
- In case of weighted graph instead of writing 1 just write the weight.
- In case of list:
4: {(2,1),(3,4),(5,3)}
- second guy is the edge weight.
```