# Chapter 18: Binary Search Trees (BST)

**What You'll Learn**: What BSTs are, WHY they enable fast searching, BST property, search/insert/delete operations, balancing concepts, and time complexity analysis.

## Table of Contents

1. [Introduction: The Fast Search Problem](#introduction-the-fast-search-problem)
2. [1. What is a Binary Search Tree?](#1-what-is-a-binary-search-tree)
3. [2. BST Node Structure](#2-bst-node-structure)
4. [3. Search in BST](#3-search-in-bst)
5. [4. Insert in BST](#4-insert-in-bst)
6. [5. Delete in BST](#5-delete-in-bst)
7. [6. Find Min and Max](#6-find-min-and-max)
8. [7. Inorder Traversal of BST](#7-inorder-traversal-of-bst)
9. [8. Complete BST Implementation](#8-complete-bst-implementation)
10. [9. Time Complexity Analysis](#9-time-complexity-analysis)
11. [10. Balancing Concepts](#10-balancing-concepts)
12. [11. Validate BST](#11-validate-bst)
13. [12. Common BST Problems](#12-common-bst-problems)
14. [Complete Example Programs](#complete-example-programs)
15. [Common Mistakes](#common-mistakes)
16. [Practice Problems](#practice-problems)
17. [Key Takeaways](#key-takeaways)
18. [What's Next?](#whats-next)

---

## Introduction: The Fast Search Problem

Imagine you have a **phone book** with 1 million names.

**Option 1: Unsorted Array**
```
[Zara, Alice, Mike, Bob, ...]
```
- Search for "Mike"? Must check ALL elements! (O(n) = slow!)

**Option 2: Sorted Array**
```
[Alice, Bob, ..., Mike, ..., Zara]
```
- Binary search! (O(log n) = fast!)
- But inserting "John"? Must shift everything right! (O(n) = slow!)

**Option 3: Linked List**
```
Zara → Alice → Mike → Bob → ...
```
- Can't binary search! (no random access)
- Search is O(n) again!

**What if you could have:**
- ✅ **Fast search** (like binary search in sorted array)
- ✅ **Fast insertion** (like linked list)
- ✅ **Fast deletion** (like linked list)

**This is a Binary Search Tree (BST)!**

---

## 1. What is a Binary Search Tree?

A **BST** is a **binary tree** with a special property:

**BST Property**:
```
For every node:
  - All values in LEFT subtree < node's value
  - All values in RIGHT subtree > node's value
```

**Visual**:
```
Valid BST:
         10
       /    \
      5      15       ✓ 5 < 10 < 15
     / \    /  \      ✓ 3 < 5 < 7
    3   7  12  20     ✓ 12 < 15 < 20

Invalid BST:
         10
       /    \
      5      15
     / \    /  \
    3  12  7  20      ✗ 12 > 10, but it's in left subtree!
```

**Why this property is powerful**:
- **Binary search** works on trees!
- Left side always smaller → right side always larger
- Can skip half the tree at each step!

---

## 2. BST Node Structure

Same as binary tree, but we maintain the BST property.

```cpp
struct Node {
    int data;
    Node* left;
    Node* right;

    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};
```

---

## 3. Search in BST

**Algorithm**: Compare with root, go left if smaller, right if larger.

```cpp
bool search(Node* root, int key) {
    // Base case: empty tree or key not found
    if (root == nullptr) return false;

    // Found it!
    if (root->data == key) return true;

    // Key is smaller → search left subtree
    if (key < root->data) {
        return search(root->left, key);
    }

    // Key is larger → search right subtree
    return search(root->right, key);
}
```

**Iterative version** (often preferred):

```cpp
bool searchIterative(Node* root, int key) {
    Node* current = root;

    while (current != nullptr) {
        if (current->data == key) return true;

        if (key < current->data) {
            current = current->left;   // Go left
        } else {
            current = current->right;  // Go right
        }
    }

    return false;
}
```

**Example**:
```
Search for 7 in this BST:
         10
       /    \
      5      15
     / \    /  \
    3   7  12  20

Step 1: 7 < 10? Yes → Go left to 5
Step 2: 7 > 5? Yes → Go right to 7
Step 3: 7 == 7? Yes → Found!

Only 3 comparisons for 7 nodes! (O(log n))
```

---

## 4. Insert in BST

**Algorithm**: Find correct position (like search), then insert.

```cpp
Node* insert(Node* root, int val) {
    // Base case: found the spot, insert here
    if (root == nullptr) {
        return new Node(val);
    }

    // Value is smaller → insert in left subtree
    if (val < root->data) {
        root->left = insert(root->left, val);
    }
    // Value is larger → insert in right subtree
    else if (val > root->data) {
        root->right = insert(root->right, val);
    }
    // Value already exists (duplicate) → do nothing or handle as needed

    return root;
}
```

**Iterative version**:

```cpp
void insertIterative(Node*& root, int val) {
    Node* newNode = new Node(val);

    if (root == nullptr) {
        root = newNode;
        return;
    }

    Node* current = root;
    Node* parent = nullptr;

    while (current != nullptr) {
        parent = current;

        if (val < current->data) {
            current = current->left;
        } else if (val > current->data) {
            current = current->right;
        } else {
            // Duplicate, don't insert
            delete newNode;
            return;
        }
    }

    // Insert as child of parent
    if (val < parent->data) {
        parent->left = newNode;
    } else {
        parent->right = newNode;
    }
}
```

**Example**:
```
Insert 6 into BST:

Before:          After:
    10              10
   /  \            /  \
  5    15         5    15
 / \             / \
3   7           3   7
                   /
                  6

Steps:
1. 6 < 10? Go left to 5
2. 6 > 5? Go right to 7
3. 7 has no left child → Insert 6 as left child of 7
```

---

## 5. Delete in BST

**Most complex operation!** Three cases:

### Case 1: Node is a Leaf (No Children)

Simply delete it!

```
Delete 3:
Before:          After:
    10              10
   /  \            /  \
  5    15         5    15
 / \               \
3   7               7
```

---

### Case 2: Node has One Child

Replace node with its child.

```
Delete 5 (has only right child 7):
Before:          After:
    10              10
   /  \            /  \
  5    15         7    15
   \
    7
```

---

### Case 3: Node has Two Children

**Most complex!**

1. Find **inorder successor** (smallest value in right subtree)
2. Replace node's value with successor's value
3. Delete successor

```
Delete 10 (has two children):
Before:          After:
    10              12
   /  \            /  \
  5    15         5    15
      /  \            /  \
     12  20          13  20
       \
       13

Steps:
1. Find inorder successor of 10 → smallest in right subtree → 12
2. Replace 10 with 12
3. Delete original 12 node
```

**Implementation**:

```cpp
Node* deleteNode(Node* root, int key) {
    if (root == nullptr) return nullptr;

    // Find the node to delete
    if (key < root->data) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->data) {
        root->right = deleteNode(root->right, key);
    } else {
        // Found the node to delete!

        // Case 1: No children (leaf)
        if (root->left == nullptr && root->right == nullptr) {
            delete root;
            return nullptr;
        }

        // Case 2: One child
        if (root->left == nullptr) {
            Node* temp = root->right;
            delete root;
            return temp;
        }
        if (root->right == nullptr) {
            Node* temp = root->left;
            delete root;
            return temp;
        }

        // Case 3: Two children
        // Find inorder successor (smallest in right subtree)
        Node* successor = findMin(root->right);

        // Replace root's value with successor's value
        root->data = successor->data;

        // Delete the successor
        root->right = deleteNode(root->right, successor->data);
    }

    return root;
}

// Helper: Find minimum value node
Node* findMin(Node* root) {
    while (root->left != nullptr) {
        root = root->left;
    }
    return root;
}
```

---

## 6. Find Min and Max

**Min**: Leftmost node (keep going left).
**Max**: Rightmost node (keep going right).

```cpp
Node* findMin(Node* root) {
    if (root == nullptr) return nullptr;

    while (root->left != nullptr) {
        root = root->left;
    }

    return root;
}

Node* findMax(Node* root) {
    if (root == nullptr) return nullptr;

    while (root->right != nullptr) {
        root = root->right;
    }

    return root;
}
```

**Visual**:
```
BST:
         10
       /    \
      5      15
     / \    /  \
    3   7  12  20

Min: 3 (leftmost)
Max: 20 (rightmost)
```

---

## 7. Inorder Traversal of BST

**Special property**: Inorder traversal of BST gives **sorted order**!

```cpp
void inorder(Node* root) {
    if (root == nullptr) return;

    inorder(root->left);
    cout << root->data << " ";
    inorder(root->right);
}
```

**Example**:
```
BST:
         10
       /    \
      5      15
     / \    /  \
    3   7  12  20

Inorder: 3 5 7 10 12 15 20 (sorted!)
```

**Why?** BST property ensures left < root < right recursively!

---

## 8. Complete BST Implementation

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;

    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

class BST {
private:
    Node* root;

    Node* insertHelper(Node* node, int val) {
        if (node == nullptr) {
            return new Node(val);
        }

        if (val < node->data) {
            node->left = insertHelper(node->left, val);
        } else if (val > node->data) {
            node->right = insertHelper(node->right, val);
        }

        return node;
    }

    bool searchHelper(Node* node, int key) {
        if (node == nullptr) return false;
        if (node->data == key) return true;

        if (key < node->data) {
            return searchHelper(node->left, key);
        }
        return searchHelper(node->right, key);
    }

    void inorderHelper(Node* node) {
        if (node == nullptr) return;
        inorderHelper(node->left);
        cout << node->data << " ";
        inorderHelper(node->right);
    }

    Node* findMin(Node* node) {
        while (node->left != nullptr) {
            node = node->left;
        }
        return node;
    }

    Node* deleteHelper(Node* node, int key) {
        if (node == nullptr) return nullptr;

        if (key < node->data) {
            node->left = deleteHelper(node->left, key);
        } else if (key > node->data) {
            node->right = deleteHelper(node->right, key);
        } else {
            // Found node to delete

            // Case 1 & 2: 0 or 1 child
            if (node->left == nullptr) {
                Node* temp = node->right;
                delete node;
                return temp;
            }
            if (node->right == nullptr) {
                Node* temp = node->left;
                delete node;
                return temp;
            }

            // Case 3: 2 children
            Node* successor = findMin(node->right);
            node->data = successor->data;
            node->right = deleteHelper(node->right, successor->data);
        }

        return node;
    }

public:
    BST() : root(nullptr) {}

    void insert(int val) {
        root = insertHelper(root, val);
    }

    bool search(int key) {
        return searchHelper(root, key);
    }

    void inorder() {
        inorderHelper(root);
        cout << endl;
    }

    void deleteNode(int key) {
        root = deleteHelper(root, key);
    }
};

int main() {
    BST tree;

    tree.insert(10);
    tree.insert(5);
    tree.insert(15);
    tree.insert(3);
    tree.insert(7);
    tree.insert(12);
    tree.insert(20);

    /*
    BST created:
         10
       /    \
      5      15
     / \    /  \
    3   7  12  20
    */

    cout << "Inorder: ";
    tree.inorder();  // 3 5 7 10 12 15 20 (sorted!)

    cout << "Search 7: " << (tree.search(7) ? "Found" : "Not found") << endl;
    cout << "Search 99: " << (tree.search(99) ? "Found" : "Not found") << endl;

    tree.deleteNode(5);
    cout << "After deleting 5: ";
    tree.inorder();  // 3 7 10 12 15 20

    return 0;
}
```

---

## 9. Time Complexity Analysis

**Best/Average Case** (Balanced tree):

| Operation | Time Complexity |
|-----------|-----------------|
| Search | O(log n) |
| Insert | O(log n) |
| Delete | O(log n) |
| Find min/max | O(log n) |

**Worst Case** (Skewed tree - looks like linked list):

```
Skewed BST (unbalanced):
1
 \
  2
   \
    3
     \
      4
       \
        5

Height = n (like linked list!)
Search/Insert/Delete = O(n) - SLOW!
```

| Operation | Time Complexity |
|-----------|-----------------|
| Search | O(n) |
| Insert | O(n) |
| Delete | O(n) |

**Key insight**: Performance depends on **tree height**!
- Balanced tree: height = O(log n) → operations are O(log n)
- Skewed tree: height = O(n) → operations are O(n)

---

## 10. Balancing Concepts

**Problem**: BST can become unbalanced with certain insertion orders.

**Example**:
```
Insert: 1, 2, 3, 4, 5 (sorted order)

Result (skewed):
1
 \
  2
   \
    3
     \
      4
       \
        5

Height = 5 (worst case!)
```

**Solution**: Use **self-balancing BSTs**!

### Self-Balancing BSTs

**AVL Tree**:
- Maintains **balance factor** (height diff between left/right ≤ 1)
- Rotations after insert/delete to rebalance
- **Guaranteed O(log n)** operations

**Red-Black Tree**:
- Each node has a color (red or black)
- Maintains balance using color rules
- Used in C++ `map`, `set`
- **Guaranteed O(log n)** operations

**B-Trees**:
- Multi-way trees (more than 2 children)
- Used in databases and file systems
- **Guaranteed O(log n)** operations

**We'll cover these in advanced chapters!**

---

## 11. Validate BST

**Problem**: Check if a binary tree is a valid BST.

**Wrong approach**:
```cpp
// WRONG! Only checks immediate children
bool isValidBST(Node* root) {
    if (root == nullptr) return true;

    if (root->left && root->left->data >= root->data) return false;
    if (root->right && root->right->data <= root->data) return false;

    return isValidBST(root->left) && isValidBST(root->right);
}

// Fails for this tree:
    10
   /  \
  5    15
      /  \
     6   20
// 6 < 10, but 6 is in right subtree of 10! Invalid!
```

**Correct approach** (use range):

```cpp
bool isValidBST(Node* root, long min, long max) {
    if (root == nullptr) return true;

    // Current node must be in range [min, max]
    if (root->data <= min || root->data >= max) {
        return false;
    }

    // Left subtree: all values must be < root->data
    // Right subtree: all values must be > root->data
    return isValidBST(root->left, min, root->data) &&
           isValidBST(root->right, root->data, max);
}

// Call with:
bool isValidBST(Node* root) {
    return isValidBST(root, LONG_MIN, LONG_MAX);
}
```

---

## 12. Common BST Problems

### Problem 1: Kth Smallest Element

**Approach**: Inorder traversal gives sorted order!

```cpp
void kthSmallestHelper(Node* root, int k, int& count, int& result) {
    if (root == nullptr) return;

    kthSmallestHelper(root->left, k, count, result);

    count++;
    if (count == k) {
        result = root->data;
        return;
    }

    kthSmallestHelper(root->right, k, count, result);
}

int kthSmallest(Node* root, int k) {
    int count = 0;
    int result = -1;
    kthSmallestHelper(root, k, count, result);
    return result;
}
```

---

### Problem 2: Lowest Common Ancestor (LCA)

```cpp
Node* lowestCommonAncestor(Node* root, int p, int q) {
    if (root == nullptr) return nullptr;

    // Both p and q are smaller → LCA is in left subtree
    if (p < root->data && q < root->data) {
        return lowestCommonAncestor(root->left, p, q);
    }

    // Both p and q are larger → LCA is in right subtree
    if (p > root->data && q > root->data) {
        return lowestCommonAncestor(root->right, p, q);
    }

    // One is left, one is right (or one is root) → root is LCA
    return root;
}
```

---

### Problem 3: Convert Sorted Array to BST

```cpp
Node* sortedArrayToBST(int arr[], int start, int end) {
    if (start > end) return nullptr;

    int mid = start + (end - start) / 2;
    Node* root = new Node(arr[mid]);

    root->left = sortedArrayToBST(arr, start, mid - 1);
    root->right = sortedArrayToBST(arr, mid + 1, end);

    return root;
}

// Usage:
int arr[] = {1, 2, 3, 4, 5, 6, 7};
Node* root = sortedArrayToBST(arr, 0, 6);
```

**Result** (balanced BST):
```
       4
     /   \
    2     6
   / \   / \
  1   3 5   7
```

---

## Complete Example Programs

### Example 1: BST Range Query

```cpp
#include <iostream>
#include <vector>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

// Find all values in range [low, high]
void rangeQuery(Node* root, int low, int high, vector<int>& result) {
    if (root == nullptr) return;

    // If current node is in range, explore both subtrees
    if (root->data >= low && root->data <= high) {
        rangeQuery(root->left, low, high, result);
        result.push_back(root->data);
        rangeQuery(root->right, low, high, result);
    }
    // If current node is too small, go right
    else if (root->data < low) {
        rangeQuery(root->right, low, high, result);
    }
    // If current node is too large, go left
    else {
        rangeQuery(root->left, low, high, result);
    }
}

int main() {
    Node* root = new Node(10);
    root->left = new Node(5);
    root->right = new Node(15);
    root->left->left = new Node(3);
    root->left->right = new Node(7);
    root->right->left = new Node(12);
    root->right->right = new Node(20);

    vector<int> result;
    rangeQuery(root, 6, 14, result);

    cout << "Values in range [6, 14]: ";
    for (int val : result) {
        cout << val << " ";
    }
    cout << endl;  // Output: 7 10 12

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Not Maintaining BST Property

```cpp
// WRONG: Just inserting anywhere
void insert(Node*& root, int val) {
    if (root == nullptr) {
        root = new Node(val);
        return;
    }
    // Always inserting to left!
    insert(root->left, val);  // Breaks BST property!
}

// FIX: Compare and go left/right
void insert(Node*& root, int val) {
    if (root == nullptr) {
        root = new Node(val);
        return;
    }
    if (val < root->data) {
        insert(root->left, val);
    } else {
        insert(root->right, val);
    }
}
```

---

### Mistake 2: Delete Without Handling All Cases

```cpp
// WRONG: Only handles leaf nodes
Node* deleteNode(Node* root, int key) {
    if (root->data == key) {
        delete root;
        return nullptr;  // What if it has children?!
    }
    // ...
}

// FIX: Handle all 3 cases (see implementation above)
```

---

### Mistake 3: Checking Only Immediate Children for Validation

```cpp
// WRONG: This misses violations
bool isValid(Node* root) {
    if (!root) return true;
    if (root->left && root->left->data > root->data) return false;
    if (root->right && root->right->data < root->data) return false;
    return isValid(root->left) && isValid(root->right);
}

// FIX: Use range approach (see section 11)
```

---

## Practice Problems

### Problem 1: Ceiling and Floor
Find ceiling (smallest value ≥ key) and floor (largest value ≤ key) in BST.

### Problem 2: Two Sum in BST
Find if there exist two nodes whose sum equals a target.

### Problem 3: BST Iterator
Implement an iterator that returns elements in sorted order.

### Problem 4: Serialize and Deserialize BST
Convert BST to string and back.

### Problem 5: Trim BST
Remove all nodes outside a given range [low, high].

---

## Key Takeaways

✅ **BST property**: Left subtree < root < right subtree
✅ **Inorder traversal** gives sorted order
✅ **Search/Insert/Delete**: O(log n) average, O(n) worst
✅ **Search**: Compare and go left/right
✅ **Insert**: Find correct position (like search), then insert
✅ **Delete**: 3 cases - leaf, one child, two children
✅ **Balanced BST**: Height = O(log n) → guaranteed O(log n) operations
✅ **Skewed BST**: Height = O(n) → degrades to O(n) operations
✅ **Self-balancing BSTs** (AVL, Red-Black) maintain O(log n)
✅ **BST vs Array**: Fast insert/delete, but no random access
✅ **BST vs Hash Table**: BST maintains order, hash table doesn't

---

## What's Next?

In **[Chapter 19](chapter-19-heaps-and-priority-queues.md)**, you'll learn about **Heaps** - special binary trees that enable efficient priority queue operations and sorting algorithms!

---

**Chapter Progress**: ✅ Chapters 1-18 Complete | 📖 Next: Chapter 19
