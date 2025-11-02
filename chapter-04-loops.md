# Chapter 4: Loops (Repeating Actions)

**What You'll Learn**: How to repeat code without copy-pasting, different types of loops, and when to use each one.

## Table of Contents

1. [Introduction: Why Loops?](#introduction-why-loops)
2. [The for Loop (Know the Count)](#1-the-for-loop-know-the-count)
3. [The while Loop (Unknown Count)](#2-the-while-loop-unknown-count)
4. [The do-while Loop (Run At Least Once)](#3-the-do-while-loop-run-at-least-once)
5. [Nested Loops (Loop Inside Loop)](#4-nested-loops-loop-inside-loop)
6. [Loop Control: break and continue](#5-loop-control-break-and-continue)
7. [Infinite Loops (Be Careful!)](#6-infinite-loops-be-careful)
8. [Complete Example Programs](#complete-example-programs)
9. [Common Mistakes](#common-mistakes)
10. [Practice Problems](#practice-problems)
11. [Key Takeaways](#key-takeaways)
12. [Loop Selection Guide](#loop-selection-guide)
13. [What's Next?](#whats-next)

---

## Introduction: Why Loops?

Imagine you want to print "Hello" 100 times. Without loops:

```cpp
cout << "Hello" << endl;
cout << "Hello" << endl;
cout << "Hello" << endl;
// ... 97 more times! 😱
```

**Problems**:
- ❌ Repetitive and tedious
- ❌ Error-prone (easy to make mistakes)
- ❌ Hard to change (what if you want 200 times?)
- ❌ Not scalable (what if count is unknown?)

**With a loop**:

```cpp
for (int i = 0; i < 100; i++) {
    cout << "Hello" << endl;
}
```

**Benefits**:
- ✅ Write once, repeat many times
- ✅ Easy to change (just modify the limit)
- ✅ Scalable (can use variables)

**Loops are like a DJ playing a song on repeat** - you set it up once, and it keeps going!

---

## 1. The for Loop (Know the Count)

Use `for` loops when you know **exactly how many times** to repeat.

**Syntax**:

```cpp
for (initialization; condition; update) {
    // Code to repeat
}
```

**Think of it as**:
```
for (START; KEEP_GOING_WHILE; AFTER_EACH_ROUND) {
    // Do this
}
```

---

### Example 1: Count 1 to 5

```cpp
for (int i = 1; i <= 5; i++) {
    cout << i << " ";
}
// Output: 1 2 3 4 5
```

**Step-by-step**:

```
Step 1: int i = 1        (START: i starts at 1)
Step 2: i <= 5?          (CHECK: 1 <= 5? YES)
Step 3: cout << 1        (EXECUTE body)
Step 4: i++              (UPDATE: i becomes 2)

Step 5: i <= 5?          (CHECK: 2 <= 5? YES)
Step 6: cout << 2        (EXECUTE body)
Step 7: i++              (UPDATE: i becomes 3)

... continues until i = 6

Step N: i <= 5?          (CHECK: 6 <= 5? NO)
Step N+1: EXIT LOOP
```

---

### Example 2: Count Down

```cpp
for (int i = 10; i >= 1; i--) {
    cout << i << " ";
}
cout << "Blast off!" << endl;
// Output: 10 9 8 7 6 5 4 3 2 1 Blast off!
```

---

### Example 3: Skip Numbers (Step by 2)

```cpp
// Print even numbers from 0 to 10
for (int i = 0; i <= 10; i += 2) {
    cout << i << " ";
}
// Output: 0 2 4 6 8 10
```

---

### Visual: for Loop Execution

```
for (int i = 0; i < 3; i++) {
    cout << i << endl;
}

Execution Flow:
┌─────────────────────┐
│ START: i = 0        │
└──────────┬──────────┘
           │
           ↓
┌─────────────────────┐
│ CHECK: i < 3?       │
│ (0 < 3 = true)      │
└──────────┬──────────┘
           │ YES
           ↓
┌─────────────────────┐
│ EXECUTE: cout << 0  │
└──────────┬──────────┘
           │
           ↓
┌─────────────────────┐
│ UPDATE: i++ (i=1)   │
└──────────┬──────────┘
           │
           ↓
┌─────────────────────┐
│ CHECK: i < 3?       │
│ (1 < 3 = true)      │
└──────────┬──────────┘
           │ YES
           ↓
... repeats ...
           │
           ↓
┌─────────────────────┐
│ CHECK: i < 3?       │
│ (3 < 3 = false)     │
└──────────┬──────────┘
           │ NO
           ↓
        EXIT LOOP
```

---

## 2. The while Loop (Unknown Count)

Use `while` loops when you **don't know how many times** you'll repeat.

**Syntax**:

```cpp
while (condition) {
    // Code to repeat
    // Must eventually make condition false!
}
```

**Think of it as**:
```
WHILE (something is true):
    Keep doing this
```

---

### Example 1: Count to 5

```cpp
int i = 1;
while (i <= 5) {
    cout << i << " ";
    i++;  // CRITICAL! Must update
}
// Output: 1 2 3 4 5
```

**Warning**: If you forget `i++`, the loop runs **forever** (infinite loop)!

---

### Example 2: Keep Asking Until Valid Input

```cpp
int age;
cout << "Enter your age (1-120): ";
cin >> age;

while (age < 1 || age > 120) {
    cout << "Invalid! Enter age (1-120): ";
    cin >> age;
}

cout << "Thank you! Age: " << age << endl;
```

**Test Run**:
```
Enter your age (1-120): 150
Invalid! Enter age (1-120): -5
Invalid! Enter age (1-120): 25
Thank you! Age: 25
```

This is perfect for **input validation**!

---

### Example 3: Sum Until User Enters 0

```cpp
int sum = 0;
int num;

cout << "Enter numbers (0 to stop):" << endl;
cin >> num;

while (num != 0) {
    sum += num;
    cin >> num;
}

cout << "Total sum: " << sum << endl;
```

**Test Run**:
```
Enter numbers (0 to stop):
5
10
3
0
Total sum: 18
```

---

## 3. The do-while Loop (Run At Least Once)

Use `do-while` when the loop must run **at least once**, then check condition.

**Syntax**:

```cpp
do {
    // Code to repeat (runs at least once!)
} while (condition);
```

**Difference from while**:
- `while`: Check condition FIRST, then execute
- `do-while`: Execute FIRST, then check condition

---

### Example 1: Menu System

```cpp
int choice;

do {
    cout << "\n=== Menu ===" << endl;
    cout << "1. Play Game" << endl;
    cout << "2. View Scores" << endl;
    cout << "3. Exit" << endl;
    cout << "Enter choice: ";
    cin >> choice;

    switch (choice) {
        case 1:
            cout << "Playing game..." << endl;
            break;
        case 2:
            cout << "Showing scores..." << endl;
            break;
        case 3:
            cout << "Goodbye!" << endl;
            break;
        default:
            cout << "Invalid choice!" << endl;
    }
} while (choice != 3);
```

The menu displays **at least once**, even before checking the condition!

---

### Visual Comparison: while vs do-while

**while loop**:
```
START
  ↓
CHECK condition?
  ├─ NO → EXIT
  └─ YES → Execute body → CHECK again
```

**do-while loop**:
```
START
  ↓
Execute body (always!)
  ↓
CHECK condition?
  ├─ NO → EXIT
  └─ YES → Execute body again
```

---

## 4. Nested Loops (Loop Inside Loop)

You can put loops **inside** other loops!

### Example 1: Multiplication Table

```cpp
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= 5; j++) {
        cout << (i * j) << "\t";  // \t = tab
    }
    cout << endl;  // New line after each row
}
```

**Output**:
```
1   2   3   4   5
2   4   6   8   10
3   6   9   12  15
4   8   12  16  20
5   10  15  20  25
```

**How it works**:
```
i=1: j runs 1,2,3,4,5 → prints 1,2,3,4,5
i=2: j runs 1,2,3,4,5 → prints 2,4,6,8,10
i=3: j runs 1,2,3,4,5 → prints 3,6,9,12,15
...
```

**Think of nested loops like a clock**:
- Outer loop (i) = hour hand (moves slowly)
- Inner loop (j) = minute hand (completes full cycle each time)

---

### Example 2: Pattern Printing

**Pattern: Right Triangle**
```cpp
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= i; j++) {
        cout << "* ";
    }
    cout << endl;
}
```

**Output**:
```
*
* *
* * *
* * * *
* * * * *
```

**Explanation**:
```
i=1: j runs 1 time       → *
i=2: j runs 2 times      → * *
i=3: j runs 3 times      → * * *
i=4: j runs 4 times      → * * * *
i=5: j runs 5 times      → * * * * *
```

---

## 5. Loop Control: break and continue

### break - Exit Loop Immediately

**Use case**: Stop loop when condition is met

```cpp
// Find first number divisible by 7
for (int i = 1; i <= 100; i++) {
    if (i % 7 == 0) {
        cout << "First number divisible by 7: " << i << endl;
        break;  // EXIT the loop now!
    }
}
// Output: First number divisible by 7: 7
```

Without `break`, it would print all numbers divisible by 7.

---

### continue - Skip Current Iteration

**Use case**: Skip certain values, but keep looping

```cpp
// Print numbers 1-10, but skip 5
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        continue;  // SKIP this iteration, go to next
    }
    cout << i << " ";
}
// Output: 1 2 3 4 6 7 8 9 10
```

**Visual**:
```
i=1: Not 5, print 1
i=2: Not 5, print 2
i=3: Not 5, print 3
i=4: Not 5, print 4
i=5: Is 5! continue → SKIP to i=6
i=6: Not 5, print 6
...
```

---

### Example: Print Only Even Numbers

```cpp
for (int i = 1; i <= 10; i++) {
    if (i % 2 != 0) {
        continue;  // Skip odd numbers
    }
    cout << i << " ";
}
// Output: 2 4 6 8 10
```

---

## 6. Infinite Loops (Be Careful!)

An **infinite loop** runs forever because the condition is always true.

### Accidental Infinite Loop (Bug!)

```cpp
int i = 1;
while (i <= 5) {
    cout << i << endl;
    // FORGOT i++!  i always stays 1!
}
// Prints 1 forever!
```

### Intentional Infinite Loop

```cpp
while (true) {
    cout << "Enter command (exit to quit): ";
    string cmd;
    cin >> cmd;

    if (cmd == "exit") {
        break;  // Exit the infinite loop
    }

    cout << "You entered: " << cmd << endl;
}
```

**When to use**: Server programs, game loops, event handlers

---

## Complete Example Programs

### Example 1: Sum of First N Numbers

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    cout << "Enter N: ";
    cin >> n;

    int sum = 0;
    for (int i = 1; i <= n; i++) {
        sum += i;
    }

    cout << "Sum of 1 to " << n << " = " << sum << endl;

    return 0;
}
```

---

### Example 2: Factorial

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    cout << "Enter number: ";
    cin >> n;

    long long factorial = 1;
    for (int i = 1; i <= n; i++) {
        factorial *= i;
    }

    cout << n << "! = " << factorial << endl;

    return 0;
}
```

---

### Example 3: Fibonacci Series

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    cout << "How many Fibonacci numbers? ";
    cin >> n;

    int a = 0, b = 1;

    cout << "Fibonacci series: ";
    for (int i = 0; i < n; i++) {
        cout << a << " ";
        int next = a + b;
        a = b;
        b = next;
    }
    cout << endl;

    return 0;
}
```

---

### Example 4: Prime Number Checker

```cpp
#include <iostream>
using namespace std;

int main() {
    int num;
    cout << "Enter a number: ";
    cin >> num;

    if (num <= 1) {
        cout << num << " is not prime" << endl;
        return 0;
    }

    bool isPrime = true;
    for (int i = 2; i * i <= num; i++) {
        if (num % i == 0) {
            isPrime = false;
            break;  // Found a divisor, not prime!
        }
    }

    if (isPrime) {
        cout << num << " is PRIME" << endl;
    } else {
        cout << num << " is NOT prime" << endl;
    }

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Off-by-One Error

```cpp
// Want to print 1 to 10 (inclusive)
for (int i = 1; i < 10; i++) {  // WRONG! Stops at 9
    cout << i << " ";
}

// CORRECT:
for (int i = 1; i <= 10; i++) {
    cout << i << " ";
}
```

### Mistake 2: Infinite Loop (Forgot Update)

```cpp
int i = 0;
while (i < 5) {
    cout << i;
    // FORGOT i++!
}
```

### Mistake 3: Semicolon After for

```cpp
for (int i = 0; i < 5; i++);  // WRONG! Semicolon ends loop
{
    cout << i;  // This executes only once!
}
```

### Mistake 4: Modifying Loop Variable Inside

```cpp
for (int i = 0; i < 10; i++) {
    cout << i;
    i += 2;  // BAD! Confusing and error-prone
}
```

---

## Practice Problems

### Problem 1: Sum of Digits
Read a number and find the sum of its digits.
Example: 12345 → 1+2+3+4+5 = 15

### Problem 2: Reverse a Number
Read a number and print it in reverse.
Example: 12345 → 54321

### Problem 3: Pattern - Pyramid
```
    *
   ***
  *****
 *******
*********
```

### Problem 4: Count Vowels
Read a string and count how many vowels (a,e,i,o,u) it has.

### Problem 5: GCD (Greatest Common Divisor)
Find GCD of two numbers using Euclidean algorithm.

### Problem 6: Print All Prime Numbers (1-100)
Use nested loops or efficient checking.

### Problem 7: Armstrong Number
Check if a number equals the sum of cubes of its digits.
Example: 153 = 1³ + 5³ + 3³ = 1 + 125 + 27

### Problem 8: Multiplication Table (Full)
Print multiplication table for 1-10 (10×10 grid).

---

## Key Takeaways

✅ **for loop**: Use when you know the count
✅ **while loop**: Use when count is unknown
✅ **do-while loop**: Use when must run at least once
✅ **Nested loops**: Loop inside loop (rows × columns)
✅ **break**: Exit loop immediately
✅ **continue**: Skip current iteration, continue loop
✅ **Always update loop variable** to avoid infinite loops
✅ **Watch for off-by-one errors** (< vs <=)

---

## Loop Selection Guide

| Situation | Use |
|-----------|-----|
| Know exact count | `for` loop |
| Unknown count | `while` loop |
| Must run at least once | `do-while` loop |
| Need row/column iteration | Nested `for` loops |
| Input validation | `while` or `do-while` |
| Menu systems | `do-while` |

---

## What's Next?

In **Chapter 5**, you'll learn about functions - how to break your code into reusable pieces and understand how the call stack works!

---

**Chapter Progress**: ✅ Chapters 1-4 Complete | 📖 Next: Chapter 5
