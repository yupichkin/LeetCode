# Trees

+ [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal)
+ [Symmetric Tree](#symmetric-tree)
+ [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)
+ [Same Tree](#same-tree)
+ [Invert Binary Tree](#invert-binary-tree)
+ [Path Sum](#path-sum)
+ [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal)
+ [Subtree of Another Tree](#subtree-of-another-tree)
+ [Kth Smallest Element in a BST](#kth-smallest-element-in-a-bst)
+ [Lowest Common Ancestor of a Binary Tree](#lowest-common-ancestor-of-a-binary-tree)
+ [Lowest Common Ancestor of a Binary Search Tree](#lowest-common-ancestor-of-a-binary-search-tree)
+ [Inorder Successor in BST](#inorder-successor-in-bst)
+ [Validate Binary Search Tree](#validate-binary-search-tree)
+ [Binary Search Tree Iterator](#binary-search-tree-iterator)

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
        if(!root) return 0;
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
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
Recursive
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
 Iterative
```C++ 
class Solution {
 public:
  vector<vector<int>> levelOrder(TreeNode* root) {
    if (!root) return {};
    queue<TreeNode*> readingLevels;
    vector<vector<int>> levels;
    readingLevels.push(root);
    while (!readingLevels.empty()) {
      vector<int> readedLevel;
      for (int i = readingLevels.size(); i > 0; i--) {
        readedLevel.push_back(readingLevels.front()->val);
        if (readingLevels.front()->left)
          readingLevels.push(readingLevels.front()->left);
        if (readingLevels.front()->right)
          readingLevels.push(readingLevels.front()->right);
        readingLevels.pop();
      }
      levels.push_back(readedLevel);
    }
    return levels;
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
 
 easier:
 ```C++ 
 bool isSubtree(TreeNode* s, TreeNode* t) {
        if(!s)
            return false;
        if (equal(s, t))
            return true;
        return isSubtree(s->left, t)||isSubtree(s->right, t);
    }
    
    bool equal(TreeNode* root1, TreeNode* root2){
        if ((root1 == NULL)&&(root2 == NULL))
            return true;
        if ((root1 == NULL)||(root2 == NULL))
            return false;
        return (root2->val == root1->val) &&(equal(root1->left, root2->left))
         &&(equal(root1->right, root2->right));

    }
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
