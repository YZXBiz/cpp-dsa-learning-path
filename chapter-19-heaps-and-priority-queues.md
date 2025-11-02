# Chapter 19: Heaps & Priority Queues

**What You'll Learn**: What heaps are, WHY they exist, min/max heap operations, priority queues, heap implementation, and real-world applications.

## Table of Contents

1. [Introduction: The "Always Get The Most Important" Problem](#introduction-the-always-get-the-most-important-problem)
2. [1. What is a Heap?](#1-what-is-a-heap)
3. [2. Complete Binary Tree Property](#2-complete-binary-tree-property)
4. [3. Array Representation (The Magic!)](#3-array-representation-the-magic)
5. [4. Heap Operations](#4-heap-operations)
6. [5. Heapify Operation](#5-heapify-operation)
7. [6. Using Priority Queue in C++](#6-using-priority-queue-in-c)
8. [7. Complete Priority Queue Example](#7-complete-priority-queue-example)
9. [8. Custom Objects in Priority Queue](#8-custom-objects-in-priority-queue)
10. [9. Implementing Min Heap from Scratch](#9-implementing-min-heap-from-scratch)
11. [10. Heap Applications](#10-heap-applications)
12. [11. Heap Sort Algorithm](#11-heap-sort-algorithm)
13. [12. Heap Applications in Algorithms](#12-heap-applications-in-algorithms)
14. [13. Time Complexity Summary](#13-time-complexity-summary)
15. [Common Mistakes](#common-mistakes)
16. [Practice Problems](#practice-problems)
17. [Key Takeaways](#key-takeaways)
18. [What's Next?](#whats-next)

---

## Introduction: The "Always Get The Most Important" Problem

Imagine you're building a hospital emergency room system:

**Problem**:
- Patients arrive continuously
- Each has a priority (critical, urgent, normal)
- **Always treat the most critical patient first**
- New critical patients can arrive while treating others

**Bad Solution (Sorted Array)**:
```cpp
vector<Patient> queue;

// Add new patient
queue.push_back(newPatient);
sort(queue.begin(), queue.end());  // O(n log n) - TOO SLOW!

// Get most critical
Patient next = queue[0];
```

**Problems**:
- Re-sorting after every addition: O(n log n)
- With 1000 patients, each new arrival = 10,000+ operations
- System becomes unusable!

**Solution: Priority Queue (Heap)**:
```cpp
priority_queue<Patient> pq;

pq.push(newPatient);     // O(log n) - Fast!
Patient next = pq.top(); // O(1) - Instant!
pq.pop();                // O(log n) - Fast!
```

**This is WHY heaps exist** - efficiently maintain and access the "most important" element!

---

## 1. What is a Heap?

A **heap** is a **complete binary tree** where each parent is more important than its children.

**Two types**:
1. **Max Heap**: Parent ≥ Children (largest at top)
2. **Min Heap**: Parent ≤ Children (smallest at top)

**Visual (Max Heap)**:
```
           100
          /   \
        80     60
       /  \   /  \
      50  40 30  20
     / \
    10  5

Property: Each parent ≥ its children
Root (100) is MAXIMUM element
```

**Visual (Min Heap)**:
```
            5
          /   \
        10     20
       /  \   /  \
      30  40 50  60
     / \
    80 100

Property: Each parent ≤ its children
Root (5) is MINIMUM element
```

**Not a heap**:
```
           100
          /   \
        120    60  ← 120 > 100 (parent < child) ✗
       /  \
      50  40
```

---

## 2. Complete Binary Tree Property

**Complete binary tree** = all levels filled except possibly the last, filled left-to-right.

**Complete** ✓:
```
        1
       / \
      2   3
     / \  /
    4  5 6
```

**Not complete** ✗:
```
        1
       / \
      2   3
         / \
        4   5  ← Gap on left!
```

**Why this matters**: Complete trees can be stored efficiently in arrays!

---

## 3. Array Representation (The Magic!)

**Heap stored in array** (no pointers needed!):

```
Max Heap:
           100
          /   \
        80     60
       /  \   /  \
      50  40 30  20

Array: [100, 80, 60, 50, 40, 30, 20]
Index:   0   1   2   3   4   5   6

Parent-Child relationships (for index i):
- Left child:  2*i + 1
- Right child: 2*i + 2
- Parent:      (i-1)/2
```

**Example**:
```
Index 1 (value 80):
- Left child:  2*1+1 = 3 (value 50)
- Right child: 2*1+2 = 4 (value 40)
- Parent:      (1-1)/2 = 0 (value 100)
```

**Memory visualization**:
```
Array in memory:
┌─────┬────┬────┬────┬────┬────┬────┐
│ 100 │ 80 │ 60 │ 50 │ 40 │ 30 │ 20 │
└─────┴────┴────┴────┴────┴────┴────┘
   0    1    2    3    4    5    6

Represents tree structure WITHOUT pointers!
```

---

## 4. Heap Operations

### Insert (Push) - O(log n)

**Process**:
1. Add element at end (maintain complete tree)
2. "Bubble up" (swap with parent if needed)

**Example: Insert 90 into max heap**:

```
Step 1: Add at end
           100
          /   \
        80     60
       /  \   /  \
      50  40 30  20
     /
    90  ← Added here

Array: [100, 80, 60, 50, 40, 30, 20, 90]

Step 2: Bubble up (90 > 50)
           100
          /   \
        80     60
       /  \   /  \
      90  40 30  20
     /
    50

Step 3: Bubble up again (90 > 80)
           100
          /   \
        90     60
       /  \   /  \
      80  40 30  20
     /
    50

Done! Heap property restored.
```

---

### Extract Max/Min (Pop) - O(log n)

**Process**:
1. Return root (max/min element)
2. Move last element to root
3. "Bubble down" (swap with larger child if needed)

**Example: Remove max (100)**:

```
Step 1: Remove root, move last to root
            50  ← Moved from end
          /   \
        90     60
       /  \   /  \
      80  40 30  20

Step 2: Bubble down (50 < 90, swap with larger child)
           90
          /   \
        50     60
       /  \   /  \
      80  40 30  20

Step 3: Bubble down again (50 < 80, swap)
           90
          /   \
        80     60
       /  \   /  \
      50  40 30  20

Done! New max is 90.
```

---

### Peek (Top) - O(1)

Simply return root element (don't remove):

```cpp
int max = heap[0];  // O(1)
```

---

## 5. Heapify Operation

**Heapify** = convert arbitrary array into heap.

**Two approaches**:
1. Insert elements one-by-one: O(n log n)
2. Build heap from bottom-up: O(n) ← Better!

**Bottom-up heapify**:
```
Start from last non-leaf node, bubble down each.

Array: [10, 20, 15, 30, 40]

Step 1: Heapify index 1 (value 20)
        Compare with children (30, 40)
        Swap with 40 (larger)

Array: [10, 40, 15, 30, 20]

Step 2: Heapify index 0 (value 10)
        Compare with children (40, 15)
        Swap with 40
        Then bubble down again

Final: [40, 30, 15, 10, 20]

Max Heap ready!
```

---

## 6. Using Priority Queue in C++

**Include**:
```cpp
#include <queue>
using namespace std;
```

### Max Heap (Default)

```cpp
priority_queue<int> maxHeap;

maxHeap.push(30);
maxHeap.push(10);
maxHeap.push(50);
maxHeap.push(20);

cout << maxHeap.top();  // 50 (maximum)
maxHeap.pop();
cout << maxHeap.top();  // 30 (new maximum)
```

---

### Min Heap (Custom Comparator)

```cpp
// Min heap: use greater<int>
priority_queue<int, vector<int>, greater<int>> minHeap;

minHeap.push(30);
minHeap.push(10);
minHeap.push(50);
minHeap.push(20);

cout << minHeap.top();  // 10 (minimum)
minHeap.pop();
cout << minHeap.top();  // 20 (new minimum)
```

---

### Basic Operations

```cpp
priority_queue<int> pq;

// Push
pq.push(10);
pq.push(30);
pq.push(20);

// Top (peek)
cout << pq.top();  // 30 (max)

// Pop
pq.pop();  // Removes 30

// Size and empty
cout << pq.size();    // 2
cout << pq.empty();   // false

// Clear (no direct method, use while loop)
while (!pq.empty()) {
    pq.pop();
}
```

---

## 7. Complete Priority Queue Example

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    priority_queue<int> pq;  // Max heap by default

    // Insert elements
    pq.push(30);
    pq.push(10);
    pq.push(50);
    pq.push(20);
    pq.push(40);

    cout << "Elements in descending order:" << endl;

    while (!pq.empty()) {
        cout << pq.top() << " ";
        pq.pop();
    }
    // Output: 50 40 30 20 10

    return 0;
}
```

---

## 8. Custom Objects in Priority Queue

**Example: Task Priority System**:

```cpp
#include <iostream>
#include <queue>
#include <string>
using namespace std;

struct Task {
    string name;
    int priority;  // Higher = more important

    // Constructor
    Task(string n, int p) : name(n), priority(p) {}
};

// Custom comparator
struct CompareTask {
    bool operator()(const Task& t1, const Task& t2) {
        return t1.priority < t2.priority;  // Max heap (higher priority first)
    }
};

int main() {
    priority_queue<Task, vector<Task>, CompareTask> tasks;

    tasks.push(Task("Write code", 3));
    tasks.push(Task("Fix bug", 5));      // Highest priority
    tasks.push(Task("Documentation", 1));
    tasks.push(Task("Testing", 4));

    cout << "Tasks in priority order:" << endl;
    while (!tasks.empty()) {
        Task t = tasks.top();
        tasks.pop();
        cout << t.name << " (priority: " << t.priority << ")" << endl;
    }
    // Output:
    // Fix bug (priority: 5)
    // Testing (priority: 4)
    // Write code (priority: 3)
    // Documentation (priority: 1)

    return 0;
}
```

---

## 9. Implementing Min Heap from Scratch

```cpp
#include <iostream>
#include <vector>
using namespace std;

class MinHeap {
private:
    vector<int> heap;

    int parent(int i) { return (i - 1) / 2; }
    int leftChild(int i) { return 2 * i + 1; }
    int rightChild(int i) { return 2 * i + 2; }

    void bubbleUp(int i) {
        while (i > 0 && heap[i] < heap[parent(i)]) {
            swap(heap[i], heap[parent(i)]);
            i = parent(i);
        }
    }

    void bubbleDown(int i) {
        int minIndex = i;
        int left = leftChild(i);
        int right = rightChild(i);

        if (left < heap.size() && heap[left] < heap[minIndex]) {
            minIndex = left;
        }
        if (right < heap.size() && heap[right] < heap[minIndex]) {
            minIndex = right;
        }

        if (i != minIndex) {
            swap(heap[i], heap[minIndex]);
            bubbleDown(minIndex);
        }
    }

public:
    void push(int value) {
        heap.push_back(value);
        bubbleUp(heap.size() - 1);
    }

    int top() {
        if (heap.empty()) {
            throw runtime_error("Heap is empty!");
        }
        return heap[0];
    }

    void pop() {
        if (heap.empty()) {
            throw runtime_error("Heap is empty!");
        }

        heap[0] = heap.back();
        heap.pop_back();

        if (!heap.empty()) {
            bubbleDown(0);
        }
    }

    bool empty() {
        return heap.empty();
    }

    int size() {
        return heap.size();
    }

    void display() {
        cout << "Heap: ";
        for (int x : heap) {
            cout << x << " ";
        }
        cout << endl;
    }
};

int main() {
    MinHeap h;

    h.push(30);
    h.push(10);
    h.push(50);
    h.push(20);
    h.push(5);

    h.display();  // Heap: 5 10 50 30 20

    cout << "Min element: " << h.top() << endl;  // 5

    h.pop();
    cout << "After removing min: ";
    h.display();  // Heap: 10 20 50 30

    return 0;
}
```

---

## 10. Heap Applications

### Application 1: Find K Largest Elements

```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

vector<int> findKLargest(vector<int>& arr, int k) {
    // Use min heap of size k
    priority_queue<int, vector<int>, greater<int>> minHeap;

    for (int num : arr) {
        minHeap.push(num);

        if (minHeap.size() > k) {
            minHeap.pop();  // Remove smallest
        }
    }

    // Min heap now contains k largest elements
    vector<int> result;
    while (!minHeap.empty()) {
        result.push_back(minHeap.top());
        minHeap.pop();
    }

    return result;
}

int main() {
    vector<int> arr = {10, 20, 5, 30, 15, 40, 25};
    int k = 3;

    vector<int> result = findKLargest(arr, k);

    cout << "3 largest elements: ";
    for (int x : result) {
        cout << x << " ";
    }
    // Output: 25 30 40

    return 0;
}
```

---

### Application 2: Merge K Sorted Arrays

```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

struct Element {
    int value;
    int arrayIndex;
    int elementIndex;

    Element(int v, int ai, int ei)
        : value(v), arrayIndex(ai), elementIndex(ei) {}
};

struct CompareElement {
    bool operator()(const Element& e1, const Element& e2) {
        return e1.value > e2.value;  // Min heap
    }
};

vector<int> mergeKArrays(vector<vector<int>>& arrays) {
    priority_queue<Element, vector<Element>, CompareElement> minHeap;
    vector<int> result;

    // Add first element from each array
    for (int i = 0; i < arrays.size(); i++) {
        if (!arrays[i].empty()) {
            minHeap.push(Element(arrays[i][0], i, 0));
        }
    }

    while (!minHeap.empty()) {
        Element curr = minHeap.top();
        minHeap.pop();

        result.push_back(curr.value);

        // Add next element from same array
        int nextIndex = curr.elementIndex + 1;
        if (nextIndex < arrays[curr.arrayIndex].size()) {
            minHeap.push(Element(
                arrays[curr.arrayIndex][nextIndex],
                curr.arrayIndex,
                nextIndex
            ));
        }
    }

    return result;
}

int main() {
    vector<vector<int>> arrays = {
        {1, 4, 7},
        {2, 5, 8},
        {3, 6, 9}
    };

    vector<int> merged = mergeKArrays(arrays);

    cout << "Merged array: ";
    for (int x : merged) {
        cout << x << " ";
    }
    // Output: 1 2 3 4 5 6 7 8 9

    return 0;
}
```

---

### Application 3: Running Median

```cpp
#include <iostream>
#include <queue>
using namespace std;

class MedianFinder {
private:
    priority_queue<int> maxHeap;  // Left half (smaller elements)
    priority_queue<int, vector<int>, greater<int>> minHeap;  // Right half (larger)

public:
    void addNum(int num) {
        // Add to max heap first
        maxHeap.push(num);

        // Balance: move largest from max to min
        minHeap.push(maxHeap.top());
        maxHeap.pop();

        // If min heap larger, move back
        if (minHeap.size() > maxHeap.size()) {
            maxHeap.push(minHeap.top());
            minHeap.pop();
        }
    }

    double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.top();
        }
        return (maxHeap.top() + minHeap.top()) / 2.0;
    }
};

int main() {
    MedianFinder mf;

    mf.addNum(1);
    mf.addNum(2);
    cout << "Median: " << mf.findMedian() << endl;  // 1.5

    mf.addNum(3);
    cout << "Median: " << mf.findMedian() << endl;  // 2

    mf.addNum(4);
    mf.addNum(5);
    cout << "Median: " << mf.findMedian() << endl;  // 3

    return 0;
}
```

---

## 11. Heap Sort Algorithm

**Idea**: Build max heap, repeatedly extract max to get sorted array.

```cpp
#include <iostream>
#include <vector>
using namespace std;

void heapify(vector<int>& arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }

    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(vector<int>& arr) {
    int n = arr.size();

    // Build max heap
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }

    // Extract elements one by one
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);  // Move max to end
        heapify(arr, i, 0);     // Heapify reduced heap
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6, 7};

    cout << "Original: ";
    for (int x : arr) cout << x << " ";
    cout << endl;

    heapSort(arr);

    cout << "Sorted: ";
    for (int x : arr) cout << x << " ";
    // Output: 5 6 7 11 12 13

    return 0;
}
```

**Time Complexity**: O(n log n)
**Space Complexity**: O(1) (in-place sorting)

---

## 12. Heap Applications in Algorithms

### Dijkstra's Shortest Path Algorithm

Priority queue extracts minimum distance vertex efficiently.

```cpp
// Simplified Dijkstra's (Chapter 20 has full version)
priority_queue<pair<int, int>,
               vector<pair<int, int>>,
               greater<pair<int, int>>> pq;

pq.push({0, start});  // {distance, vertex}

while (!pq.empty()) {
    auto [dist, u] = pq.top();
    pq.pop();

    // Process neighbors, add to pq if shorter path found
}
```

### Huffman Coding (Data Compression)

Min heap builds optimal encoding tree.

### A* Pathfinding

Priority queue selects most promising path.

---

## 13. Time Complexity Summary

| Operation | Time Complexity |
|-----------|-----------------|
| **Insert** | O(log n) |
| **Extract Max/Min** | O(log n) |
| **Peek Top** | O(1) |
| **Build Heap** | O(n) |
| **Heapify** | O(log n) |
| **Heap Sort** | O(n log n) |

**Space Complexity**: O(n)

---

## Common Mistakes

### Mistake 1: Confusing Max and Min Heap

```cpp
priority_queue<int> pq;  // MAX heap (default)
pq.push(10);
pq.push(30);
pq.push(20);
cout << pq.top();  // 30 (not 10!)

// For min heap:
priority_queue<int, vector<int>, greater<int>> minPq;
```

---

### Mistake 2: Modifying Heap Elements Directly

```cpp
priority_queue<int> pq;
pq.push(10);

// Can't do: pq[0] = 50;  ✗ No direct access!

// Must use push/pop operations
```

---

### Mistake 3: Forgetting to Pop

```cpp
priority_queue<int> pq;
pq.push(30);
pq.push(10);

while (!pq.empty()) {
    cout << pq.top();
    // FORGOT pq.pop()!  Infinite loop!
}

// FIX:
while (!pq.empty()) {
    cout << pq.top();
    pq.pop();  // Don't forget!
}
```

---

## Practice Problems

### Problem 1: Kth Largest Element
Find the kth largest element in an unsorted array.

### Problem 2: Sort Nearly Sorted Array
Array where each element is at most k positions away from target position.

### Problem 3: Top K Frequent Words
Find k most frequent words in a document.

### Problem 4: Meeting Rooms II
Find minimum number of meeting rooms needed.

### Problem 5: Reorganize String
Rearrange string so no two same characters are adjacent.

---

## Key Takeaways

✅ **Heap** = complete binary tree with parent-child ordering
✅ **Max heap**: Parent ≥ children, **Min heap**: Parent ≤ children
✅ **Array representation**: No pointers needed, parent at (i-1)/2
✅ **Priority queue** = heap implementation in C++
✅ **Operations**: Insert O(log n), Extract O(log n), Peek O(1)
✅ **Applications**: K largest, merge K arrays, running median, Dijkstra
✅ **Heap sort**: O(n log n) time, O(1) space
✅ **Use priority_queue** for "always get most important" problems

---

## What's Next?

In **Chapter 20**, you'll learn about **Graphs** - the ultimate data structure for modeling relationships and networks like social media, maps, and the internet!

---

**Chapter Progress**: ✅ Chapters 1-19 Complete | 📖 Next: Chapter 20
