# C++ Quick Reference Guide

This is a quick lookup guide for common C++ syntax and concepts. Use this while coding to quickly check syntax!

## Table of Contents
1. [Basic Template](#basic-template)
2. [Data Types](#data-types)
3. [Operators](#operators)
4. [Control Structures](#control-structures)
5. [Loops](#loops)
6. [Functions](#functions)
7. [Arrays & Strings](#arrays--strings)
8. [Pointers](#pointers)
9. [Classes](#classes-basic)
10. [STL Containers](#stl-containers)
11. [Common STL Functions](#common-stl-functions)

---

## Basic Template

```cpp
#include <iostream>      // For input/output
#include <vector>        // For vectors
#include <string>        // For strings
#include <algorithm>     // For sort, find, etc.
using namespace std;

int main() {
    // Your code here
    return 0;
}
```

---

## Data Types

```cpp
// Integer types
int age = 25;                  // -2.1B to +2.1B
unsigned int count = 100;      // 0 to 4.2B
short num = 1000;              // -32K to +32K
long big = 1000000000;         // Much larger

// Floating point
float price = 19.99f;          // 6-7 decimal places
double pi = 3.14159265;        // 15-16 decimal places

// Character and Boolean
char grade = 'A';              // Single character
bool is_true = true;           // true or false

// String
string name = "John";          // Text (need #include <string>)

// Size of a type
cout << sizeof(int) << endl;   // Usually 4 bytes
```

---

## Operators

```cpp
// Arithmetic
int a = 10, b = 3;
a + b;      // 13 (addition)
a - b;      // 7 (subtraction)
a * b;      // 30 (multiplication)
a / b;      // 3 (division, integer)
a % b;      // 1 (modulo/remainder)
a++;        // 11 (increment)
a--;        // 9 (decrement)
a += 5;     // a = a + 5

// Comparison
a == b;     // false (equal)
a != b;     // true (not equal)
a > b;      // true (greater than)
a < b;      // false (less than)
a >= b;     // true (greater or equal)
a <= b;     // false (less or equal)

// Logical
true && false;   // false (AND - both must be true)
true || false;   // true (OR - at least one must be true)
!true;           // false (NOT - inverts)

// Assignment
x = 5;      // Assign 5 to x
x += 3;     // x = x + 3
x -= 2;     // x = x - 2
x *= 4;     // x = x * 4
x /= 2;     // x = x / 2

// Ternary (short if-else)
int max = (a > b) ? a : b;  // if a > b, max = a, else max = b
```

---

## Control Structures

### If Statements
```cpp
if (condition) {
    // Do something if true
}

if (condition) {
    // Do this if true
} else {
    // Do this if false
}

if (condition1) {
    // ...
} else if (condition2) {
    // ...
} else if (condition3) {
    // ...
} else {
    // ...
}
```

### Switch Statement
```cpp
switch (variable) {
    case 1:
        cout << "One";
        break;
    case 2:
        cout << "Two";
        break;
    default:
        cout << "Other";
}
```

---

## Loops

### For Loop
```cpp
// Basic for loop
for (int i = 0; i < 5; i++) {
    cout << i << " ";  // Prints: 0 1 2 3 4
}

// Counting down
for (int i = 5; i > 0; i--) {
    cout << i << " ";  // Prints: 5 4 3 2 1
}

// Loop over vector/array
vector<int> arr = {1, 2, 3};
for (int i = 0; i < arr.size(); i++) {
    cout << arr[i] << " ";
}

// Modern range-based for (C++11+)
for (int num : arr) {
    cout << num << " ";
}
```

### While Loop
```cpp
int i = 0;
while (i < 5) {
    cout << i << " ";
    i++;
}

// Input until valid
int x;
while (x < 0) {
    cout << "Enter positive: ";
    cin >> x;
}
```

### Do-While Loop
```cpp
int i = 0;
do {
    cout << i << " ";
    i++;
} while (i < 5);
// Runs at least once!
```

### Loop Control
```cpp
// Break: exit loop
for (int i = 0; i < 10; i++) {
    if (i == 5) break;  // Exit when i=5
}

// Continue: skip to next iteration
for (int i = 0; i < 5; i++) {
    if (i == 2) continue;  // Skip i=2
    cout << i << " ";  // Prints: 0 1 3 4
}
```

---

## Functions

```cpp
// Function declaration (before main or in header)
int add(int a, int b);

// Function definition
int add(int a, int b) {
    return a + b;
}

// Function call
int result = add(5, 3);  // result = 8

// Function with no return
void greet(string name) {
    cout << "Hello " << name << endl;
}

// Function overloading (same name, different parameters)
int multiply(int a, int b) {
    return a * b;
}

double multiply(double a, double b) {
    return a * b;
}

// Recursive function
int factorial(int n) {
    if (n <= 1) return 1;  // Base case
    return n * factorial(n - 1);  // Recursive case
}
```

---

## Arrays & Strings

### Arrays
```cpp
// Create array
int arr[5];                           // Array of 5 integers
int arr[5] = {1, 2, 3, 4, 5};       // Initialize with values
int arr[] = {10, 20, 30};           // Size inferred from values

// Access elements (0-indexed)
arr[0] = 10;    // Set first element
cout << arr[0]; // Print first element: 10

// Loop through array
for (int i = 0; i < 5; i++) {
    cout << arr[i] << " ";
}

// Array size
int size = sizeof(arr) / sizeof(arr[0]);  // Number of elements

// 2D array
int matrix[3][3];
matrix[0][0] = 5;
```

### Strings (C-style)
```cpp
// C-style string (character array)
char str[20];
str[0] = 'H';
str[1] = 'i';
str[2] = '\0';  // Null terminator

// String input
cin >> str;           // Input single word
cin.getline(str, 20); // Input whole line
```

### Strings (C++ style - recommended)
```cpp
#include <string>

string str = "Hello";
string str2 = "World";

// String operations
str.length();           // Length
str + " " + str2;       // Concatenation: "Hello World"
str[0];                 // First character: 'H'
str.substr(1, 3);       // Substring from index 1, length 3: "ell"
str.find("ll");         // Find position of "ll": 2
str.replace(1, 2, "y"); // Replace 2 chars at index 1 with "y"

// String to number
int num = stoi("123");      // String to int
double d = stod("3.14");    // String to double

// Number to string
string s = to_string(123);  // Int to string: "123"
```

---

## Pointers

```cpp
int x = 10;
int* ptr = &x;      // Pointer to x (& = address of)

cout << ptr;        // Print address (memory location)
cout << *ptr;       // Print value (dereference with *)
cout << &ptr;       // Print address of pointer

*ptr = 20;          // Change x through pointer
cout << x;          // 20

// Pointer arithmetic
int arr[5] = {1, 2, 3, 4, 5};
int* p = arr;
cout << *p;         // 1 (first element)
cout << *(p + 1);   // 2 (second element)
p++;                // Point to next element

// Dynamic memory
int* ptr = new int;        // Allocate memory
*ptr = 100;
delete ptr;                // Free memory
ptr = nullptr;             // Set to null

// Dynamic array
int* arr = new int[5];     // Array of 5 integers
delete[] arr;              // Free (note the [] for arrays)
```

---

## Classes (Basic)

```cpp
class Student {
private:     // Only accessible inside class
    int id;

public:      // Accessible from outside
    string name;

    // Constructor (initialize object)
    Student(string n, int i) {
        name = n;
        id = i;
    }

    // Member function
    void display() {
        cout << name << " (ID: " << id << ")" << endl;
    }

    // Getter
    int getId() {
        return id;
    }
};

// Create and use object
Student s("John", 101);
s.display();        // John (ID: 101)
cout << s.name;     // John

// Access through pointer
Student* ptr = new Student("Alice", 102);
ptr->display();     // Alice (ID: 102)
delete ptr;
```

---

## STL Containers

### Vector (Dynamic Array)
```cpp
#include <vector>

vector<int> v;          // Empty vector
vector<int> v(5, 0);    // Size 5, all zeros
vector<int> v = {1, 2, 3};

// Operations
v.push_back(4);         // Add at end: {1, 2, 3, 4}
v.pop_back();           // Remove from end: {1, 2, 3}
v.size();               // Size: 3
v[0];                   // Access: 1
v.at(0);                // Safe access (throws error if out of bounds)
v.front();              // First element: 1
v.back();               // Last element: 3
v.insert(v.begin(), 0); // Insert at beginning
v.erase(v.begin());     // Erase first element
v.clear();              // Remove all elements
v.empty();              // Is empty? true/false

// Iterate
for (int i = 0; i < v.size(); i++) cout << v[i] << " ";
for (int num : v) cout << num << " ";
for (auto it = v.begin(); it != v.end(); it++) cout << *it << " ";
```

### Stack (LIFO)
```cpp
#include <stack>

stack<int> s;
s.push(1);      // Add: {1}
s.push(2);      // Add: {1, 2}
s.push(3);      // Add: {1, 2, 3}

s.top();        // Top element: 3
s.pop();        // Remove top: {1, 2}
s.size();       // Size: 2
s.empty();      // Is empty? false
```

### Queue (FIFO)
```cpp
#include <queue>

queue<int> q;
q.push(1);      // Add: {1}
q.push(2);      // Add: {1, 2}
q.push(3);      // Add: {1, 2, 3}

q.front();      // Front element: 1
q.pop();        // Remove front: {2, 3}
q.size();       // Size: 2
q.empty();      // Is empty? false
```

### Set (Sorted, Unique)
```cpp
#include <set>

set<int> s;
s.insert(3);    // {3}
s.insert(1);    // {1, 3}
s.insert(2);    // {1, 2, 3}
s.insert(1);    // {1, 2, 3} (1 already exists, not added)

s.size();       // 3
s.count(2);     // 1 (2 is in set)
s.count(5);     // 0 (5 is not in set)
s.find(2);      // Iterator to 2, or s.end() if not found
s.erase(2);     // {1, 3}

for (int num : s) cout << num << " ";  // Prints: 1 3
```

### Map (Key-Value Pairs)
```cpp
#include <map>

map<string, int> m;
m["apple"] = 5;         // {apple: 5}
m["banana"] = 3;        // {apple: 5, banana: 3}
m["cherry"] = 8;        // {apple: 5, banana: 3, cherry: 8}

m["apple"];             // 5
m.count("apple");       // 1 (key exists)
m.count("grape");       // 0 (key doesn't exist)
m.find("apple");        // Iterator to pair
m.erase("apple");       // Remove key-value pair
m.size();               // 2
m.empty();              // false

// Iterate through map
for (auto pair : m) {
    cout << pair.first << ": " << pair.second << endl;
}

// Or with iterator
for (auto it = m.begin(); it != m.end(); it++) {
    cout << it->first << ": " << it->second << endl;
}
```

---

## Common STL Functions

```cpp
#include <algorithm>

vector<int> v = {5, 2, 8, 1, 9};

// Sorting
sort(v.begin(), v.end());           // {1, 2, 5, 8, 9}
sort(v.begin(), v.end(), greater<int>());  // {9, 8, 5, 2, 1}

// Searching
find(v.begin(), v.end(), 5);        // Iterator to 5
binary_search(v.begin(), v.end(), 5);  // true/false

// Min and Max
*min_element(v.begin(), v.end());   // 1
*max_element(v.begin(), v.end());   // 9
min(a, b);                          // Smaller of a and b
max(a, b);                          // Larger of a and b

// Other
reverse(v.begin(), v.end());        // Reverse order
count(v.begin(), v.end(), 5);       // Count occurrences of 5
fill(v.begin(), v.end(), 0);        // Fill all with 0

#include <numeric>
accumulate(v.begin(), v.end(), 0);  // Sum all elements
```

---

## Input/Output

```cpp
#include <iostream>
using namespace std;

// Output
cout << "Hello";           // Print text
cout << 42;                // Print number
cout << 3.14;              // Print decimal
cout << endl;              // Print newline and flush
cout << "\n";              // Print newline

// Input
int x;
cin >> x;                  // Read single integer
cin >> x >> y;             // Read two values

string name;
cin >> name;               // Read single word
getline(cin, name);        // Read entire line

// Formatted output
cout << fixed;             // Fixed decimal notation
cout << setprecision(2);   // 2 decimal places
cout << 3.14159;           // Prints: 3.14

// Multiple outputs
cout << "Value: " << x << ", Name: " << name << endl;
```

---

## Useful Commands

### Compilation
```bash
# Basic compilation
g++ -o program filename.cpp

# With all warnings
g++ -Wall -Wextra -o program filename.cpp

# Debug mode
g++ -g -o program filename.cpp

# Run program
./program

# Input from file
./program < input.txt

# Output to file
./program > output.txt
```

---

## Quick Checklist Before Submitting Code

- [ ] All variables initialized?
- [ ] Correct data types used?
- [ ] Off-by-one errors in loops?
- [ ] Array/vector bounds correct?
- [ ] Functions returning correct values?
- [ ] Memory properly allocated/deallocated?
- [ ] Edge cases handled (empty, single element, max values)?
- [ ] Code compiles without warnings?
- [ ] Logic tested with sample inputs?

---

**Happy Coding!** 🚀

Remember: When in doubt, check cppreference.com or try it in code!
