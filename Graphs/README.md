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

#### Number of Provinces
```
for(i from 1 to V):
	if(vis[i]==0):
		cnt++
		dfs(i)
```
- Change adjacency matrix to a list.
```cpp
class Solution {
private:

    void dfs(int node, vector<vector<int>> adj, vector<int> &vis, vector<int> &ls)
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
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int v=isConnected.size();
        vector<int> vis(v,0);
        vector<int> ls;
        vector<vector<int>> adj(v);
        for(int i=0; i<v; i++)
        {
            for(int j=0; j<v; j++)
            {
                if(isConnected[i][j]==1 && i!=j)
                {
                    adj[j].push_back(i);
                    adj[i].push_back(j);
                }
            }
        }
        int cnt=0;
        for(int i=0; i<v; i++)
        {
            if(vis[i]==0)
            {
                cnt++;
                dfs(i,adj,vis,ls);
            }
        }  
        return cnt;
    }
};
```
- space complexity: visited array + stack space
- time complexity:$O(N)+O(V+2E)$

#### Rotten Oranges
- 0 means empty, 1 means fresh, 2 means rotten.
- We need level wise traversal, BFS.
- Use a queue structure and maintain a rotten matrix to mark rotten oranges.
- After each step increase the time period associated with each square.`(x,y,t)`
```cpp
int orangesRotting(vector<vector<int>>& grid)
{
	int n=grid.size();
	int m=grid[0].size();

	queue<pair<pair<int,int>,int>> q;
	vector<vector<vis>> vis;
	for(int i=0; i<n; i++)
	{
		for(int j=0; j<m; j++)
		{
			if(grid[i][j]==2)
			{
				q.push({{i,j},0});
				vis[i][j]=2;
			}
		}
	}

	int tm=0;
	int drow[]={-1,0,+1,0};
	int dcol[]={0,1,0,-1};
	while(!q.empty())
	{
		int r=q.front().first.first;
		int c=q.front().first.second;
		int t=q.front().second;
		tm=max(tm,t);
		q.pop();
		for(int i=0; i<4; i++)
		{
			int nrow=r + drow[i];
			int ncol=c+ dcol[i];
			if(nrow>=0 && nrow<n && ncol>=0 && ncol<m && vis[nrow][ncol]!=2 && grid[nrow][ncol]==1)
			{
				q.push({{nrow,ncol},t+1});
				vis[nrow][ncol]=1;
			}
		}
	}

	for(int i=0; i<n; i++)
	{
		for(int j=0; j<m; j++)
		{
			if(grid[i][j]!=2 && grid[i][j]==1) return -1;
		}
	}
	
}
```
#### Flood Fill
- We'll be using DFS.
```cpp
private:
	void dfs(int row, int col, vector<vector<int>>& ans,vector<vector<int>>& image, int newColor,int delRow[],int delCol[])
	{
		ans[row][col]=newColor;
		for(int i=0; i<4; i++)
		{
			int nrow=row+delRow[i];
			int ncol=col+delCol[i]; 
			if(nrow>=0 && nrow<n && ncol>=0 && ncol<m && image[nrow][ncol]==iniColor && ans[nrow][ncol]!=new)
			{
				dfs(nrow,ncol,ans,image,newColor,delRow,delCol);
			}
		}
		
	}
public:
vector<vector<int>> floodFill(vector<vector<int>& image, int sr, int sc, int newColor){
int iniColor=image[sr][sc];
vector<vector<int>> ans=image;
int delRow[]={-1,0,+1,0};
int delCol[]={0,+1,0,-1};
dfs(sr,sc,ans,image,newColor,delRow, delCol);
}
```
- At worst case N x M nodes.
- For every node 4 times.
- Time complexity: $O(N*M)$
- Space complexity: $O(N*M)+O(N*M)$