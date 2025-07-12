## Table of Contents
- [Introduction](#introduction)
- [Traversal Techniques](#traversal-techniques)  
  - [Pre-order Traversal](#pre-order-traversal)  
  - [In-order Traversal](#in-order-traversal)  
  - [Post-order Traversal](#post-order-traversal)  
  - [Level-order Traversal (BFS)](#level-order-traversal-bfs)
  - [Three Traversal in one](#three-traversal-in-one)
- [Maximum Depth in Binary Tree](#maximum-depth-in-binary-tree)
- [Check for balanced Binary Tree](#check-for-balanced-binary-tree)
- [Diameter of binary Tree](#diameter-of-binary-tree)
- [Max Path Sum](#max-path-sum)
- [Same Tree Check](#same-tree)
- [Zig Zag Traversal](#zig-zag-traversal)
- [Boundary Traversal](#boundary-traversal)


#### Introduction
- Each node can be attached to two other nodes. 
- The starting point is called the `root`.
- The nodes beneath another node is called `children`.
- Node without children: `leaf node`.
- `subtree` for a node A has A as it's root.
-  `ancestor`

##### Types:
1. Full Binary Tree: Either has 0 or 2 children.
2. Complete Binary Trees: 
	- All levels should be filled. 
	- If last level is not filled, the nodes should be as left as possible.
3. Perfect Binary Trees:
	 - All leaf nodes should be at the same level.
4. Balanced Binary Trees:
	 - Height of tree at max $log(N)$, where N: number of nodes.
5. Degenerate Trees: Every node has only one child.

##### C++ representation:
```cpp
struct Node
{
	int data;
	struct Node *left;
	struct Node *right;

	Node(int val)
	{
		data=val;
		left=right=null;
	}
}

int main(void)
{
	struct Node* root=new Node(1);
	root->left=new Node(2);
	root->right=new Node(3);
	root->left->right=new Node(5);
}
```

#### Traversal Techniques
![](attachments/Pasted%20image%2020250710184957.png)

- In-order traversal: left *root* right, `[4,2,8,5,1,6,3,9,7,10]`
- Pre-order traversal: *root* left right, `[1,2,4,5,8,3,6,7,9,10]`
- Post-order traversal: left right *root*, `[4,8,5,2,6,9,10,7,3,1]`
- BFS (Breadth first): `[1,2,3,4,5,6,7,8,9,10]`
- In, pre and post order traversal are parts of DFS.
#### Pre-order traversal
- Root Left Right
**Using Recursion:**

```cpp
void preorder(node)
{
	if(node==null) return;
	print(node->data);
	preorder(node->left);
	preorder(node->right);
}
```
- Time complexity: $O(n)$
- space complexity: Height of the tree, $O(H)$

**Using Iterative method:**

- We use a stack. 
- Put root onto stack. Print it out. 
- Then put it's right node and left node onto the stack. 
- Take the topmost element and print it out.
- Repeat this until the stack becomes empty.
```cpp
vector<int> preorder;
if(root==nullptr) return preorder;

stack<TreeNode*> st;
st.push(root);
while(!st.empty())
{
	root=st.top();
	st.pop();
	preorder.push_back(root->val);
	if(root->right!=nullptr)
	{
		st.push(root->right);
	}
	if(root->left!=nullptr)
	{
		st.push(root->left);
	}
}
return preorder;
```
- Time complexity: $O(n)$
- Space complexity: $O(n)$
##### In-order traversal
- Left Root Right

 **Using recursion:**
 
```cpp
void inorder(node)
{
	if(node==null) return;
	inorder(node->left);
	print(node->data);
	inorder(node->right);
}
```
- Time complexity: $O(n)$
- Each node is visited exactly once. 
- Space complexity: Height of tree, $O(H)$

**Using iterative method:**

- We'll use a stack. LIFO data structure.
```cpp
stack<TreeNode*> st;
TreeNode* node = root;
vector<int> inorder;
while(true)
{
	if(node!=nullptr)
	{
		st.push(node);
		node=node->left;
	}
	else
	{
		if(st.empty()==true) break;
		node = st.top();
		st.pop();
		inorder.push_back(node->val);
		node = node-> right;
	}
}
return inorder;

```
- Time and space complexity is same.
##### Post-order traversal
- Left Right Root

**Using Recursion:**

```cpp
void postorder(node)
{
	if(node==null) return;
	postorder(node->left);
	postorder(node->right);
	print(node->data);
}
```
- Time complexity: $O(n)$
- Space complexity: $O(H)$

**Using iterative method:**

- Using two stacks.
- Use two stack st1 and st2.
- Put the root in st1, When the iteration starts take out the top from st1 and put it into st2. 
- Check if the node has left and right, if it does put it into stack 1. Again, take out the top element and repeat the process till stack 1 gets empty.
```cpp
 vector<int> postorder;
 if(root==nullptr) return postorder;
 stack<TreeNode*> st1,st2;
 st1.push(root);
 while(!st1.empty())
 {
	 root=st1.top();
	 st1.pop();
	 st2.push(root);
	 if(root->left!=nullptr) st1.push(root->left);
	 if(root->right!=nullptr) st1.push(root->right);
 }
 while(!st2.empty())
 {
	 postorder.push_back(st2.top());
	 st2.pop();
 }
 return postorder;
```


**Using a single stack**
- `curr` points to the `root` of the tree.
-  Time complexity: $O(2n)$
```cpp
while(curr!=nullptr || !st.isempty())
{
	if(curr!=null){
		st.push(curr);
		curr=curr->left;
	}
	else
	{
		temp=st.top()->right;
		if(temp==nullptr)
		{
			temp=st.top();
			st.pop();
			post.push_back(temp);
			while(!st.empty && temp==st.top()->right)
			{
				temp=st.top(),st.pop();
				post.push_back(temp->val)
			}
		}
		else
		{
			curr=temp;
		}
	}
}
```
##### Level-order traversal BFS
- BFS
- We need a queue data structure.
```cpp
class Solution
{
public:
	vector<vector<int>> levelOrder(TreeNode* root)
	{
		vector<vector<int>> ans;
		if(root==nullptr) return ans;
		queue<TreeNode*> q;
		q.push(root);
		while(!q.empty())
		{
			int size=q.size();
			vector<int> level;
			for(int i=0; i<size; i++){
				TreeNode *node=q.front();
				q.pop();
				if(node->left!=nullptr) q.push(node->left);
				if(node->right!=nullptr) q.push(node->right);
				level.push_back(node->val);
			}
			ans.push_back(level);
		}
		return ans;
	}
}
```

- Time complexity: $O(n)$
- Space complexity: $O(n)$

##### Three traversal in one
- maintain a single stack with (value ,number)
- Algorithm:
```python
if num==1:
	preorder
	++
	left
if num==2
	inorder
	right
if num==3
	postorder
	erase
```
- Time complexity: $O(3n)$
- Space complexity: $O(4n)$
 ```cpp
 stack<pair<TreeNode*,int>> st;
 st.push({root,1});
 vector<int> pre,in,post;
 if(root==nullptr) return;
 while(!st.empty())
 {
	 auto it=st.top();
	 st.pop();
	 if(it.second==1)
	 {
		 pre.push_back(it.first->val);
		 it.second++;
		 st.push(it)
		 if(it.first->left !=nullptr)
		 {
			 st.push(it.first->left,1);
		 }
	 }
	 else if(it.second==2)
	 {
		 in.push_back(it.first->val);
		 it.second++;
		 st.push(it);
		 if(it.first->right != nullptr)
		 {
			 st.push({it.first->right,1});
		 }
	 }
	 else
	 {
		 post.push_back(it.first->val);
	 }
 }
```

#### Maximum Depth in Binary Tree
- Recursive: queue $O(n)$
- Level order: auxiliary space $O(height)$ 
```cpp
if(root==nullptr) return o;
int lh=maxDepth(root->left);
int rh=maxDepth(root->right);
return 1+max(lh,rh);
```
- Time complexity: $O(n)$
- Space complexity: $O(n)$
#### Check for Balanced Binary Tree
- For every node, height of left - height of right <= 1.
- basic solution:
```cpp
bool check(Node)
	if node==null
		return true
		// the part below adds the extra complexity
	lh=findHleft(node->left);
	rh=findHRight(node->right);

	if(abs(rh-lh)>1) return false;

	bool left=check(node->left);
	bool right=check(node->right);

	if(!left||!right) return false;
	// if either returns false
	return true;
```
- Time complexity: $O(n^{2})$
- We need to remove this $O(n)$ complexity. 
```cpp
 int height(TreeNode* root)
 {
	 if(root==nullptr) return 0;
	 int leftheight=height(root->left);
	 if(leftheight==-1) return -1;
	 int rightheight=height(root->right);
	 if(rightheight==-1) return -1;
	 if(abs(leftheight-rightheight)>1) return -1;
	 return max(leftheight,rightheight)+1;
 }
```
- Time complexity: $O(n)$
- Space complexity: $O(n)$

#### Diameter of binary tree
- longest path between two nodes. (number of edges)
- Path doesn't need to pass via root.
- Basically do recursive traversal + find $max(lh+rh)$
- Basic method:
```cpp
// pseudo code
void findmax(node){
	if(root==nullptr) return;
	lh=findleft(node->left);
	rh=findright(node->right);
	max = max(max,lh+rh);
	findmax(node->left);
	findmax(node->right);
}
```
- Takes $O(n^{2})$ time complexity.
- Optimized method:
```cpp
int findMax(Node* node, int &maxi) {
    if (node == NULL)
        return 0;

    int lh = findMax(node->left, maxi);
    int rh = findMax(node->right, maxi);

    maxi = max(maxi, lh + rh);

    return 1 + max(lh, rh);
}

```
- Time complexity: $O(n)$
- Space complexity: $O(n)$

#### Max path sum
- Node A to Node B.
- which path gives max sum.
- 
![](attachments/Pasted%20image%2020250711214327.png)
- maxi=42
```cpp
int maxPathSum(TreeNode* root)
{
	int maxi=INT_MIN;
	maxPathDown(root,maxi);
	return maxi;
}
int maxPathDown(TreeNode* node, int& maxi)
{
	if(node==nullptr) return 0;
	int left=max(0,maxPathDown(node->left, maxi));
	int right=max(0,maxPathDown(node->right,maxi));
	maxi = max(maxi, left+right+node->val);
	return max(left,right) + node->val;
}
```
#### Same Tree
```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p && !q) return true;          
        if (!p || !q) return false;         
        if (p->val != q->val) return false;
        return isSameTree(p->left, q->left) &&
               isSameTree(p->right, q->right);
    }
};
```
#### Zig Zag Traversal
- Maintain a flag, `leftToRight` which is 0 by default. 
- In the for loop add `int index=leftToRight?i:size-i-1`

#### Boundary Traversal
- anti-clock wise boundary traversal.
- Order:
	 - Left boundary excluding leaves.
	 - Leaf nodes.
	 - Right boundary (reverse) excluding leaves.
- ds, root into ds. Next add the left node if available, else the right node. Do this till you reach the leaf nodes.
-  Then do an In-order traversal. Only add the leaf nodes.
- Do the first step, but in reverse order. i.e. take right.
```cpp
void addLeftBoundary(Node* node, vector<int> &res)
{
	Node* cur= root->left;
	while(cur){
		if(!isLeaf(cur)) res.push_back(cur->data);
		if(cur->left) cur=cur->left;
		else cur = cur->right;
	}
}
void addLeaves(Node* node, vector<int> &res)
{
	if(isLeaf(node))
	{
		res.push_back(node->data);
		return;
	}
	if(root->left) addLeaves(root->left,res);
	if(root->right) addLeaves(root->right, res);
}
void addRightBoundary(Node* node, vector<int> &res)
{
	Node* cur=root->right;
	vector<int> tmp;
	while(cur)
	{
		if(!isLeaf(cur)) tmp.push_back(cur->data);
		if(cur->right) cur=cur->right;
		else cur=cur->left;
	}
	// you take the reverse
	for(int i=tmp.size()-1; i>=0; --i)
	{
		res.push_back(tmp[i]);
	}
}
public:
	vector<int> printBoundary(Node* root)
	{
		vector<int> res;
		if(!root) return res;
		if(!isleaf(root)) res.push_back(root->data)
		addLeftBoundary(root, res);
		addLeaves(root,res);
		addRightBoundary(root, res);
		return res;
	}
```
- Time complexity: $O(n)+O(n)+O(H)$
- Space complexity: $O(n)$ (Algorithmic complexity)

#### Vertical Order Traversal
