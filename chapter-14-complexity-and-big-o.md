# Chapter 14: Complexity & Big-O Notation

**What You'll Learn**: What complexity analysis is, WHY it matters, how to analyze algorithms, and understanding Big-O notation intuitively.

## Table of Contents

1. [What is Complexity Analysis?](#1-what-is-complexity-analysis)
2. [Big-O Notation: The Language](#2-big-o-notation-the-language)
3. [Common Complexities (From Fast to Slow)](#3-common-complexities-from-fast-to-slow)
4. [Complexity Comparison](#4-complexity-comparison)
5. [Analyzing Code: Rules](#5-analyzing-code-rules)
6. [Examples: Analyzing Algorithms](#6-examples-analyzing-algorithms)
7. [Space Complexity](#7-space-complexity)
8. [Best, Average, and Worst Case](#8-best-average-and-worst-case)
9. [Amortized Analysis](#9-amortized-analysis)
10. [Practice Problems](#practice-problems)
11. [Key Takeaways](#key-takeaways)

---

## Introduction: The Speed Problem

You have two programs that find if a number exists in a list:

**Program A**:
```cpp
// Check each element one by one
for (int i = 0; i < n; i++) {
    if (arr[i] == target) return true;
}
```

**Program B**:
```cpp
// Binary search (assumes sorted)
int left = 0, right = n-1;
while (left <= right) {
    int mid = (left + right) / 2;
    if (arr[mid] == target) return true;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
}
```

**Question**: Which is faster?

**Answer**: Depends on **n** (size of input)!

- For n = 10: Both are fast (~0.000001 seconds)
- For n = 1,000,000: 
  - Program A: ~1 second
  - Program B: ~0.00002 seconds (50,000× faster!)

**This is WHY we need complexity analysis** - to predict how algorithms scale!

---

## 1. What is Complexity Analysis?

**Complexity analysis** measures how an algorithm's runtime or memory grows as input size increases.

**Two types**:
1. **Time Complexity**: How runtime grows with input size
2. **Space Complexity**: How memory usage grows with input size

**Think of it like**: Estimating travel time
- 10 miles: ~15 minutes (car)
- 100 miles: ~1.5 hours (car scales linearly)
- 1000 miles: ~15 hours (not 150 hours - airplane is better!)

---

## 2. Big-O Notation: The Language

**Big-O** describes the **worst-case** growth rate.

**Notation**: O(f(n)) where f(n) is a function of input size n

**Read as**: "Order of f(n)" or "Big-O of f(n)"

**Examples**:
- O(1): Constant time
- O(log n): Logarithmic time
- O(n): Linear time
- O(n log n): Linearithmic time
- O(n²): Quadratic time
- O(2ⁿ): Exponential time

---

## 3. Common Complexities (From Fast to Slow)

### O(1) - Constant Time

**Meaning**: Always takes same time, regardless of input size

**Examples**:
```cpp
// Array access
int x = arr[5];  // O(1)

// Math operation
int result = a + b;  // O(1)

// Hash map lookup
int val = map[key];  // O(1) average
```

**Visual**:
```
Time
  ↑
  |  ─────────────────  (flat line)
  |
  └──────────────────→ Input size (n)
```

---

### O(log n) - Logarithmic Time

**Meaning**: Doubles input size → adds constant time

**Examples**:
```cpp
// Binary search
int binarySearch(int arr[], int n, int target) {
    int left = 0, right = n-1;
    while (left <= right) {
        int mid = (left + right) / 2;
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
// O(log n) - halves search space each time
```

**Why log?** Each step cuts problem in half:
- n = 1000 → ~10 steps (log₂ 1000 ≈ 10)
- n = 1,000,000 → ~20 steps (log₂ 1,000,000 ≈ 20)

**Visual**:
```
Time
  ↑
  |      ────────────  (grows slowly)
  |   ──
  | ─
  └──────────────────→ Input size (n)
```

---

### O(n) - Linear Time

**Meaning**: Doubles input size → doubles time

**Examples**:
```cpp
// Find max element
int findMax(int arr[], int n) {
    int max = arr[0];
    for (int i = 1; i < n; i++) {
        if (arr[i] > max) max = arr[i];
    }
    return max;
}
// O(n) - must check each element
```

**Visual**:
```
Time
  ↑
  |            ──
  |        ──
  |    ──
  |  ─
  └──────────────────→ Input size (n)
```

---

### O(n log n) - Linearithmic Time

**Meaning**: Efficient sorting algorithms

**Examples**:
```cpp
// Merge sort, Quick sort, Heap sort
sort(arr, arr + n);  // O(n log n)
```

**Why?** Divide-and-conquer + processing all elements

**Visual**:
```
Time
  ↑
  |               ───
  |           ──
  |      ──
  |  ──
  └──────────────────→ Input size (n)
```

---

### O(n²) - Quadratic Time

**Meaning**: Doubles input size → quadruples time

**Examples**:
```cpp
// Bubble sort
void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n-1; i++) {
        for (int j = 0; j < n-i-1; j++) {
            if (arr[j] > arr[j+1]) {
                swap(arr[j], arr[j+1]);
            }
        }
    }
}
// O(n²) - nested loops over same data
```

**Visual**:
```
Time
  ↑
  |                    ──
  |              ──
  |        ──
  |  ──
  └──────────────────→ Input size (n)
```

---

### O(2ⁿ) - Exponential Time

**Meaning**: Adds 1 to input → doubles time (very slow!)

**Examples**:
```cpp
// Naive Fibonacci
int fib(int n) {
    if (n <= 1) return n;
    return fib(n-1) + fib(n-2);
}
// O(2ⁿ) - exponential explosion!
```

**Why avoid?** Becomes impractical very quickly:
- n = 10: ~1,000 operations
- n = 20: ~1,000,000 operations
- n = 30: ~1,000,000,000 operations

---

## 4. Complexity Comparison

| Big-O | Name | n=10 | n=100 | n=1000 | Example |
|-------|------|------|-------|--------|---------|
| O(1) | Constant | 1 | 1 | 1 | Array access |
| O(log n) | Logarithmic | 3 | 7 | 10 | Binary search |
| O(n) | Linear | 10 | 100 | 1,000 | Find max |
| O(n log n) | Linearithmic | 30 | 700 | 10,000 | Merge sort |
| O(n²) | Quadratic | 100 | 10,000 | 1,000,000 | Bubble sort |
| O(2ⁿ) | Exponential | 1,024 | 2³⁰ | 2¹⁰⁰⁰ | Naive Fibonacci |

**Key insight**: For large n, O(log n) and O(n) are acceptable, O(n²) is slow, O(2ⁿ) is impossible!

---

## 5. Analyzing Code: Rules

### Rule 1: Drop Constants

```cpp
for (int i = 0; i < n; i++) {
    // Do something
}
for (int i = 0; i < n; i++) {
    // Do something else
}
```

**Analysis**: 2n operations → **O(n)** (not O(2n))

**Why?** For large n, 2n and n grow at same rate

---

### Rule 2: Drop Lower Order Terms

```cpp
for (int i = 0; i < n; i++) {           // O(n)
    for (int j = 0; j < n; j++) {       // O(n²)
        // Do something
    }
}
for (int i = 0; i < n; i++) {           // O(n)
    // Do something
}
```

**Analysis**: O(n²) + O(n) = **O(n²)** (drop O(n))

**Why?** For large n, n² dominates n

---

### Rule 3: Different Inputs = Different Variables

```cpp
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        // Do something
    }
}
```

**Analysis**: **O(n × m)** (not O(n²)!)

**Why?** n and m are different inputs

---

### Rule 4: Sequential Loops = Add

```cpp
for (int i = 0; i < n; i++) { }  // O(n)
for (int i = 0; i < n; i++) { }  // O(n)
```

**Analysis**: O(n) + O(n) = **O(n)**

---

### Rule 5: Nested Loops = Multiply

```cpp
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        // O(1) operation
    }
}
```

**Analysis**: O(n) × O(n) = **O(n²)**

---

## 6. Examples: Analyzing Algorithms

### Example 1: Find Pair Sum

```cpp
// Find two numbers that add up to target
bool hasPairSum(int arr[], int n, int target) {
    for (int i = 0; i < n; i++) {              // O(n)
        for (int j = i+1; j < n; j++) {        // O(n)
            if (arr[i] + arr[j] == target) {
                return true;
            }
        }
    }
    return false;
}
```

**Analysis**:
- Outer loop: n iterations
- Inner loop: ~n iterations
- Nested → multiply
- **Time Complexity: O(n²)**

---

### Example 2: Better Pair Sum (Using Set)

```cpp
bool hasPairSum(int arr[], int n, int target) {
    unordered_set<int> seen;
    for (int i = 0; i < n; i++) {              // O(n)
        int complement = target - arr[i];
        if (seen.find(complement) != seen.end()) {  // O(1)
            return true;
        }
        seen.insert(arr[i]);                   // O(1)
    }
    return false;
}
```

**Analysis**:
- One loop: n iterations
- Set operations inside: O(1) each
- **Time Complexity: O(n)**
- **Space Complexity: O(n)** (for set)

**Tradeoff**: Used more space to get better time!

---

### Example 3: Binary Search

```cpp
int binarySearch(int arr[], int n, int target) {
    int left = 0, right = n-1;
    while (left <= right) {                    // O(log n)
        int mid = (left + right) / 2;
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

**Analysis**:
- Each iteration halves search space
- n → n/2 → n/4 → n/8 → ... → 1
- **Time Complexity: O(log n)**

---

### Example 4: Nested Variable Loops

```cpp
for (int i = 0; i < n; i++) {
    for (int j = i; j < n; j++) {
        cout << i << " " << j << endl;
    }
}
```

**Analysis**:
- i=0: n iterations
- i=1: n-1 iterations
- i=2: n-2 iterations
- ...
- Total: n + (n-1) + (n-2) + ... + 1 = n(n+1)/2 = **O(n²)**

---

## 7. Space Complexity

**Space complexity** measures extra memory used (not counting input).

### Example 1: Constant Space

```cpp
int sum(int arr[], int n) {
    int total = 0;        // O(1) space
    for (int i = 0; i < n; i++) {
        total += arr[i];
    }
    return total;
}
```

**Space Complexity: O(1)** (only uses a few variables)

---

### Example 2: Linear Space

```cpp
vector<int> createCopy(vector<int>& v) {
    vector<int> copy = v;  // O(n) space
    return copy;
}
```

**Space Complexity: O(n)** (creates copy of input)

---

### Example 3: Recursion Space

```cpp
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n-1);
}
```

**Space Complexity: O(n)** (call stack depth is n)

---

## 8. Best, Average, and Worst Case

Most algorithms have different cases:

**Example: Linear Search**
```cpp
for (int i = 0; i < n; i++) {
    if (arr[i] == target) return i;
}
```

- **Best case**: Element at index 0 → O(1)
- **Average case**: Element at middle → O(n/2) → O(n)
- **Worst case**: Element at end or not present → O(n)

**Big-O typically refers to WORST case!**

---

## 9. Amortized Analysis

**Amortized** = average time per operation over a sequence.

**Example: Vector push_back()**

Most insertions: O(1)
Occasional resize: O(n) (copy all elements)

**Amortized time: O(1)** (averaging over many operations)

---

## Practice Problems

### Problem 1: Analyze These Algorithms

```cpp
// A
for (int i = 0; i < n; i++) {
    for (int j = 0; j < 100; j++) {
        // O(1) operation
    }
}

// B
for (int i = 1; i <= n; i *= 2) {
    // O(1) operation
}

// C
for (int i = 0; i < n; i++) {
    for (int j = 0; j < i; j++) {
        // O(1) operation
    }
}
```

### Problem 2: Optimize This

```cpp
// Find if array has duplicates (current: O(n²))
bool hasDuplicates(int arr[], int n) {
    for (int i = 0; i < n; i++) {
        for (int j = i+1; j < n; j++) {
            if (arr[i] == arr[j]) return true;
        }
    }
    return false;
}
// Can you make it O(n)?
```

---

## Key Takeaways

✅ **Big-O measures growth rate** as input size increases
✅ **Drop constants** and **lower-order terms**
✅ **Common complexities**: O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ)
✅ **Nested loops usually = multiplication** (O(n²))
✅ **Sequential loops usually = addition** (O(n))
✅ **Space-time tradeoffs** exist (faster code often uses more memory)
✅ **Big-O = worst case** by default
✅ **For large inputs**, complexity matters more than constant factors

---

## Complexity Cheat Sheet

| Operation | Array | Vector | Set | Map | Hash Set | Hash Map |
|-----------|-------|--------|-----|-----|----------|----------|
| Access | O(1) | O(1) | - | - | - | - |
| Search | O(n) | O(n) | O(log n) | O(log n) | O(1)* | O(1)* |
| Insert | - | O(1)* | O(log n) | O(log n) | O(1)* | O(1)* |
| Delete | - | O(n) | O(log n) | O(log n) | O(1)* | O(1)* |

*Amortized or average case

---

## What's Next?

In **Chapter 15**, you'll dive deep into **Array Problems & Techniques** - using arrays to solve complex problems efficiently!

---

**Chapter Progress**: ✅ Chapters 1-14 Complete | 📖 Next: Chapter 15
