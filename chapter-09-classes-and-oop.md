# Chapter 9: Classes & Object-Oriented Programming (OOP)

**What You'll Learn**: What objects and classes are, WHY OOP exists, encapsulation, constructors, destructors, and the basics of organizing complex programs.

## Table of Contents

1. [What is a Class?](#1-what-is-a-class)
2. [Defining a Class](#2-defining-a-class)
3. [Creating Objects](#3-creating-objects)
4. [Member Functions (Methods)](#4-member-functions-methods)
5. [Encapsulation: Public vs Private](#5-encapsulation-public-vs-private)
6. [Constructors: Object Initialization](#6-constructors-object-initialization)
7. [Destructors: Cleanup](#7-destructors-cleanup)
8. [The `this` Pointer](#8-the-this-pointer)
9. [Complete Examples](#9-complete-examples)
10. [Static Members](#10-static-members)
11. [Introduction to Templates (Generic Programming)](#11-introduction-to-templates-generic-programming)
12. [Advanced OOP Topics (Not Covered)](#12-advanced-oop-topics-not-covered)
13. [Common Mistakes](#13-common-mistakes-)
14. [Practice Problems](#practice-problems)
15. [Key Takeaways](#key-takeaways)

---

## Introduction: The Organization Problem

Imagine you're building a program to manage a library with 1000 books. For each book you need:
- Title
- Author
- ISBN
- Price
- Year
- Available status

**Without classes (nightmare!)**:
```cpp
// For book 1:
string title1 = "Harry Potter";
string author1 = "J.K. Rowling";
string isbn1 = "123456";
double price1 = 29.99;
int year1 = 1997;
bool available1 = true;

// For book 2:
string title2 = "The Hobbit";
string author2 = "J.R.R. Tolkien";
// ... and so on for 1000 books! 😱
```

**Problems**:
- ❌ 6000 separate variables! (6 × 1000)
- ❌ No way to group related data
- ❌ Can't pass "a book" to a function easily
- ❌ Hard to maintain and error-prone

**With a class**:
```cpp
class Book {
public:
    string title;
    string author;
    string isbn;
    double price;
    int year;
    bool available;
};

Book book1;  // One variable, all data grouped!
book1.title = "Harry Potter";
book1.author = "J.K. Rowling";
// ...
```

**Classes are like blueprints** - define once, create many instances!

---

## 1. What is a Class?

A **class** is a **blueprint** (template) for creating objects.

An **object** is an **instance** (actual thing) created from that blueprint.

**Real-world analogy**:
```
Class     = Car blueprint (design on paper)
Object    = Actual car (Toyota, Honda, etc.)

One blueprint → Many cars
One class    → Many objects
```

---

## 2. Defining a Class

**Syntax**:
```cpp
class ClassName {
public:
    // Data members (variables)
    int data;

    // Member functions (methods)
    void doSomething() {
        // ...
    }
};  // Don't forget the semicolon!
```

**Example: Student Class**:
```cpp
class Student {
public:
    // Data members
    string name;
    int age;
    double gpa;

    // Member function
    void display() {
        cout << "Name: " << name << endl;
        cout << "Age: " << age << endl;
        cout << "GPA: " << gpa << endl;
    }
};
```

---

## 3. Creating Objects

**Syntax**:
```cpp
ClassName objectName;
```

**Example**:
```cpp
class Student {
public:
    string name;
    int age;
    double gpa;
};

int main() {
    // Create objects (instances)
    Student student1;
    Student student2;
    Student student3;

    // Set data
    student1.name = "Alice";
    student1.age = 20;
    student1.gpa = 3.8;

    // Access data
    cout << student1.name << endl;  // Alice

    return 0;
}
```

**Memory visualization**:
```
Stack:
┌─────────────────────┐
│ student1            │
│  - name: "Alice"    │
│  - age: 20          │
│  - gpa: 3.8         │
├─────────────────────┤
│ student2            │
│  - name: ""         │
│  - age: 0           │
│  - gpa: 0.0         │
├─────────────────────┤
│ student3            │
│  - name: ""         │
│  - age: 0           │
│  - gpa: 0.0         │
└─────────────────────┘

Each object is SEPARATE and INDEPENDENT!
```

---

## 4. Member Functions (Methods)

Functions inside a class that operate on the object's data.

**Definition inside class**:
```cpp
class Student {
public:
    string name;
    int age;

    // Method defined inside class
    void greet() {
        cout << "Hi, I'm " << name << endl;
    }
};
```

**Definition outside class**:
```cpp
class Student {
public:
    string name;
    int age;

    void greet();  // Declaration only
};

// Definition outside (use ::)
void Student::greet() {
    cout << "Hi, I'm " << name << endl;
}
```

**Using member functions**:
```cpp
Student s;
s.name = "Bob";
s.greet();  // Hi, I'm Bob
```

---

## 5. Encapsulation: Public vs Private

**Encapsulation** = hiding internal details, showing only what's necessary.

**Access specifiers**:
- `public`: Accessible from anywhere
- `private`: Accessible only inside the class
- `protected`: Accessible in class and derived classes (Chapter 10+)

**Example**:
```cpp
class BankAccount {
private:
    double balance;  // Hidden! Can't access directly

public:
    // Public interface
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }

    double getBalance() {
        return balance;
    }
};
```

**Usage**:
```cpp
BankAccount account;

account.balance = 1000000;  // ERROR! balance is private

account.deposit(100);       // OK! Public method
cout << account.getBalance();  // OK! Public method
```

**Why hide data?**
- ✅ Prevent invalid states (negative balance)
- ✅ Control how data is modified
- ✅ Can change implementation without breaking code

**Real-world analogy**: A vending machine
- Private: Internal mechanism (you can't touch)
- Public: Buttons and coin slot (interface you use)

---

## 6. Constructors: Object Initialization

A **constructor** is a special function that runs **automatically** when an object is created.

**Purpose**: Initialize object data

**Syntax**:
```cpp
class Student {
public:
    string name;
    int age;

    // Constructor (same name as class, no return type)
    Student() {
        name = "Unknown";
        age = 0;
        cout << "Student created!" << endl;
    }
};
```

**Usage**:
```cpp
Student s;  // Constructor runs automatically!
// Output: Student created!
```

---

### Parameterized Constructor

Accept arguments to initialize with specific values:

```cpp
class Student {
public:
    string name;
    int age;

    // Constructor with parameters
    Student(string n, int a) {
        name = n;
        age = a;
    }
};
```

**Usage**:
```cpp
Student s1("Alice", 20);  // name="Alice", age=20
Student s2("Bob", 22);    // name="Bob", age=22
```

---

### Constructor Overloading

Multiple constructors with different parameters:

```cpp
class Student {
public:
    string name;
    int age;

    // Default constructor
    Student() {
        name = "Unknown";
        age = 0;
    }

    // Constructor with name only
    Student(string n) {
        name = n;
        age = 0;
    }

    // Constructor with both
    Student(string n, int a) {
        name = n;
        age = a;
    }
};
```

**Usage**:
```cpp
Student s1;                  // Calls default
Student s2("Alice");         // Calls second
Student s3("Bob", 22);       // Calls third
```

---

### Member Initializer List (Efficient!)

**Better way** to initialize:

```cpp
class Student {
public:
    string name;
    int age;

    // Using initializer list
    Student(string n, int a) : name(n), age(a) {
        // Body can be empty or have additional logic
    }
};
```

**Why better?**
- More efficient (direct initialization, not assignment)
- Required for const members and references

---

## 7. Destructors: Cleanup

A **destructor** runs automatically when an object is **destroyed**.

**Purpose**: Clean up resources (free memory, close files, etc.)

**Syntax**:
```cpp
class Student {
public:
    string name;

    // Constructor
    Student() {
        cout << "Student created" << endl;
    }

    // Destructor (~ + class name, no parameters, no return)
    ~Student() {
        cout << "Student destroyed" << endl;
    }
};
```

**When destructors run**:
```cpp
int main() {
    Student s1;
    {
        Student s2;
    }  // s2 destroyed here (out of scope)

    return 0;
}  // s1 destroyed here
```

**Output**:
```
Student created
Student created
Student destroyed
Student destroyed
```

---

### Example: Dynamic Memory Cleanup

```cpp
class Array {
private:
    int* data;
    int size;

public:
    // Constructor: allocate memory
    Array(int s) : size(s) {
        data = new int[size];
        cout << "Array of size " << size << " created" << endl;
    }

    // Destructor: free memory
    ~Array() {
        delete[] data;
        cout << "Array destroyed" << endl;
    }
};
```

**Without destructor** → memory leak!
**With destructor** → automatic cleanup!

---

## 8. The `this` Pointer

`this` is a pointer to the **current object**.

**Use case 1: Resolve naming conflicts**:
```cpp
class Student {
private:
    string name;
    int age;

public:
    Student(string name, int age) {
        this->name = name;  // this->name is member, name is parameter
        this->age = age;
    }
};
```

**Use case 2: Return current object**:
```cpp
class Counter {
private:
    int count;

public:
    Counter() : count(0) {}

    Counter& increment() {
        count++;
        return *this;  // Return current object
    }

    void display() {
        cout << "Count: " << count << endl;
    }
};

int main() {
    Counter c;
    c.increment().increment().increment();  // Chaining!
    c.display();  // Count: 3
}
```

---

## 9. Complete Examples

### Example 1: Rectangle Class

```cpp
#include <iostream>
using namespace std;

class Rectangle {
private:
    double width;
    double height;

public:
    // Constructor
    Rectangle(double w, double h) : width(w), height(h) {}

    // Calculate area
    double area() {
        return width * height;
    }

    // Calculate perimeter
    double perimeter() {
        return 2 * (width + height);
    }

    // Display
    void display() {
        cout << "Rectangle: " << width << " x " << height << endl;
        cout << "Area: " << area() << endl;
        cout << "Perimeter: " << perimeter() << endl;
    }
};

int main() {
    Rectangle r1(5.0, 3.0);
    r1.display();

    Rectangle r2(10.0, 7.5);
    r2.display();

    return 0;
}
```

---

### Example 2: Bank Account

```cpp
#include <iostream>
#include <string>
using namespace std;

class BankAccount {
private:
    string owner;
    int accountNumber;
    double balance;

public:
    // Constructor
    BankAccount(string name, int accNum)
        : owner(name), accountNumber(accNum), balance(0.0) {
        cout << "Account created for " << owner << endl;
    }

    // Deposit money
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            cout << "Deposited: $" << amount << endl;
        } else {
            cout << "Invalid amount!" << endl;
        }
    }

    // Withdraw money
    void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            cout << "Withdrawn: $" << amount << endl;
        } else {
            cout << "Insufficient funds or invalid amount!" << endl;
        }
    }

    // Get balance
    double getBalance() {
        return balance;
    }

    // Display account info
    void displayInfo() {
        cout << "\n=== Account Info ===" << endl;
        cout << "Owner: " << owner << endl;
        cout << "Account #: " << accountNumber << endl;
        cout << "Balance: $" << balance << endl;
    }
};

int main() {
    BankAccount account("Alice", 12345);

    account.deposit(1000);
    account.withdraw(250);
    account.withdraw(2000);  // Insufficient funds

    account.displayInfo();

    return 0;
}
```

---

### Example 3: Student Grade Manager

```cpp
#include <iostream>
#include <string>
using namespace std;

class Student {
private:
    string name;
    int rollNumber;
    double marks[5];

public:
    // Constructor
    Student(string n, int roll) : name(n), rollNumber(roll) {
        for (int i = 0; i < 5; i++) {
            marks[i] = 0.0;
        }
    }

    // Input marks
    void inputMarks() {
        cout << "Enter marks for 5 subjects:" << endl;
        for (int i = 0; i < 5; i++) {
            cout << "Subject " << (i + 1) << ": ";
            cin >> marks[i];
        }
    }

    // Calculate total
    double getTotal() {
        double total = 0;
        for (int i = 0; i < 5; i++) {
            total += marks[i];
        }
        return total;
    }

    // Calculate average
    double getAverage() {
        return getTotal() / 5.0;
    }

    // Get grade
    char getGrade() {
        double avg = getAverage();
        if (avg >= 90) return 'A';
        if (avg >= 80) return 'B';
        if (avg >= 70) return 'C';
        if (avg >= 60) return 'D';
        return 'F';
    }

    // Display report
    void displayReport() {
        cout << "\n=== Student Report ===" << endl;
        cout << "Name: " << name << endl;
        cout << "Roll Number: " << rollNumber << endl;
        cout << "Total Marks: " << getTotal() << " / 500" << endl;
        cout << "Average: " << getAverage() << "%" << endl;
        cout << "Grade: " << getGrade() << endl;
    }
};

int main() {
    Student student("Bob", 101);
    student.inputMarks();
    student.displayReport();

    return 0;
}
```

---

## 10. Static Members

**Static members** belong to the **class**, not individual objects.

**Static data member**:
```cpp
class Counter {
public:
    static int count;  // Shared by ALL objects

    Counter() {
        count++;
    }
};

// Must define outside class
int Counter::count = 0;

int main() {
    Counter c1;
    Counter c2;
    Counter c3;

    cout << Counter::count;  // 3 (shared counter)
}
```

**Static member function**:
```cpp
class Math {
public:
    static int square(int x) {
        return x * x;
    }
};

int main() {
    cout << Math::square(5);  // 25 (no object needed!)
}
```

---

## 11. Introduction to Templates (Generic Programming)

**Templates** allow you to write code that works with **any data type**.

**Why templates matter**: This is HOW the STL works! Understanding templates helps you understand why `vector<int>`, `vector<string>`, `vector<double>` all use the same code.

---

### The Problem Templates Solve

Imagine you want a function to find the maximum of two numbers:

```cpp
int max(int a, int b) {
    return (a > b) ? a : b;
}
```

But what if you need max for doubles? floats? strings?

**Without templates** (bad):
```cpp
int maxInt(int a, int b) { return (a > b) ? a : b; }
double maxDouble(double a, double b) { return (a > b) ? a : b; }
float maxFloat(float a, float b) { return (a > b) ? a : b; }
// Repeating same logic! 😱
```

**With templates** (good):
```cpp
template<typename T>
T max(T a, T b) {
    return (a > b) ? a : b;
}

// Works for ANY type!
int x = max(5, 10);           // T = int
double y = max(3.14, 2.71);   // T = double
string s = max("apple", "banana"); // T = string
```

**Think of templates as**: A blueprint that compiler fills in with actual types.

---

### Function Templates

**Syntax**:
```cpp
template<typename T>
T functionName(T param) {
    // Use T like a normal type
}
```

**Example: Swap Function**:
```cpp
template<typename T>
void swap(T& a, T& b) {
    T temp = a;
    a = b;
    b = temp;
}

int main() {
    int x = 5, y = 10;
    swap(x, y);  // Works with int

    string s1 = "hello", s2 = "world";
    swap(s1, s2);  // Works with string

    double d1 = 3.14, d2 = 2.71;
    swap(d1, d2);  // Works with double
}
```

---

### Class Templates

**Syntax**:
```cpp
template<typename T>
class ClassName {
    T data;
public:
    void set(T value) { data = value; }
    T get() { return data; }
};
```

**Example: Box That Holds Any Type**:
```cpp
template<typename T>
class Box {
private:
    T value;

public:
    Box(T v) : value(v) {}

    T getValue() {
        return value;
    }

    void setValue(T v) {
        value = v;
    }
};

int main() {
    Box<int> intBox(42);
    cout << intBox.getValue() << endl;  // 42

    Box<string> stringBox("Hello");
    cout << stringBox.getValue() << endl;  // Hello

    Box<double> doubleBox(3.14);
    cout << doubleBox.getValue() << endl;  // 3.14
}
```

**This is exactly how `vector<T>` works!**

---

### How STL Uses Templates

Now you understand why we write:

```cpp
vector<int> numbers;      // vector template with T = int
vector<string> words;     // vector template with T = string
map<string, int> ages;    // map template with Key = string, Value = int
```

The STL containers are **class templates** that work with any type!

---

### Template Terminology

- `template<typename T>`: Declare a template with type parameter T
- `typename`: Keyword meaning "this is a type" (can also use `class` instead)
- `T`: Type parameter (common convention; can be any name)
- **Instantiation**: When compiler generates actual code for a specific type

---

### Multiple Template Parameters

You can have multiple type parameters:

```cpp
template<typename T, typename U>
class Pair {
public:
    T first;
    U second;

    Pair(T f, U s) : first(f), second(s) {}
};

int main() {
    Pair<int, string> p1(1, "one");
    Pair<string, double> p2("pi", 3.14);
}
```

**This is how `map<string, int>` works!** (Key type, Value type)

---

### When You'll Use Templates

**As a beginner**:
- **Using templates**: You'll use STL containers (vector, map, set) which are templates
- **Writing templates**: Rare for beginners, common in advanced C++

**Where templates appear**:
- All STL containers: `vector<T>`, `list<T>`, `deque<T>`, `stack<T>`, `queue<T>`
- All STL maps/sets: `map<K,V>`, `set<T>`, `unordered_map<K,V>`
- STL algorithms: `sort`, `find`, `binary_search` work with any type

---

### Key Takeaways

✅ **Templates = code that works with any type**
✅ **Function templates**: `template<typename T>` before function
✅ **Class templates**: `template<typename T>` before class
✅ **STL uses templates extensively** (vector, map, set, etc.)
✅ **Compiler generates code** for each type you use
✅ **`<int>`, `<string>` in angle brackets** = specifying the type

**Don't worry if templates seem complex** - you'll mostly be **using** them (STL containers) rather than **writing** them. The important thing is understanding why `vector<int>` and `vector<string>` both exist!

---

## 12. Advanced OOP Topics (Not Covered)

This course focuses on fundamentals needed for data structures. The following advanced OOP topics are important but beyond our current scope:

### **Inheritance**
- Creating classes based on other classes
- Code reuse through parent-child relationships
- Example: `class Student : public Person`

### **Polymorphism**
- Objects behaving differently based on their type
- Virtual functions
- Runtime type decisions

### **Why not covered here?**
- **Focus**: This course prioritizes data structures & algorithms
- **Complexity**: These topics deserve dedicated chapters
- **Prerequisites**: You now have the foundation to learn them

**Where to learn**: After completing this course, study inheritance and polymorphism from C++ OOP resources or advanced C++ courses.

---

## 13. Common Mistakes ⚠️

### Mistake 1: Forgetting Semicolon After Class

```cpp
class Student {
    // ...
}  // ERROR! Missing semicolon

// FIX:
class Student {
    // ...
};  // Semicolon required!
```

---

### Mistake 2: Accessing Private Members

```cpp
class Student {
private:
    string name;
};

int main() {
    Student s;
    s.name = "Alice";  // ERROR! name is private
}
```

---

### Mistake 3: Not Initializing Members

```cpp
class Student {
public:
    string name;
    int age;

    Student() {
        // FORGOT to initialize!
    }
};

int main() {
    Student s;
    cout << s.age;  // Garbage value!
}
```

---

### Mistake 4: Memory Leak (No Destructor)

```cpp
class Array {
    int* data;
public:
    Array(int size) {
        data = new int[size];
    }

    // FORGOT destructor!
    // Memory leaks when object destroyed!
};

// FIX: Add destructor
~Array() {
    delete[] data;
}
```

---

## Practice Problems

### Problem 1: Circle Class
Create a class with radius, area(), and circumference() methods.

### Problem 2: Time Class
Create a class to represent time (hours, minutes, seconds) with add() method.

### Problem 3: Book Class
Create a Book class with title, author, price, and discount() method.

### Problem 4: Complex Number Class
Create a class for complex numbers with add() and multiply() methods.

### Problem 5: Array Class
Create a custom array class with dynamic memory, copy constructor, and destructor.

---

## Key Takeaways

✅ **Class = blueprint**, **Object = instance**
✅ **Encapsulation** = hide data, expose interface
✅ **public** = accessible everywhere, **private** = accessible only inside class
✅ **Constructor** = runs automatically when object created
✅ **Destructor** = runs automatically when object destroyed
✅ **`this` pointer** = pointer to current object
✅ **Member functions** = functions that operate on object data
✅ **Static members** = shared by all objects of the class

---

## OOP Benefits

| Benefit | Description |
|---------|-------------|
| **Organization** | Group related data and functions |
| **Encapsulation** | Hide complexity, show only interface |
| **Reusability** | Create many objects from one class |
| **Maintainability** | Change implementation without breaking code |
| **Modularity** | Each class is a self-contained unit |

---

## What's Next?

In **[Chapter 10](chapter-10-vectors-dynamic-arrays.md)**, you'll learn about **Vectors** - dynamic arrays that grow automatically, and the gateway to the Standard Template Library (STL)!

---

**Chapter Progress**: ✅ Chapters 1-9 Complete (Part 1 DONE!) | 📖 Next: Chapter 10 (Part 2: STL)
