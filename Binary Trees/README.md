## Table of Contents
- [Introduction](#introduction)




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