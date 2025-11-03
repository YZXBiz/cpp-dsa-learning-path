# Chapter 6: Pointers & Memory Management

**What You'll Learn**: What pointers are, WHY they exist, how memory addresses work, and dynamic memory allocation.

## Table of Contents

1. [Introduction: The Library Address Analogy](#introduction-the-library-address-analogy)
2. [What is a Pointer?](#1-what-is-a-pointer)
3. [Declaring and Using Pointers](#2-declaring-and-using-pointers)
4. [Dereferencing: Following the Address](#3-dereferencing-following-the-address)
5. [Why Pointers Exist: Real Use Cases](#4-why-pointers-exist-real-use-cases)
6. [Pointer Arithmetic](#5-pointer-arithmetic)
7. [Dynamic Memory Allocation](#6-dynamic-memory-allocation)
8. [Null Pointers](#7-null-pointers)
9. [Common Pointer Mistakes](#8-common-pointer-mistakes)
10. [Pointers vs References](#9-pointers-vs-references)
11. [Complete Example Programs](#complete-example-programs)
12. [Practice Problems](#practice-problems)
13. [Key Takeaways](#key-takeaways)
14. [Smart Pointers (Modern C++)](#smart-pointers-modern-c)
15. [What's Next?](#whats-next)

---

## Introduction: The Library Address Analogy

Imagine you wrote a **huge encyclopedia** (1000 pages). You want to share it with your friend.

**Option 1**: Photocopy all 1000 pages
- ❌ Expensive
- ❌ Slow
- ❌ Wastes space
- ❌ If you update your copy, friend's copy doesn't change

**Option 2**: Give them the **library address** where your book is stored
- ✅ Fast (just a small piece of paper)
- ✅ Cheap
- ✅ If you update the book, everyone sees the update
- ✅ Multiple people can access the same book

**This is exactly what pointers do!**

A **pointer** stores the **address** (location) of data instead of the data itself.

---

## 1. What is a Pointer?

A pointer is a variable that stores a **memory address**.

**Analogy**:
```
Regular variable  =  A box containing treasure
Pointer          =  A map showing WHERE the treasure is
```

### Memory Addresses

Every variable lives at a specific location in memory:

```
Memory (like a street with houses):

Address   Variable    Value
-------   --------    -----
0x1000    [  age  ]    25      ← The actual data
0x1004    [ height]    180
0x1008    [ score ]    95
0x100C    [  ptr  ]  0x1000    ← Points to address 0x1000
```

The `&` operator gives you the address:

```cpp
int age = 25;

cout << age;      // Prints: 25 (the value)
cout << &age;     // Prints: 0x1000 (the address)
```

---

## 2. Declaring and Using Pointers

**Syntax**:

```cpp
int* ptr;        // ptr is a pointer to an integer
int *ptr;        // Same thing (style preference)
int * ptr;       // Same thing
```

**Example**:

```cpp
int age = 25;
int* ptr = &age;  // ptr stores the address of age

cout << age;      // 25 (value)
cout << &age;     // 0x1000 (address)
cout << ptr;      // 0x1000 (pointer stores address)
cout << *ptr;     // 25 (dereference: go to address and get value)
```

**The `*` has TWO meanings**:

1. **Declaration**: `int* ptr` means "ptr is a pointer to int"
2. **Dereferencing**: `*ptr` means "go to the address and get/set the value"

---

## 3. Dereferencing: Following the Address

**Dereferencing** means "go to this address and access what's there".

```cpp
int age = 25;
int* ptr = &age;

cout << *ptr;     // 25 (follow ptr to age, get value)

*ptr = 30;        // Change value at address
cout << age;      // 30 (age changed!)
```

**Visual**:

```
Memory:
┌────────────────────┐       ┌────────────────────┐
│ Address: 0x1000    │   ┌──│ Address: 0x1004    │
│ Variable: age      │   │  │ Variable: ptr      │
│ Value: 25 → 30     │◄──┘  │ Value: 0x1000      │
└────────────────────┘       └────────────────────┘
                             (ptr points to age)

*ptr = 30  means:
1. Go to address in ptr (0x1000)
2. Change value there to 30
```

---

## 4. Why Pointers Exist: Real Use Cases

### Use Case 1: Modify Function Parameters

**Without pointers (doesn't work)**:

```cpp
void tryToChange(int x) {
    x = 100;  // Only changes local copy
}

int age = 25;
tryToChange(age);
cout << age;  // Still 25!
```

**With pointers (works!)**:

```cpp
void actuallyChange(int* ptr) {
    *ptr = 100;  // Changes original
}

int age = 25;
actuallyChange(&age);  // Pass address
cout << age;  // Now 100!
```

---

### Use Case 2: Saving Memory for Large Data

```cpp
struct HugeData {
    char data[10000];  // 10,000 bytes!
};

// Pass by value (BAD):
void processData(HugeData data) {  // Copies ALL 10,000 bytes!
    // ...
}

// Pass by pointer (GOOD):
void processData(HugeData* data) {  // Copies only 8 bytes (address)!
    // ...
}
```

---

### Use Case 3: Dynamic Memory (Create Variables at Runtime)

```cpp
int n;
cout << "How many numbers? ";
cin >> n;

int* arr = new int[n];  // Create array of size n at runtime!

// Use array...
for (int i = 0; i < n; i++) {
    arr[i] = i * 10;
}

delete[] arr;  // Free memory when done
```

---

## 5. Pointer Arithmetic

Pointers can move through memory:

```cpp
int arr[5] = {10, 20, 30, 40, 50};
int* ptr = arr;  // Points to first element

cout << *ptr;        // 10 (arr[0])
cout << *(ptr + 1);  // 20 (arr[1])
cout << *(ptr + 2);  // 30 (arr[2])

ptr++;               // Move to next element
cout << *ptr;        // 20
```

**Memory Layout**:

```
arr[0]    arr[1]    arr[2]    arr[3]    arr[4]
[  10  ]  [  20  ]  [  30  ]  [  40  ]  [  50  ]
   ↑        ↑         ↑         ↑         ↑
   ptr    ptr+1     ptr+2     ptr+3     ptr+4
```

---

## 6. Dynamic Memory Allocation

**Static allocation** (compile-time):

```cpp
int arr[100];  // Size must be known at compile time
```

**Dynamic allocation** (runtime):

```cpp
int* arr = new int[n];  // Size can be decided at runtime!
```

### new and delete

**Allocate memory**:

```cpp
int* ptr = new int;        // Allocate one int
*ptr = 42;

int* arr = new int[10];    // Allocate array of 10 ints
```

**Free memory** (CRITICAL!):

```cpp
delete ptr;                // Free single variable
delete[] arr;              // Free array (note the [])
```

**Why free?** Memory is limited! If you don't free, you get a **memory leak**.

---

## 7. Null Pointers

A **null pointer** points to nothing:

```cpp
int* ptr = nullptr;  // Modern C++ (use this!)
int* ptr = NULL;     // Old style
int* ptr = 0;        // Even older style
```

**Always check before dereferencing**:

```cpp
if (ptr != nullptr) {
    cout << *ptr;  // Safe
} else {
    cout << "Pointer is null!" << endl;
}
```

**Dereferencing null pointer = CRASH!**

---

## 8. Common Pointer Mistakes ⚠️

### Mistake 1: Uninitialized Pointer

```cpp
int* ptr;        // Points to random memory!
cout << *ptr;    // CRASH! Undefined behavior
```

**Fix**:

```cpp
int* ptr = nullptr;  // Initialize to null
```

---

### Mistake 2: Dangling Pointer

```cpp
int* ptr = new int(25);
delete ptr;      // Memory freed
cout << *ptr;    // CRASH! ptr points to freed memory
```

**Fix**:

```cpp
delete ptr;
ptr = nullptr;   // Set to null after deleting
```

---

### Mistake 3: Memory Leak

```cpp
void createNumber() {
    int* ptr = new int(25);
    // FORGOT to delete!
}  // ptr goes out of scope, memory lost forever!
```

**Fix**:

```cpp
void createNumber() {
    int* ptr = new int(25);
    // ... use ptr ...
    delete ptr;  // Always free!
}
```

**Rule**: Every `new` must have a matching `delete`!

---

### Mistake 4: Deleting Stack Variables

```cpp
int age = 25;
int* ptr = &age;
delete ptr;      // WRONG! age is on stack, not heap!
```

**Rule**: Only delete what you `new`!

---

## 9. Pointers vs References

C++ has **references** which are safer than pointers:

**Pointers**:

```cpp
void change(int* ptr) {
    *ptr = 100;  // Need * to dereference
}

int age = 25;
change(&age);    // Need & to get address
```

**References** (simpler):

```cpp
void change(int& ref) {
    ref = 100;   // No * needed!
}

int age = 25;
change(age);     // No & needed!
```

**When to use**:
- **References**: Prefer for function parameters (safer, easier)
- **Pointers**: When you need dynamic memory, arrays, or null values

---

## Complete Example Programs

### Example 1: Swap Using Pointers

```cpp
#include <iostream>
using namespace std;

void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int x = 5, y = 10;

    cout << "Before: x=" << x << ", y=" << y << endl;
    swap(&x, &y);
    cout << "After: x=" << x << ", y=" << y << endl;

    return 0;
}
```

---

### Example 2: Dynamic Array

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    cout << "How many numbers? ";
    cin >> n;

    // Allocate dynamic array
    int* arr = new int[n];

    // Input
    cout << "Enter " << n << " numbers:" << endl;
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }

    // Calculate sum
    int sum = 0;
    for (int i = 0; i < n; i++) {
        sum += arr[i];
    }

    cout << "Sum: " << sum << endl;

    // Free memory
    delete[] arr;

    return 0;
}
```

---

## Practice Problems

### Problem 1: Pointer Basics
Declare an int variable, create a pointer to it, and modify the value through the pointer.

### Problem 2: Array with Pointers
Create an array using pointers, fill it with values, and print using pointer arithmetic.

### Problem 3: Find Maximum (with pointers)
Write a function that takes an array pointer and size, returns the maximum element.

### Problem 4: Reverse Array (with pointers)
Use two pointers (start and end) to reverse an array in-place.

### Problem 5: Dynamic 2D Array
Allocate a 2D array dynamically and free it properly.

---

## Key Takeaways

✅ **Pointer = variable storing memory address**
✅ **`&` = address-of operator** ("where is this variable?")
✅ **`*` = dereference operator** ("go to address, get value")
✅ **Why pointers?**
   - Modify original data in functions
   - Save memory (pass address, not copy)
   - Dynamic memory allocation
✅ **`new` = allocate**, **`delete` = free**
✅ **Always initialize pointers** (nullptr if nothing yet)
✅ **Every `new` needs a `delete`** (avoid memory leaks)
✅ **References are safer** for most cases

---

## Smart Pointers (Modern C++)

**Note**: This chapter covers **raw pointers** (`int*`, `new`, `delete`) which are fundamental to understanding how memory works. However, modern C++ (C++11 and later) provides **smart pointers** that handle memory automatically.

### What Are Smart Pointers?

Smart pointers are **wrapper classes** that automatically manage memory. They delete memory when it's no longer needed - **no manual `delete` required**!

**Types of smart pointers**:
1. **`unique_ptr`**: Exclusive ownership (one owner only)
2. **`shared_ptr`**: Shared ownership (multiple owners)
3. **`weak_ptr`**: Temporary access without ownership

### Example: unique_ptr

```cpp
#include <memory>  // For smart pointers

int main() {
    // Raw pointer (manual management)
    int* rawPtr = new int(42);
    // ... use it ...
    delete rawPtr;  // Must remember to delete!

    // Smart pointer (automatic management)
    std::unique_ptr<int> smartPtr = std::make_unique<int>(42);
    // ... use it ...
    // No delete needed! Automatically freed when smartPtr goes out of scope
}
```

### Why Learn Raw Pointers First?

Even though smart pointers are better for production code, understanding raw pointers is crucial because:

1. **Foundation**: You need to understand how memory works
2. **Legacy code**: Lots of existing code uses raw pointers
3. **Data structures**: When implementing linked lists, trees, etc., you'll work with pointers
4. **Interview questions**: Often test pointer knowledge
5. **Smart pointers are built on raw pointers**: Understanding the base helps you use smart pointers better

### When to Use What?

**In this course**:
- We use **raw pointers** to teach fundamentals and implement data structures

**In production code** (after you're comfortable with pointers):
- Prefer **smart pointers** (`unique_ptr`, `shared_ptr`)
- Use raw pointers only when necessary (low-level code, performance-critical sections)

**Recommendation**: After completing this course, study smart pointers in depth. They're the modern, safer way to manage memory in C++!

---

## What's Next?

In **Chapter 7**, you'll learn about arrays in depth - how they're stored in memory, 2D arrays, and array operations!

---

**Chapter Progress**: ✅ Chapters 1-6 Complete | 📖 Next: Chapter 7
