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