# Chapter 5: Functions & The Call Stack

**What You'll Learn**: Why functions exist, how the call stack works, pass by value vs reference, and recursion basics.

## Table of Contents

1. [Introduction: The Problem Functions Solve](#introduction-the-problem-functions-solve)
2. [Function Basics](#1-function-basics)
3. [The Call Stack: How Functions Work Behind the Scenes](#2-the-call-stack-how-functions-work-behind-the-scenes)
4. [Pass by Value: Making a Copy](#3-pass-by-value-making-a-copy)
5. [Pass by Reference: Sharing the Original](#4-pass-by-reference-sharing-the-original)
6. [Return Values](#5-return-values)
7. [Function Overloading: Same Name, Different Parameters](#6-function-overloading-same-name-different-parameters)
8. [Recursion: Function Calling Itself](#7-recursion-function-calling-itself)
9. [Complete Example Programs](#complete-example-programs)
10. [Common Mistakes](#common-mistakes)
11. [Practice Problems](#practice-problems)
12. [Key Takeaways](#key-takeaways)
13. [What's Next?](#whats-next)

---

## Introduction: The Problem Functions Solve

Imagine you need to calculate the average of marks for 100 students. Without functions:

```cpp
// Student 1
int sum1 = mark1_sub1 + mark1_sub2 + mark1_sub3;
float avg1 = sum1 / 3.0;

// Student 2
int sum2 = mark2_sub1 + mark2_sub2 + mark2_sub3;
float avg2 = sum2 / 3.0;

// ... 98 more times! 😱
```

**Problems**:
- ❌ Repetitive code (violates DRY - Don't Repeat Yourself)
- ❌ Hard to maintain (fix bug in 100 places!)
- ❌ Error-prone
- ❌ Not reusable

**With a function**:

```cpp
float calculateAverage(int a, int b, int c) {
    return (a + b + c) / 3.0;
}

// Use it anywhere!
float avg1 = calculateAverage(80, 90, 85);
float avg2 = calculateAverage(70, 75, 80);
// ... easy!
```

**Functions are like recipes** - write once, use many times!

---

## 1. Function Basics

**Syntax**:

```cpp
return_type function_name(parameters) {
    // Function body
    return value;  // if return_type is not void
}
```

**Parts**:
1. **Return type**: What the function gives back (int, float, void, etc.)
2. **Function name**: What you call it
3. **Parameters**: Input values (can be empty)
4. **Function body**: Code to execute
5. **Return statement**: Value to send back

---

### Example 1: Simple Function

```cpp
// Function to add two numbers
int add(int a, int b) {
    return a + b;
}

int main() {
    int result = add(5, 3);
    cout << "Sum: " << result << endl;  // Sum: 8
    return 0;
}
```

---

### Example 2: Function with No Return (void)

```cpp
void greet(string name) {
    cout << "Hello, " << name << "!" << endl;
    // No return statement needed
}

int main() {
    greet("Alice");  // Hello, Alice!
    greet("Bob");    // Hello, Bob!
    return 0;
}
```

**void** means "returns nothing" - like a doorbell (does something but doesn't give anything back).

---

## 2. The Call Stack: How Functions Work Behind the Scenes

When you call a function, the computer uses a **call stack** (like a stack of trays).

**Think of it like a cafeteria**:
```
┌─────────────┐
│  Top Tray   │  ← Most recent function (always work on this)
├─────────────┤
│             │
├─────────────┤
│             │
├─────────────┤
│  Bottom     │  ← main() function (first one)
└─────────────┘
```

**Rules**:
- Add to TOP (push)
- Remove from TOP (pop)
- LIFO: Last In, First Out

---

### Visual Example

```cpp
void third() {
    cout << "In third()" << endl;
}

void second() {
    cout << "In second(), calling third()" << endl;
    third();
    cout << "Back in second()" << endl;
}

void first() {
    cout << "In first(), calling second()" << endl;
    second();
    cout << "Back in first()" << endl;
}

int main() {
    cout << "In main(), calling first()" << endl;
    first();
    cout << "Back in main()" << endl;
    return 0;
}
```

**Call Stack Evolution**:

```
Step 1: main() starts
┌─────────────┐
│   main()    │
└─────────────┘

Step 2: main() calls first()
┌─────────────┐
│   first()   │ ← Executing
├─────────────┤
│   main()    │ ← Paused
└─────────────┘

Step 3: first() calls second()
┌─────────────┐
│  second()   │ ← Executing
├─────────────┤
│   first()   │ ← Paused
├─────────────┤
│   main()    │ ← Paused
└─────────────┘

Step 4: second() calls third()
┌─────────────┐
│   third()   │ ← Executing
├─────────────┤
│  second()   │ ← Paused
├─────────────┤
│   first()   │ ← Paused
├─────────────┤
│   main()    │ ← Paused
└─────────────┘

Step 5: third() finishes, returns to second()
┌─────────────┐
│  second()   │ ← Resumes
├─────────────┤
│   first()   │
├─────────────┤
│   main()    │
└─────────────┘

... and so on until back to main()
```

---

## 3. Pass by Value: Making a Copy

By default, C++ **copies** the value when you pass it to a function.

```cpp
void tryToChange(int x) {
    x = 100;  // Changes the COPY
    cout << "Inside function: " << x << endl;  // 100
}

int main() {
    int num = 25;
    tryToChange(num);
    cout << "After function: " << num << endl;  // Still 25!
    return 0;
}
```

**Why?** Because `tryToChange` gets a **photocopy** of `num`, not the original!

**Memory Diagram**:

```
Call Stack:
┌──────────────────────┐
│  tryToChange()       │
│  x = 25 (COPY)       │  ← Changes this copy
├──────────────────────┤
│  main()              │
│  num = 25 (original) │  ← Original unchanged
└──────────────────────┘
```

**Real-World Analogy**: You photocopy a document and give the copy to someone. They mark up the copy, but your original is unchanged.

---

## 4. Pass by Reference: Sharing the Original

To modify the original, use **reference** (`&`):

```cpp
void actuallyChange(int& x) {  // Note the &
    x = 100;  // Changes the ORIGINAL
    cout << "Inside function: " << x << endl;  // 100
}

int main() {
    int num = 25;
    actuallyChange(num);
    cout << "After function: " << num << endl;  // Now 100!
    return 0;
}
```

**Memory Diagram**:

```
Call Stack:
┌──────────────────────┐
│  actuallyChange()    │
│  x → refers to num   │  ← x is an ALIAS for num
├──────────────────────┤
│  main()              │
│  num = 25            │  ← x directly modifies this
└──────────────────────┘
```

**Real-World Analogy**: Instead of a photocopy, you give the library location of the original. Any changes affect the original.

---

### Example: Swap Function

```cpp
// Pass by value (DOESN'T work)
void swapWrong(int a, int b) {
    int temp = a;
    a = b;
    b = temp;
    // Swaps copies, not originals!
}

// Pass by reference (WORKS!)
void swapCorrect(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
    // Swaps originals!
}

int main() {
    int x = 5, y = 10;

    swapWrong(x, y);
    cout << "After swapWrong: x=" << x << ", y=" << y << endl;
    // Output: x=5, y=10 (unchanged)

    swapCorrect(x, y);
    cout << "After swapCorrect: x=" << x << ", y=" << y << endl;
    // Output: x=10, y=5 (swapped!)

    return 0;
}
```

---

## 5. Return Values

Functions can **return** a value to the caller:

```cpp
int multiply(int a, int b) {
    return a * b;  // Send result back
}

int main() {
    int result = multiply(5, 3);  // result = 15
    cout << result << endl;
    return 0;
}
```

**Think of return as**: "Here's your answer, I'm done!"

---

## 6. Function Overloading: Same Name, Different Parameters

C++ allows multiple functions with the **same name** but **different parameters**:

```cpp
int add(int a, int b) {
    return a + b;
}

double add(double a, double b) {
    return a + b;
}

int add(int a, int b, int c) {
    return a + b + c;
}

int main() {
    cout << add(5, 3) << endl;         // Calls first: 8
    cout << add(5.5, 3.2) << endl;     // Calls second: 8.7
    cout << add(1, 2, 3) << endl;      // Calls third: 6
    return 0;
}
```

The compiler chooses the right version based on **argument types**.

---

## 7. Recursion: Function Calling Itself

A **recursive function** calls itself!

```cpp
int factorial(int n) {
    if (n <= 1) return 1;          // Base case
    return n * factorial(n - 1);    // Recursive case
}

// factorial(5) = 5 * factorial(4)
//              = 5 * 4 * factorial(3)
//              = 5 * 4 * 3 * factorial(2)
//              = 5 * 4 * 3 * 2 * factorial(1)
//              = 5 * 4 * 3 * 2 * 1 = 120
```

**Call Stack for factorial(3)**:

```
Step 1: factorial(3) calls factorial(2)
┌──────────────────┐
│  factorial(2)    │
├──────────────────┤
│  factorial(3)    │
├──────────────────┤
│  main()          │
└──────────────────┘

Step 2: factorial(2) calls factorial(1)
┌──────────────────┐
│  factorial(1)    │
├──────────────────┤
│  factorial(2)    │
├──────────────────┤
│  factorial(3)    │
├──────────────────┤
│  main()          │
└──────────────────┘

Step 3: factorial(1) returns 1
┌──────────────────┐
│  factorial(2)    │ ← Gets 1, returns 2*1=2
├──────────────────┤
│  factorial(3)    │ ← Gets 2, returns 3*2=6
├──────────────────┤
│  main()          │ ← Gets 6
└──────────────────┘
```

**Critical**: Must have a **base case** (stopping condition) or it runs forever!

---

## Complete Example Programs

### Example 1: Calculator

```cpp
#include <iostream>
using namespace std;

int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }
int multiply(int a, int b) { return a * b; }
double divide(int a, int b) {
    if (b == 0) {
        cout << "Error: Division by zero!" << endl;
        return 0;
    }
    return (double)a / b;
}

int main() {
    int a, b;
    char op;

    cout << "Enter expression (a op b): ";
    cin >> a >> op >> b;

    switch (op) {
        case '+': cout << a << " + " << b << " = " << add(a, b) << endl; break;
        case '-': cout << a << " - " << b << " = " << subtract(a, b) << endl; break;
        case '*': cout << a << " * " << b << " = " << multiply(a, b) << endl; break;
        case '/': cout << a << " / " << b << " = " << divide(a, b) << endl; break;
        default: cout << "Invalid operator!" << endl;
    }

    return 0;
}
```

---

### Example 2: Check Prime (with function)

```cpp
#include <iostream>
using namespace std;

bool isPrime(int n) {
    if (n <= 1) return false;

    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) return false;
    }

    return true;
}

int main() {
    int num;
    cout << "Enter number: ";
    cin >> num;

    if (isPrime(num)) {
        cout << num << " is PRIME" << endl;
    } else {
        cout << num << " is NOT prime" << endl;
    }

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Forgetting to Return

```cpp
int add(int a, int b) {
    int sum = a + b;
    // FORGOT return!
}  // Undefined behavior!
```

### Mistake 2: Using Pass by Value When Need Reference

```cpp
void swap(int a, int b) {  // Pass by value
    int temp = a;
    a = b;
    b = temp;
}  // Doesn't work! Need int& a, int& b
```

### Mistake 3: Infinite Recursion

```cpp
int bad(int n) {
    return bad(n - 1);  // NO BASE CASE! Runs forever!
}
```

---

## Practice Problems

### Problem 1: Power Function
Write `int power(int base, int exp)` that calculates base^exp.

### Problem 2: GCD Function
Write `int gcd(int a, int b)` using Euclidean algorithm.

### Problem 3: Fibonacci (Recursive)
Write `int fib(int n)` that returns nth Fibonacci number.

### Problem 4: Array Sum
Write `int arraySum(int arr[], int size)` that returns sum of array elements.

### Problem 5: Print Pattern
Write `void printStars(int n)` that prints n stars.

---

## Key Takeaways

✅ **Functions = reusable code blocks**
✅ **Call stack = LIFO** (Last In, First Out)
✅ **Pass by value = copy** (doesn't change original)
✅ **Pass by reference = share** (changes original)
✅ **Return = send value back**
✅ **Overloading = same name, different parameters**
✅ **Recursion = function calls itself** (need base case!)

---

## What's Next?

In **Chapter 6**, you'll learn about pointers and memory management - the most powerful (and tricky!) feature of C++!

---

**Chapter Progress**: ✅ Chapters 1-5 Complete | 📖 Next: Chapter 6
