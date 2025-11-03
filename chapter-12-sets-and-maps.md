# Chapter 12: Sets & Maps

**What You'll Learn**: What sets and maps are, WHY they're useful, hash tables behind the scenes, fast lookups, and when to use them.

## Table of Contents

1. [Set: Unique Elements, Fast Lookup](#1-set-unique-elements-fast-lookup)
2. [Using Set in C++](#2-using-set-in-c)
3. [Unordered Set: Faster Lookups!](#3-unordered-set-faster-lookups)
4. [Set Applications](#4-set-applications)
5. [Map: Key-Value Pairs](#5-map-key-value-pairs)
6. [Using Map in C++](#6-using-map-in-c)
7. [Unordered Map: Faster Lookups!](#7-unordered-map-faster-lookups)
8. [Map Applications](#8-map-applications)
9. [MultiSet and MultiMap](#9-multiset-and-multimap)
10. [Set vs Map vs Vector](#10-set-vs-map-vs-vector)
11. [Hash Tables Explained](#11-hash-tables-explained)
12. [Common Mistakes](#common-mistakes-)
13. [Practice Problems](#practice-problems)
14. [Key Takeaways](#key-takeaways)

---

## Introduction: The Lookup Problem

**Problem 1: Check if Element Exists**
```cpp
vector<int> v = {5, 2, 8, 1, 9, 3, 7};

// Check if 8 exists
bool found = false;
for (int x : v) {
    if (x == 8) {
        found = true;
        break;
    }
}
// O(n) time - must check each element!
```

**Problem 2: Count Word Frequency**
```cpp
// Count how many times each word appears
string words[] = {"apple", "banana", "apple", "cherry", "banana"};

// Need to track:
// apple → 2
// banana → 2
// cherry → 1

// How to store this efficiently? 🤔
```

**Solution**: Sets (for membership) and Maps (for key-value pairs)!

---

## 1. Set: Unique Elements, Fast Lookup

A **set** is a collection of **unique elements** with **fast lookup**.

**Key Properties**:
- ✅ No duplicates (automatically removes them)
- ✅ Elements are sorted (by default)
- ✅ Fast operations: insert, delete, search (O(log n))

**Think of it like**: A checklist where you can't add the same item twice.

---

## 2. Using Set in C++

**Include**:
```cpp
#include <set>
using namespace std;
```

### Creating a Set

```cpp
set<int> s;  // Empty set of integers
```

---

### Basic Operations

**Insert**:
```cpp
set<int> s;

s.insert(10);
s.insert(20);
s.insert(10);  // Duplicate! Ignored

// s = {10, 20} (only unique elements)
```

**Find**:
```cpp
if (s.find(10) != s.end()) {
    cout << "Found!";
} else {
    cout << "Not found";
}
```

**Erase**:
```cpp
s.erase(10);  // Removes 10
```

**Size and Empty**:
```cpp
cout << s.size();    // Number of elements
cout << s.empty();   // Check if empty
```

**Clear**:
```cpp
s.clear();  // Remove all elements
```

---

### Iterating Through Set

```cpp
set<int> s = {30, 10, 20};  // Note: will be sorted!

for (int x : s) {
    cout << x << " ";
}
// Output: 10 20 30 (sorted!)
```

---

### Complete Set Example

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> s;

    // Insert elements
    s.insert(30);
    s.insert(10);
    s.insert(20);
    s.insert(10);  // Duplicate, ignored

    cout << "Set elements: ";
    for (int x : s) {
        cout << x << " ";
    }
    // Output: 10 20 30 (sorted, unique)

    // Check membership
    if (s.find(20) != s.end()) {
        cout << "\n20 is in the set";
    }

    // Remove element
    s.erase(20);

    cout << "\nAfter erasing 20: ";
    for (int x : s) {
        cout << x << " ";
    }
    // Output: 10 30

    return 0;
}
```

---

## 3. Unordered Set: Faster Lookups!

**unordered_set** uses **hash table** (even faster: O(1) average!)

**Include**:
```cpp
#include <unordered_set>
```

**Usage**:
```cpp
unordered_set<int> us;

us.insert(10);
us.insert(20);
us.insert(30);

// Fast lookup! O(1) average
if (us.find(20) != us.end()) {
    cout << "Found!";
}

// NOT sorted!
for (int x : us) {
    cout << x << " ";  // Could be: 30 10 20 (any order)
}
```

**When to use**:
- `set`: Need sorted order
- `unordered_set`: Only need fast lookup (don't care about order)

---

## 4. Set Applications

### Application 1: Remove Duplicates

```cpp
#include <iostream>
#include <set>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {10, 20, 10, 30, 20, 40};

    set<int> s(v.begin(), v.end());  // Set removes duplicates

    cout << "Unique elements: ";
    for (int x : s) {
        cout << x << " ";
    }
    // Output: 10 20 30 40

    return 0;
}
```

---

### Application 2: Check for Common Elements

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> s1 = {1, 2, 3, 4, 5};
    set<int> s2 = {4, 5, 6, 7, 8};

    cout << "Common elements: ";
    for (int x : s1) {
        if (s2.find(x) != s2.end()) {
            cout << x << " ";
        }
    }
    // Output: 4 5

    return 0;
}
```

---

## 5. Map: Key-Value Pairs

A **map** stores **key-value pairs** with **fast lookup by key**.

**Think of it like**: A dictionary (word → definition)

**Key Properties**:
- Keys are unique
- Keys are sorted
- Fast lookup, insert, delete (O(log n))

---

## 6. Using Map in C++

**Include**:
```cpp
#include <map>
using namespace std;
```

### Creating a Map

```cpp
map<string, int> m;  // Key: string, Value: int
```

---

### Basic Operations

**Insert/Update**:
```cpp
map<string, int> ages;

ages["Alice"] = 25;
ages["Bob"] = 30;
ages["Alice"] = 26;  // Updates value!

// ages = { {"Alice", 26}, {"Bob", 30} }
```

**Access**:
```cpp
cout << ages["Alice"];  // 26

// If key doesn't exist, creates it with default value (0 for int)
cout << ages["Charlie"];  // 0 (creates new entry!)
```

**Find** (safe way):
```cpp
if (ages.find("Alice") != ages.end()) {
    cout << "Alice's age: " << ages["Alice"];
}
```

**Erase**:
```cpp
ages.erase("Bob");  // Removes key-value pair
```

**Size and Empty**:
```cpp
cout << ages.size();
cout << ages.empty();
```

---

### Iterating Through Map

```cpp
map<string, int> ages = {{"Alice", 25}, {"Bob", 30}, {"Charlie", 28}};

for (auto pair : ages) {
    cout << pair.first << ": " << pair.second << endl;
}
// Output (sorted by key):
// Alice: 25
// Bob: 30
// Charlie: 28

// Modern C++ (structured binding):
for (auto [key, value] : ages) {
    cout << key << ": " << value << endl;
}
```

---

### Complete Map Example

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    map<string, int> wordCount;

    // Count words
    string words[] = {"apple", "banana", "apple", "cherry", "banana", "apple"};

    for (string word : words) {
        wordCount[word]++;
    }

    // Display counts
    cout << "Word frequencies:" << endl;
    for (auto [word, count] : wordCount) {
        cout << word << ": " << count << endl;
    }
    // Output:
    // apple: 3
    // banana: 2
    // cherry: 1

    return 0;
}
```

---

## 7. Unordered Map: Faster Lookups!

**unordered_map** uses **hash table** (O(1) average lookup!)

**Include**:
```cpp
#include <unordered_map>
```

**Usage**:
```cpp
unordered_map<string, int> um;

um["Alice"] = 25;
um["Bob"] = 30;

// Fast lookup! O(1) average
cout << um["Alice"];  // 25

// NOT sorted!
for (auto [key, value] : um) {
    cout << key << ": " << value << endl;  // Any order
}
```

**When to use**:
- `map`: Need sorted keys
- `unordered_map`: Only need fast lookup (don't care about order)

---

## 8. Map Applications

### Application 1: Character Frequency Counter

```cpp
#include <iostream>
#include <unordered_map>
#include <string>
using namespace std;

int main() {
    string text = "hello world";
    unordered_map<char, int> freq;

    for (char c : text) {
        if (c != ' ') {
            freq[c]++;
        }
    }

    cout << "Character frequencies:" << endl;
    for (auto [ch, count] : freq) {
        cout << ch << ": " << count << endl;
    }

    return 0;
}
```

---

### Application 2: Two Sum Problem

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

// Find two numbers that add up to target
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> seen;  // value → index

    for (int i = 0; i < nums.size(); i++) {
        int complement = target - nums[i];

        if (seen.find(complement) != seen.end()) {
            return {seen[complement], i};  // Found!
        }

        seen[nums[i]] = i;
    }

    return {};  // Not found
}

int main() {
    vector<int> nums = {2, 7, 11, 15};
    int target = 9;

    vector<int> result = twoSum(nums, target);

    cout << "Indices: " << result[0] << ", " << result[1] << endl;
    // Output: 0, 1 (nums[0] + nums[1] = 2 + 7 = 9)

    return 0;
}
```

---

### Application 3: Group Anagrams

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<string> words = {"eat", "tea", "tan", "ate", "nat", "bat"};
    unordered_map<string, vector<string>> anagrams;

    for (string word : words) {
        string sorted = word;
        sort(sorted.begin(), sorted.end());
        anagrams[sorted].push_back(word);
    }

    cout << "Anagram groups:" << endl;
    for (auto [key, group] : anagrams) {
        for (string word : group) {
            cout << word << " ";
        }
        cout << endl;
    }
    // Output:
    // eat tea ate
    // tan nat
    // bat

    return 0;
}
```

---

## 9. MultiSet and MultiMap

**multiset**: Allows duplicates (unlike set)

```cpp
#include <set>

multiset<int> ms;
ms.insert(10);
ms.insert(10);
ms.insert(20);

// ms = {10, 10, 20} (duplicates allowed)
```

**multimap**: Allows duplicate keys (unlike map)

```cpp
#include <map>

multimap<string, int> mm;
mm.insert({"Alice", 25});
mm.insert({"Alice", 26});  // Duplicate key allowed!

// Both entries exist
```

---

## 10. Set vs Map vs Vector

| Container | Use Case | Duplicates | Sorted | Lookup Speed |
|-----------|----------|------------|--------|--------------|
| **vector** | Ordered list | Yes | No | O(n) |
| **set** | Unique elements | No | Yes | O(log n) |
| **unordered_set** | Unique, fast lookup | No | No | O(1) avg |
| **map** | Key-value pairs | No (keys) | Yes (keys) | O(log n) |
| **unordered_map** | Key-value, fast | No (keys) | No | O(1) avg |

---

## 11. Hash Tables Explained

**How unordered_set and unordered_map work**:

```
Hash Function: key → hash value → bucket index

Example: Insert "apple"
1. hash("apple") = 12345
2. index = 12345 % bucket_count = 5
3. Store in bucket 5

Buckets:
┌─────┬─────┬─────┬─────┬─────┬──────────┬─────┐
│  0  │  1  │  2  │  3  │  4  │    5     │  6  │
└─────┴─────┴─────┴─────┴─────┴──────────┴─────┘
                              │ "apple"  │

Lookup: O(1) - go directly to bucket!
```

**Collision handling**: Multiple keys in same bucket (uses linked lists or similar)

---

## Common Mistakes ⚠️

### Mistake 1: Modifying Map While Iterating

```cpp
map<string, int> m = {{"a", 1}, {"b", 2}};

for (auto [key, val] : m) {
    m.erase(key);  // ERROR! Invalidates iterator
}

// FIX: Collect keys first, then erase
vector<string> toRemove;
for (auto [key, val] : m) {
    if (val < 2) toRemove.push_back(key);
}
for (string key : toRemove) {
    m.erase(key);
}
```

---

### Mistake 2: Using [] When Key May Not Exist

```cpp
map<string, int> ages;

cout << ages["Unknown"];  // Creates new entry with value 0!

// FIX: Use find()
if (ages.find("Unknown") != ages.end()) {
    cout << ages["Unknown"];
}
```

---

## Practice Problems

### Problem 1: First Unique Character
Find first non-repeating character in a string.

### Problem 2: Isomorphic Strings
Check if two strings are isomorphic (e.g., "egg" and "add").

### Problem 3: Subarray Sum Equals K
Find number of subarrays with sum equal to k (use map).

### Problem 4: LRU Cache
Implement Least Recently Used cache (use map + list).

### Problem 5: Top K Frequent Elements
Find k most frequent elements in array.

---

## Key Takeaways

✅ **Set** = unique elements, sorted, O(log n) operations
✅ **Unordered_set** = unique elements, O(1) average operations
✅ **Map** = key-value pairs, sorted keys, O(log n) operations
✅ **Unordered_map** = key-value pairs, O(1) average operations
✅ **Use sets** for membership testing, removing duplicates
✅ **Use maps** for counting, grouping, fast lookups
✅ **Hash tables** power unordered containers (O(1) magic!)
✅ **Prefer unordered** when order doesn't matter (faster!)

---

## What's Next?

In **[Chapter 13](chapter-13-stl-algorithms.md)**, you'll learn about **STL Algorithms** - powerful functions like sort, find, binary_search that work with all containers!

---

**Chapter Progress**: ✅ Chapters 1-12 Complete | 📖 Next: Chapter 13
