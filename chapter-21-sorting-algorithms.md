# Chapter 21: Sorting Algorithms

**What You'll Learn**: WHY different sorting algorithms exist, how they work, their time complexity, and when to use each one.

## Table of Contents

1. [Why Sorting Matters](#1-why-sorting-matters)
2. [Bubble Sort: The Simplest (but Slowest)](#2-bubble-sort-the-simplest-but-slowest)
3. [Selection Sort: Find and Place](#3-selection-sort-find-and-place)
4. [Insertion Sort: Build Sorted Array](#4-insertion-sort-build-sorted-array)
5. [Merge Sort: Divide and Conquer](#5-merge-sort-divide-and-conquer)
6. [Quick Sort: Pivot and Partition](#6-quick-sort-pivot-and-partition)
7. [Algorithm Comparison Table](#7-algorithm-comparison-table)
8. [Choosing the Right Algorithm](#8-choosing-the-right-algorithm)
9. [Complete Example: Sort Student Grades](#9-complete-example-sort-student-grades)
10. [Common Mistakes](#common-mistakes)
11. [Practice Problems](#practice-problems)
12. [Key Takeaways](#key-takeaways)

---

## Introduction: The Ordering Problem

Imagine you have a deck of shuffled playing cards. You want them sorted by value (2, 3, 4, ..., K, A).

**Different people sort differently**:
- **Person A**: Find the smallest card, put it first. Then find next smallest. Repeat. (Selection Sort)
- **Person B**: Compare adjacent cards, swap if wrong order. Keep doing passes. (Bubble Sort)
- **Person C**: Pick cards one by one, insert each into correct position. (Insertion Sort)
- **Person D**: Split deck in half recursively, sort halves, merge them. (Merge Sort)
- **Person E**: Pick a middle value, partition around it, sort recursively. (Quick Sort)

**Which is fastest?** It depends on the situation!

**This is WHY multiple sorting algorithms exist** - different problems need different approaches.

---

## 1. Why Sorting Matters

**Real-world applications**:
- Search engines: Sort results by relevance
- E-commerce: Sort products by price/rating
- Databases: Sort records for faster queries
- File systems: Sort files by name/date
- Social media: Sort posts by timestamp

**Key insight**: Sorted data enables **binary search** (O(log n) instead of O(n))!

---

## 2. Bubble Sort: The Simplest (but Slowest)

**Idea**: Repeatedly compare adjacent elements and swap if they're in wrong order. Larger values "bubble up" to the end.

**Think of it like**: Bubbles in water rising to the surface - heavy elements sink to the right.

**Visual** (sorting [5, 2, 8, 1, 9]):

```
Pass 1:
[5, 2, 8, 1, 9]  → Compare 5 and 2, swap
[2, 5, 8, 1, 9]  → Compare 5 and 8, no swap
[2, 5, 8, 1, 9]  → Compare 8 and 1, swap
[2, 5, 1, 8, 9]  → Compare 8 and 9, no swap
[2, 5, 1, 8, 9]  → Largest (9) bubbled to end!

Pass 2:
[2, 5, 1, 8, 9]  → Compare 2 and 5, no swap
[2, 5, 1, 8, 9]  → Compare 5 and 1, swap
[2, 1, 5, 8, 9]  → Compare 5 and 8, no swap
[2, 1, 5, 8, 9]  → 8 already in place

Pass 3:
[2, 1, 5, 8, 9]  → Compare 2 and 1, swap
[1, 2, 5, 8, 9]  → Done! ✓
```

**Code**:

```cpp
#include <iostream>
using namespace std;

void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n-1; i++) {           // n-1 passes
        bool swapped = false;

        for (int j = 0; j < n-i-1; j++) {     // Compare adjacent
            if (arr[j] > arr[j+1]) {
                swap(arr[j], arr[j+1]);       // Swap if wrong order
                swapped = true;
            }
        }

        if (!swapped) break;  // Optimization: Stop if no swaps
    }
}

int main() {
    int arr[] = {5, 2, 8, 1, 9};
    int n = 5;

    bubbleSort(arr, n);

    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";  // 1 2 5 8 9
    }

    return 0;
}
```

**Time Complexity**:
- **Best case**: O(n) - already sorted, one pass
- **Average case**: O(n²) - random order
- **Worst case**: O(n²) - reverse sorted

**Space Complexity**: O(1) - in-place sorting

**When to use**: Almost never! Only for teaching or very small arrays (n < 10).

---

## 3. Selection Sort: Find and Place

**Idea**: Repeatedly find the minimum element from unsorted part and place it at the beginning.

**Think of it like**: Picking the smallest card from your hand and placing it in a sorted pile.

**Visual** (sorting [5, 2, 8, 1, 9]):

```
[5, 2, 8, 1, 9]  → Find min (1), swap with first
[1, 2, 8, 5, 9]  → Find min in rest (2), already in place
[1, 2, 8, 5, 9]  → Find min in rest (5), swap with third
[1, 2, 5, 8, 9]  → Find min in rest (8), already in place
[1, 2, 5, 8, 9]  → Done! ✓

 ↑           ↑
Sorted    Unsorted
```

**Code**:

```cpp
void selectionSort(int arr[], int n) {
    for (int i = 0; i < n-1; i++) {
        int minIndex = i;

        // Find minimum in unsorted part
        for (int j = i+1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }

        // Swap with first unsorted element
        swap(arr[i], arr[minIndex]);
    }
}
```

**Time Complexity**:
- **All cases**: O(n²) - always does n² comparisons
- Even if sorted, still checks everything!

**Space Complexity**: O(1)

**When to use**:
- Small arrays where simplicity matters
- When memory writes are expensive (only n swaps, not n² like bubble)

---

## 4. Insertion Sort: Build Sorted Array

**Idea**: Build sorted array one element at a time, inserting each new element into its correct position.

**Think of it like**: Sorting playing cards in your hand - pick a card, slide it into the right spot.

**Visual** (sorting [5, 2, 8, 1, 9]):

```
[5 | 2, 8, 1, 9]  → 5 is "sorted"
[2, 5 | 8, 1, 9]  → Insert 2 before 5
[2, 5, 8 | 1, 9]  → Insert 8 after 5
[1, 2, 5, 8 | 9]  → Insert 1 at start
[1, 2, 5, 8, 9]   → Insert 9 at end ✓

  Sorted | Unsorted
```

**Code**:

```cpp
void insertionSort(int arr[], int n) {
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        // Shift elements greater than key to the right
        while (j >= 0 && arr[j] > key) {
            arr[j+1] = arr[j];
            j--;
        }

        arr[j+1] = key;  // Insert key in correct position
    }
}
```

**Time Complexity**:
- **Best case**: O(n) - already sorted
- **Average case**: O(n²)
- **Worst case**: O(n²) - reverse sorted

**Space Complexity**: O(1)

**When to use**:
- Small arrays (n < 50)
- Nearly sorted data (very efficient!)
- Online algorithms (sorting as data arrives)

**Real-world**: Used in `std::sort` for small subarrays!

---

## 5. Merge Sort: Divide and Conquer

**Idea**: Divide array in half recursively until single elements, then merge sorted halves.

**Think of it like**: Organizing a messy pile of papers by splitting into smaller piles, sorting each, then merging.

**Visual** (sorting [5, 2, 8, 1, 9, 3]):

```
Divide:
                [5, 2, 8, 1, 9, 3]
                /                \
        [5, 2, 8]              [1, 9, 3]
        /      \                /      \
    [5]      [2, 8]          [1]      [9, 3]
             /    \                    /    \
           [2]    [8]               [9]    [3]

Conquer (Merge):
           [2]    [8]               [9]    [3]
             \    /                    \    /
            [2, 8]                    [3, 9]
        /      \                /      \
    [5]      [2, 8]          [1]      [3, 9]
        \      /                \      /
      [2, 5, 8]              [1, 3, 9]
                \                /
            [1, 2, 3, 5, 8, 9]  ✓
```

**Code**:

```cpp
void merge(int arr[], int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // Create temp arrays
    int L[n1], R[n2];

    for (int i = 0; i < n1; i++) L[i] = arr[left + i];
    for (int i = 0; i < n2; i++) R[i] = arr[mid + 1 + i];

    // Merge back
    int i = 0, j = 0, k = left;

    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k++] = L[i++];
        } else {
            arr[k++] = R[j++];
        }
    }

    // Copy remaining
    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}

void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;

        mergeSort(arr, left, mid);      // Sort left half
        mergeSort(arr, mid + 1, right); // Sort right half
        merge(arr, left, mid, right);   // Merge them
    }
}
```

**Time Complexity**:
- **All cases**: O(n log n) - guaranteed!
- Dividing: log n levels
- Merging: n work per level
- Total: n × log n

**Space Complexity**: O(n) - needs extra arrays for merging

**When to use**:
- Large datasets
- When O(n log n) guarantee is needed
- Linked lists (better than quick sort)
- External sorting (files too big for memory)

---

## 6. Quick Sort: Pivot and Partition

**Idea**: Pick a "pivot" element, partition array so smaller elements go left, larger go right, then sort recursively.

**Think of it like**: Organizing students by height - pick someone as reference, shorter go left, taller go right.

**Visual** (sorting [5, 2, 8, 1, 9], pivot = 5):

```
[5, 2, 8, 1, 9]  → Choose pivot (5)
[2, 1, 5, 8, 9]  → Partition: ≤5 left, >5 right
  ↑    ↑    ↑
 ≤5  pivot  >5

Recursively sort left:  [2, 1]  →  [1, 2]
Recursively sort right: [8, 9]  →  [8, 9]

Result: [1, 2, 5, 8, 9] ✓
```

**Code**:

```cpp
int partition(int arr[], int low, int high) {
    int pivot = arr[high];  // Choose last element as pivot
    int i = low - 1;        // Index of smaller element

    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }

    swap(arr[i+1], arr[high]);  // Place pivot in correct position
    return i + 1;
}

void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);

        quickSort(arr, low, pi - 1);   // Sort left of pivot
        quickSort(arr, pi + 1, high);  // Sort right of pivot
    }
}
```

**Time Complexity**:
- **Best case**: O(n log n) - balanced partitions
- **Average case**: O(n log n)
- **Worst case**: O(n²) - already sorted, bad pivot choice!

**Space Complexity**: O(log n) - recursion stack

**Optimizations**:
- Choose random pivot (avoids worst case)
- Use median-of-three pivot
- Switch to insertion sort for small subarrays

**When to use**:
- General-purpose sorting (fastest in practice!)
- In-place sorting needed
- Cache-friendly access patterns

**Real-world**: Default algorithm in `std::sort` and many languages!

---

## 7. Algorithm Comparison Table

| Algorithm | Best | Average | Worst | Space | Stable? | Notes |
|-----------|------|---------|-------|-------|---------|-------|
| **Bubble** | O(n) | O(n²) | O(n²) | O(1) | Yes | Teaching only |
| **Selection** | O(n²) | O(n²) | O(n²) | O(1) | No | Few swaps |
| **Insertion** | O(n) | O(n²) | O(n²) | O(1) | Yes | Good for small/nearly sorted |
| **Merge** | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes | Guaranteed O(n log n) |
| **Quick** | O(n log n) | O(n log n) | O(n²) | O(log n) | No | Fastest in practice |

**Stable**: Maintains relative order of equal elements (important for multi-key sorting).

---

## 8. Choosing the Right Algorithm

**Decision Tree**:

```
Is array very small (n < 50)?
├─ YES → Insertion Sort
└─ NO
    ├─ Need guaranteed O(n log n)?
    │   └─ YES → Merge Sort
    └─ NO
        ├─ Need in-place sorting?
        │   └─ YES → Quick Sort
        └─ NO → Use std::sort (hybrid approach)
```

**Real-world**: `std::sort` uses **Introsort** (hybrid):
- Quick sort for most cases
- Heap sort if recursion depth gets too deep (avoids O(n²))
- Insertion sort for small subarrays

---

## 9. Complete Example: Sort Student Grades

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Student {
    string name;
    int grade;
};

// Custom comparator
bool compareGrade(Student a, Student b) {
    return a.grade > b.grade;  // Descending order
}

int main() {
    vector<Student> students = {
        {"Alice", 85},
        {"Bob", 92},
        {"Charlie", 78},
        {"Diana", 92},
        {"Eve", 88}
    };

    // Sort using std::sort (O(n log n))
    sort(students.begin(), students.end(), compareGrade);

    cout << "Students sorted by grade:" << endl;
    for (const Student& s : students) {
        cout << s.name << ": " << s.grade << endl;
    }

    // Output:
    // Bob: 92
    // Diana: 92
    // Eve: 88
    // Alice: 85
    // Charlie: 78

    return 0;
}
```

---

## Common Mistakes

### Mistake 1: Using Bubble Sort in Production

```cpp
// Bad for n > 50
void sortData(int arr[], int n) {
    bubbleSort(arr, n);  // O(n²) - too slow!
}

// Good - use std::sort
void sortData(int arr[], int n) {
    sort(arr, arr + n);  // O(n log n)
}
```

### Mistake 2: Forgetting Base Case in Recursion

```cpp
// Bug: Infinite recursion!
void quickSort(int arr[], int low, int high) {
    int pi = partition(arr, low, high);
    quickSort(arr, low, pi - 1);
    quickSort(arr, pi + 1, high);
    // Missing: if (low < high) check!
}
```

### Mistake 3: Not Checking for Sorted Data

```cpp
// Inefficient on nearly sorted data
quickSort(arr, n);  // O(n²) on sorted input!

// Better: Use insertion sort for nearly sorted
insertionSort(arr, n);  // O(n) on nearly sorted!
```

---

## Practice Problems

### Problem 1: Count Inversions
Count how many swaps are needed to sort an array (bubble sort metric).

### Problem 2: Sort 0s, 1s, 2s
Sort array containing only 0, 1, 2 in single pass (Dutch Flag problem).

### Problem 3: Kth Largest Element
Find kth largest element without fully sorting (use quick select).

### Problem 4: Sort by Frequency
Sort array elements by their frequency of occurrence.

### Problem 5: Merge K Sorted Arrays
Merge multiple sorted arrays into one sorted array efficiently.

---

## Key Takeaways

✅ **Simple sorts** (bubble, selection, insertion) are **O(n²)** - use only for small arrays
✅ **Merge sort** is **O(n log n) guaranteed** but uses **O(n) extra space**
✅ **Quick sort** is **fastest in practice** but **worst case O(n²)**
✅ **Insertion sort** is **best for nearly sorted data**
✅ **std::sort** uses **hybrid approach** (quick + heap + insertion)
✅ **Choose algorithm based on**: size, memory, stability needs
✅ **Divide-and-conquer** (merge/quick) beats **simple comparison** sorts

---

## Algorithm Complexity Cheat Sheet

```
Input Size          | Recommended Algorithm
--------------------|---------------------
n < 10              | Insertion sort
10 ≤ n < 50         | Insertion sort
n ≥ 50, random      | Quick sort / std::sort
n ≥ 50, nearly sorted| Insertion sort
Need guaranteed time| Merge sort
Limited memory      | Quick sort (in-place)
Need stability      | Merge sort
```

---

## What's Next?

In **[Chapter 22](chapter-22-searching-algorithms.md)**, you'll learn about **Searching Algorithms** - how to efficiently find elements in sorted and unsorted data!

---

**Chapter Progress**: ✅ Chapters 1-21 Complete | 📖 Next: Chapter 22
