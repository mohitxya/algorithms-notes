## Table of Contents

#### Introduction
- Left Node should be less than the middle one, which should be less than the Right Node. 
- Left and right subtree should be a BST as well. 
- Ideally duplicates are not allowed.  
- Search in Binary Search Tree: 
```cpp
TreeNode* searchBST(TreeNode* root, int val)
{
	while(root!=NULL && root->val!=val)
	{
		root = val<root->val? root->left: root->right;
	}
	return root;
}
```
#### Finding Max & Min 
- For max just keep following the right pointer. 
- For min follow the left pointer. 
```cpp
int findMax(Node *root) 
{
	while(root->right!=NULL)
	{
		root=root->right;
	}
	return root->data; 
}

int findMin(Node *root) {
	while(root->left!=NULL)
	{
		root=root->left;
	}
	return root->data;
}
```
#### Ceil & Floor
- We have to find the lowest `val` which is greater than or equal to `key`.
- `val >= key`
- Ceil: 
```cpp
int findCeil(BinaryTreeNode<int> *root, int key)
{
	int ceil=-1; 
	while(root)
	{
		if(root->data == key)
		{
			ceil=root->data;
			return ceil;
		}
		
		if(key > root-> data)
		{
			root=root->right;
		}
		else
		{
			ceil=root->data;
			root=root->left;
		}
	}
	return ceil;
}
```
- Floor: greatest value node of the BST which is smaller than or equal to x.
- 