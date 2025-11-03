# Chapter 11: Stacks & Queues

**What You'll Learn**: What stacks and queues are, WHY they exist, LIFO vs FIFO, real-world applications, and how to use them in C++.

## Table of Contents

1. [Stack: Last In, First Out (LIFO)](#1-stack-last-in-first-out-lifo)
2. [Using Stack in C++](#2-using-stack-in-c)
3. [Stack Applications](#3-stack-applications)
4. [Queue: First In, First Out (FIFO)](#4-queue-first-in-first-out-fifo)
5. [Using Queue in C++](#5-using-queue-in-c)
6. [Queue Applications](#6-queue-applications)
7. [Stack vs Queue Comparison](#7-stack-vs-queue-comparison)
8. [Implementing Stack Using Array](#8-implementing-stack-using-array)
9. [Implementing Queue Using Array](#9-implementing-queue-using-array)
10. [Deque (Double-Ended Queue)](#10-deque-double-ended-queue)
11. [Common Mistakes](#common-mistakes-)
12. [Practice Problems](#practice-problems)
13. [Key Takeaways](#key-takeaways)

---

## Introduction: The Access Pattern Problem

Sometimes you don't want random access to data. You want **specific rules** for how data goes in and comes out.

**Problem 1: Browser Back Button**
- Click links: A → B → C → D
- Press back: D → C → B → A (reverse order!)
- **Need**: Last clicked, first to go back

**Problem 2: Print Queue**
- Documents sent: Doc1, Doc2, Doc3
- Printer prints: Doc1, then Doc2, then Doc3 (same order!)
- **Need**: First sent, first printed

**Solution**: Stack (for back button) and Queue (for printer)

---

## 1. Stack: Last In, First Out (LIFO)

A **stack** is a data structure where the **last element added is the first to be removed**.

**Think of it like**:
- **Stack of plates**: Add/remove from top only
- **Browser history**: Back button goes to most recent page
- **Undo in editor**: Last action undone first

**Visual**:
```
         ← pop (remove from top)
       ┌────┐
       │ 30 │ ← Top (most recent)
       ├────┤
       │ 20 │
       ├────┤
       │ 10 │ ← Bottom (oldest)
       └────┘
         ↑
       push (add to top)
```

**Operations**:
- **Push**: Add element to top
- **Pop**: Remove element from top
- **Top/Peek**: View top element (without removing)
- **Empty**: Check if stack is empty

---

## 2. Using Stack in C++

**Include**:
```cpp
#include <stack>
using namespace std;
```

### Creating a Stack

```cpp
stack<int> s;  // Empty stack of integers
```

---

### Basic Operations

**Push** (add to top):
```cpp
stack<int> s;

s.push(10);  // Stack: [10]
s.push(20);  // Stack: [10, 20]
s.push(30);  // Stack: [10, 20, 30] (30 on top)
```

**Pop** (remove from top):
```cpp
s.pop();  // Removes 30, Stack: [10, 20]
s.pop();  // Removes 20, Stack: [10]
```

**Top** (view top element):
```cpp
cout << s.top();  // 10 (doesn't remove)
```

**Empty** (check if empty):
```cpp
if (s.empty()) {
    cout << "Stack is empty";
}
```

**Size**:
```cpp
cout << s.size();  // Number of elements
```

---

### Complete Stack Example

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    stack<int> s;

    // Push elements
    s.push(10);
    s.push(20);
    s.push(30);

    cout << "Top element: " << s.top() << endl;  // 30

    // Pop and print all
    while (!s.empty()) {
        cout << s.top() << " ";
        s.pop();
    }
    // Output: 30 20 10 (reverse order!)

    return 0;
}
```

---

## 3. Stack Applications

### Application 1: Reverse a String

```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

int main() {
    string str = "Hello";
    stack<char> s;

    // Push all characters
    for (char c : str) {
        s.push(c);
    }

    // Pop to reverse
    string reversed = "";
    while (!s.empty()) {
        reversed += s.top();
        s.pop();
    }

    cout << reversed << endl;  // olleH

    return 0;
}
```

---

### Application 2: Balanced Parentheses

**Problem**: Check if parentheses are balanced
- `"(())"` ✓ Valid
- `"(()"` ✗ Invalid
- `"())("` ✗ Invalid

**Solution** (using stack):

```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

bool isBalanced(string s) {
    stack<char> st;

    for (char c : s) {
        if (c == '(' || c == '{' || c == '[') {
            st.push(c);  // Opening bracket: push
        } else {
            if (st.empty()) return false;  // Closing without opening

            char top = st.top();
            if ((c == ')' && top == '(') ||
                (c == '}' && top == '{') ||
                (c == ']' && top == '[')) {
                st.pop();  // Matching pair
            } else {
                return false;  // Mismatch
            }
        }
    }

    return st.empty();  // Should be empty if balanced
}

int main() {
    cout << isBalanced("(())") << endl;    // 1 (true)
    cout << isBalanced("(()") << endl;     // 0 (false)
    cout << isBalanced("{[()]}") << endl;  // 1 (true)

    return 0;
}
```

---

### Application 3: Undo Mechanism

```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

int main() {
    stack<string> history;
    string current = "";

    // User types
    current = "Hello";
    history.push(current);

    current = "Hello World";
    history.push(current);

    current = "Hello World!";
    history.push(current);

    cout << "Current: " << current << endl;

    // Undo
    if (!history.empty()) {
        history.pop();
        current = history.empty() ? "" : history.top();
    }

    cout << "After undo: " << current << endl;  // Hello World

    return 0;
}
```

---

## 4. Queue: First In, First Out (FIFO)

A **queue** is a data structure where the **first element added is the first to be removed**.

**Think of it like**:
- **Line at a store**: First person in line is served first
- **Printer queue**: First document sent is first printed
- **Task queue**: First task added is first executed

**Visual**:
```
Front                             Back
  ↓                                ↓
┌────┬────┬────┬────┐
│ 10 │ 20 │ 30 │ 40 │
└────┴────┴────┴────┘
  ↑                    ↑
dequeue (remove)    enqueue (add)
```

**Operations**:
- **Enqueue**: Add element to back
- **Dequeue**: Remove element from front
- **Front**: View front element (without removing)
- **Back**: View back element
- **Empty**: Check if queue is empty

---

## 5. Using Queue in C++

**Include**:
```cpp
#include <queue>
using namespace std;
```

### Creating a Queue

```cpp
queue<int> q;  // Empty queue of integers
```

---

### Basic Operations

**Enqueue** (add to back):
```cpp
queue<int> q;

q.push(10);  // Queue: [10]
q.push(20);  // Queue: [10, 20]
q.push(30);  // Queue: [10, 20, 30]
```

**Dequeue** (remove from front):
```cpp
q.pop();  // Removes 10, Queue: [20, 30]
```

**Front** (view front):
```cpp
cout << q.front();  // 20
```

**Back** (view back):
```cpp
cout << q.back();  // 30
```

**Empty and Size**:
```cpp
if (q.empty()) {
    cout << "Queue is empty";
}

cout << q.size();  // Number of elements
```

---

### Complete Queue Example

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    queue<int> q;

    // Enqueue elements
    q.push(10);
    q.push(20);
    q.push(30);

    cout << "Front element: " << q.front() << endl;  // 10
    cout << "Back element: " << q.back() << endl;    // 30

    // Dequeue and print all
    while (!q.empty()) {
        cout << q.front() << " ";
        q.pop();
    }
    // Output: 10 20 30 (same order!)

    return 0;
}
```

---

## 6. Queue Applications

### Application 1: BFS (Breadth-First Search)

Queues are essential for level-order tree/graph traversal (Chapter 20).

---

### Application 2: Task Scheduler

```cpp
#include <iostream>
#include <queue>
#include <string>
using namespace std;

int main() {
    queue<string> tasks;

    // Add tasks
    tasks.push("Task 1: Write code");
    tasks.push("Task 2: Test code");
    tasks.push("Task 3: Deploy");

    // Process tasks (FIFO)
    cout << "Processing tasks:" << endl;
    while (!tasks.empty()) {
        cout << "Doing: " << tasks.front() << endl;
        tasks.pop();
    }

    return 0;
}
```

---

### Application 3: Ticket Counter Simulation

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    queue<int> customers;

    // Customers arrive
    for (int i = 1; i <= 5; i++) {
        customers.push(i);
        cout << "Customer " << i << " joined queue" << endl;
    }

    cout << "\nServing customers:" << endl;

    // Serve customers (FIFO)
    while (!customers.empty()) {
        cout << "Serving customer " << customers.front() << endl;
        customers.pop();
    }

    return 0;
}
```

---

## 7. Stack vs Queue Comparison

| Feature | Stack (LIFO) | Queue (FIFO) |
|---------|--------------|--------------|
| Order | Last In, First Out | First In, First Out |
| Add | `push()` to top | `push()` to back |
| Remove | `pop()` from top | `pop()` from front |
| View | `top()` | `front()`, `back()` |
| Analogy | Stack of plates | Line at store |
| Use cases | Undo, backtrack, recursion | Scheduling, BFS, printer |

---

## 8. Implementing Stack Using Array

**Understanding internals**:

```cpp
#include <iostream>
using namespace std;

class Stack {
private:
    int arr[100];
    int topIndex;

public:
    Stack() : topIndex(-1) {}

    void push(int x) {
        if (topIndex >= 99) {
            cout << "Stack overflow!" << endl;
            return;
        }
        arr[++topIndex] = x;
    }

    void pop() {
        if (topIndex < 0) {
            cout << "Stack underflow!" << endl;
            return;
        }
        topIndex--;
    }

    int top() {
        if (topIndex < 0) {
            cout << "Stack is empty!" << endl;
            return -1;
        }
        return arr[topIndex];
    }

    bool empty() {
        return topIndex < 0;
    }
};

int main() {
    Stack s;
    s.push(10);
    s.push(20);
    cout << s.top() << endl;  // 20
    s.pop();
    cout << s.top() << endl;  // 10

    return 0;
}
```

---

## 9. Implementing Queue Using Array

```cpp
#include <iostream>
using namespace std;

class Queue {
private:
    int arr[100];
    int frontIndex, backIndex;

public:
    Queue() : frontIndex(0), backIndex(-1) {}

    void push(int x) {
        if (backIndex >= 99) {
            cout << "Queue overflow!" << endl;
            return;
        }
        arr[++backIndex] = x;
    }

    void pop() {
        if (frontIndex > backIndex) {
            cout << "Queue underflow!" << endl;
            return;
        }
        frontIndex++;
    }

    int front() {
        if (frontIndex > backIndex) {
            cout << "Queue is empty!" << endl;
            return -1;
        }
        return arr[frontIndex];
    }

    int back() {
        if (frontIndex > backIndex) {
            cout << "Queue is empty!" << endl;
            return -1;
        }
        return arr[backIndex];
    }

    bool empty() {
        return frontIndex > backIndex;
    }
};

int main() {
    Queue q;
    q.push(10);
    q.push(20);
    cout << q.front() << endl;  // 10
    q.pop();
    cout << q.front() << endl;  // 20

    return 0;
}
```

---

## 10. Deque (Double-Ended Queue)

**Deque** allows insertion/deletion from **both ends**!

**Include**:
```cpp
#include <deque>
```

**Operations**:
```cpp
deque<int> dq;

dq.push_back(10);   // Add to back
dq.push_front(5);   // Add to front
dq.pop_back();      // Remove from back
dq.pop_front();     // Remove from front

cout << dq.front();
cout << dq.back();
```

**Use case**: When you need flexibility of both stack and queue!

---

## Complete Example Programs

### Example 1: Expression Evaluation

```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    return 0;
}

string infixToPostfix(string infix) {
    stack<char> s;
    string postfix = "";

    for (char c : infix) {
        if (isalnum(c)) {
            postfix += c;
        } else if (c == '(') {
            s.push(c);
        } else if (c == ')') {
            while (!s.empty() && s.top() != '(') {
                postfix += s.top();
                s.pop();
            }
            s.pop();  // Remove '('
        } else {
            while (!s.empty() && precedence(s.top()) >= precedence(c)) {
                postfix += s.top();
                s.pop();
            }
            s.push(c);
        }
    }

    while (!s.empty()) {
        postfix += s.top();
        s.pop();
    }

    return postfix;
}

int main() {
    string infix = "a+b*c";
    cout << "Infix: " << infix << endl;
    cout << "Postfix: " << infixToPostfix(infix) << endl;
    // Output: abc*+

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Accessing Empty Stack/Queue

```cpp
stack<int> s;
cout << s.top();  // CRASH! Stack is empty

// FIX:
if (!s.empty()) {
    cout << s.top();
}
```

---

### Mistake 2: Forgetting to Pop

```cpp
stack<int> s;
s.push(10);

while (!s.empty()) {
    cout << s.top();
    // FORGOT s.pop()! Infinite loop!
}
```

---

## Practice Problems

### Problem 1: Next Greater Element
Use stack to find next greater element for each element in array.

### Problem 2: Stock Span
Calculate span of stock prices (days since price was ≤ current).

### Problem 3: Implement Queue Using Two Stacks
Build a queue using only stack operations.

### Problem 4: First Non-Repeating Character
Use queue to find first non-repeating character in a stream.

### Problem 5: Sliding Window Maximum
Find maximum in each sliding window of size k using deque.

---

## Key Takeaways

✅ **Stack = LIFO** (Last In, First Out)
✅ **Queue = FIFO** (First In, First Out)
✅ **Stack operations**: push(), pop(), top()
✅ **Queue operations**: push(), pop(), front(), back()
✅ **Always check empty()** before accessing
✅ **Stack uses**: Undo, backtrack, recursion, expression evaluation
✅ **Queue uses**: Scheduling, BFS, task management
✅ **Deque**: Both stack and queue operations

---

## What's Next?

In **[Chapter 12](chapter-12-sets-and-maps.md)**, you'll learn about **Sets and Maps** - powerful containers for fast lookups and key-value storage!

---

**Chapter Progress**: ✅ Chapters 1-11 Complete | 📖 Next: Chapter 12
