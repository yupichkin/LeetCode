# Binary tree

Definition for a binary tree node.

```C++ 
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
```


# Binary Tree Inorder Traversal

https://leetcode.com/problems/binary-tree-inorder-traversal/
```C++ 
class Solution {
private:
    vector<int> array;
public:
    vector<int> inorderTraversal(TreeNode* root) {
        InOrder(root);
        return array;
    }
    void InOrder(TreeNode* root) {
        if (!root)
            return;
        InOrder(root->left);
        array.push_back(root->val);
        InOrder(root->right);
    }
};
 ```
 
 # Symmetric Tree

https://leetcode.com/problems/symmetric-tree/
```C++ 
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root)
            return isSpecular(root->left, root->right);
        return true;
    }
private:
    bool isSpecular(TreeNode* tree1, TreeNode* tree2) {
        if (tree1 && tree2) {
            if (tree1->val == tree2->val && isSpecular(tree1->left, tree2->right) && isSpecular(tree1->right, tree2->left))
                return true;
            return false;

        }
        if (tree1 == tree2) //that mean "nulls" must be at the same level of tree
            return true;
        return false;
    }
};
 ```
 
 # Maximum Depth of Binary Tree

https://leetcode.com/problems/maximum-depth-of-binary-tree/
```C++ 
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root)
            return 0;
        return maxDepth(root->left, root->right, 1);
    }
    int maxDepth(TreeNode* tree1, TreeNode* tree2, int depth) {
        if (tree1 && tree2) {
            int depth1 = maxDepth(tree1->left, tree1->right, depth + 1);
            int depth2 = maxDepth(tree2->left, tree2->right, depth + 1);
            return depth1 > depth2 ? depth1 : depth2;
        }
        if(tree1)
            return maxDepth(tree1->left, tree1->right, depth + 1);
        if(tree2)
            return maxDepth(tree2->left, tree2->right, depth + 1);
        return depth;
    }
};
 ```
 
 # Same Tree

https://leetcode.com/problems/same-tree/
```C++ 
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p && !q)
            return true;
        if ((p && q) && (p->val == q->val)) {
            return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
        }
        return false;
    }
};
 ```
 
 # Invert Binary Tree

https://leetcode.com/problems/invert-binary-tree/
```C++ 
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        swapChildrens(root);
        return root;
    }
    void swapChildrens(TreeNode* root){
        if (root) {
            swap(root->left, root->right);
            swapChildrens(root->left);
            swapChildrens(root->right);
        }
        return;
    }
};
 ```
 
 # Path Sum

https://leetcode.com/problems/path-sum/
```C++ 
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (!root)
            return false;
        if (!root->left && !root->right)
            return sum == root->val;
        return hasPathSum(root->left, sum - root->val) || hasPathSum(root->right, sum - root->val);
    }
};
 ```
 
 # Binary Tree Level Order Traversal

https://leetcode.com/problems/binary-tree-level-order-traversal/
```C++ 
class Solution {
private:
vector<vector<int>> levels;

public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        levels.push_back({});
        writeLevel(root, 0);
        levels.pop_back();
        return levels;
    }
    void writeLevel(TreeNode* root, int level) {
        if (root) {
            if (levels[level].empty())
                levels.push_back({});
            levels[level].push_back(root->val);
            writeLevel(root->left, level + 1);
            writeLevel(root->right, level + 1);
        }
        return;
        
    }
};
 ```
 
 # Subtree of Another Tree

https://leetcode.com/problems/subtree-of-another-tree/
```C++ 
class Solution {
private:
    vector<TreeNode*> subTrees;
    int peekVal;
public:
    bool isSubtree(TreeNode* s, TreeNode* t) {
        peekVal = t->val;
        findElements(s);
        for(int i = 0; i < subTrees.size(); i++) {
            if (isSameTree(t, subTrees[i]))
                return true;
        }
        return false;
    }
    void findElements(TreeNode* s) {
        if (s) {
            if(s->val == peekVal)
                subTrees.push_back(s);
            findElements(s->left);
            findElements(s->right);
        }
        return;
    }
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p && !q)
            return true;
        if ((p && q) && (p->val == q->val)) {
            return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
        }
        return false;
    }
};
 ```
 
#  Kth Smallest Element in a BST

https://leetcode.com/problems/kth-smallest-element-in-a-bst/
```C++ 
class Solution {
private:
    vector<int> treeArray;
public:
    int kthSmallest(TreeNode* root, int k) {
        fillArray(root);
        return treeArray[k-1];
    }
    void fillArray(TreeNode* root) {
        if (root) {
            fillArray(root->left);
            treeArray.push_back(root->val);
            fillArray(root->right);
        }
        return;
    }
};
 ```
