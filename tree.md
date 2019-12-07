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
    if (!root) return;
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
    if (root) return isSpecular(root->left, root->right);
    return true;
  }

 private:
  bool isSpecular(TreeNode* tree1, TreeNode* tree2) {
    if (tree1 && tree2)
      return (tree1->val == tree2->val &&
              isSpecular(tree1->left, tree2->right) &&
              isSpecular(tree1->right, tree2->left));
    return tree1 ==
           tree2;  // that mean "nulls" must be at the same level of tree
  }
};
 ```
 
 # Maximum Depth of Binary Tree

https://leetcode.com/problems/maximum-depth-of-binary-tree/
```C++ 
class Solution {
 public:
  int maxDepth(TreeNode* root) {
    if (!root) return 0;
    return maxDepth(root->left, root->right, 1);
  }
  int maxDepth(TreeNode* tree1, TreeNode* tree2, int depth) {
    if (tree1 && tree2) {
      int depth1 = maxDepth(tree1->left, tree1->right, depth + 1);
      int depth2 = maxDepth(tree2->left, tree2->right, depth + 1);
      return depth1 > depth2 ? depth1 : depth2;
    }
    if (tree1) return maxDepth(tree1->left, tree1->right, depth + 1);
    if (tree2) return maxDepth(tree2->left, tree2->right, depth + 1);
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
    if (!p && !q) return true;
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
    if (root) {
      swap(root->left, root->right);
      invertTree(root->left);
      invertTree(root->right);
    }
    return root;
  }
};
 ```
 
 # Path Sum

https://leetcode.com/problems/path-sum/
```C++ 
class Solution {
 public:
  bool hasPathSum(TreeNode* root, int sum) {
    if (!root) return false;
    if (!root->left && !root->right) return sum == root->val;
    return hasPathSum(root->left, sum - root->val) ||
           hasPathSum(root->right, sum - root->val);
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
      if (levels[level].empty()) levels.push_back({});
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
    for (int i = 0; i < subTrees.size(); i++) {
      if (isSameTree(t, subTrees[i])) return true;
    }
    return false;
  }
  void findElements(TreeNode* s) {
    if (s) {
      if (s->val == peekVal) subTrees.push_back(s);
      findElements(s->left);
      findElements(s->right);
    }
    return;
  }
  bool isSameTree(TreeNode* p, TreeNode* q) {
    if (!p && !q) return true;
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
    return treeArray[k - 1];
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
 
 #   Lowest Common Ancestor of a Binary Tree
 
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/
```C++ 
class Solution {
 public:
  TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root) return NULL;
    if (root == p || root == q) return root;
    TreeNode* ancestorLeft = lowestCommonAncestor(root->left, p, q);
    TreeNode* ancestorRight = lowestCommonAncestor(root->right, p, q);
    if (ancestorLeft && ancestorRight) return root;
    return ancestorLeft ? ancestorLeft : ancestorRight;
  }
};
 ```
 
  #   Lowest Common Ancestor of a Binary Search Tree
 
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/
```C++ 
class Solution {
 public:
  TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (p->val > q->val) swap(p, q);
    return findLowestAncestor(root, p, q);
  }
  TreeNode* findLowestAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if ((p->val < root->val && root->val < q->val) || root == p || root == q)
      return root;
    if (q->val < root->val) return findLowestAncestor(root->left, p, q);
    if (p->val > root->val) return findLowestAncestor(root->right, p, q);
    return NULL;
  }
};
};
 ```
  #    Inorder Successor in BST
 
https://www.lintcode.com/problem/inorder-successor-in-bst/description
```C++ 
class Solution {
 public:
  vector<TreeNode*> inOrderWay;
  /*
   * @param root: The root of the BST.
   * @param p: You need find the successor node of p.
   * @return: Successor of p.
   */
  TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
    if (!root) return NULL;
    InOrder(root);
    if (p == inOrderWay.back()) return NULL;
    vector<TreeNode*>::iterator it =
        find(inOrderWay.begin(), inOrderWay.end(), p);
    return *(++it);
    // write your code here
  }
  void InOrder(TreeNode* root) {
    if (!root) return;
    InOrder(root->left);
    inOrderWay.push_back(root);
    InOrder(root->right);
  }
};
 ```
   #   Validate Binary Search Tree
 
https://leetcode.com/problems/validate-binary-search-tree/
```C++ 
class Solution {
  vector<int> stack;
  vector<int> direction;  // 0 - left, 1 - right
 public:
  bool isValidBST(TreeNode* root) {
    if (!root) return true;
    for (int i = 0; i < stack.size(); i++) {
      if (direction[i]) {  // right
        if (root->val <= stack[i]) return false;
      } else {  // left
        if (root->val >= stack[i]) return false;
      }
    }
    stack.push_back(root->val);
    direction.push_back(1);
    if (!isValidBST(root->right)) return false;
    direction.pop_back();

    direction.push_back(0);
    if (!isValidBST(root->left)) return false;
    direction.pop_back();
    stack.pop_back();

    return true;
  }
};
 ```
 
 
   #   Binary Search Tree Iterator
 
https://leetcode.com/problems/binary-search-tree-iterator/
```C++ 
class BSTIterator {
  vector<int> iteratorArray;
  int position;

 public:
  BSTIterator(TreeNode* root) {
    InOrderTraversal(root);
    position = -1;
  }

  /** @return the next smallest number */
  int next() {
    position++;
    return iteratorArray[position];
  }

  /** @return whether we have a next smallest number */
  bool hasNext() { return position + 1 < iteratorArray.size(); }

 private:
  void InOrderTraversal(TreeNode* root) {
    if (!root) return;
    InOrderTraversal(root->left);
    iteratorArray.push_back(root->val);
    InOrderTraversal(root->right);
  }
};

 ```
