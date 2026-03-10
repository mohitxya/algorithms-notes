## Table of Contents
- [Introduction](#introduction)
- [Connected Components](#connected-components)
- [BFS Traversal](#bfs-traversal)
- [DFS Traversal](#dfs-traversal)
- [Number of Provinces](#number-of-provinces)
- [Rotten Oranges](#rotten-oranges)
- [Flood Fill](#flood-fill)
- [Cycle Detection in Undirected Graph](#cycle-detection-in-undirected-graph)
- [0/1 Matrix](#01-matrix)
- [Surrounded Regions](#surrounded-regions)
- [Number of Enclaves](#number-of-enclaves)
- [Word Ladder I](#word-ladder-i)
- [Word Ladder II](#word-ladder-ii)
- [Number of Distinct Islands](#number-of-distinct-islands)
- [Bipartite Graph](#bipartite-graph)
- [Course Schedule II](#course-schedule-ii)
- [Topological Sort](#topological-sort)
- [Kahn's Algorithm](#kahns-algorithm)
- [Cycle Detection (BFS)](#cycle-detection-bfs)
- [Safe States](#safe-states)
- [Alien Dictionary](#alien-dictionary)
- [Shortest Path in Undirected Graph](#shortest-path-in-undirected-graph)  
- [Shortest Path in DAG](#shortest-path-in-dag)   
- [Dijkstra's Algorithm](#dijkstras-algorithm)  
- [Print Shortest Path](#print-shortest-path)   
- [Shortest Distance in a Binary Maze](#shortest-distance-in-a-binary-maze)   
- [Shortest Path in Binary Matrix](#shortest-path-in-binary-matrix)    
- [Path With Minimum Effort](#path-with-minimum-effort)
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
**Convert to adjacency list**
```cpp
vector<vector<int>> adj(V);
for (auto &e : edges) {
    int u = e[0], v = e[1];
    adj[u].push_back(v);
    adj[v].push_back(u); // undirected
}
```
#### Connected Components
- Graph could contain several components. 
- Size of visited array should be 1 greater than the number of vertices. 
```
for i to n:
	if(!vis[i])
		traversal(i)
```
#### BFS Traversal
- Breadth First Search technique.
- Next level at equivalent distance, starting from the starting node.
- Use a Queue DS. First in First out. 
- Create a visited array. Mark the starting node as 1. 
- The queue contains the starting node initially. 
- Pop the element of the queue and check it's neighbors. 
- If visited set it as 1. 
```cpp
vector<int> bfsOfGraph(int V, vector<int> adj[])
{
	int vis[n]={0};
	vis[0]=1;
	queue<int> q;
	q.push(0);
	vector<int> bfs;
	while(!q.empty())
	{
		int node=q.front();
		q.pop();
		// take out the node. 
		bfs.push(node);
		// go through all the neigbours of the selected element. 
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
	vis[node]=1
	list.add(node)
	// Now you're gonna travel the neighbours. 
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
- Consider the starting pixel, plus any pixels connected to it of the same color. 
- Replace the color of all the aforementioned pixels with the `newColor`. 
```cpp
private:
	void dfs(int row, int col, vector<vector<int>>& ans,vector<vector<int>>& image, int newColor,int delRow[],int delCol[])
	{
		ans[row][col]=newColor;
		for(int i=0; i<4; i++)
		{
			int nrow=row+delRow[i];
			int ncol=col+delCol[i]; 
			if(nrow>=0 && nrow<n && ncol>=0 && ncol<m && image[nrow][ncol]==iniColor && ans[nrow][ncol]!=newColor)
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
#### Cycle Detection in Undirected Graph
BFS Traversal:
- For the graph create an adjacency list.
- **BFS traversal** goes level-wise.
- Maintain a Q DS and a visited array.
- Go to the adjacency list and add node to Q along with parent node. 
- For connected components: We would need to call the function for each component.
```cpp
class Solution
{
	private: 
	bool detect(int src, vector<int> adj[], int vis[])
	{
		vis[src]=1;
		queue<pair<int,int>> q;
		q.push({src,-1});
		while(!q.empty())
		{
			int node=q.front().first;
			int parent=q.front().second;
			q.pop();
			
			for(auto adjacentNode: adj[node])
			{
				if(!vis[adjacentNode])
				{
					vis[adjacentNode]=1;
					q.push({adjacentNode,node});
				}
				else if(parent!=adjacentNode)
				{
					return true;
				}
			}
		}
		return false;
	}
	public: 
	bool isCycle(int V, vector<int> adj[])
	{
		int vis[V]={0};
		for(int i=0; i<V; i++)
		{
			if(!vis[i])
			{
				if(detect(i,adj,vis)) return true;
			}
		}
		return false;
	}
}
```
- Time complexity: Summation of all adjacent nodes is the summation of all degrees. 
- $O(N+2E)$ 
- Space complexity: $O(N)$

DFS Traversal:
- If we reach any node which has been previously visited we call it a cycle.
- Initialize a visited array and mark everything as zero apart from the source node. 
- Carry the parent as well.
- Parent will be -1 for the initial node.
- If you came from 1 and it is visited, we can't count it as a cycle. 
- If any one function call return `true`, every other call should also return a `true`.
- Visited and not a parent: Cycle.
```
dfs(node, parent)
{
	vis[node]=1
	for(auto it: adj[node])
	{
		if(vis[it]==0)
		{
			if(dfs(i,node)==true)
			{
				return true;
			}
		}
		else if(it!=parent)
		{
			return true;
		}
	}
	return false;
}

main()
{
	for(i: 1 to V)
	{
		if(!visited)
		{
			dfs(i,-1)
		}
	}
}
```
- Space Complexity: $O(N)+O(N= O(N))$ (Recursive stack space + Visited array)
- Time Complexity: $O(N+2E)+O(N)$ (DFS traversal + for loop)
#### 0/1 Matrix
- We'll use BFS since it goes level wise. 
- Create a visited matrix and distance matrix. 
- Create a queue DS: 
	- Add `(1,1),0`: co-ordinate and distance.
	- Add `(2,2),0` and so on.
	- Mark their corresponding visited values as 1. 
- Update the distance matrix for `(1,1)`.
- For every queue element traverse in all 4 directions and add the elements with their distances to the queue. 
- 4 directions: row->`{-1,0,+1,0}`, col->`{0,+1,0,-1}`
```cpp
nearest(vector<vector<int>>grid)
{
	int n=grid.size();
	int m=grid[0].size();
	
	vector<vector<int>> vis(n,vector<int> (m,0));
	vector<vector<int>> dist(n,vector<int> (m,0));
	queue<pair<pair<int,int>,int>> q;
	for(int i=0; i<n; i++)
	{
		for(int j=0; j<m; j++)
		{
			if(grid[i][j]==1)
			{
				q.push({{i,j},0});
				vis[i][j]=1;
			}
			else
			{
				vis[i][j]=0;
			}
		}
	}	
	int delrow[]={-1,0,+1,0};
	int delcol[]={0,+1,0,-1};
	// n x m x4
	while(!q.empty())
	{
		int row=q.front().first.first;
		int col=q.front().first.second;
		int steps=q.front().second;
		q.pop();
		dist[row][col]=steps;
		
		for(int i=0; i<4; i++)
		{
			int nrow=row+delrow[i];
			int ncol=col+delrow[i];
			
			if(nrow>=0 && nrow<n && ncol>=0 && ncol<m &&vis[nrow][ncol]==0)
			{
				vis[nrow][ncol]=1;
				q.push({nrow,ncol},steps+1)
				
			}
		}
	}
	return dist;
}
```
#### Surrounded Regions
- Replace all 'O' with 'X' that are surrounded by 'X'. 
- If 'O' is connected to a boundary, it can't be surrounded. 
- Algorithm: 
	- Start from the boundary zeros and mark them. 
	- Convert the remaining to 'X'.
```cpp
class Solution
{
private: 
		void dfs(int row, int col, vector<vector<int>> &vis, vector<vector<char>> &mat, int delrow[], int delcol[])
	{
		vis[row][col]=1;
		int n=mat.size();
		int m=mat[0].size();
		// check for top, right, bottom, left. 
		for(int i=0;j i<4; i++)
		{
			int nrow = row + delrow[i];
			int ncol = col + delcol[i];
			if(nrow>=0 && nrow<n && ncol>=0 && ncol<m && !vis[nrow][ncol] && mat[nrow][ncol]=='0')
			{
				dfs(nrow, col, vis, mat, delrow, delcol);
			}
		}
		
	}
public: 
	vector<vector<char>> fill(int n, int m, vector<vector<char>> mat)
	{
		int n=mat.size();
		int m=mat[0].size();
		int delrow[]={-1,0,+1,0};
		int delcol[]={0,1,0,-1};
		vector<vector<int>> vis(n, vector<int>(m,0));
		
		for(int j=0; j<m; j++)
		{
			// first row
			if(!vis[0][j] && mat[0][j]=='O')
			{
				dfs(0,j,vis,mat);
			}
			// last row
			if(!vis[n-1][j] && mat[n-1][j]=='O')
			{
				dfs(n-1,j,vis,mat);
			}
		}
		
		for(int i=0; i<n; i++)
		{
			// first column
			if(!vis[i][0] && mat[i][0]=='O')
			{
				dfs(i,0,vis,mat);
			}
			// last column
			if(!vis[i][m-1] && mat[i][m-1]=='O')
			{
				dfs(i,m-1,vis,mat);
			}
		}
		
		for(int i=0; i<n; i++)
		{
			for(int j=0; j<m; j++)
			{
				if(!vis[i][j] && mat[i][j]=='O') mat[i][j]='X';
			}
		}
		
		return mat;
		
	}
}
```
#### Number of Enclaves
- A move consists of walking from one land cell to another adjacent land cell or walking off the boundary of the grid. 
- count the number of enclaves (you can't move out from).
- ![[attachments/Pasted image 20260109101642.png|100]]
- In the above example there are zero enclaves. 
- Something connected to boundary 1's will never be the answer. 
- Approach 1: take boundary 1's and flood fill with 0's. 
```cpp
class Solution {
private:
    void dfs(int row, int col,vector<vector<int>>& ans,vector<vector<int>>& image, int iniColor, int newColor,int delRow[],int delCol[])

    {
        ans[row][col]=newColor;
        int n=image.size();
        int m=image[0].size();
        for(int i=0; i<4; i++)
        {
            int nrow = row + delRow[i];
            int ncol = col + delCol[i];
            if(nrow>=0 && nrow < n && ncol>=0 && ncol<m && image[nrow][ncol]==iniColor && ans[nrow][ncol]!=newColor)
            {
                dfs(nrow, ncol, ans, image,iniColor,  newColor, delRow, delCol);
            }
        }
    }
public:
    int numEnclaves(vector<vector<int>>& grid) {
        int n=grid.size();
        int m=grid[0].size();
        vector<vector<int>> ans=grid;
        int delrow[]={-1,0,+1,0};
        int delcol[]={0,1,0,-1};

        for(int j=0; j<m; j++)
        {
            if(grid[0][j]==1)
            {
                dfs(0,j,ans,grid,1,0,delrow,delcol);
            }
            if(grid[n-1][j]==1)
            {
                dfs(n-1,j,ans,grid,1,0,delrow,delcol);
            }
        }
        for(int i=0;i<n; i++)
        {
            if(grid[i][0]==1)
            {
                dfs(i,0,ans,grid,1,0,delrow,delcol);
            }
            if(grid[i][m-1]==1)
            {
                dfs(i,m-1,ans,grid,1,0,delrow,delcol);
            }
        }
        int count=0;
        for(int i=0; i<n; i++)
        {
            for(int j=0; j<m; j++)
            {
                if(ans[i][j]==1)
                {
                    count++;
                }
            }
        }
        return count;
    }
};
```
#### Word Ladder I
- Given two words `beginWord` and `endWord`, and a dictionary `wordList`, return the number of words in the shortest transformation sequence.
- Every adjacent pair of words differs by a single letter. 
- `beginWord` doesn't need to be in `wordList`.   
- Initialize a queue with a pair {`startWord`, 1} representing the word and its current transformation steps. 
- `wordList` to `unordered_set` for `O(1)` lookups. 
- Stop at the first appearance of the `endWord`.
```cpp
class Solution
{
public:
	int wordLadderLength(string startWord, string targetWord, vector<string> &wordList)
	{
		queue<pair<string, int>> q;
		q.push({startWord, 1});
		
		unordered_set<string> st(wordList.begin(), wordList.end());
		st.erase(startWord);
		
		while(!q.empty()){
			string word = q.front().first;
			int steps = q.front().second; 
			q.pop();
			
			if (word==targetWord) return steps; 
			
			for(int i=0; i<word.size(); i++)
			{
				char original = word[i];
				for(char ch='a'; ch<='z'; ch++)
				{
					word[i]=ch;
					if(st.find(word)!=st.end())
					{
						st.erase(word);
						q.push({word,steps+1});
					}
				}
				word[i]=original;
				// revert back the character we changed. 
			}
		}
		return 0;
	}
}
```
#### Word Ladder II
- Don't erase when you're working on the same level.
- In the queue store complete lists. 
```cpp
class Solution
{
public: 
	vector<vector<string>> findSequences(string beginWord, string endWord, vector<string> &wordList)
	{
		unordered_set<string> st(wordList.begin(), wordList.end());
		
		queue<vector<string>> q;
		q.push({beginWord});
		
		// vector defined to store words being used currently. 
		vector<string> usedOnLevel;
		usedOnLevel.push_back(beginWord);
		int level = 0;
		vector<vector<string>> ans;
		while(!q.empty())
		{
			vector<string> vec = q.front();
			q.pop();
			if(vec.size() > level)
			{
				level++;
				for(auto it: usedOnLevel)
				{
					st.erase(it);
				}
				usedOnLevel.clear();
			}
			string word = vec.back();
			if(word == endWord)
			{
				if(ans.size()==0)
				{
					ans.push_back(vec);
				}
				else if(ans[0].size()== vec.size())
				{
					ans.push_back(vec);
				}
			}
			for(int i=0; i<word.size(); i++)
			{
				char original = word[i];
				for(char c='a'; c<='z';c++)
				{
					word[i]=c;
					if(st.count(word)>0)
					{
						vec.push_back(word);
						q.push_back(vec);
						usedOnLevel.push_back(word);  
						vec.pop_back();
					}
				}
				word[i]=original;
			}
		}
		return ans;
	}
}
```
- Optimized approach for Leetcode.
	- Add words to queue. 
	- Remove from queue and add to map along with the level.
	- Backtrack in the map from end to beginning.
	- This saves the time.
```cpp
class Solution
{
	unordered_map<string, int> mpp;
	vector<vector<string>> ans;
private:
	void dfs(string word, vector<string> &seq)
	{
		if(word==b)
		{
			reverse(seq.begin(), seq.end());
			ans.push_back(seq);
			reverse(seq.begin(), seq.end());
			return;
		}
		int steps=mpp[word];
		for(int i=0; i<sz; i++)
		{
			char original = word[i];
			for(char ch='a'; ch<='z'; ch++)
			{
				word[i]=ch;
				if(mpp.find(word)!= mpp.end() && mpp[word]+1 == steps)
				{
					seq.push_back(word);
					dfs(word, seq);
					seq.pop_back();
				}
			}
			word[i]=original;
		}
	}
public:
	vector<vector<string>> findLadders(string beginWord, string endWord, vector<string> &wordList)
	{ 
		unordered_set<string> st(wordList.begin(), wordList.end());
		queue<string> q;
		b=beginWord;
		q.push({beginWord});
		mpp[beginWord]=1;
		int size=beginWord.size();
		st.erase(beginWord);
		while(!q.empty())
		{
			string word=q.front();
			int steps = mpp[word];
			q.pop();
			if(word == endWord)
			{
				break;
			}
			for(int i=0; i<sz; i++)
			{
				char original=word[i];
				for(char ch='a'; ch<='z'; ch++)
				{
					word[i]=ch;
					if(st.count(word))
					{
						q.push(word);
						st.erase(word);
						mpp[word]= steps + 1;
					}
				}
				word[i]=original;
			}
		}
		if(mpp.find(endWord) != mpp.end())
		{
			vector<string> seq;
			seq.push_back(endWord);
			dfs(endWord, seq);
		}
		return ans;
	}
}
```
#### Number of Distinct Islands
- Find distinct islands. 
- create an array for directions: Left, Right, Down, Up. 
- Also, count the 'B' backtracking step to distinguish between similar. 
```cpp
class Solution {
private:
    void dfs(int row, int col, vector<vector<int>> &grid, vector<vector<int>> &vis, int delrow[], int delcol[],char dir[], string &path)
    {
        vis[row][col]=1;
        int n=grid.size();
        int m=grid[0].size();
        for(int i=0; i<4; i++)
        {
            int nrow=row+delrow[i];
            int ncol=col+delcol[i];
            if(nrow>=0 && nrow < n && ncol>=0 && ncol<m && vis[nrow][ncol]!=1 && grid[nrow][ncol]==1){
                path.push_back(dir[i]);   
                dfs(nrow, ncol, grid, vis, delrow, delcol, dir, path);
                path.push_back('B');      
            }
        }
    }
  public:
    int countDistinctIslands(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();

        vector<vector<int>> vis(n, vector<int>(m, 0));
        set<string> st;

        int delrow[] = {0, 0, 1, -1};  
        int delcol[] = {-1, 1, 0, 0};
        char dir[]   = {'L','R','D','U'};

        for(int i = 0; i < n; i++)
        {
            for(int j = 0; j < m; j++)
            {
                if(!vis[i][j] && grid[i][j] == 1)
                {
                    string path = "S"; 
                    dfs(i, j, grid, vis, delrow, delcol, dir, path);
                    st.insert(path);
                }
            }
        }
        return st.size();
        
    }
};
```
#### Bipartite Graph
- Any graph containing odd length cycle can't be bipartite. 
- We use DFS. 
- We take a colour array. 
- No adjacent nodes should have same colour. 
```cpp
class Solution {
private:
    bool dfs(int node, int col, vector<int>& color, vector<vector<int>>& adj) {
        color[node] = col;

        for (auto it : adj[node]) {
            if (color[it] == -1) {
                // uncolored → color with opposite color
                if (!dfs(it, !col, color, adj)) 
                    return false;
            }
            else if (color[it] == col) {
                // same color on adjacent nodes → not bipartite
                return false;
            }
        }
        return true;
    }

public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> color(n, -1);

        // handle disconnected graph
        for (int i = 0; i < n; i++) {
            if (color[i] == -1) {
                if (!dfs(i, 0, color, graph))
                    return false;
            }
        }
        return true;
    }
};

```
#### Course Schedule II
- Return ordering of courses you should take to finish all courses. 
- If many valid answers, return any of them. 
- In a directed graph, our old cycle check algorithm will fail. 
- On the same path the node has to be visited again. 
- visited array, path visited array. 
- both visited and path visited should be 1 for cycle. 
- Loop detection in a directed graph: 
```cpp
class Solution
{
private:
bool dfsCheck(int node, vector<int> adj[], int vis[], int pathVis[])
{
	vis[node]=1;
	pathVis[node]=1;
	// traverse for adjacent nodes
	for(auto it: adj[node])
	{
		if(!vis[it])
		{
			if(dfsCheck(it, adj, vis, pathVis)==true) return true;
		}
		else if(pathVis[it])
		{
			return true; 
		}
	}
	pathVis[node]=0;
	return false;
}
public:
bool isCyclic(int V, vector<int> adj[])
{
	int vis[V]={0};
	int pathVis[V]={0};
	
	for(int i=0; i<V; i++)
	{
		if(!vis[i])
		{
			if(dfsCheck(i, adj, vis, pathVis)==true) return true;
		}
	}
	return false;
}
}
```
- Leetcode problem solution: 
```cpp
class Solution {
private:
    bool dfsCheck(int node, vector<vector<int>> &adj,
                  vector<int> &vis, vector<int> &pathVis,
                  vector<int> &topo)
    {
        vis[node] = 1;
        pathVis[node] = 1;

        for (auto it : adj[node])
        {
            if (!vis[it])
            {
                if (dfsCheck(it, adj, vis, pathVis, topo))
                // if in future it ends up becoming true. 
                    return true;
            }
            else if (pathVis[it])
            {
                return true; // cycle detected
            }
        }

        pathVis[node] = 0;
        topo.push_back(node); // backtracking
        return false;
    }

public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites)
    {
        vector<vector<int>> adj(numCourses);

        for (auto it : prerequisites)
        {
            int course = it[0];
            int prereq = it[1];
            adj[prereq].push_back(course);
        }

        vector<int> vis(numCourses, 0);
        vector<int> pathVis(numCourses, 0);
        vector<int> topo;

        for (int i = 0; i < numCourses; i++)
        {
            if (!vis[i])
            {
                if (dfsCheck(i, adj, vis, pathVis, topo))
                    return {}; // cycle found
            }
        }
        reverse(topo.begin(), topo.end());
        return topo;
    }
};
```
- If a cycle is found it is not possible to complete all the courses. 
#### Topological Sort
- Only works on Directed Acyclic graph. 
- Linear ordering where for every directed edge from vertex u to v, u always comes before v in the list, showing dependency. 
- Declare a visited array and a stack.
```cpp
class Solution
{
	private:
	void dfs(int node, int vis[], stack<int> &st, vector<int> adj[])
	{
		vis[node]=1;
		for(auto it: adj[node])
		{
			if(!vis[it]) dfs(it, vis, st, adj);
		}
		st.push(node);
	}
	public: 
	vector<int> topoSort(int V, vector<int> adj[])
	{
		int vis[V]={0};
		stack<int> st;
		for(int i=0; i<V; i++)
		{
			if(!vis[i]){
				dfs(i,vis,st);
			}
		}
		vector<int> ans;
		while(!st.empty())
		{
			ans.push_back(st.top());
			st.pop()
		}
		return ans;
	}
}
```
#### Kahn's Algorithm
- Indegree: Number of incoming edges to a node. 
- If indegree 0, can be placed at the starting. 
- Add them to a queue. 
- Take elements from queue and remove edges. Basically, reduce indegree of adjacent nodes. 
- Check for indegree 0 after this step. 
```cpp
class Solution
{
	public:
	vector<int> topoSort(int V, vector<int> adj[])
	{
		int indegree[V] = {0};
		for(int i=0; i<V; i++)
		{
			 for(auto it: adj[i])
			 {
				 indegree[it]++;
			 }
		}
		
		queue<int> q;
		for(int i=0; i<V; i++)
		{
			if(indegree[i]==0)
			{
				q.push(i);
			}
		}
		
		vector<int> topo;
		while(!q.empty())
		{
			int node = q.front();
			q.pop();
			topo.push_back(node);
			// remove node from indegree
			for(auto it: adj[node])
			{
				indegree[it]--;
				if(indegree[it]==0) q.push(it);
			}
			
		}
		
		return topo;
	}
}
```
#### Cycle Detection BFS
 - Create a queue data structure. 
 - If topo sort contains N elements then it's a DAG. 
 - Otherwise it contains a cycle. 
 ```cpp
 class Solution
{
	public:
	vector<int> topoSort(int V, vector<int> adj[])
	{
		int indegree[V] = {0};
		for(int i=0; i<V; i++)
		{
			 for(auto it: adj[i])
			 {
				 indegree[it]++;
			 }
		}
		
		queue<int> q;
		for(int i=0; i<V; i++)
		{
			if(indegree[i]==0)
			{
				q.push(i);
			}
		}
		
		int cnt=0;
		while(!q.empty())
		{
			int node = q.front();
			q.pop();
			cnt++;
			// remove node from indegree
			for(auto it: adj[node])
			{
				indegree[it]--;
				if(indegree[it]==0) q.push(it);
			}
			
		}
		
		if(cnt==N) return false;
		return true;
	}
};
 ```
- Can use this to solve "Course Schedule I & II" problem. 
#### Safe States
- Topological sort works on indegree logic. 
- But terminal nodes are detected using outdegree. 
- So we reverse all the edges in the graph.
- Just do reverse process then sort the safe nodes list. 
```cpp
class Solution
{
public: 
	vector<int> eventualSafeNodes(int V, vector<int> adj[])
	{
		vector<int> adjRev[V]; 
		vector<vector<int>> adjRev(V);
		for(int i=0; i<V; i++)
		{
			// i--> it
			// it--> i
			for(auto it: adj[i])
			{
				adj[it].push_back(i);
				indegree[i]++;
			}
		}
		queue<int> q;
		vector<int> safeNodes;
		for(int i=0; i<V; i++)
		{
			if(indegree[i]==0)
			{
				q.push(i);
			}
		}
		while(!q.empty())
		{
			int node = q.front();
			q.pop();
			safeNodes.push_back(node); 
			for(auto it: adjRev[node])
			{
				indegree[it]--;
				if(indegree[it]==0) q.push(it);
			}
		}
		sort(safeNodes.begin(), safeNodes.end());
		return safeNodes;
	}
}
```
#### Alien Dictionary
- We are given a vector of strings sorted in lexicographically correct order. 
-  return a string containing unique characters sorted in lexicographical order. 
- Apply topological sort. 
- Algorithm:
	- Adjacency list, indegree array and a queue. 
	- loop through all words and compare adjacent pairs. 
```cpp
class Solution{
private:
	vector<int> topoSort(int V, vector<int> adj[])
	{
		int indegree[V] = {0};
		for(int i=0; i<V; i++)
		{
			 for(auto it: adj[i])
			 {
				 indegree[it]++;
			 }
		}
		
		queue<int> q;
		for(int i=0; i<V; i++)
		{
			if(indegree[i]==0)
			{
				q.push(i);
			}
		}
		
		vector<int> topo;
		while(!q.empty())
		{
			int node = q.front();
			q.pop();
			topo.push_back(node);
			// remove node from indegree
			for(auto it: adj[node])
			{
				indegree[it]--;
				if(indegree[it]==0) q.push(it);
			}
			
		}
		
		return topo;
	}
public:
	string findOrder(string dict[], int N, int K)
	{
		 // Step 1: Collect unique characters
        set<char> unique;
        for (auto &word : words)
            for (char c : word)
                unique.insert(c);

        // Step 2: Map characters to indices
        unordered_map<char, int> mp;
        int idx = 0;
        for (char c : unique)
            mp[c] = idx++;

        int k = unique.size();
        int n = words.size();

        // Step 3: Build adjacency list
        vector<vector<int>> adj(k);

        for (int i = 0; i < n - 1; i++)
        {
            string s1 = words[i];
            string s2 = words[i + 1];
            int len = min(s1.size(), s2.size());

            bool found = false;
            for (int j = 0; j < len; j++)
            {
                if (s1[j] != s2[j])
                {
                    adj[mp[s1[j]]].push_back(mp[s2[j]]);
                    found = true;
                    break;
                }
            }

            // Invalid case: prefix problem
            if (!found && s1.size() > s2.size())
                return "";
        }

        // Step 4: Topological Sort
        vector<int> topo = topoSort(k, adj);

        // Step 5: Cycle detection
        if (topo.size() != k)
            return "";

        // Step 6: Convert indices back to characters
        vector<char> rev(k);
        for (auto &p : mp)
            rev[p.second] = p.first;

        string ans = "";
        for (int node : topo)
            ans += rev[node];

        return ans;
	}
}
```
#### Shortest Path in Undirected Graph 
- **BFS algorithm.** 
- Start with a queue data structure.  
- populate distance array with N infinities. 
- In queue: {node, distance}.
- If any node was unreachable `distance[node]` would be infinity.
```cpp
class Solution{
public:
	vector<int> shortestPath(vector<vector<int>> &edges, int N, int M, int src)
	{
		vector<int> adj(N);
		for(auto it: edges)
		{
			adj[it[0]].push_back(it[1]);
			adj[it[1]].push_back(it[0]);
		}
		
		int dist[N];
		for(int i=0; i<N; i++)
		{
			dist[i]=1e9;
		}
		dist[src]=0;
		
		queue<int> q;
		q.push(src);
		while(!q.empty())
		{
			int node = q.front();
			q.pop();
			for(auto it: adj[node])
			{
				if(dist[node]+1 < dist[it])
				{
					dist[it] = 1 + dist[node];
					q.push(it);
				}
			}
		}
		vector<int> ans(N, -1);
		for(int i=0; i<M; i++)
		{
			if(dist[i]!=1e9)
			{
				ans[i]=dist[i];
			}
		}
		return ans;
	}
}
```
- Time complexity roughly $O(V+2E)$. 
#### Shortest Path in DAG
- Adjacency list stores pairs (adjacent node and weight of that edge). 
- Algorithm: 
	- Do a topo sort. 
	- DFS method: stack and visited array. 
	- DFS call over -> put it in the stack.
	- Take the nodes out of the stack and relax the edges. 
	- Declare a distance array and mark everything as infinity. Mark source node as 0. 
	- take elements from stack, check adjacent nodes, check corresponding weight and update the distance array. 
```cpp
class Solution{
	private: 
		void topoSort(int node, vector<pair<int,int>> adj[],
		int vis[], stack<int> &st)
		{
			vis[node]=1; 
			for(auto it: adj[node])
			{
				int v = it.first; 
				if(!vis[v]){
					topoSort(v, adj, vis, st);
				}
			}
			
			st.push(node);
		}
	public: 
		vector<int> shortestPath(int N, int M, vector<int> edges[])
		{
			vector<pair<int, int>> adj[N];
			for(int i=0; i<M; i++)
			{
				int u=edges[i][0];
				int v=edges[i][1];
				int wt=edges[i][2]
				adj[u].push_back({v,wt});
			}
			// find the topo sort
			int vis[N]={0};
			stack<int> st;
			for(int i=0; i<N; i++)
			{
				if(!vis[i])
				{
					topoSort(i, adj, vis, st);
				}
			}
			// do the distance thing
			vector<int> dist(N);
			for(int i=0; i<N; i++) dist[i]=INT_MAX;
			dist[0]=0; 
			while(!st.empty())
			{
				int node=st.top(); 
				st.pop();
				
				for(auto it: adj[node])
				{
					int v = it.first; 
					int wt= it.second; 
					
					if(dist[node]+wt < dist[v])
					{
						dist[v]=dist[node]+wt;
					}
				}
			}
			return dist; 
		}
}
```
#### Dijsktra's Algorithm
>Pronounced `"dike.struh"`
- **Using priority queue:** 
- Can be implemented using either priority queues or set. 
- **Priority queue** called "min heap" and a distance array. 
- You also have an adjacency list. 
```cpp
class Solution
{
	public: 
	vector<int> dijkstra(int V, vector<vector<int>> adj[], int S)
	{
		priority_queue<pair<int, int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
		// declare a minimum-heap. 
		vector<int> dist(V);
		for(int i=0;j i<V; i++) dist[i]=1e9; 
		dist[S]=0; 
		pq.push({0,S});
		
		while(!pq.empty())
		{
			int dis=pq.top().first;
			int node=pq.top().second; 
			pq.pop();
			
			for(auto it: adj[node])
			{
				int edgeWeight = it[1];
				int adjNode = it[0];
				
				if(dis + edgeWeight < dist[adjNode])
				{
					dist[adjNode]=dis + edgeWeight; 
					pq.push({dist[adjNode], adjNode});
				}
			}
		}
		return dist; 
	}
}
```
- Dijkstra doesn't work with negative weight. 
- Time complexity: $E*log(V)$
- E: Total number of edges, V: total number of nodes. 
- **Using Set:** 
- Erase pair from set if you find a better distance. 
- Remaining sets are same. 
```cpp
class Solution
{
	public: 
	vector<int> dijkstra(int V, vector<vector<int>> adj[], int S)
	{
		set<pair<int,int>> st;
		vector<int> dist(V, 1e9); 
		
		st.insert({0,S}); 
		dist[S] = 0; 
		
		while(!st.empty())
		{
			auto it = *(st.begin()); 
			int node = it.second; 
			int dis = it.first; 
			st.erase(it); 
			
			for(auto it: adj[node])
			{
				int adjNode = it[0]; 
				int edgW = it[1]; 
				
				if(dis + edgW < dist[adjNode])
				{
					//erase if it existed
					if(dist[adjNode]!=1e9)
						st.erase({dist[adjNode], adjNode}); 
					dist[adjNode] = dis + edgW; 
					st.insert({dist[adjNode], adjNode});
				}
			}
		}
		return dist; 
	}
}
```
- Why use priority queue over a normal queue? 
	- If we add (7,3) then (3,3) to our priority queue. 
	- (7,3) would not even be considered. 
	- Thus, saving computation and time. 
- $E*log(V)$ : Derivation
	- E: Total number of edges, V: total number of nodes. 
	- Worst case: Each node connected to every other. 
	- Heap size is $V^{2}$.
	- $V^{2}$ is roughly equal to $E=(V)(V-1)$.
	- $O\left(V \times \left(\log(\text{heap size}) + ne \times \log(\text{heap size})\right)\right)$
	- $O\left(V \times \left(\log(\text{heap size}) \times (ne + 1)\right)\right)$
	- $ne \le V - 1$
	- $O\left(V \times V \times \log(\text{heap size})\right)$
	- $O\left(V^2 \log(\text{heap size})\right)$
	- $\text{heap size} \approx V^2$
	- $O\left(V^2 \log(V^2)\right)$
	- $\log(V^2) = 2\log V$
	- $O\left(V^2 \times 2\log V\right)$
	- $E = V^2$
	- $O(E \log V)$
#### Print Shortest Path
- Shortest path in weighted undirected graph. 
- priority queue, distance array and memoization array called "parent".
- Add your starting node and distance to your priority queue. 
- In parent store where you came to the node from. 
- Now trace the parent list back to origin. 
```cpp
class Solution
{
	public: 
		vector<int> shortestPath(int n, int m, vector<vector<int>> &edges)
		{
			vector<pair<int,int>> adj[n+1]; 
			for(auto it: edges)
			{
				adj[it[0]].push_back({it[1], it[2]});
				adj[it[1]].push_back({it[0], it[2]});
			}
			priority_queue<pair<int,int>, 
			vector<pair<int,int>>, greater<pair<int,int>>> pq;
			vector<int> dist(n+1, 1e9), parent(n+1);
			for(int i=1; i<=n; i++) parent[i]=i;
			dist[1]=0; 
			while(!pq.empty())
			{
				auto it=pq.top(); 
				int node = it.second; 
				int dis = it.first; 
				pq.pop(); 
				
				for(auto it: adj[node])
				{
					int adjNode = it.first; 
					int edW = it.second; 
					if(dis + edW < dist[adjNode])
					{
						dist[adjNode]=dis+edW; 
						pq.push({dist+edW, adjNode}); 
						parent[adjNode]=node; 
					}
				}
			}
			if(dist[n]==1e9) return {-1}; 
			
			vector<int> path; 
			int node = n; 
			while(parent[node]!=node)
			{
				path.push_back(node); 
				node = parent[node]; 
			} 
			path.push_back(1); 
			reverse(path.begin(), path.end()); 
			return path; 
		}
}
```
- Time complexity: $O(E*log(V))+O(n)$
#### Shortest Distance in a Binary Maze
- clear path: top-left cell to bottom-right such that all visited cells of the path are `0`. 
- We do not need a priority queue since the distances increase uniformly. 
- `(row,col)`: +1 and -1 from top/bottom and left/right. 
```cpp
class Solution
{
	public: 
		int shortestPath(vector<vector<int>> &grid, pair<int, int> source, pair<int, int> destination)
		{
			queue<pair<int, pair<int,int>>> q;
			vector<vector<int>> dist(n,vector<int>(m,1e9));
			dist[source.first][source.second]=0;
			q.push({0,{source.first,source.second}}); 
			int dr[]={-1,0,1,0};
			int dc[]={0,1,0,-1};
			while(!q.empty())
			{
				auto it = q.front();
				q.pop(); 
				int dis = it.first; 
				int r = it.second.first; 
				int c=it.second.second; 
				for(int i=0; i<4; i++)
				{
					int newr=r+dr[i];
					int newc=c+dc[i];
					
					if(newr>=0 && newr<n && newc>=0 && newc<m
					&& grid[newr][newc]==1 && dis+1<dist[newr][newc])
					{
						dist[newr][newc]=1+dis;
						if(newr==destination.first && newc==destination.second) return dis+1; 
						q.push({1+dis,{newr,newc}});
					}
				}
			}
			return -1; 
		}
}
```
- Leetcode problem allows 8 directions and detect 0 instead of 1:
```cpp
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int n = grid.size();
        if(grid[0][0] == 1 || grid[n-1][n-1] == 1)
            return -1
        queue<pair<int,pair<int,int>>> q;
        vector<vector<int>> dist(n, vector<int>(n, 1e9));
        dist[0][0] = 1;
        q.push({1,{0,0}});
        
        int dr[] = {-1,-1,-1,0,0,1,1,1};
        int dc[] = {-1,0,1,-1,1,-1,0,1};

        while(!q.empty()) {
            auto it = q.front();
            q.pop();
            int dis = it.first;
            int r = it.second.first;
            int c = it.second.second;
            if(r == n-1 && c == n-1)
                return dis;
            for(int i = 0; i < 8; i++) {
                int newr = r + dr[i];
                int newc = c + dc[i];
                if(newr >= 0 && newr < n && newc >= 0 && newc < n &&
                   grid[newr][newc] == 0 && dis + 1 < dist[newr][newc]) {
                    dist[newr][newc] = dis + 1;
                    q.push({dis+1,{newr,newc}});
                }
            }
        }
        return -1;
    }
};
```
#### Path With Minimum Effort
- Route's effort is the maximum absolute difference in heights between two consecutive cells of the route. 