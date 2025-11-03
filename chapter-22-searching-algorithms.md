# Chapter 22: Searching Algorithms

**What You'll Learn**: Different searching techniques, WHY binary search is so fast, search variations, and how to search in complex data structures.

## Table of Contents

1. [Linear Search: Check Everything](#1-linear-search-check-everything)
2. [Binary Search: Divide and Conquer](#2-binary-search-divide-and-conquer)
3. [Binary Search Variations](#3-binary-search-variations)
4. [Ternary Search: Divide into Three](#4-ternary-search-divide-into-three)
5. [Interpolation Search: Smart Guessing](#5-interpolation-search-smart-guessing)
6. [Search in 2D Matrix](#6-search-in-2d-matrix)
7. [Jump Search: Block-by-Block](#7-jump-search-block-by-block)
8. [Exponential Search: Doubling Range](#8-exponential-search-doubling-range)
9. [Search Algorithm Comparison](#9-search-algorithm-comparison)
10. [Complete Example Programs](#complete-example-programs)
11. [Common Mistakes](#common-mistakes)
12. [Practice Problems](#practice-problems)
13. [Key Takeaways](#key-takeaways)

---

## Introduction: The Finding Problem

You're looking for a word in a dictionary. Two approaches:

**Approach A**: Start from page 1, check every word until you find it.
- "aardvark" - No
- "abandon" - No
- "ability" - No
- ... (checking thousands of words)

**Approach B**: Open to middle, check if your word comes before/after, eliminate half the pages, repeat.
- Open to middle: "monkey" - your word "python" comes after
- Eliminate first half, open middle of second half: "tiger" - comes before
- Eliminate second half of that section, open middle: "python" - FOUND!

**Approach A = Linear Search** (slow, O(n))
**Approach B = Binary Search** (fast, O(log n))

**This is WHY we need different search algorithms** - right approach can be 1000× faster!

---

## 1. Linear Search: Check Everything

**Idea**: Check each element one by one until you find the target.

**Think of it like**: Looking for a sock in a messy drawer - check each item until you find it.

**Visual** (searching for 7 in [4, 2, 7, 1, 9]):

```
[4, 2, 7, 1, 9]
 ↑
Check 4 - Not 7

[4, 2, 7, 1, 9]
    ↑
Check 2 - Not 7

[4, 2, 7, 1, 9]
       ↑
Check 7 - FOUND! ✓
```

**Code**:

```cpp
#include <iostream>
using namespace std;

int linearSearch(int arr[], int n, int target) {
    for (int i = 0; i < n; i++) {
        if (arr[i] == target) {
            return i;  // Return index
        }
    }
    return -1;  // Not found
}

int main() {
    int arr[] = {4, 2, 7, 1, 9};
    int n = 5;
    int target = 7;

    int result = linearSearch(arr, n, target);

    if (result != -1) {
        cout << "Found at index " << result << endl;  // Found at index 2
    } else {
        cout << "Not found" << endl;
    }

    return 0;
}
```

**Time Complexity**:
- **Best case**: O(1) - element at index 0
- **Average case**: O(n/2) = O(n)
- **Worst case**: O(n) - element at end or not present

**Space Complexity**: O(1)

**When to use**:
- Small arrays (n < 100)
- Unsorted data (no choice!)
- Searching once (not worth sorting first)

---

## 2. Binary Search: Divide and Conquer

**Idea**: If array is sorted, compare target with middle element, eliminate half the array, repeat.

**Think of it like**: Guessing a number 1-100. Each guess tells you "higher" or "lower" - you can find it in 7 guesses max!

**Visual** (searching for 7 in [1, 2, 4, 7, 9]):

```
Step 1:
[1, 2, 4, 7, 9]
       ↑
mid = 4, target = 7
7 > 4, search right half

Step 2:
[7, 9]
 ↑
mid = 7, target = 7
FOUND! ✓
```

**Code**:

```cpp
int binarySearch(int arr[], int n, int target) {
    int left = 0;
    int right = n - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;  // Avoid overflow

        if (arr[mid] == target) {
            return mid;  // Found!
        }
        else if (arr[mid] < target) {
            left = mid + 1;  // Search right half
        }
        else {
            right = mid - 1;  // Search left half
        }
    }

    return -1;  // Not found
}
```

**Recursive Version**:

```cpp
int binarySearchRecursive(int arr[], int left, int right, int target) {
    if (left > right) return -1;  // Base case: not found

    int mid = left + (right - left) / 2;

    if (arr[mid] == target) return mid;
    if (arr[mid] < target) {
        return binarySearchRecursive(arr, mid + 1, right, target);
    }
    return binarySearchRecursive(arr, left, mid - 1, target);
}
```

**Time Complexity**:
- **All cases**: O(log n)
- n = 1,000 → ~10 comparisons
- n = 1,000,000 → ~20 comparisons!

**Space Complexity**:
- Iterative: O(1)
- Recursive: O(log n) - call stack

**Critical**: **Array MUST be sorted!**

---

## 3. Binary Search Variations

### Variation 1: Find First Occurrence

**Problem**: Array has duplicates, find **first** occurrence of target.

**Example**: [1, 2, 2, 2, 3, 4], target = 2 → return 1 (not 2 or 3)

```cpp
int findFirst(int arr[], int n, int target) {
    int left = 0, right = n - 1;
    int result = -1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] == target) {
            result = mid;        // Found, but keep searching left
            right = mid - 1;     // Continue in left half
        }
        else if (arr[mid] < target) {
            left = mid + 1;
        }
        else {
            right = mid - 1;
        }
    }

    return result;
}
```

**Key insight**: Even after finding target, keep searching left to find first occurrence.

---

### Variation 2: Find Last Occurrence

**Problem**: Find **last** occurrence of target.

**Example**: [1, 2, 2, 2, 3, 4], target = 2 → return 3

```cpp
int findLast(int arr[], int n, int target) {
    int left = 0, right = n - 1;
    int result = -1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] == target) {
            result = mid;        // Found, but keep searching right
            left = mid + 1;      // Continue in right half
        }
        else if (arr[mid] < target) {
            left = mid + 1;
        }
        else {
            right = mid - 1;
        }
    }

    return result;
}
```

---

### Variation 3: Count Occurrences

**Problem**: Count how many times target appears.

**Solution**: Find first and last occurrence, count = last - first + 1

```cpp
int countOccurrences(int arr[], int n, int target) {
    int first = findFirst(arr, n, target);
    if (first == -1) return 0;  // Not found

    int last = findLast(arr, n, target);
    return last - first + 1;
}

// Example: [1, 2, 2, 2, 3, 4], target = 2
// first = 1, last = 3, count = 3 - 1 + 1 = 3 ✓
```

---

### Variation 4: Search in Rotated Sorted Array

**Problem**: Array was sorted, then rotated. Find target.

**Example**: [4, 5, 6, 7, 0, 1, 2] (rotated [0, 1, 2, 4, 5, 6, 7])

**Idea**: One half is always sorted. Check which half, then decide where to search.

```cpp
int searchRotated(int arr[], int n, int target) {
    int left = 0, right = n - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] == target) return mid;

        // Check which half is sorted
        if (arr[left] <= arr[mid]) {  // Left half is sorted
            if (target >= arr[left] && target < arr[mid]) {
                right = mid - 1;  // Target in left half
            } else {
                left = mid + 1;   // Target in right half
            }
        }
        else {  // Right half is sorted
            if (target > arr[mid] && target <= arr[right]) {
                left = mid + 1;   // Target in right half
            } else {
                right = mid - 1;  // Target in left half
            }
        }
    }

    return -1;
}
```

**Visual** (searching for 0):

```
[4, 5, 6, 7, 0, 1, 2]
          ↑
mid = 7, left half [4,5,6,7] is sorted
0 not in [4,7], search right half

[0, 1, 2]
 ↑
mid = 0, FOUND! ✓
```

---

## 4. Ternary Search: Divide into Three

**Idea**: Like binary search but divide into **three** parts instead of two.

**When to use**: Finding maximum/minimum in **unimodal** functions (single peak/valley).

```cpp
int ternarySearch(int arr[], int left, int right, int target) {
    if (left <= right) {
        int mid1 = left + (right - left) / 3;
        int mid2 = right - (right - left) / 3;

        if (arr[mid1] == target) return mid1;
        if (arr[mid2] == target) return mid2;

        if (target < arr[mid1]) {
            return ternarySearch(arr, left, mid1 - 1, target);
        }
        else if (target > arr[mid2]) {
            return ternarySearch(arr, mid2 + 1, right, target);
        }
        else {
            return ternarySearch(arr, mid1 + 1, mid2 - 1, target);
        }
    }

    return -1;
}
```

**Time Complexity**: O(log₃ n) - similar to binary search

**Note**: Binary search is usually faster in practice (fewer comparisons per iteration).

---

## 5. Interpolation Search: Smart Guessing

**Idea**: Instead of always choosing middle, estimate position based on value.

**Think of it like**: Looking up "Smith" in phone book - you don't open to middle, you open near end because "S" is near end of alphabet!

**Formula**:
```
pos = left + ((target - arr[left]) / (arr[right] - arr[left])) * (right - left)
```

**Code**:

```cpp
int interpolationSearch(int arr[], int n, int target) {
    int left = 0, right = n - 1;

    while (left <= right && target >= arr[left] && target <= arr[right]) {
        if (left == right) {
            if (arr[left] == target) return left;
            return -1;
        }

        // Estimate position
        int pos = left + ((target - arr[left]) * (right - left)) /
                         (arr[right] - arr[left]);

        if (arr[pos] == target) {
            return pos;
        }
        else if (arr[pos] < target) {
            left = pos + 1;
        }
        else {
            right = pos - 1;
        }
    }

    return -1;
}
```

**Time Complexity**:
- **Best/Average**: O(log log n) - faster than binary search!
- **Worst case**: O(n) - if values not uniformly distributed

**When to use**: Uniformly distributed sorted data (e.g., dictionary, phone book)

---

## 6. Search in 2D Matrix

### Problem 1: Matrix Sorted Row-Wise and Column-Wise

**Example**:
```
[1,  4,  7,  11]
[2,  5,  8,  12]
[3,  6,  9,  16]
[10, 13, 14, 17]
```

**Idea**: Start from top-right corner. If target is smaller, go left. If larger, go down.

```cpp
bool searchMatrix(int matrix[][4], int rows, int cols, int target) {
    int i = 0;           // Start at top-right
    int j = cols - 1;

    while (i < rows && j >= 0) {
        if (matrix[i][j] == target) {
            return true;  // Found!
        }
        else if (matrix[i][j] > target) {
            j--;  // Target is smaller, go left
        }
        else {
            i++;  // Target is larger, go down
        }
    }

    return false;
}
```

**Visual** (searching for 8):

```
Start: [1,  4,  7,  11] ← Start here (11)
       [2,  5,  8,  12]   8 < 11, go left
       [3,  6,  9,  16]
       [10, 13, 14, 17]

Step 2: [1,  4,  7,  11]
        [2,  5,  8,  12] ← Now at 7
        [3,  6,  9,  16]   8 > 7, go down
        [10, 13, 14, 17]

Step 3: [1,  4,  7,  11]
        [2,  5,  8,  12] ← At 8, FOUND! ✓
        [3,  6,  9,  16]
        [10, 13, 14, 17]
```

**Time Complexity**: O(rows + cols)

---

### Problem 2: Each Row Sorted, First Element of Row > Last of Previous

**Example**:
```
[1,  3,  5,  7]
[10, 11, 16, 20]
[23, 30, 34, 60]
```

**Idea**: Treat as 1D sorted array, use binary search!

```cpp
bool searchMatrix2(int matrix[][4], int rows, int cols, int target) {
    int left = 0;
    int right = rows * cols - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        // Convert 1D index to 2D
        int row = mid / cols;
        int col = mid % cols;
        int value = matrix[row][col];

        if (value == target) {
            return true;
        }
        else if (value < target) {
            left = mid + 1;
        }
        else {
            right = mid - 1;
        }
    }

    return false;
}
```

**Time Complexity**: O(log(rows × cols))

---

## 7. Jump Search: Block-by-Block

**Idea**: Jump ahead by fixed steps, then do linear search in block.

**Optimal jump size**: √n

```cpp
#include <cmath>

int jumpSearch(int arr[], int n, int target) {
    int step = sqrt(n);
    int prev = 0;

    // Jump to find block
    while (arr[min(step, n) - 1] < target) {
        prev = step;
        step += sqrt(n);
        if (prev >= n) return -1;
    }

    // Linear search in block
    while (arr[prev] < target) {
        prev++;
        if (prev == min(step, n)) return -1;
    }

    if (arr[prev] == target) return prev;
    return -1;
}
```

**Time Complexity**: O(√n)

**When to use**: When binary search is not possible (e.g., linked lists) but data is sorted.

---

## 8. Exponential Search: Doubling Range

**Idea**: Find range where element exists by doubling index, then binary search.

```cpp
int exponentialSearch(int arr[], int n, int target) {
    if (arr[0] == target) return 0;

    // Find range by doubling
    int i = 1;
    while (i < n && arr[i] <= target) {
        i *= 2;
    }

    // Binary search in found range
    return binarySearch(arr, i/2, min(i, n-1), target);
}
```

**Time Complexity**: O(log n)

**When to use**: Unbounded/infinite arrays (don't know size)

---

## 9. Search Algorithm Comparison

| Algorithm | Time (Avg) | Space | Prerequisites | Best For |
|-----------|------------|-------|---------------|----------|
| **Linear** | O(n) | O(1) | None | Unsorted, small arrays |
| **Binary** | O(log n) | O(1) | Sorted | General sorted search |
| **Ternary** | O(log n) | O(log n) | Sorted | Finding peak/valley |
| **Interpolation** | O(log log n) | O(1) | Sorted, uniform | Phone book, dictionary |
| **Jump** | O(√n) | O(1) | Sorted | Linked lists |
| **Exponential** | O(log n) | O(1) | Sorted | Unbounded arrays |

---

## Complete Example Programs

### Example 1: Find Peak Element

**Problem**: Array has peaks (element greater than neighbors). Find any peak.

```cpp
int findPeak(int arr[], int n) {
    int left = 0, right = n - 1;

    while (left < right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] < arr[mid + 1]) {
            left = mid + 1;  // Peak is on right
        } else {
            right = mid;     // Peak is on left or mid
        }
    }

    return left;  // Peak index
}

// Example: [1, 3, 20, 4, 1, 0]
//          Peak at index 2 (value 20)
```

---

### Example 2: Search Insert Position

**Problem**: Find index where target should be inserted to keep array sorted.

```cpp
int searchInsert(int arr[], int n, int target) {
    int left = 0, right = n - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] == target) {
            return mid;  // Found exact position
        }
        else if (arr[mid] < target) {
            left = mid + 1;
        }
        else {
            right = mid - 1;
        }
    }

    return left;  // Insert position
}

// Example: [1, 3, 5, 6], target = 2
// Result: 1 (insert between 1 and 3)
```

---

## Common Mistakes

### Mistake 1: Using Binary Search on Unsorted Data

```cpp
int arr[] = {5, 2, 8, 1, 9};  // UNSORTED!
int result = binarySearch(arr, 5, 7);  // Wrong results!

// Fix: Sort first
sort(arr, arr + 5);
int result = binarySearch(arr, 5, 7);  // Correct
```

### Mistake 2: Integer Overflow in Mid Calculation

```cpp
// Bad: Can overflow if left + right > INT_MAX
int mid = (left + right) / 2;

// Good: Avoids overflow
int mid = left + (right - left) / 2;
```

### Mistake 3: Wrong Loop Condition

```cpp
// Bug: Infinite loop if target not found
while (left < right) {  // Missing =
    // ...
}

// Fix: Use <=
while (left <= right) {
    // ...
}
```

---

## Practice Problems

### Problem 1: Square Root
Find integer square root using binary search (e.g., sqrt(10) = 3).

### Problem 2: Find Minimum in Rotated Array
Find minimum element in rotated sorted array.

### Problem 3: Search for Range
Find starting and ending position of target in sorted array with duplicates.

### Problem 4: Find Missing Number
Array of n-1 unique numbers from 1 to n. Find missing number in O(log n).

### Problem 5: Median of Two Sorted Arrays
Find median of two sorted arrays in O(log(min(n, m))).

---

## Key Takeaways

✅ **Linear search** is O(n) - use for **unsorted** data or **small** arrays
✅ **Binary search** is O(log n) - requires **sorted** data
✅ **Binary search is 1000× faster** than linear for large arrays
✅ **Variations exist** for first/last occurrence, rotated arrays, 2D matrices
✅ **Interpolation search** can be O(log log n) for **uniform data**
✅ **Always check**: Is data sorted? Can I use binary search?
✅ **Mid calculation**: Use `left + (right - left) / 2` to avoid overflow

---

## Search Decision Tree

```
Is data sorted?
├─ NO → Linear Search O(n)
└─ YES
    ├─ Is data uniformly distributed?
    │   └─ YES → Interpolation Search O(log log n)
    └─ NO
        ├─ Standard 1D array?
        │   └─ YES → Binary Search O(log n)
        └─ Special structure?
            ├─ 2D matrix → Matrix Search O(rows + cols)
            ├─ Rotated → Modified Binary Search
            └─ Unbounded → Exponential Search
```

---

## What's Next?

In **[Chapter 23](chapter-23-recursion.md)**, you'll learn about **Recursion** - one of the most powerful problem-solving techniques that powers binary search, merge sort, and much more!

---

**Chapter Progress**: ✅ Chapters 1-22 Complete | 📖 Next: Chapter 23
