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
#### Connected Components
```
for i to n:
	if(!vis[i])
		traversal(i)
```
#### BFS Traversal
- Breadth First Search technique.
- Next level at equivalent distance, starting from the starting node.
- Use a Queue DS.
- Create a visited array. Mark the starting node as 1. 
- Create an adjacency list.
```cpp
vector<int> bfs(int V, vector<int> adj[])
{
	int vis[n]={0};
	vis[0]=1;
	queue<int> a;
	q.push(0);
	vector<int> bfs;
	while(!q.empty())
	{
		int node=q.front();
		q.pop();
		bfs.push(node);
		for(auto it: adj[node])
		{
			if(!vis[it])
			{
				vis[it]=1;
				q.push(it);
			}
		}
	}
	return bfs;
}
```
- Space complexity: $O(3N)$
- Time complexity: N nodes + total degrees(2E) 

#### DFS Traversal
- ![](attachments/Pasted%20image%2020250731114201.png)
- `1-2-5-6-3-7-8-4`
- Algorithm:
```
1. Store the graph in an adjacency list. 
2. Create a visited array. 
3. Mark starting node as visited. 
4. dfs(starting node)

dfs(node)
{
	vis[node1]
	list.add(node)
	for(auto i: adj[node])
	{
		if(!vis[it])
		{
			dfs(it)
		}
		
	}
}
```
- 
```cpp
void dfs(int node, vector<int> adj[], int vis[], vector<int> &ls)
{
	vis[node]=1;
	ls.push_back(node);
	for(auto it: adj[node])
	{
		if(!vis[it])
		{
			dfs(it,adj,vis,ls);
		}
	}
}
vector<int> dfsOfGraph(int V, vector<int> adj[])
{
	int vis[V]={0};
	int start=0;
	vector<int> ls;
	dfs(start, adj, vis, ls);
	return ls;
}
```
- Space complexity: $O(N)+O(N)+O(N)$
- n nodes visited + visited nodes + stack space
- Time complexity: $O(N)+O(2E)$
