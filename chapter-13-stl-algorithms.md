# Chapter 13: STL Algorithms

**What You'll Learn**: Powerful STL algorithms, iterators, how to sort/search/transform data, and writing cleaner code with STL.

## Table of Contents

1. [What are STL Algorithms?](#1-what-are-stl-algorithms)
2. [Iterators: The Bridge](#2-iterators-the-bridge)
3. [Sorting Algorithms](#3-sorting-algorithms)
4. [Searching Algorithms](#4-searching-algorithms)
5. [Modifying Algorithms](#5-modifying-algorithms)
6. [Transformation Algorithms](#6-transformation-algorithms)
7. [Counting and Aggregation](#7-counting-and-aggregation)
8. [Min/Max Algorithms](#8-minmax-algorithms)
9. [Permutations](#9-permutations)
10. [Set Operations (on Sorted Ranges)](#10-set-operations-on-sorted-ranges)
11. [Common Mistakes](#common-mistakes-)
12. [Practice Problems](#practice-problems)
13. [Key Takeaways](#key-takeaways)

---

## Introduction: The Reinventing Wheel Problem

**Problem**: Every programmer writes the same basic algorithms:

```cpp
// Sorting (bubble sort)
for (int i = 0; i < n-1; i++) {
    for (int j = 0; j < n-i-1; j++) {
        if (arr[j] > arr[j+1]) {
            swap(arr[j], arr[j+1]);
        }
    }
}

// Finding element
int index = -1;
for (int i = 0; i < n; i++) {
    if (arr[i] == target) {
        index = i;
        break;
    }
}
```

**Problems**:
- ❌ Reinventing the wheel (why write again?)
- ❌ Bug-prone (easy to make mistakes)
- ❌ Inefficient (naive implementations)
- ❌ Verbose (lots of code for simple tasks)

**Solution**: Use **STL Algorithms** - pre-written, tested, optimized!

```cpp
sort(arr, arr + n);  // Done!
auto it = find(arr, arr + n, target);  // Done!
```

**STL Algorithms are like power tools** - use them instead of manual labor!

---

## 1. What are STL Algorithms?

The Standard Template Library provides **80+ algorithms** that work with containers.

**Categories**:
- **Non-modifying**: find, count, search
- **Modifying**: copy, fill, replace, transform
- **Sorting**: sort, partial_sort, nth_element
- **Binary search**: binary_search, lower_bound, upper_bound
- **Set operations**: set_union, set_intersection
- **Heap operations**: make_heap, push_heap, pop_heap
- **Min/Max**: min, max, min_element, max_element

**Include**:
```cpp
#include <algorithm>
```

---

## 2. Iterators: The Bridge

**Iterators** are like pointers that work with all containers.

**Types**:
- `begin()`: Points to first element
- `end()`: Points to one past last element

**Visual**:
```
Vector: [10, 20, 30, 40, 50]
         ↑               ↑
      begin()         end() (one past last)
```

**Usage**:
```cpp
vector<int> v = {10, 20, 30};

auto it = v.begin();  // Points to 10
cout << *it;          // 10 (dereference)

++it;                 // Move to next
cout << *it;          // 20
```

**Why iterators?** Algorithms work with ANY container (vector, list, array, etc.)!

---

## 3. Sorting Algorithms

### sort()

Sort elements in ascending order (O(n log n)):

```cpp
#include <algorithm>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {50, 20, 40, 10, 30};

    sort(v.begin(), v.end());
    // v = {10, 20, 30, 40, 50}

    return 0;
}
```

**For arrays**:
```cpp
int arr[] = {50, 20, 40, 10, 30};
int n = 5;

sort(arr, arr + n);  // arr + n is end pointer
```

---

### Custom Sorting (Descending)

```cpp
sort(v.begin(), v.end(), greater<int>());
// v = {50, 40, 30, 20, 10}
```

---

### Custom Comparator

```cpp
bool customCompare(int a, int b) {
    return a > b;  // Sort in descending order
}

sort(v.begin(), v.end(), customCompare);
```

**Lambda function** (modern C++):
```cpp
sort(v.begin(), v.end(), [](int a, int b) {
    return a > b;  // Descending
});
```

---

### Sorting Pairs/Structs

```cpp
vector<pair<int, string>> students = {{85, "Alice"}, {90, "Bob"}, {75, "Charlie"}};

// Sort by score (first element)
sort(students.begin(), students.end());

// Sort by name (second element)
sort(students.begin(), students.end(), [](auto a, auto b) {
    return a.second < b.second;
});
```

---

### partial_sort()

Sort only first k elements:

```cpp
vector<int> v = {50, 20, 40, 10, 30};

partial_sort(v.begin(), v.begin() + 3, v.end());
// First 3 elements are sorted: {10, 20, 30, ?, ?}
```

---

### nth_element()

Put nth element in correct position:

```cpp
vector<int> v = {50, 20, 40, 10, 30};

nth_element(v.begin(), v.begin() + 2, v.end());
// v[2] is in correct position (3rd smallest = 30)
// Elements before v[2] are smaller, after are larger
```

---

## 4. Searching Algorithms

### find()

Find first occurrence (O(n)):

```cpp
vector<int> v = {10, 20, 30, 40, 50};

auto it = find(v.begin(), v.end(), 30);

if (it != v.end()) {
    cout << "Found at index: " << (it - v.begin());  // 2
} else {
    cout << "Not found";
}
```

---

### binary_search()

Check if element exists (O(log n), requires sorted array!):

```cpp
vector<int> v = {10, 20, 30, 40, 50};  // Must be sorted!

if (binary_search(v.begin(), v.end(), 30)) {
    cout << "Found!";
} else {
    cout << "Not found";
}
```

---

### lower_bound() and upper_bound()

**lower_bound**: First position where element can be inserted to maintain order

```cpp
vector<int> v = {10, 20, 30, 30, 40, 50};

auto it = lower_bound(v.begin(), v.end(), 30);
// Points to first 30 (index 2)
```

**upper_bound**: First position AFTER last occurrence

```cpp
auto it = upper_bound(v.begin(), v.end(), 30);
// Points to 40 (index 4)
```

**Count occurrences**:
```cpp
int count = upper_bound(v.begin(), v.end(), 30) - lower_bound(v.begin(), v.end(), 30);
// 2 (two 30s)
```

---

## 5. Modifying Algorithms

### fill()

Fill range with value:

```cpp
vector<int> v(5);
fill(v.begin(), v.end(), 10);
// v = {10, 10, 10, 10, 10}
```

---

### copy()

Copy elements:

```cpp
vector<int> src = {1, 2, 3};
vector<int> dest(3);

copy(src.begin(), src.end(), dest.begin());
// dest = {1, 2, 3}
```

---

### replace()

Replace all occurrences:

```cpp
vector<int> v = {10, 20, 10, 30, 10};

replace(v.begin(), v.end(), 10, 99);
// v = {99, 20, 99, 30, 99}
```

---

### remove()

Remove elements (doesn't actually erase, moves to end!):

```cpp
vector<int> v = {10, 20, 10, 30, 10};

auto newEnd = remove(v.begin(), v.end(), 10);
v.erase(newEnd, v.end());  // Actually remove
// v = {20, 30}
```

---

### unique()

Remove consecutive duplicates (array must be sorted!):

```cpp
vector<int> v = {10, 10, 20, 20, 30};

auto newEnd = unique(v.begin(), v.end());
v.erase(newEnd, v.end());
// v = {10, 20, 30}
```

---

### reverse()

Reverse elements:

```cpp
vector<int> v = {10, 20, 30, 40, 50};

reverse(v.begin(), v.end());
// v = {50, 40, 30, 20, 10}
```

---

### rotate()

Rotate elements:

```cpp
vector<int> v = {10, 20, 30, 40, 50};

rotate(v.begin(), v.begin() + 2, v.end());
// v = {30, 40, 50, 10, 20}
//       ↑ this becomes first
```

---

## 6. Transformation Algorithms

### transform()

Apply function to each element:

```cpp
vector<int> v = {1, 2, 3, 4, 5};
vector<int> result(5);

transform(v.begin(), v.end(), result.begin(), [](int x) {
    return x * x;  // Square each element
});
// result = {1, 4, 9, 16, 25}
```

---

## 7. Counting and Aggregation

### count()

Count occurrences:

```cpp
vector<int> v = {10, 20, 10, 30, 10};

int cnt = count(v.begin(), v.end(), 10);
// cnt = 3
```

---

### count_if()

Count elements satisfying condition:

```cpp
int cnt = count_if(v.begin(), v.end(), [](int x) {
    return x > 15;
});
// Counts elements > 15
```

---

### accumulate()

Sum all elements (need `<numeric>`):

```cpp
#include <numeric>

vector<int> v = {10, 20, 30, 40, 50};

int sum = accumulate(v.begin(), v.end(), 0);  // 0 is initial value
// sum = 150
```

**Product**:
```cpp
int product = accumulate(v.begin(), v.end(), 1, [](int a, int b) {
    return a * b;
});
```

---

## 8. Min/Max Algorithms

### min_element() and max_element()

Find min/max element:

```cpp
vector<int> v = {50, 20, 40, 10, 30};

auto minIt = min_element(v.begin(), v.end());
auto maxIt = max_element(v.begin(), v.end());

cout << "Min: " << *minIt << endl;  // 10
cout << "Max: " << *maxIt << endl;  // 50

cout << "Min index: " << (minIt - v.begin()) << endl;  // 3
```

---

### minmax_element()

Get both at once:

```cpp
auto [minIt, maxIt] = minmax_element(v.begin(), v.end());

cout << "Min: " << *minIt << endl;
cout << "Max: " << *maxIt << endl;
```

---

## 9. Permutations

### next_permutation()

Generate next permutation:

```cpp
vector<int> v = {1, 2, 3};

do {
    for (int x : v) cout << x << " ";
    cout << endl;
} while (next_permutation(v.begin(), v.end()));

// Output:
// 1 2 3
// 1 3 2
// 2 1 3
// 2 3 1
// 3 1 2
// 3 2 1
```

---

## 10. Set Operations (on Sorted Ranges)

### set_union()

Union of two sorted sets:

```cpp
vector<int> a = {1, 2, 3, 4};
vector<int> b = {3, 4, 5, 6};
vector<int> result(10);

auto it = set_union(a.begin(), a.end(), b.begin(), b.end(), result.begin());
result.resize(it - result.begin());
// result = {1, 2, 3, 4, 5, 6}
```

---

### set_intersection()

Intersection of two sorted sets:

```cpp
auto it = set_intersection(a.begin(), a.end(), b.begin(), b.end(), result.begin());
result.resize(it - result.begin());
// result = {3, 4}
```

---

### set_difference()

Elements in first but not in second:

```cpp
auto it = set_difference(a.begin(), a.end(), b.begin(), b.end(), result.begin());
result.resize(it - result.begin());
// result = {1, 2}
```

---

## Complete Example Programs

### Example 1: Sorting Students by Grade

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Student {
    string name;
    int grade;
};

int main() {
    vector<Student> students = {
        {"Alice", 85},
        {"Bob", 90},
        {"Charlie", 75},
        {"Diana", 95}
    };

    // Sort by grade (descending)
    sort(students.begin(), students.end(), [](const Student& a, const Student& b) {
        return a.grade > b.grade;
    });

    cout << "Students sorted by grade:" << endl;
    for (const auto& s : students) {
        cout << s.name << ": " << s.grade << endl;
    }

    return 0;
}
```

---

### Example 2: Finding Median

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

double findMedian(vector<int> v) {
    int n = v.size();
    nth_element(v.begin(), v.begin() + n/2, v.end());

    if (n % 2 == 1) {
        return v[n/2];
    } else {
        nth_element(v.begin(), v.begin() + n/2 - 1, v.end());
        return (v[n/2] + v[n/2 - 1]) / 2.0;
    }
}

int main() {
    vector<int> v = {50, 20, 40, 10, 30};
    cout << "Median: " << findMedian(v) << endl;  // 30

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Binary Search on Unsorted Array

```cpp
vector<int> v = {50, 20, 40, 10, 30};  // NOT sorted!
binary_search(v.begin(), v.end(), 30);  // WRONG! Undefined behavior

// FIX: Sort first
sort(v.begin(), v.end());
binary_search(v.begin(), v.end(), 30);  // Now correct
```

---

### Mistake 2: Not Using Return Value of remove()

```cpp
vector<int> v = {10, 20, 10, 30, 10};
remove(v.begin(), v.end(), 10);  // Doesn't actually remove!

// FIX: Use erase
auto newEnd = remove(v.begin(), v.end(), 10);
v.erase(newEnd, v.end());
```

---

## Practice Problems

### Problem 1: Kth Smallest Element
Find kth smallest element using nth_element().

### Problem 2: Remove Duplicates
Remove all duplicates from unsorted vector.

### Problem 3: Anagram Groups
Group strings that are anagrams of each other.

### Problem 4: Merge Intervals
Merge overlapping intervals using sort and algorithms.

### Problem 5: Next Greater Permutation
Find next lexicographically greater permutation.

---

## Key Takeaways

✅ **STL algorithms save time** and reduce bugs
✅ **Iterators** (begin(), end()) work with all containers
✅ **sort()** for sorting (O(n log n))
✅ **find()** for linear search, **binary_search()** for sorted
✅ **lower_bound/upper_bound** for finding insertion points
✅ **transform()** for applying functions
✅ **accumulate()** for summing/aggregating
✅ **Always sort before** binary search or set operations
✅ **Use lambdas** for custom comparators

---

## Algorithm Cheat Sheet

| Task | Algorithm | Complexity |
|------|-----------|------------|
| Sort | `sort()` | O(n log n) |
| Find | `find()` | O(n) |
| Binary search | `binary_search()` | O(log n) |
| Min/Max | `min_element()`, `max_element()` | O(n) |
| Count | `count()` | O(n) |
| Reverse | `reverse()` | O(n) |
| Remove duplicates | `sort()` + `unique()` | O(n log n) |
| Transform | `transform()` | O(n) |
| Sum | `accumulate()` | O(n) |

---

## What's Next?

In **Chapter 14**, you'll learn about **Big-O Notation and Complexity Analysis** - understanding why some algorithms are faster than others!

---

**Chapter Progress**: ✅ Chapters 1-13 Complete (Part 2 DONE!) | 📖 Next: Chapter 14 (Part 3: Data Structures)
