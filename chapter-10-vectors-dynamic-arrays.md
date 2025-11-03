# Chapter 10: Vectors & Dynamic Arrays

**What You'll Learn**: What vectors are, WHY they're better than arrays, how they grow automatically, capacity vs size, and common vector operations.

## Table of Contents

1. [What is a Vector?](#1-what-is-a-vector)
2. [Including and Creating Vectors](#2-including-and-creating-vectors)
3. [Size vs Capacity: The Key Concept](#3-size-vs-capacity-the-key-concept)
4. [Adding Elements](#4-adding-elements)
5. [Accessing Elements](#5-accessing-elements)
6. [Removing Elements](#6-removing-elements)
7. [Iterating Through Vectors](#7-iterating-through-vectors)
8. [Vector Memory Management](#8-vector-memory-management)
9. [Common Vector Operations](#9-common-vector-operations)
10. [2D Vectors (Vector of Vectors)](#10-2d-vectors-vector-of-vectors)
11. [Vectors vs Arrays](#11-vectors-vs-arrays)
12. [Common Mistakes](#common-mistakes-)
13. [Practice Problems](#practice-problems)
14. [Key Takeaways](#key-takeaways)

---

## Introduction: The Fixed Size Problem

Remember arrays from Chapter 7? They have a **big limitation**:

```cpp
int arr[5];  // Size is FIXED at 5, forever!

// What if you need to add a 6th element? CAN'T DO IT!
// What if user wants 100 elements? TOO BAD!
```

**Problems with arrays**:
- ❌ Fixed size (must know size at compile time)
- ❌ Can't grow or shrink
- ❌ No built-in functions (can't easily insert, delete, find)
- ❌ Need to manually track size

**Enter `vector`**: A **dynamic array** that can grow and shrink automatically!

```cpp
vector<int> v;  // Starts empty

v.push_back(10);  // Add element → size becomes 1
v.push_back(20);  // Add element → size becomes 2
v.push_back(30);  // Add element → size becomes 3
// Can grow infinitely (until memory runs out)!
```

**Vector is like a magic backpack** - automatically expands to fit whatever you put in!

---

## 1. What is a Vector?

A **vector** is a **dynamic array** from the C++ Standard Template Library (STL).

**Key features**:
- ✅ Size can change at runtime (grow/shrink)
- ✅ Stored contiguously in memory (like arrays)
- ✅ Fast random access (like arrays)
- ✅ Rich functionality (insert, delete, search, etc.)
- ✅ Automatic memory management

**Think of it as**: A smart array that manages its own size!

---

## 2. Including and Creating Vectors

**Include header**:
```cpp
#include <vector>
using namespace std;
```

### Declaration and Initialization

**Method 1: Empty vector**:
```cpp
vector<int> v;  // Empty vector of integers
```

**Method 2: With size**:
```cpp
vector<int> v(5);  // Vector of 5 elements (all 0)
```

**Method 3: With size and initial value**:
```cpp
vector<int> v(5, 100);  // Vector of 5 elements (all 100)
```

**Method 4: From array or list**:
```cpp
vector<int> v = {10, 20, 30, 40, 50};
```

**Method 5: Copy from another vector**:
```cpp
vector<int> v1 = {1, 2, 3};
vector<int> v2 = v1;  // Copy of v1
```

---

## 3. Size vs Capacity: The Key Concept

**Size** = number of elements currently in the vector
**Capacity** = amount of allocated memory (in elements)

```cpp
vector<int> v;

v.push_back(10);
// Size = 1, Capacity = 1 (usually)

v.push_back(20);
// Size = 2, Capacity = 2

v.push_back(30);
// Size = 3, Capacity = 4 (vector doubled its capacity!)
```

**Why separate?**
- Allocating memory is **expensive**
- Vector allocates **extra space** to avoid frequent reallocations
- When full, vector typically **doubles** its capacity

**Visual**:
```
Capacity = 4 (allocated space)
┌────┬────┬────┬────┐
│ 10 │ 20 │ 30 │    │
└────┴────┴────┴────┘
       ↑
    Size = 3 (elements used)
```

---

### Checking Size and Capacity

```cpp
vector<int> v = {10, 20, 30};

cout << v.size();      // 3 (number of elements)
cout << v.capacity();  // Could be 3, 4, or more (depends on implementation)
cout << v.empty();     // false (not empty)
```

---

## 4. Adding Elements

### Method 1: `push_back()` (Add to End)

```cpp
vector<int> v;

v.push_back(10);  // v = {10}
v.push_back(20);  // v = {10, 20}
v.push_back(30);  // v = {10, 20, 30}
```

**Most common way** to add elements!

---

### Method 2: `insert()` (Add at Position)

```cpp
vector<int> v = {10, 20, 30};

// Insert 99 at position 1 (before 20)
v.insert(v.begin() + 1, 99);
// v = {10, 99, 20, 30}
```

**Note**: Inserting in middle is **slow** (O(n)) - must shift elements!

---

### Method 3: `emplace_back()` (Efficient!)

```cpp
vector<int> v;

v.emplace_back(10);  // Similar to push_back, but more efficient
```

**Difference**: `emplace_back` constructs element in-place (no copy), slightly faster.

---

## 5. Accessing Elements

### Method 1: Index Operator `[]`

```cpp
vector<int> v = {10, 20, 30, 40, 50};

cout << v[0];  // 10 (first element)
cout << v[2];  // 30
cout << v[4];  // 50 (last element)

v[1] = 99;     // Modify
// v = {10, 99, 30, 40, 50}
```

**Warning**: No bounds checking! Accessing `v[100]` when size is 5 → **crash**!

---

### Method 2: `at()` (Safe Access)

```cpp
vector<int> v = {10, 20, 30};

cout << v.at(0);   // 10
cout << v.at(10);  // EXCEPTION! Out of bounds (safer than [])
```

**Use `at()` when you want bounds checking**.

---

### Method 3: `front()` and `back()`

```cpp
vector<int> v = {10, 20, 30, 40, 50};

cout << v.front();  // 10 (first element)
cout << v.back();   // 50 (last element)
```

---

## 6. Removing Elements

### Method 1: `pop_back()` (Remove Last)

```cpp
vector<int> v = {10, 20, 30, 40, 50};

v.pop_back();  // v = {10, 20, 30, 40}
v.pop_back();  // v = {10, 20, 30}
```

**Most common way** to remove elements!

---

### Method 2: `erase()` (Remove at Position)

```cpp
vector<int> v = {10, 20, 30, 40, 50};

// Erase element at index 2 (value 30)
v.erase(v.begin() + 2);
// v = {10, 20, 40, 50}
```

**Erase range**:
```cpp
vector<int> v = {10, 20, 30, 40, 50};

// Erase from index 1 to 3 (20, 30)
v.erase(v.begin() + 1, v.begin() + 3);
// v = {10, 40, 50}
```

---

### Method 3: `clear()` (Remove All)

```cpp
vector<int> v = {10, 20, 30, 40, 50};

v.clear();  // v = {} (size becomes 0)
```

---

## 7. Iterating Through Vectors

### Method 1: Index Loop

```cpp
vector<int> v = {10, 20, 30, 40, 50};

for (int i = 0; i < v.size(); i++) {
    cout << v[i] << " ";
}
// Output: 10 20 30 40 50
```

---

### Method 2: Range-Based Loop (Modern!)

```cpp
vector<int> v = {10, 20, 30, 40, 50};

for (int x : v) {
    cout << x << " ";
}
// Output: 10 20 30 40 50
```

**Modify elements** (use reference):
```cpp
for (int& x : v) {
    x *= 2;  // Double each element
}
// v = {20, 40, 60, 80, 100}
```

---

### Method 3: Iterators

```cpp
vector<int> v = {10, 20, 30, 40, 50};

for (vector<int>::iterator it = v.begin(); it != v.end(); ++it) {
    cout << *it << " ";
}
// Output: 10 20 30 40 50
```

**Modern shorthand** (auto):
```cpp
for (auto it = v.begin(); it != v.end(); ++it) {
    cout << *it << " ";
}
```

---

## 8. Vector Memory Management

### How Vectors Grow

When capacity is full and you add an element:
1. **Allocate** new larger array (usually 2× current capacity)
2. **Copy** all elements to new array
3. **Delete** old array
4. **Add** new element

**Visual**:
```
Step 1: Vector full
Capacity = 2
┌────┬────┐
│ 10 │ 20 │
└────┴────┘
Size = 2

Step 2: Want to add 30
Allocate new array (capacity = 4)
┌────┬────┬────┬────┐
│    │    │    │    │
└────┴────┴────┴────┘

Step 3: Copy old elements
┌────┬────┬────┬────┐
│ 10 │ 20 │    │    │
└────┴────┴────┴────┘

Step 4: Delete old array, add new element
┌────┬────┬────┬────┐
│ 10 │ 20 │ 30 │    │
└────┴────┴────┴────┘
Size = 3, Capacity = 4
```

---

### reserve() and resize()

**`reserve(n)`**: Allocate space for n elements (doesn't change size)
```cpp
vector<int> v;
v.reserve(100);  // Allocates space for 100 elements

cout << v.size();      // 0 (no elements yet)
cout << v.capacity();  // 100 (space allocated)
```

**`resize(n)`**: Change size to n (adds/removes elements)
```cpp
vector<int> v = {10, 20, 30};
v.resize(5);  // v = {10, 20, 30, 0, 0}

v.resize(2);  // v = {10, 20} (removed last 3)
```

---

### shrink_to_fit()

Free unused capacity:
```cpp
vector<int> v = {10, 20, 30};
v.reserve(1000);  // Capacity = 1000, Size = 3

v.shrink_to_fit();  // Capacity = 3, Size = 3 (freed extra space)
```

---

## 9. Common Vector Operations

### Sorting

```cpp
#include <algorithm>

vector<int> v = {50, 20, 40, 10, 30};
sort(v.begin(), v.end());
// v = {10, 20, 30, 40, 50}
```

---

### Reversing

```cpp
#include <algorithm>

vector<int> v = {10, 20, 30, 40, 50};
reverse(v.begin(), v.end());
// v = {50, 40, 30, 20, 10}
```

---

### Finding

```cpp
#include <algorithm>

vector<int> v = {10, 20, 30, 40, 50};

auto it = find(v.begin(), v.end(), 30);
if (it != v.end()) {
    cout << "Found at index: " << (it - v.begin());  // 2
} else {
    cout << "Not found";
}
```

---

### Minimum and Maximum

```cpp
#include <algorithm>

vector<int> v = {50, 20, 40, 10, 30};

int minVal = *min_element(v.begin(), v.end());  // 10
int maxVal = *max_element(v.begin(), v.end());  // 50
```

---

## 10. 2D Vectors (Vector of Vectors)

Like a 2D array, but dynamic!

**Declaration**:
```cpp
vector<vector<int>> matrix;
```

**Initialization**:
```cpp
// 3x3 matrix
vector<vector<int>> matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

**Create empty matrix**:
```cpp
int rows = 3, cols = 4;
vector<vector<int>> matrix(rows, vector<int>(cols, 0));
// 3x4 matrix filled with 0
```

**Access elements**:
```cpp
cout << matrix[0][0];  // 1
cout << matrix[1][2];  // 6

matrix[0][0] = 99;     // Modify
```

**Iterate**:
```cpp
for (int i = 0; i < matrix.size(); i++) {
    for (int j = 0; j < matrix[i].size(); j++) {
        cout << matrix[i][j] << " ";
    }
    cout << endl;
}
```

---

## Complete Example Programs

### Example 1: Dynamic Input Array

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> numbers;
    int n;

    cout << "How many numbers? ";
    cin >> n;

    cout << "Enter " << n << " numbers:" << endl;
    for (int i = 0; i < n; i++) {
        int num;
        cin >> num;
        numbers.push_back(num);
    }

    // Calculate sum and average
    int sum = 0;
    for (int num : numbers) {
        sum += num;
    }

    double average = (double)sum / n;

    cout << "Sum: " << sum << endl;
    cout << "Average: " << average << endl;

    return 0;
}
```

---

### Example 2: Remove Duplicates

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> v = {10, 20, 10, 30, 20, 40, 10};

    cout << "Original: ";
    for (int x : v) cout << x << " ";
    cout << endl;

    // Sort first
    sort(v.begin(), v.end());

    // Remove adjacent duplicates
    auto it = unique(v.begin(), v.end());

    // Erase the "removed" elements
    v.erase(it, v.end());

    cout << "After removing duplicates: ";
    for (int x : v) cout << x << " ";
    cout << endl;

    return 0;
}
```

---

### Example 3: Matrix Operations

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    // Create 3x3 matrix
    vector<vector<int>> matrix(3, vector<int>(3));

    // Input
    cout << "Enter 3x3 matrix:" << endl;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cin >> matrix[i][j];
        }
    }

    // Calculate row sums
    cout << "\nRow sums:" << endl;
    for (int i = 0; i < 3; i++) {
        int rowSum = 0;
        for (int j = 0; j < 3; j++) {
            rowSum += matrix[i][j];
        }
        cout << "Row " << i << ": " << rowSum << endl;
    }

    // Calculate column sums
    cout << "\nColumn sums:" << endl;
    for (int j = 0; j < 3; j++) {
        int colSum = 0;
        for (int i = 0; i < 3; i++) {
            colSum += matrix[i][j];
        }
        cout << "Column " << j << ": " << colSum << endl;
    }

    return 0;
}
```

---

## 11. Vectors vs Arrays

| Feature | Array | Vector |
|---------|-------|--------|
| Size | Fixed | Dynamic |
| Memory | Stack or heap | Heap |
| Bounds checking | No | Yes (with `at()`) |
| Functions | Few | Many (push_back, insert, etc.) |
| Speed | Slightly faster | Slightly slower |
| Safety | Unsafe | Safer |
| **Use when** | Size known, need max speed | Size unknown, need flexibility |

**Recommendation**: Use vectors for most cases! Only use arrays when you need absolute maximum performance and know exact size.

---

## Common Mistakes ⚠️

### Mistake 1: Using [] on Empty Vector

```cpp
vector<int> v;
v[0] = 10;  // CRASH! Vector is empty

// FIX:
v.push_back(10);
```

---

### Mistake 2: Infinite Loop with size()

```cpp
vector<int> v = {10, 20, 30};

for (int i = 0; i < v.size(); i++) {
    v.push_back(i);  // BAD! size() keeps growing
}

// FIX: Store size before loop
int n = v.size();
for (int i = 0; i < n; i++) {
    v.push_back(i);
}
```

---

### Mistake 3: Iterator Invalidation

```cpp
vector<int> v = {10, 20, 30};

for (auto it = v.begin(); it != v.end(); ++it) {
    v.push_back(100);  // BAD! Invalidates iterators
}

// FIX: Don't modify vector while iterating
```

---

## Practice Problems

### Problem 1: Merge Two Sorted Vectors
Merge two sorted vectors into one sorted vector.

### Problem 2: Rotate Vector
Rotate vector to the right by k positions.

### Problem 3: Find Missing Number
Given vector of 1 to n with one missing, find the missing number.

### Problem 4: Transpose Matrix
Transpose a 2D vector (swap rows and columns).

### Problem 5: Stock Prices
Given daily stock prices, find maximum profit from one buy and one sell.

---

## Key Takeaways

✅ **Vector = dynamic array** (can grow/shrink)
✅ **Size** = current elements, **Capacity** = allocated space
✅ **`push_back()`** to add, **`pop_back()`** to remove
✅ **Random access** like arrays (O(1))
✅ **Automatic memory management** (no new/delete needed)
✅ **Rich functionality** (sort, find, reverse, etc.)
✅ **2D vectors** for dynamic matrices
✅ **Prefer vectors over arrays** for flexibility

---

## What's Next?

In **Chapter 11**, you'll learn about **Stacks and Queues** - two fundamental data structures with specific access patterns (LIFO and FIFO)!

---

**Chapter Progress**: ✅ Chapters 1-10 Complete | 📖 Next: Chapter 11
