## Table of Contents
- [Introduction](#introduction)
- [Traversal techniques](#traversal-techniques)



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