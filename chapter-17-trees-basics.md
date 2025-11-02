# Chapter 17: Trees Basics

**What You'll Learn**: What trees are, WHY hierarchical data needs trees, tree terminology, binary trees, tree traversals, and basic tree operations.

## Table of Contents

1. [Introduction: The Hierarchical Data Problem](#introduction-the-hierarchical-data-problem)
2. [1. What is a Tree?](#1-what-is-a-tree)
3. [2. Tree Terminology](#2-tree-terminology)
4. [3. Binary Tree](#3-binary-tree)
5. [4. Binary Tree Implementation](#4-binary-tree-implementation)
6. [5. Tree Traversals](#5-tree-traversals)
7. [6. Traversal Comparison](#6-traversal-comparison)
8. [7. Common Tree Operations](#7-common-tree-operations)
9. [8. Types of Binary Trees](#8-types-of-binary-trees)
10. [9. Complete Binary Tree Example](#9-complete-binary-tree-example)
11. [10. Deleting a Tree](#10-deleting-a-tree)
12. [Complete Example Programs](#complete-example-programs)
13. [Common Mistakes](#common-mistakes)
14. [Practice Problems](#practice-problems)
15. [Key Takeaways](#key-takeaways)
16. [What's Next?](#whats-next)

---

## Introduction: The Hierarchical Data Problem

Imagine organizing these things:

**Problem 1: Company Structure**
```
CEO
  ├── VP Engineering
  │     ├── Dev Team 1
  │     └── Dev Team 2
  ├── VP Sales
  │     ├── Sales Team 1
  │     └── Sales Team 2
  └── VP Marketing
```

**Problem 2: File System**
```
/
├── home/
│   ├── user1/
│   │   ├── documents/
│   │   └── photos/
│   └── user2/
└── var/
```

**Problem 3: Family Tree**
```
Grandparent
  ├── Parent1
  │     ├── Child1
  │     └── Child2
  └── Parent2
        └── Child3
```

**Can you use arrays or linked lists?**
- ❌ Arrays: Can't represent parent-child relationships
- ❌ Linked Lists: Only show linear sequence (A→B→C)

**You need hierarchical structure = TREES!**

---

## 1. What is a Tree?

A **tree** is a hierarchical data structure with:
- **One root** (top node)
- **Parent-child relationships** (nodes can have children)
- **No cycles** (can't loop back)

**Real-world examples**:
- File system directories
- Organization charts
- HTML DOM (web pages)
- Family trees
- Decision trees
- Tournament brackets

**Visual**:
```
         1           ← Root
       /   \
      2     3        ← Level 1 (children of 1)
     / \     \
    4   5     6      ← Level 2 (children of 2 and 3)
```

---

## 2. Tree Terminology

```
         A           ← Root (top node, no parent)
       /   \
      B     C        ← B and C are children of A
     / \     \          A is parent of B and C
    D   E     F      ← D, E, F are leaf nodes (no children)
```

**Key terms**:

| Term | Definition | Example |
|------|------------|---------|
| **Root** | Top node (no parent) | A |
| **Parent** | Node with children | A is parent of B, C |
| **Child** | Node with a parent | B, C are children of A |
| **Leaf** | Node with no children | D, E, F |
| **Internal node** | Node with at least one child | A, B, C |
| **Sibling** | Nodes with same parent | B and C are siblings |
| **Ancestor** | Parent, grandparent, etc. | A is ancestor of D |
| **Descendant** | Child, grandchild, etc. | D is descendant of A |
| **Subtree** | Tree rooted at any node | Tree rooted at B: B→D,E |

**Depth, Height, Level**:

```
         A           Level 0, Depth 0
       /   \
      B     C        Level 1, Depth 1
     / \     \
    D   E     F      Level 2, Depth 2

Height of tree: 2 (longest path from root to leaf)
```

| Term | Definition |
|------|------------|
| **Level** | Distance from root (root is level 0) |
| **Depth** | Distance from root to a node |
| **Height** | Longest path from a node to a leaf |
| **Tree height** | Height of root = longest path to any leaf |

---

## 3. Binary Tree

A **binary tree** is a tree where each node has **at most 2 children** (left and right).

```
         1
       /   \
      2     3
     / \
    4   5

Node 1: left child = 2, right child = 3
Node 2: left child = 4, right child = 5
Node 3: left child = none, right child = none
```

**Node structure**:

```cpp
struct Node {
    int data;
    Node* left;
    Node* right;

    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};
```

---

## 4. Binary Tree Implementation

### Creating a Binary Tree

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;

    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

int main() {
    // Create root
    Node* root = new Node(1);

    // Create level 1
    root->left = new Node(2);
    root->right = new Node(3);

    // Create level 2
    root->left->left = new Node(4);
    root->left->right = new Node(5);

    /*
    Tree created:
         1
       /   \
      2     3
     / \
    4   5
    */

    return 0;
}
```

---

## 5. Tree Traversals

**Traversal** = visiting all nodes in a specific order.

**Four main types**:
1. **Inorder** (Left → Root → Right)
2. **Preorder** (Root → Left → Right)
3. **Postorder** (Left → Right → Root)
4. **Level-order** (Level by level)

---

### Inorder Traversal (Left → Root → Right)

**Order**: Visit left subtree, then root, then right subtree.

```cpp
void inorder(Node* root) {
    if (root == nullptr) return;

    inorder(root->left);           // Left
    cout << root->data << " ";     // Root
    inorder(root->right);          // Right
}
```

**Example**:
```
Tree:    1
       /   \
      2     3
     / \
    4   5

Inorder: 4 2 5 1 3
(Left subtree: 4, 2, 5 → Root: 1 → Right subtree: 3)
```

**Use case**: Binary Search Trees (gives sorted order!)

---

### Preorder Traversal (Root → Left → Right)

**Order**: Visit root first, then left subtree, then right subtree.

```cpp
void preorder(Node* root) {
    if (root == nullptr) return;

    cout << root->data << " ";     // Root
    preorder(root->left);          // Left
    preorder(root->right);         // Right
}
```

**Example**:
```
Tree:    1
       /   \
      2     3
     / \
    4   5

Preorder: 1 2 4 5 3
(Root: 1 → Left subtree: 2, 4, 5 → Right subtree: 3)
```

**Use case**: Create a copy of tree, prefix expressions

---

### Postorder Traversal (Left → Right → Root)

**Order**: Visit left subtree, then right subtree, then root.

```cpp
void postorder(Node* root) {
    if (root == nullptr) return;

    postorder(root->left);         // Left
    postorder(root->right);        // Right
    cout << root->data << " ";     // Root
}
```

**Example**:
```
Tree:    1
       /   \
      2     3
     / \
    4   5

Postorder: 4 5 2 3 1
(Left subtree: 4, 5 → Right subtree: 3 → Root: 1)
```

**Use case**: Delete tree (delete children before parent), postfix expressions

---

### Level-Order Traversal (BFS)

**Order**: Visit level by level (left to right).

```cpp
#include <queue>

void levelOrder(Node* root) {
    if (root == nullptr) return;

    queue<Node*> q;
    q.push(root);

    while (!q.empty()) {
        Node* current = q.front();
        q.pop();

        cout << current->data << " ";

        if (current->left != nullptr) {
            q.push(current->left);
        }
        if (current->right != nullptr) {
            q.push(current->right);
        }
    }
}
```

**Example**:
```
Tree:    1
       /   \
      2     3
     / \
    4   5

Level-order: 1 2 3 4 5
(Level 0: 1 → Level 1: 2, 3 → Level 2: 4, 5)
```

**Use case**: Find shortest path, level-by-level processing

---

## 6. Traversal Comparison

```
Tree:
         1
       /   \
      2     3
     / \   / \
    4   5 6   7

Inorder:    4 2 5 1 6 3 7  (Left-Root-Right)
Preorder:   1 2 4 5 3 6 7  (Root-Left-Right)
Postorder:  4 5 2 6 7 3 1  (Left-Right-Root)
Level-order: 1 2 3 4 5 6 7  (Level by level)
```

**Memory**:
```
Inorder:   In - Left - Root - Right  → "In the middle"
Preorder:  Pre - Root first          → "Before" children
Postorder: Post - Root last          → "After" children
```

---

## 7. Common Tree Operations

### Height of Tree

**Height** = longest path from root to leaf.

```cpp
int height(Node* root) {
    if (root == nullptr) return -1;  // Empty tree: height -1
    // OR: return 0 for single node as height 0

    int leftHeight = height(root->left);
    int rightHeight = height(root->right);

    return max(leftHeight, rightHeight) + 1;
}
```

**Example**:
```
Tree:    1           Height = 2
       /   \
      2     3        Height of 2 = 1
     / \             Height of 3 = 0
    4   5            Height of 4, 5 = 0
```

---

### Count Nodes

```cpp
int countNodes(Node* root) {
    if (root == nullptr) return 0;

    return 1 + countNodes(root->left) + countNodes(root->right);
}
```

---

### Sum of All Nodes

```cpp
int sumNodes(Node* root) {
    if (root == nullptr) return 0;

    return root->data + sumNodes(root->left) + sumNodes(root->right);
}
```

---

### Count Leaf Nodes

```cpp
int countLeaves(Node* root) {
    if (root == nullptr) return 0;

    if (root->left == nullptr && root->right == nullptr) {
        return 1;  // This is a leaf
    }

    return countLeaves(root->left) + countLeaves(root->right);
}
```

---

### Maximum Value in Tree

```cpp
int maxValue(Node* root) {
    if (root == nullptr) return INT_MIN;

    int leftMax = maxValue(root->left);
    int rightMax = maxValue(root->right);

    return max(root->data, max(leftMax, rightMax));
}
```

---

## 8. Types of Binary Trees

### Full Binary Tree

**Every node has 0 or 2 children** (no node with 1 child).

```
Valid Full Binary Tree:
         1
       /   \
      2     3
     / \
    4   5

Invalid (node 3 has only 1 child):
         1
       /   \
      2     3
           /
          4
```

---

### Complete Binary Tree

**All levels filled except possibly last, filled left to right**.

```
Valid Complete Binary Tree:
         1
       /   \
      2     3
     / \   /
    4   5 6

Invalid (not filled left to right):
         1
       /   \
      2     3
     /       \
    4         5
```

**Use**: Heaps are complete binary trees!

---

### Perfect Binary Tree

**All internal nodes have 2 children, all leaves at same level**.

```
Perfect Binary Tree:
         1
       /   \
      2     3
     / \   / \
    4   5 6   7

(All leaves at level 2, all internal nodes have 2 children)
```

**Property**: Has exactly 2^h - 1 nodes (h = height).

---

### Balanced Binary Tree

**Height of left and right subtrees differ by at most 1**.

```
Balanced:
         1
       /   \
      2     3
     / \
    4   5
(Height difference = 1)

Unbalanced:
         1
       /
      2
     /
    3
   /
  4
(Height difference = 3, looks like linked list!)
```

**Why balance matters**: Balanced trees ensure O(log n) operations!

---

## 9. Complete Binary Tree Example

```cpp
#include <iostream>
#include <queue>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;

    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

class BinaryTree {
private:
    Node* root;

public:
    BinaryTree() : root(nullptr) {}

    // Insert in level-order (creates complete binary tree)
    void insert(int val) {
        Node* newNode = new Node(val);

        if (root == nullptr) {
            root = newNode;
            return;
        }

        queue<Node*> q;
        q.push(root);

        while (!q.empty()) {
            Node* current = q.front();
            q.pop();

            if (current->left == nullptr) {
                current->left = newNode;
                return;
            } else {
                q.push(current->left);
            }

            if (current->right == nullptr) {
                current->right = newNode;
                return;
            } else {
                q.push(current->right);
            }
        }
    }

    void inorder(Node* node) {
        if (node == nullptr) return;
        inorder(node->left);
        cout << node->data << " ";
        inorder(node->right);
    }

    void levelOrder(Node* node) {
        if (node == nullptr) return;

        queue<Node*> q;
        q.push(node);

        while (!q.empty()) {
            Node* current = q.front();
            q.pop();
            cout << current->data << " ";

            if (current->left) q.push(current->left);
            if (current->right) q.push(current->right);
        }
    }

    Node* getRoot() { return root; }
};

int main() {
    BinaryTree tree;

    tree.insert(1);
    tree.insert(2);
    tree.insert(3);
    tree.insert(4);
    tree.insert(5);

    /*
    Tree created (complete binary tree):
         1
       /   \
      2     3
     / \
    4   5
    */

    cout << "Inorder: ";
    tree.inorder(tree.getRoot());
    cout << endl;

    cout << "Level-order: ";
    tree.levelOrder(tree.getRoot());
    cout << endl;

    return 0;
}
```

---

## 10. Deleting a Tree

**Important**: Free memory to avoid memory leaks!

```cpp
void deleteTree(Node* root) {
    if (root == nullptr) return;

    // Delete children first (postorder!)
    deleteTree(root->left);
    deleteTree(root->right);

    // Then delete root
    cout << "Deleting node " << root->data << endl;
    delete root;
}
```

**Why postorder?** Must delete children before parent!

---

## Complete Example Programs

### Example 1: Expression Tree

```cpp
#include <iostream>
using namespace std;

struct Node {
    char data;
    Node* left;
    Node* right;

    Node(char val) : data(val), left(nullptr), right(nullptr) {}
};

// Evaluate expression tree
int evaluate(Node* root) {
    if (root == nullptr) return 0;

    // Leaf node: return the number
    if (root->left == nullptr && root->right == nullptr) {
        return root->data - '0';  // Convert char to int
    }

    // Evaluate left and right subtrees
    int leftVal = evaluate(root->left);
    int rightVal = evaluate(root->right);

    // Apply operator
    if (root->data == '+') return leftVal + rightVal;
    if (root->data == '-') return leftVal - rightVal;
    if (root->data == '*') return leftVal * rightVal;
    if (root->data == '/') return leftVal / rightVal;

    return 0;
}

int main() {
    /*
    Expression: (5 + 3) * 2
    Tree:
           *
          / \
         +   2
        / \
       5   3
    */

    Node* root = new Node('*');
    root->left = new Node('+');
    root->right = new Node('2');
    root->left->left = new Node('5');
    root->left->right = new Node('3');

    cout << "Result: " << evaluate(root) << endl;  // 16

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Not Checking nullptr

```cpp
void inorder(Node* root) {
    cout << root->data << " ";  // CRASH if root is nullptr!
    inorder(root->left);
    inorder(root->right);
}

// FIX: Always check
void inorder(Node* root) {
    if (root == nullptr) return;  // Base case!
    inorder(root->left);
    cout << root->data << " ";
    inorder(root->right);
}
```

---

### Mistake 2: Memory Leak

```cpp
void deleteTree(Node* root) {
    delete root;  // WRONG! Children not deleted!
}

// FIX: Delete children first
void deleteTree(Node* root) {
    if (root == nullptr) return;
    deleteTree(root->left);
    deleteTree(root->right);
    delete root;
}
```

---

### Mistake 3: Wrong Traversal Order

```cpp
// Want to sum all nodes
int sum(Node* root) {
    return root->data + sum(root->left) + sum(root->right);
    // CRASH! Doesn't check nullptr!
}

// FIX:
int sum(Node* root) {
    if (root == nullptr) return 0;
    return root->data + sum(root->left) + sum(root->right);
}
```

---

## Practice Problems

### Problem 1: Mirror Tree
Create a mirror image of a binary tree (swap left and right children).

### Problem 2: Diameter of Tree
Find the longest path between any two nodes.

### Problem 3: Check if Same Tree
Compare two trees to see if they're identical.

### Problem 4: Lowest Common Ancestor
Find the lowest common ancestor of two nodes.

### Problem 5: Zigzag Level Order
Print level order but alternate left-to-right and right-to-left.

---

## Key Takeaways

✅ **Tree = hierarchical structure with root and parent-child relationships**
✅ **Binary tree = each node has at most 2 children (left, right)**
✅ **Traversals**:
   - **Inorder** (Left-Root-Right): sorted order for BST
   - **Preorder** (Root-Left-Right): copy tree
   - **Postorder** (Left-Right-Root): delete tree
   - **Level-order**: level by level (uses queue)
✅ **Common operations**: height, count, sum, max, leaves
✅ **Tree types**: Full, Complete, Perfect, Balanced
✅ **Always check nullptr** before accessing node
✅ **Delete postorder** to avoid memory leaks
✅ **Recursion is natural** for tree operations

---

## What's Next?

In **Chapter 18**, you'll learn about **Binary Search Trees (BST)** - sorted binary trees that enable fast searching, insertion, and deletion!

---

**Chapter Progress**: ✅ Chapters 1-17 Complete | 📖 Next: Chapter 18
