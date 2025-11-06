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
```cpp
int floor(Node* root, int x) {
        // largest value which is less than x. 
        int floor=-1; 
        
        while(root)
        {
            if(root->data==x)
            {
                floor=root->data;
                return floor;
            }
            
            if(x < root->data)
            {
                root=root->left;
            }
            else
            {
                floor=root->data;
                root=root->right;
            }
        }
        return floor;
    }
```
#### Insert a Node
```cpp
TreeNode* insertIntoBST(TreeNode* root, int val) {
        if(root==NULL) return new TreeNode(val);
        TreeNode* curr=root;
        while(true)
        {
            if(curr->val <= val)
            {
                if(curr->right != NULL)
                {
                    curr=curr->right;
                }
                else
                {
                    curr->right=new TreeNode(val);
                    break;
                }
            }
            else
            {
                if(curr->left != NULL)
                {
                    curr=curr->left;p
                }
                else
                {
                    curr->left=new TreeNode(val);
                    break;
                }
            }
        }
        return root;
    }
```
#### Delete a Node
- Search for the node. 
- Make sure the parent node's left is pointing to the deleted node's left.
- Then go to the extreme right and attach the deleted node's right to it. 
```cpp
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == NULL) {
            return NULL;
        }

        if (root->val == key) {
            return helper(root);
        }

        TreeNode* dummy = root;

        while (root != NULL) {
            if (root->val > key) {
                if (root->left != NULL && root->left->val == key) {
                    root->left = helper(root->left);
                    break;
                } else {
                    root = root->left;
                }
            } else {
                if (root->right != NULL && root->right->val == key) {
                    root->right = helper(root->right);
                    break;
                } else {
                    root = root->right;
                }
            }
        }

        return dummy;
    }

    TreeNode* helper(TreeNode* root) {
        if (root->left == NULL) {
            return root->right;
        } 
        else if (root->right == NULL) {
            return root->left;
        }

        TreeNode* rightChild = root->right;
        TreeNode* lastRight = findLastRight(root->left);

        lastRight->right = rightChild;
        return root->left;
    }

    TreeNode* findLastRight(TreeNode* root) {
        if (root->right == NULL) {
            return root;
        }
        return findLastRight(root->right);
    }
};
```
- Time Complexity: Height of the Tree. 
- Space Complexity: None. 
#### Kth Smallest Element
- Return the kth smallest value (1-indexed). 
- **My approach:** Traverse the BST using in-order traversal and append every value into a list. Return the element at index `k-1`.
```cpp
int kthSmallest(TreeNode* root, int k) {
    if (root == NULL) {
        return -1;
    }
    vector<int> v;
    inorder(root, v);
    return v[k - 1];
}

void inorder(TreeNode* root, vector<int> &v) {
    if (!root) return;
    inorder(root->left, v);
    v.push_back(root->val);
    inorder(root->right, v);
}
```
- We are still using $O(n)$ to save extra space. 
- We could maintain a counter to track the kth smallest value. 
```cpp
void inorder(TreeNode* node, int& counter, int k, int& kSmallest) {
    if (!node || counter >= k) return;

    // Traverse left subtree
    inorder(node->left, counter, k, kSmallest);

    // Visit current node
    counter++;
    if (counter == k) {
        kSmallest = node->val;
        return;
    }

    // Traverse right subtree
    inorder(node->right, counter, k, kSmallest);
}
```
#### Validate BST
- **My Approach:** Do an in-order traversal of the tree and check if all the elements are sorted. If they are then the tree is valid. 
```cpp
bool isValidBST(TreeNode* root) {
    if (root == NULL) return true;

    vector<int> v;
    inorder(root, v);

    int n = v.size();
    for (int i = 0; i < n - 1; i++) {
        if (v[i + 1] <= v[i]) {
            return false;
        }
    }

    return true;
}

void inorder(TreeNode* root, vector<int>& v) {
    if (!root) return;
    inorder(root->left, v);
    v.push_back(root->val);
    inorder(root->right, v);
}
```
- Optimal Approach: 
```cpp
public:
    bool isValidBST(TreeNode* root) {
        return validate(root, LONG_MIN, LONG_MAX);
    }

private:
    bool validate(TreeNode* node, long minVal, long maxVal) {
        if (node == nullptr) return true;

        // Node must be strictly between minVal and maxVal
        if (node->val <= minVal || node->val >= maxVal)
            return false;

        return validate(node->left, minVal, node->val) &&
               validate(node->right, node->val, maxVal);
    }
```