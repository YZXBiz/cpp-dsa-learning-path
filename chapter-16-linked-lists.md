# Chapter 16: Linked Lists

**What You'll Learn**: What linked lists are, WHY use them vs arrays, node-based memory layout, singly/doubly/circular linked lists, and when to choose them.

## Table of Contents

1. [Introduction: The Fixed Memory Problem](#introduction-the-fixed-memory-problem)
2. [1. What is a Linked List?](#1-what-is-a-linked-list)
3. [2. Node Structure](#2-node-structure)
4. [3. Singly Linked List: Complete Implementation](#3-singly-linked-list-complete-implementation)
5. [4. Insert Operations](#4-insert-operations)
6. [5. Delete Operations](#5-delete-operations)
7. [6. Traversal and Search](#6-traversal-and-search)
8. [7. Doubly Linked List](#7-doubly-linked-list)
9. [8. Circular Linked List](#8-circular-linked-list)
10. [9. Linked List vs Array](#9-linked-list-vs-array)
11. [10. Common Linked List Problems](#10-common-linked-list-problems)
12. [Complete Example Programs](#complete-example-programs)
13. [Common Mistakes](#common-mistakes)
14. [Practice Problems](#practice-problems)
15. [Key Takeaways](#key-takeaways)
16. [What's Next?](#whats-next)

---

## Introduction: The Fixed Memory Problem

Imagine you're managing a **playlist** of songs on your phone.

**Array approach**:
```
[Song1][Song2][Song3][Song4][Song5]
```
- ❌ Adding a song in middle? Must shift EVERYTHING right
- ❌ Removing a song? Must shift EVERYTHING left
- ❌ Need contiguous (connected) memory
- ❌ Fixed size (or expensive to resize)

**What if songs could "point" to the next song, anywhere in memory?**

```
Song1 → Song3 → Song2 → Song5 → Song4
  (in scattered memory locations)
```

- ✅ Add song anywhere? Just change one pointer!
- ✅ Remove song? Just skip it!
- ✅ No shifting needed
- ✅ Memory can be scattered

**This is a Linked List!**

---

## 1. What is a Linked List?

A **linked list** is a data structure where elements (called **nodes**) are **scattered in memory**, and each node **points** to the next node.

**Array vs Linked List**:

**Array** (contiguous memory):
```
Memory:
┌────┬────┬────┬────┬────┐
│ 10 │ 20 │ 30 │ 40 │ 50 │
└────┴────┴────┴────┴────┘
  ↑                      ↑
  Address 0x1000         Address 0x1010
  (all elements together)
```

**Linked List** (scattered memory):
```
Memory (nodes can be anywhere):

Node 1 (at 0x1000)          Node 2 (at 0x2000)
┌──────┬────────┐          ┌──────┬────────┐
│ 10   │ 0x2000 │────────→│ 20   │ 0x3000 │──────┐
└──────┴────────┘          └──────┴────────┘      │
 data    next               data    next          │
                                                   ↓
Node 3 (at 0x3000)          Node 4 (at 0x4000)
┌──────┬────────┐          ┌──────┬────────┐
│ 30   │ 0x4000 │←─────────│ 40   │ nullptr│
└──────┴────────┘          └──────┴────────┘
 data    next               data    next (end)
```

Each **node** contains:
1. **Data** (the actual value)
2. **Pointer** to next node (address)

---

## 2. Node Structure

A node is typically a **struct** or **class** with data and pointer.

```cpp
struct Node {
    int data;        // The value
    Node* next;      // Pointer to next node
};
```

**Creating nodes**:

```cpp
// Create first node
Node* head = new Node();
head->data = 10;
head->next = nullptr;  // No next node yet

// Create second node
Node* second = new Node();
second->data = 20;
second->next = nullptr;

// Link them
head->next = second;

// Visual:
// head → [10|●]→[20|nullptr]
```

---

## 3. Singly Linked List: Complete Implementation

A **singly linked list** where each node points to the **next** node only.

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;

    Node(int val) : data(val), next(nullptr) {}
};

class LinkedList {
private:
    Node* head;

public:
    LinkedList() : head(nullptr) {}

    // Insert at beginning (O(1))
    void insertAtHead(int val) {
        Node* newNode = new Node(val);
        newNode->next = head;
        head = newNode;
    }

    // Insert at end (O(n))
    void insertAtTail(int val) {
        Node* newNode = new Node(val);

        if (head == nullptr) {
            head = newNode;
            return;
        }

        Node* temp = head;
        while (temp->next != nullptr) {
            temp = temp->next;
        }
        temp->next = newNode;
    }

    // Display list
    void display() {
        Node* temp = head;
        while (temp != nullptr) {
            cout << temp->data << " → ";
            temp = temp->next;
        }
        cout << "nullptr" << endl;
    }

    // Delete node by value
    void deleteNode(int val) {
        if (head == nullptr) return;

        // If head needs to be deleted
        if (head->data == val) {
            Node* temp = head;
            head = head->next;
            delete temp;
            return;
        }

        // Find node before the one to delete
        Node* temp = head;
        while (temp->next != nullptr && temp->next->data != val) {
            temp = temp->next;
        }

        if (temp->next == nullptr) return;  // Not found

        Node* toDelete = temp->next;
        temp->next = temp->next->next;
        delete toDelete;
    }

    // Search for value
    bool search(int val) {
        Node* temp = head;
        while (temp != nullptr) {
            if (temp->data == val) return true;
            temp = temp->next;
        }
        return false;
    }

    // Destructor to free memory
    ~LinkedList() {
        Node* temp = head;
        while (temp != nullptr) {
            Node* next = temp->next;
            delete temp;
            temp = next;
        }
    }
};

int main() {
    LinkedList list;

    list.insertAtTail(10);
    list.insertAtTail(20);
    list.insertAtTail(30);
    list.insertAtHead(5);

    list.display();  // 5 → 10 → 20 → 30 → nullptr

    list.deleteNode(20);
    list.display();  // 5 → 10 → 30 → nullptr

    cout << "Search 10: " << (list.search(10) ? "Found" : "Not found") << endl;

    return 0;
}
```

---

## 4. Insert Operations

### Insert at Beginning (O(1))

**Fast!** Just change head pointer.

```cpp
void insertAtHead(int val) {
    Node* newNode = new Node(val);
    newNode->next = head;
    head = newNode;
}
```

**Visual**:
```
Before:  head → [10]→[20]→nullptr

After:   head → [5]→[10]→[20]→nullptr
         (new node)
```

---

### Insert at End (O(n))

**Slow!** Must traverse entire list.

```cpp
void insertAtTail(int val) {
    Node* newNode = new Node(val);

    if (head == nullptr) {
        head = newNode;
        return;
    }

    Node* temp = head;
    while (temp->next != nullptr) {  // Find last node
        temp = temp->next;
    }
    temp->next = newNode;
}
```

---

### Insert at Position (O(n))

```cpp
void insertAtPosition(int val, int pos) {
    if (pos == 0) {
        insertAtHead(val);
        return;
    }

    Node* newNode = new Node(val);
    Node* temp = head;

    for (int i = 0; i < pos - 1 && temp != nullptr; i++) {
        temp = temp->next;
    }

    if (temp == nullptr) return;  // Position out of bounds

    newNode->next = temp->next;
    temp->next = newNode;
}
```

**Visual** (insert 99 at position 2):
```
Before:  [10]→[20]→[30]→nullptr
          0    1    2

After:   [10]→[20]→[99]→[30]→nullptr
          0    1    2    3
```

---

## 5. Delete Operations

### Delete by Value

```cpp
void deleteNode(int val) {
    if (head == nullptr) return;

    // Delete head?
    if (head->data == val) {
        Node* temp = head;
        head = head->next;
        delete temp;
        return;
    }

    // Find node before target
    Node* temp = head;
    while (temp->next != nullptr && temp->next->data != val) {
        temp = temp->next;
    }

    if (temp->next == nullptr) return;  // Not found

    Node* toDelete = temp->next;
    temp->next = temp->next->next;
    delete toDelete;
}
```

**Visual** (delete 20):
```
Before:  [10]→[20]→[30]→nullptr
               ↑
           (to delete)

After:   [10]────→[30]→nullptr
         (skip over deleted node)
```

---

### Delete at Position

```cpp
void deleteAtPosition(int pos) {
    if (head == nullptr) return;

    if (pos == 0) {
        Node* temp = head;
        head = head->next;
        delete temp;
        return;
    }

    Node* temp = head;
    for (int i = 0; i < pos - 1 && temp != nullptr; i++) {
        temp = temp->next;
    }

    if (temp == nullptr || temp->next == nullptr) return;

    Node* toDelete = temp->next;
    temp->next = temp->next->next;
    delete toDelete;
}
```

---

## 6. Traversal and Search

### Display All Nodes

```cpp
void display() {
    Node* temp = head;
    while (temp != nullptr) {
        cout << temp->data << " → ";
        temp = temp->next;
    }
    cout << "nullptr" << endl;
}
```

---

### Search for Value

```cpp
bool search(int val) {
    Node* temp = head;
    while (temp != nullptr) {
        if (temp->data == val) return true;
        temp = temp->next;
    }
    return false;
}
```

---

### Count Nodes

```cpp
int count() {
    int cnt = 0;
    Node* temp = head;
    while (temp != nullptr) {
        cnt++;
        temp = temp->next;
    }
    return cnt;
}
```

---

## 7. Doubly Linked List

Each node has **two pointers**: one to next, one to previous.

```cpp
struct Node {
    int data;
    Node* next;
    Node* prev;  // NEW!

    Node(int val) : data(val), next(nullptr), prev(nullptr) {}
};
```

**Visual**:
```
nullptr←[10]⇄[20]⇄[30]→nullptr
        ↑
       head
```

### Doubly Linked List Implementation

```cpp
class DoublyLinkedList {
private:
    Node* head;

public:
    DoublyLinkedList() : head(nullptr) {}

    void insertAtHead(int val) {
        Node* newNode = new Node(val);

        if (head != nullptr) {
            head->prev = newNode;
        }
        newNode->next = head;
        head = newNode;
    }

    void insertAtTail(int val) {
        Node* newNode = new Node(val);

        if (head == nullptr) {
            head = newNode;
            return;
        }

        Node* temp = head;
        while (temp->next != nullptr) {
            temp = temp->next;
        }

        temp->next = newNode;
        newNode->prev = temp;
    }

    void displayForward() {
        Node* temp = head;
        while (temp != nullptr) {
            cout << temp->data << " ⇄ ";
            temp = temp->next;
        }
        cout << "nullptr" << endl;
    }

    void displayBackward() {
        if (head == nullptr) return;

        // Find last node
        Node* temp = head;
        while (temp->next != nullptr) {
            temp = temp->next;
        }

        // Traverse backward
        while (temp != nullptr) {
            cout << temp->data << " ⇄ ";
            temp = temp->prev;
        }
        cout << "nullptr" << endl;
    }
};
```

**Advantages of Doubly Linked Lists**:
- ✅ Can traverse **backward**
- ✅ Delete node easily (have prev pointer)
- ✅ Insert before a node easily

**Disadvantages**:
- ❌ More memory (extra prev pointer)
- ❌ More complex code

---

## 8. Circular Linked List

Last node points back to **head** (forms a circle).

**Visual**:
```
    ┌──────────────────┐
    ↓                  │
   [10]→[20]→[30]→[40]─┘
    ↑
   head
```

```cpp
struct Node {
    int data;
    Node* next;
    Node(int val) : data(val), next(nullptr) {}
};

class CircularLinkedList {
private:
    Node* head;

public:
    CircularLinkedList() : head(nullptr) {}

    void insert(int val) {
        Node* newNode = new Node(val);

        if (head == nullptr) {
            head = newNode;
            newNode->next = head;  // Points to itself
            return;
        }

        Node* temp = head;
        while (temp->next != head) {
            temp = temp->next;
        }

        temp->next = newNode;
        newNode->next = head;
    }

    void display() {
        if (head == nullptr) return;

        Node* temp = head;
        do {
            cout << temp->data << " → ";
            temp = temp->next;
        } while (temp != head);
        cout << "(back to head)" << endl;
    }
};
```

**Use cases**:
- Round-robin scheduling
- Music playlist (loop forever)
- Board game (players take turns)

---

## 9. Linked List vs Array

| Feature | Array | Linked List |
|---------|-------|-------------|
| **Memory** | Contiguous | Scattered |
| **Size** | Fixed (or expensive resize) | Dynamic (easy to grow) |
| **Access** | O(1) random access | O(n) (must traverse) |
| **Insert at head** | O(n) (shift all) | O(1) |
| **Insert at tail** | O(1) | O(n) without tail pointer |
| **Insert at middle** | O(n) (shift) | O(n) (traverse) |
| **Delete** | O(n) (shift) | O(1) if have pointer |
| **Memory overhead** | None | Extra pointer(s) per node |
| **Cache friendly** | ✅ Yes | ❌ No (scattered) |

**When to use Linked Lists**:
- ✅ Frequent insertions/deletions at beginning
- ✅ Don't know size in advance
- ✅ Don't need random access
- ✅ Implementing stacks, queues, graphs

**When to use Arrays/Vectors**:
- ✅ Need fast random access
- ✅ Size is known or rarely changes
- ✅ Memory is limited (no pointer overhead)
- ✅ Cache performance matters

---

## 10. Common Linked List Problems

### Problem 1: Reverse Linked List

```cpp
void reverse() {
    Node* prev = nullptr;
    Node* current = head;
    Node* next = nullptr;

    while (current != nullptr) {
        next = current->next;    // Save next
        current->next = prev;    // Reverse pointer
        prev = current;          // Move prev forward
        current = next;          // Move current forward
    }

    head = prev;
}
```

**Visual**:
```
Before:  [10]→[20]→[30]→nullptr

After:   nullptr←[10]←[20]←[30]
                            ↑
                          head
```

---

### Problem 2: Detect Cycle (Floyd's Algorithm)

**Two pointers**: slow (moves 1 step), fast (moves 2 steps).

```cpp
bool hasCycle() {
    if (head == nullptr) return false;

    Node* slow = head;
    Node* fast = head;

    while (fast != nullptr && fast->next != nullptr) {
        slow = slow->next;
        fast = fast->next->next;

        if (slow == fast) return true;  // Cycle detected!
    }

    return false;
}
```

**Why it works**: If there's a cycle, fast will eventually catch up to slow!

---

### Problem 3: Find Middle Element

```cpp
Node* findMiddle() {
    Node* slow = head;
    Node* fast = head;

    while (fast != nullptr && fast->next != nullptr) {
        slow = slow->next;
        fast = fast->next->next;
    }

    return slow;  // Slow is at middle
}
```

---

### Problem 4: Merge Two Sorted Lists

```cpp
Node* mergeSorted(Node* l1, Node* l2) {
    if (l1 == nullptr) return l2;
    if (l2 == nullptr) return l1;

    Node* result;

    if (l1->data < l2->data) {
        result = l1;
        result->next = mergeSorted(l1->next, l2);
    } else {
        result = l2;
        result->next = mergeSorted(l1, l2->next);
    }

    return result;
}
```

---

## Complete Example Programs

### Example 1: Student Records

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Student {
    string name;
    int id;
    Student* next;

    Student(string n, int i) : name(n), id(i), next(nullptr) {}
};

class StudentList {
private:
    Student* head;

public:
    StudentList() : head(nullptr) {}

    void addStudent(string name, int id) {
        Student* newStudent = new Student(name, id);
        newStudent->next = head;
        head = newStudent;
    }

    void display() {
        Student* temp = head;
        while (temp != nullptr) {
            cout << "ID: " << temp->id << ", Name: " << temp->name << endl;
            temp = temp->next;
        }
    }

    Student* findByID(int id) {
        Student* temp = head;
        while (temp != nullptr) {
            if (temp->id == id) return temp;
            temp = temp->next;
        }
        return nullptr;
    }
};

int main() {
    StudentList students;

    students.addStudent("Alice", 101);
    students.addStudent("Bob", 102);
    students.addStudent("Charlie", 103);

    students.display();

    Student* found = students.findByID(102);
    if (found) {
        cout << "\nFound: " << found->name << endl;
    }

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Losing Head Pointer

```cpp
Node* temp = head;
while (temp != nullptr) {
    head = head->next;  // WRONG! Lost original head!
    temp = temp->next;
}

// FIX: Use temp, don't modify head
Node* temp = head;
while (temp != nullptr) {
    temp = temp->next;
}
```

---

### Mistake 2: Memory Leak

```cpp
void deleteList() {
    head = nullptr;  // WRONG! Memory leaked!
}

// FIX: Delete each node
void deleteList() {
    Node* temp = head;
    while (temp != nullptr) {
        Node* next = temp->next;
        delete temp;
        temp = next;
    }
    head = nullptr;
}
```

---

### Mistake 3: Dereferencing nullptr

```cpp
head->data = 10;  // CRASH if head is nullptr!

// FIX: Always check
if (head != nullptr) {
    head->data = 10;
}
```

---

## Practice Problems

### Problem 1: Remove Duplicates
Remove duplicate values from an unsorted linked list.

### Problem 2: Nth Node from End
Find the nth node from the end in one pass.

### Problem 3: Palindrome Check
Check if linked list values form a palindrome.

### Problem 4: Intersection Point
Find where two linked lists intersect (if they do).

### Problem 5: Add Two Numbers
Add two numbers represented as linked lists (digits in reverse).

---

## Key Takeaways

✅ **Linked List = nodes scattered in memory, connected by pointers**
✅ **Each node has data + pointer(s) to next/prev**
✅ **Singly linked**: next pointer only
✅ **Doubly linked**: next + prev pointers
✅ **Circular**: last points back to first
✅ **Insert at head**: O(1) - very fast!
✅ **Insert at tail**: O(n) without tail pointer
✅ **Random access**: O(n) - slow (must traverse)
✅ **Use when**: frequent insertions/deletions, don't need random access
✅ **Always free memory**: delete each node in destructor

---

## What's Next?

In **[Chapter 17](chapter-17-trees-basics.md)**, you'll learn about **Trees** - hierarchical data structures perfect for representing parent-child relationships!

---

**Chapter Progress**: ✅ Chapters 1-16 Complete | 📖 Next: Chapter 17
