# Chapter 7: Arrays

**What You'll Learn**: What arrays are, WHY they exist, how they're stored in memory, array operations, and 2D arrays.

## Table of Contents

1. [Introduction: The Storage Problem](#introduction-the-storage-problem)
2. [What is an Array?](#1-what-is-an-array)
3. [Declaring and Initializing Arrays](#2-declaring-and-initializing-arrays)
4. [Memory Layout: How Arrays Work](#3-memory-layout-how-arrays-work)
5. [Accessing Array Elements](#4-accessing-array-elements)
6. [Common Array Operations](#5-common-array-operations)
7. [Arrays and Functions](#6-arrays-and-functions)
8. [Multi-Dimensional Arrays (2D Arrays)](#7-multi-dimensional-arrays-2d-arrays)
9. [Array Name is a Pointer!](#8-array-name-is-a-pointer)
10. [Common Array Mistakes](#9-common-array-mistakes)
11. [Complete Example Programs](#complete-example-programs)
12. [Practice Problems](#practice-problems)
13. [Key Takeaways](#key-takeaways)
14. [Arrays vs Other Data Structures](#arrays-vs-other-data-structures)
15. [What's Next?](#whats-next)

---

## Introduction: The Storage Problem

Imagine you need to store test scores for 100 students:

**Without arrays (nightmare!)**:
```cpp
int student1_score = 85;
int student2_score = 90;
int student3_score = 78;
// ... 97 more variables! 😱
```

**Problems**:
- ❌ Need 100 different variable names
- ❌ Can't loop through them easily
- ❌ Hard to pass to functions
- ❌ No way to calculate average without typing 100 names!

**With an array**:
```cpp
int scores[100];  // One name, 100 values!

// Easy to loop through:
for (int i = 0; i < 100; i++) {
    cout << scores[i] << " ";
}
```

**Arrays are like a row of lockers** - numbered, side-by-side, same size, easy to access!

---

## 1. What is an Array?

An **array** is a collection of elements of the **same type**, stored in **contiguous memory** (side-by-side).

**Key Properties**:
1. **Fixed size** (decided at creation, can't grow)
2. **Same type** (all elements must be same type)
3. **Contiguous memory** (elements are neighbors in memory)
4. **Zero-indexed** (first element is at index 0)

---

## 2. Declaring and Initializing Arrays

### Declaration

**Syntax**:
```cpp
type array_name[size];
```

**Examples**:
```cpp
int scores[5];        // Array of 5 integers
double prices[10];    // Array of 10 doubles
char letters[26];     // Array of 26 characters
```

---

### Initialization

**Method 1: Initialize with values**:
```cpp
int arr[5] = {10, 20, 30, 40, 50};
```

**Method 2: Partial initialization** (rest become 0):
```cpp
int arr[5] = {10, 20};  // arr = {10, 20, 0, 0, 0}
```

**Method 3: Size inferred automatically**:
```cpp
int arr[] = {1, 2, 3, 4, 5};  // Size is 5 (compiler counts)
```

**Method 4: Initialize all to zero**:
```cpp
int arr[100] = {0};  // All 100 elements are 0
```

---

## 3. Memory Layout: How Arrays Work

**Code**:
```cpp
int arr[5] = {10, 20, 30, 40, 50};
```

**Memory** (assuming int is 4 bytes):
```
Address    Index    Value
--------   -----    -----
0x1000     arr[0]   10
0x1004     arr[1]   20
0x1008     arr[2]   30
0x100C     arr[3]   40
0x1010     arr[4]   50
```

**Key Observations**:
- Elements are **contiguous** (no gaps)
- Each int takes 4 bytes
- Address increases by 4 each time (size of int)
- `arr[0]` is at address 0x1000
- `arr[1]` is at address 0x1004 (0x1000 + 4)
- `arr[2]` is at address 0x1008 (0x1004 + 4)

**This is WHY array access is O(1) fast!**

---

## 4. Accessing Array Elements

Use **index** (position) inside square brackets:

```cpp
int arr[5] = {10, 20, 30, 40, 50};

cout << arr[0];  // 10 (first element)
cout << arr[2];  // 30 (third element)
cout << arr[4];  // 50 (last element)

arr[1] = 99;     // Change second element
cout << arr[1];  // 99
```

**CRITICAL**: Indices start at **0**, not 1!

```
Index:  0   1   2   3   4
Value: 10  20  30  40  50
```

---

### Array Index Formula

**How does `arr[i]` work?**

```
Address of arr[i] = Base Address + (i × Size of Element)
```

**Example**:
```cpp
int arr[5];  // Base address = 0x1000

arr[3] = ?
Address = 0x1000 + (3 × 4) = 0x1000 + 12 = 0x100C
```

**This is why array access is instant (O(1))** - just math, no searching!

---

## 5. Common Array Operations

### Operation 1: Input Elements

```cpp
int arr[5];

cout << "Enter 5 numbers:" << endl;
for (int i = 0; i < 5; i++) {
    cin >> arr[i];
}
```

---

### Operation 2: Print Elements

```cpp
int arr[5] = {10, 20, 30, 40, 50};

for (int i = 0; i < 5; i++) {
    cout << arr[i] << " ";
}
// Output: 10 20 30 40 50
```

---

### Operation 3: Sum of Elements

```cpp
int arr[5] = {10, 20, 30, 40, 50};
int sum = 0;

for (int i = 0; i < 5; i++) {
    sum += arr[i];
}

cout << "Sum: " << sum << endl;  // Sum: 150
```

---

### Operation 4: Find Maximum

```cpp
int arr[5] = {10, 50, 30, 20, 40};
int max = arr[0];  // Assume first is max

for (int i = 1; i < 5; i++) {
    if (arr[i] > max) {
        max = arr[i];
    }
}

cout << "Maximum: " << max << endl;  // Maximum: 50
```

---

### Operation 5: Search for Element

```cpp
int arr[5] = {10, 20, 30, 40, 50};
int target = 30;
int found_index = -1;

for (int i = 0; i < 5; i++) {
    if (arr[i] == target) {
        found_index = i;
        break;
    }
}

if (found_index != -1) {
    cout << "Found at index " << found_index << endl;
} else {
    cout << "Not found" << endl;
}
```

---

### Operation 6: Reverse Array

```cpp
int arr[5] = {10, 20, 30, 40, 50};
int n = 5;

// Use two pointers
int left = 0;
int right = n - 1;

while (left < right) {
    // Swap
    int temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;

    left++;
    right--;
}

// arr is now {50, 40, 30, 20, 10}
```

**Visual**:
```
Initial:  [10, 20, 30, 40, 50]
           ↑               ↑
          left           right

Swap:     [50, 20, 30, 40, 10]
               ↑       ↑
              left   right

Swap:     [50, 40, 30, 20, 10]
                   ↑
              left/right (stop)
```

---

## 6. Arrays and Functions

### Passing Arrays to Functions

**Arrays are passed by reference** (automatically!):

```cpp
void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
}

void modifyArray(int arr[], int size) {
    arr[0] = 999;  // Changes original!
}

int main() {
    int numbers[5] = {10, 20, 30, 40, 50};

    printArray(numbers, 5);     // 10 20 30 40 50
    modifyArray(numbers, 5);
    printArray(numbers, 5);     // 999 20 30 40 50

    return 0;
}
```

**Why by reference?** Copying large arrays is expensive! Passing address is cheap.

---

### Returning Array Size

**Problem**: Arrays don't know their own size!

```cpp
int arr[5];
// No way to ask "how big are you?"
```

**Solution**: Always pass size as a separate parameter:

```cpp
void processArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {  // Use size parameter
        // ...
    }
}
```

---

## 7. Multi-Dimensional Arrays (2D Arrays)

A **2D array** is an "array of arrays" - like a table with rows and columns.

**Declaration**:
```cpp
int matrix[3][4];  // 3 rows, 4 columns
```

**Visualization**:
```
      Col 0  Col 1  Col 2  Col 3
Row 0   [  ]   [  ]   [  ]   [  ]
Row 1   [  ]   [  ]   [  ]   [  ]
Row 2   [  ]   [  ]   [  ]   [  ]
```

---

### Initialization

```cpp
int matrix[3][3] = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

**Result**:
```
1  2  3
4  5  6
7  8  9
```

---

### Accessing Elements

```cpp
int matrix[3][3] = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

cout << matrix[0][0];  // 1 (row 0, col 0)
cout << matrix[1][2];  // 6 (row 1, col 2)
cout << matrix[2][1];  // 8 (row 2, col 1)

matrix[0][0] = 99;     // Change element
```

---

### Memory Layout of 2D Array

**Code**:
```cpp
int matrix[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};
```

**In memory** (row-major order):
```
Address    Element      Value
--------   --------     -----
0x1000     [0][0]       1
0x1004     [0][1]       2
0x1008     [0][2]       3
0x100C     [1][0]       4
0x1010     [1][1]       5
0x1014     [1][2]       6
```

**Stored as**: `1 2 3 4 5 6` (one row after another, contiguous!)

---

### Looping Through 2D Array

```cpp
int matrix[3][3] = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Nested loop
for (int row = 0; row < 3; row++) {
    for (int col = 0; col < 3; col++) {
        cout << matrix[row][col] << " ";
    }
    cout << endl;  // New line after each row
}
```

**Output**:
```
1 2 3
4 5 6
7 8 9
```

**Think of nested loops**:
- Outer loop (row) = moves DOWN the rows
- Inner loop (col) = moves ACROSS the columns

---

## 8. Array Name is a Pointer!

**Surprise**: The array name is actually a pointer to the first element!

```cpp
int arr[5] = {10, 20, 30, 40, 50};

cout << arr;       // 0x1000 (address of first element)
cout << &arr[0];   // 0x1000 (same!)
cout << *arr;      // 10 (dereference = first element)
```

**This means**:
```cpp
arr[i]  ≡  *(arr + i)  // Equivalent!

arr[0]  ≡  *arr
arr[1]  ≡  *(arr + 1)
arr[2]  ≡  *(arr + 2)
```

---

## 9. Common Array Mistakes ⚠️

### Mistake 1: Index Out of Bounds

```cpp
int arr[5] = {10, 20, 30, 40, 50};

cout << arr[5];   // ERROR! Index 5 doesn't exist (0-4 only)
cout << arr[10];  // CRASH! Way out of bounds
```

**C++ doesn't check bounds!** This is a common source of bugs.

---

### Mistake 2: Not Initializing

```cpp
int arr[5];       // Contains garbage values!

for (int i = 0; i < 5; i++) {
    cout << arr[i];  // Prints random junk
}

// FIX:
int arr[5] = {0};  // All zeros
```

---

### Mistake 3: Comparing Arrays with ==

```cpp
int arr1[3] = {1, 2, 3};
int arr2[3] = {1, 2, 3};

if (arr1 == arr2) {  // WRONG! Compares addresses, not contents
    cout << "Equal";
}

// FIX: Compare element by element
bool equal = true;
for (int i = 0; i < 3; i++) {
    if (arr1[i] != arr2[i]) {
        equal = false;
        break;
    }
}
```

---

### Mistake 4: Forgetting Array Size

```cpp
void process(int arr[]) {
    for (int i = 0; i < ???; i++) {  // Size unknown!
        // ...
    }
}

// FIX: Always pass size
void process(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        // ...
    }
}
```

---

## Complete Example Programs

### Example 1: Calculate Average

```cpp
#include <iostream>
using namespace std;

int main() {
    int scores[5];
    int sum = 0;

    cout << "Enter 5 test scores:" << endl;
    for (int i = 0; i < 5; i++) {
        cin >> scores[i];
        sum += scores[i];
    }

    double average = sum / 5.0;
    cout << "Average: " << average << endl;

    return 0;
}
```

---

### Example 2: Find Second Largest

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[5] = {10, 50, 30, 20, 40};
    int n = 5;

    int largest = arr[0];
    int secondLargest = -1;

    for (int i = 1; i < n; i++) {
        if (arr[i] > largest) {
            secondLargest = largest;
            largest = arr[i];
        } else if (arr[i] > secondLargest && arr[i] != largest) {
            secondLargest = arr[i];
        }
    }

    cout << "Largest: " << largest << endl;
    cout << "Second Largest: " << secondLargest << endl;

    return 0;
}
```

---

### Example 3: Matrix Transpose

```cpp
#include <iostream>
using namespace std;

int main() {
    int matrix[3][3] = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };

    cout << "Original Matrix:" << endl;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << matrix[i][j] << " ";
        }
        cout << endl;
    }

    // Transpose
    int transpose[3][3];
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            transpose[j][i] = matrix[i][j];
        }
    }

    cout << "\nTransposed Matrix:" << endl;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << transpose[i][j] << " ";
        }
        cout << endl;
    }

    return 0;
}
```

---

## Practice Problems

### Problem 1: Rotate Array
Rotate an array to the right by k positions.
Example: `[1,2,3,4,5]` rotated by 2 → `[4,5,1,2,3]`

### Problem 2: Remove Duplicates
Remove duplicates from a sorted array in-place.

### Problem 3: Merge Two Sorted Arrays
Given two sorted arrays, merge them into one sorted array.

### Problem 4: Matrix Diagonal Sum
Find the sum of diagonal elements in a square matrix.

### Problem 5: Spiral Matrix Print
Print a 2D matrix in spiral order (clockwise from outside to inside).

### Problem 6: Array Leaders
Find all leader elements (element greater than all elements to its right).

---

## Key Takeaways

✅ **Array = fixed-size collection of same-type elements**
✅ **Zero-indexed** (first element is at index 0)
✅ **Contiguous memory** (elements are neighbors)
✅ **Fast access** (O(1)) using index
✅ **Array name = pointer** to first element
✅ **Passed by reference** to functions (automatically)
✅ **Always pass size** when using functions
✅ **2D arrays** stored in row-major order
✅ **No bounds checking** in C++ (be careful!)

---

## Arrays vs Other Data Structures

| Feature | Array | Vector (Chapter 10) |
|---------|-------|---------------------|
| Size | Fixed | Dynamic (can grow) |
| Memory | Stack | Heap |
| Speed | Fast | Slightly slower |
| Safety | No bounds check | Bounds checking available |

---

## What's Next?

In **[Chapter 8](chapter-08-strings.md)**, you'll learn about strings - how they're stored, C-strings vs std::string, and string manipulation!

---

**Chapter Progress**: ✅ Chapters 1-7 Complete | 📖 Next: Chapter 8
