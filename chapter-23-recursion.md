# Chapter 23: Recursion

**What You'll Learn**: What recursion is, WHY it exists, how the call stack works with recursion, classic recursive problems, and when to use recursion vs iteration.

## Table of Contents

1. [What is Recursion?](#1-what-is-recursion)
2. [Simple Example: Countdown](#2-simple-example-countdown)
3. [The Call Stack Visualization](#3-the-call-stack-visualization)
4. [Classic Problem: Factorial](#4-classic-problem-factorial)
5. [Classic Problem: Fibonacci](#5-classic-problem-fibonacci)
6. [Classic Problem: Tower of Hanoi](#6-classic-problem-tower-of-hanoi)
7. [Binary Search - Recursive Version](#7-binary-search---recursive-version)
8. [Recursion on Arrays and Strings](#8-recursion-on-arrays-and-strings)
9. [Tail Recursion Optimization](#9-tail-recursion-optimization)
10. [Recursion vs Iteration](#10-recursion-vs-iteration)
11. [Recursion Comparison Table](#11-recursion-comparison-table)
12. [Common Recursion Patterns](#12-common-recursion-patterns)
13. [Complete Example Programs](#complete-example-programs)
14. [Common Mistakes](#common-mistakes)
15. [Practice Problems](#practice-problems)
16. [Key Takeaways](#key-takeaways)

---

## Introduction: The Self-Similar Problem

Imagine you're organizing a company with teams:

```
CEO
├── Manager A
│   ├── Engineer 1
│   ├── Engineer 2
│   └── Engineer 3
├── Manager B
│   ├── Engineer 4
│   └── Engineer 5
└── Manager C
    └── Engineer 6
```

**Task**: Count total employees.

**Iterative thinking**: Loop through each person... but how deep do we go?

**Recursive thinking**: "Total employees = 1 (me) + employees under each of my managers"
- CEO: 1 + employees(ManagerA) + employees(ManagerB) + employees(ManagerC)
- Each manager does the same!

**This is WHY recursion exists** - some problems are **self-similar** (smaller versions of themselves).

---

## 1. What is Recursion?

**Recursion**: A function that **calls itself** to solve a problem by breaking it into smaller versions of the same problem.

**Think of it like**:
- **Russian dolls**: Open one, find smaller version inside
- **Mirrors facing each other**: Each reflection contains a smaller reflection
- **Fractals**: Patterns that repeat at different scales

**Key components**:
1. **Base case**: Stopping condition (prevents infinite recursion)
2. **Recursive case**: Function calls itself with smaller input

---

## 2. Simple Example: Countdown

**Iterative (loop)**:

```cpp
void countdown(int n) {
    for (int i = n; i >= 1; i--) {
        cout << i << " ";
    }
    cout << "Blast off!" << endl;
}
```

**Recursive**:

```cpp
void countdown(int n) {
    if (n == 0) {                    // Base case
        cout << "Blast off!" << endl;
        return;
    }

    cout << n << " ";                // Print current
    countdown(n - 1);                // Recursive call (smaller problem)
}

// countdown(3):
// Print 3, call countdown(2)
//   Print 2, call countdown(1)
//     Print 1, call countdown(0)
//       Print "Blast off!"
// Output: 3 2 1 Blast off!
```

**Critical**: Without base case, infinite recursion!

---

## 3. The Call Stack Visualization

**Remember Chapter 5?** Functions use a **call stack**. With recursion, the function keeps adding itself to the stack!

**Example**: `countdown(3)`

```
Call Stack Evolution:

Step 1: countdown(3) called
┌─────────────────┐
│ countdown(3)    │ ← Print 3, call countdown(2)
├─────────────────┤
│ main()          │
└─────────────────┘

Step 2: countdown(2) called
┌─────────────────┐
│ countdown(2)    │ ← Print 2, call countdown(1)
├─────────────────┤
│ countdown(3)    │ ← Waiting...
├─────────────────┤
│ main()          │
└─────────────────┘

Step 3: countdown(1) called
┌─────────────────┐
│ countdown(1)    │ ← Print 1, call countdown(0)
├─────────────────┤
│ countdown(2)    │ ← Waiting...
├─────────────────┤
│ countdown(3)    │ ← Waiting...
├─────────────────┤
│ main()          │
└─────────────────┘

Step 4: countdown(0) called (BASE CASE!)
┌─────────────────┐
│ countdown(0)    │ ← Print "Blast off!", RETURN
├─────────────────┤
│ countdown(1)    │ ← Waiting...
├─────────────────┤
│ countdown(2)    │ ← Waiting...
├─────────────────┤
│ countdown(3)    │ ← Waiting...
├─────────────────┤
│ main()          │
└─────────────────┘

Step 5: countdown(0) returns
┌─────────────────┐
│ countdown(1)    │ ← Resumes, returns
├─────────────────┤
│ countdown(2)    │ ← Waiting...
├─────────────────┤
│ countdown(3)    │ ← Waiting...
├─────────────────┤
│ main()          │
└─────────────────┘

... continues until all return to main()
```

**Key insight**: Recursion uses **stack space** - deep recursion can cause **stack overflow**!

---

## 4. Classic Problem: Factorial

**Definition**: n! = n × (n-1) × (n-2) × ... × 1
- 5! = 5 × 4 × 3 × 2 × 1 = 120

**Recursive insight**: n! = n × (n-1)!

**Code**:

```cpp
int factorial(int n) {
    // Base case
    if (n <= 1) {
        return 1;
    }

    // Recursive case
    return n * factorial(n - 1);
}

// Trace factorial(5):
// factorial(5) = 5 * factorial(4)
//              = 5 * 4 * factorial(3)
//              = 5 * 4 * 3 * factorial(2)
//              = 5 * 4 * 3 * 2 * factorial(1)
//              = 5 * 4 * 3 * 2 * 1
//              = 120 ✓
```

**Call Stack Depth**: n levels (5 levels for factorial(5))

---

## 5. Classic Problem: Fibonacci

**Definition**: Each number is sum of previous two
- 0, 1, 1, 2, 3, 5, 8, 13, 21, ...
- fib(0) = 0, fib(1) = 1
- fib(n) = fib(n-1) + fib(n-2)

**Naive Recursive**:

```cpp
int fib(int n) {
    if (n <= 1) return n;         // Base case
    return fib(n-1) + fib(n-2);   // Recursive case
}
```

**Problem**: **Exponential time O(2ⁿ)** - very slow!

**Why so slow?** Repeated calculations!

```
fib(5)
├── fib(4)
│   ├── fib(3)
│   │   ├── fib(2)
│   │   │   ├── fib(1) → 1
│   │   │   └── fib(0) → 0
│   │   └── fib(1) → 1
│   └── fib(2)  ← Calculated again!
│       ├── fib(1) → 1
│       └── fib(0) → 0
└── fib(3)  ← Calculated again!
    ├── fib(2)  ← Calculated AGAIN!
    │   ├── fib(1) → 1
    │   └── fib(0) → 0
    └── fib(1) → 1
```

**Solution**: **Memoization** (save results):

```cpp
#include <map>

map<int, int> memo;

int fibMemo(int n) {
    if (n <= 1) return n;

    if (memo.find(n) != memo.end()) {
        return memo[n];  // Return cached result
    }

    memo[n] = fibMemo(n-1) + fibMemo(n-2);  // Cache result
    return memo[n];
}

// Now O(n) time! Each fib(i) calculated only once.
```

---

## 6. Classic Problem: Tower of Hanoi

**Problem**: Move n disks from rod A to rod C using rod B as helper.
- Only move one disk at a time
- Never place larger disk on smaller disk

**Visual** (3 disks):

```
Start:
Rod A    Rod B    Rod C
 |        |        |
[1]       |        |
[2]       |        |
[3]       |        |

Goal:
Rod A    Rod B    Rod C
 |        |        |
 |        |       [1]
 |        |       [2]
 |        |       [3]
```

**Recursive insight**:
1. Move n-1 disks from A to B (using C)
2. Move largest disk from A to C
3. Move n-1 disks from B to C (using A)

**Code**:

```cpp
void towerOfHanoi(int n, char from, char to, char aux) {
    if (n == 1) {
        cout << "Move disk 1 from " << from << " to " << to << endl;
        return;
    }

    towerOfHanoi(n-1, from, aux, to);   // Move n-1 to aux
    cout << "Move disk " << n << " from " << from << " to " << to << endl;
    towerOfHanoi(n-1, aux, to, from);   // Move n-1 to destination
}

// towerOfHanoi(3, 'A', 'C', 'B');
// Output:
// Move disk 1 from A to C
// Move disk 2 from A to B
// Move disk 1 from C to B
// Move disk 3 from A to C
// Move disk 1 from B to A
// Move disk 2 from B to C
// Move disk 1 from A to C
```

**Moves required**: 2ⁿ - 1 (for n disks)

---

## 7. Binary Search - Recursive Version

**From Chapter 22**, here's recursive binary search:

```cpp
int binarySearch(int arr[], int left, int right, int target) {
    if (left > right) {
        return -1;  // Base case: not found
    }

    int mid = left + (right - left) / 2;

    if (arr[mid] == target) {
        return mid;  // Base case: found
    }
    else if (arr[mid] < target) {
        return binarySearch(arr, mid + 1, right, target);  // Search right
    }
    else {
        return binarySearch(arr, left, mid - 1, target);   // Search left
    }
}
```

**Call stack depth**: O(log n) - much better than O(n) for factorial!

---

## 8. Recursion on Arrays and Strings

### Sum of Array

```cpp
int arraySum(int arr[], int n) {
    if (n == 0) return 0;              // Base case
    return arr[n-1] + arraySum(arr, n-1);  // Last + sum of rest
}

// arraySum([1,2,3,4,5], 5)
// = 5 + arraySum([1,2,3,4], 4)
// = 5 + 4 + arraySum([1,2,3], 3)
// = 5 + 4 + 3 + arraySum([1,2], 2)
// = 5 + 4 + 3 + 2 + arraySum([1], 1)
// = 5 + 4 + 3 + 2 + 1 + arraySum([], 0)
// = 5 + 4 + 3 + 2 + 1 + 0
// = 15 ✓
```

### Reverse a String

```cpp
#include <string>
using namespace std;

string reverseString(string s) {
    if (s.length() <= 1) {
        return s;  // Base case
    }

    // Last char + reverse of rest
    return s[s.length()-1] + reverseString(s.substr(0, s.length()-1));
}

// reverseString("hello")
// = 'o' + reverseString("hell")
// = 'o' + 'l' + reverseString("hel")
// = 'o' + 'l' + 'l' + reverseString("he")
// = 'o' + 'l' + 'l' + 'e' + reverseString("h")
// = 'o' + 'l' + 'l' + 'e' + 'h'
// = "olleh" ✓
```

### Check Palindrome

```cpp
bool isPalindrome(string s, int left, int right) {
    if (left >= right) {
        return true;  // Base case: single char or empty
    }

    if (s[left] != s[right]) {
        return false;  // Mismatch
    }

    return isPalindrome(s, left + 1, right - 1);  // Check inner
}

// isPalindrome("racecar", 0, 6)
// = (r == r) && isPalindrome("aceca", 1, 5)
// = (a == a) && isPalindrome("cec", 2, 4)
// = (c == c) && isPalindrome("e", 3, 3)
// = true ✓
```

---

## 9. Tail Recursion Optimization

**Tail recursion**: Recursive call is the **last** operation (nothing after it).

**Not tail-recursive** (factorial):

```cpp
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n-1);  // Multiplication AFTER recursive call
}
```

**Tail-recursive** (factorial with accumulator):

```cpp
int factorialTail(int n, int acc = 1) {
    if (n <= 1) return acc;
    return factorialTail(n-1, n * acc);  // Recursive call is LAST
}

// factorialTail(5, 1)
// = factorialTail(4, 5)
// = factorialTail(3, 20)
// = factorialTail(2, 60)
// = factorialTail(1, 120)
// = 120 ✓
```

**Benefit**: Compiler can optimize to loop (no stack growth)!

---

## 10. Recursion vs Iteration

### When to Use Recursion

✅ **Use recursion when**:
- Problem is naturally recursive (trees, graphs, divide-and-conquer)
- Code is **much clearer** than iterative version
- Stack depth is manageable (O(log n) or small O(n))

**Examples**:
- Tree traversal
- Binary search
- Merge sort / Quick sort
- Backtracking (N-Queens, Sudoku)
- Graph DFS

### When to Use Iteration

✅ **Use iteration when**:
- Simple sequential processing
- Deep recursion (risk of stack overflow)
- Performance critical (recursion has function call overhead)

**Examples**:
- Fibonacci (use loop or DP)
- Factorial (simple loop is better)
- Array processing (most cases)

---

## 11. Recursion Comparison Table

| Problem | Recursive Time | Iterative Time | Recursion Better? |
|---------|---------------|----------------|-------------------|
| Factorial | O(n) | O(n) | No (iteration simpler) |
| Fibonacci (naive) | O(2ⁿ) | O(n) | No (too slow) |
| Fibonacci (memo) | O(n) | O(n) | Equal |
| Binary Search | O(log n) | O(log n) | Equal (recursion cleaner) |
| Tree Traversal | O(n) | O(n) | Yes (much cleaner!) |
| Merge Sort | O(n log n) | O(n log n) | Yes (natural fit) |
| Tower of Hanoi | O(2ⁿ) | Very complex | Yes (iteration impractical) |

---

## 12. Common Recursion Patterns

### Pattern 1: Divide and Conquer

**Idea**: Break problem into smaller subproblems, solve recursively, combine results.

```cpp
// Merge sort (from Chapter 21)
void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);      // Divide
        mergeSort(arr, mid+1, right);   // Divide
        merge(arr, left, mid, right);   // Conquer
    }
}
```

### Pattern 2: Backtracking

**Idea**: Try all possibilities, backtrack when invalid.

```cpp
// Generate all permutations of string
void permute(string s, int l, int r) {
    if (l == r) {
        cout << s << endl;  // Print permutation
        return;
    }

    for (int i = l; i <= r; i++) {
        swap(s[l], s[i]);          // Choose
        permute(s, l+1, r);        // Explore
        swap(s[l], s[i]);          // Unchoose (backtrack)
    }
}

// permute("ABC", 0, 2) prints:
// ABC, ACB, BAC, BCA, CAB, CBA
```

### Pattern 3: Tree Recursion

**Idea**: Multiple recursive calls (like Fibonacci).

```cpp
// Count paths in grid (can only move right or down)
int countPaths(int m, int n) {
    if (m == 1 || n == 1) return 1;  // Base case
    return countPaths(m-1, n) + countPaths(m, n-1);  // Up + Left
}
```

---

## Complete Example Programs

### Example 1: Power Function

```cpp
// Calculate base^exp using recursion
int power(int base, int exp) {
    if (exp == 0) return 1;              // Base case
    if (exp == 1) return base;           // Base case

    int half = power(base, exp / 2);     // Divide

    if (exp % 2 == 0) {
        return half * half;              // Even exponent
    } else {
        return base * half * half;       // Odd exponent
    }
}

// Efficient: O(log n) instead of O(n)
// power(2, 10)
// = power(2, 5) * power(2, 5)
// = (2 * power(2, 2) * power(2, 2)) squared
// ...
```

---

### Example 2: Print All Subsets

```cpp
#include <vector>
#include <iostream>
using namespace std;

void printSubsets(vector<int>& arr, vector<int>& current, int index) {
    if (index == arr.size()) {
        cout << "{ ";
        for (int x : current) cout << x << " ";
        cout << "}" << endl;
        return;
    }

    // Exclude current element
    printSubsets(arr, current, index + 1);

    // Include current element
    current.push_back(arr[index]);
    printSubsets(arr, current, index + 1);
    current.pop_back();  // Backtrack
}

int main() {
    vector<int> arr = {1, 2, 3};
    vector<int> current;
    printSubsets(arr, current, 0);

    // Output:
    // { }
    // { 3 }
    // { 2 }
    // { 2 3 }
    // { 1 }
    // { 1 3 }
    // { 1 2 }
    // { 1 2 3 }

    return 0;
}
```

---

## Common Mistakes

### Mistake 1: Missing Base Case

```cpp
int bad(int n) {
    return n + bad(n-1);  // NO BASE CASE!
}
// Stack overflow! Infinite recursion!

// Fix:
int good(int n) {
    if (n == 0) return 0;  // Base case
    return n + good(n-1);
}
```

### Mistake 2: Wrong Base Case

```cpp
int factorial(int n) {
    if (n == 1) return 1;  // Bug: what if n = 0?
    return n * factorial(n-1);
}

// Fix: Handle n <= 1
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n-1);
}
```

### Mistake 3: Not Making Progress Toward Base Case

```cpp
int bad(int n) {
    if (n == 0) return 0;
    return n + bad(n);  // Bug: n never changes!
}

// Fix: Decrease n
int good(int n) {
    if (n == 0) return 0;
    return n + good(n-1);  // Progress!
}
```

---

## Practice Problems

### Problem 1: GCD (Greatest Common Divisor)
Implement Euclidean algorithm recursively: gcd(a, b) = gcd(b, a % b).

### Problem 2: Sum of Digits
Calculate sum of digits of a number (e.g., 123 → 6).

### Problem 3: Print N to 1 and 1 to N
Print numbers from N to 1, then 1 to N using only recursion.

### Problem 4: Count Ways to Reach Nth Stair
You can climb 1 or 2 steps at a time. Count ways to reach step N.

### Problem 5: Generate Parentheses
Generate all valid combinations of N pairs of parentheses.

---

## Key Takeaways

✅ **Recursion** = function calling itself
✅ **Must have base case** to prevent infinite recursion
✅ **Recursive case** makes progress toward base case
✅ **Uses call stack** - can cause stack overflow if too deep
✅ **Three key patterns**: Divide-and-conquer, backtracking, tree recursion
✅ **Tail recursion** can be optimized to iteration by compiler
✅ **Memoization** can optimize repeated calculations (Fibonacci)
✅ **Use when**: problem is naturally recursive (trees, graphs)
✅ **Avoid when**: deep recursion or simple iteration is clearer

---

## Recursion Decision Tree

```
Is the problem naturally self-similar?
├─ NO → Use iteration
└─ YES
    ├─ Will recursion depth be > 10,000?
    │   └─ YES → Use iteration (avoid stack overflow)
    └─ NO
        ├─ Are there repeated subproblems?
        │   └─ YES → Use memoization or DP
        └─ NO
            ├─ Is code much clearer with recursion?
            │   └─ YES → Use recursion!
            └─ NO → Use iteration
```

---

## Recursion Debugging Tips

**Tip 1**: Trace with small inputs (n=3, not n=100)

**Tip 2**: Print function entry/exit:

```cpp
int factorial(int n) {
    cout << "Entering factorial(" << n << ")" << endl;
    if (n <= 1) {
        cout << "Returning 1" << endl;
        return 1;
    }
    int result = n * factorial(n-1);
    cout << "Returning " << result << endl;
    return result;
}
```

**Tip 3**: Draw the recursion tree on paper!

---

## What's Next?

In **[Chapter 24](chapter-24-backtracking.md)**, you'll learn about **Backtracking** - a powerful recursive technique for systematically exploring all possibilities, perfect for solving constraint satisfaction problems like N-Queens and Sudoku!

You'll continue your journey through:
- Backtracking for exploring all possibilities
- Dynamic Programming for optimization
- Advanced algorithms and real-world applications

---

**Chapter Progress**: ✅ Chapters 1-23 Complete | 🎉 Course Complete!
