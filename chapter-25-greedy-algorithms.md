# Chapter 25: Greedy Algorithms

**What You'll Learn**: What greedy algorithms are, WHY they work for some problems, the greedy choice property, activity selection, fractional knapsack, coin change, and when greedy works vs when it fails.

## Table of Contents

1. [What is a Greedy Algorithm?](#1-what-is-a-greedy-algorithm)
2. [Greedy Choice Property](#2-greedy-choice-property)
3. [Example 1: Activity Selection](#3-example-1-activity-selection)
4. [Example 2: Fractional Knapsack](#4-example-2-fractional-knapsack)
5. [Example 3: Coin Change (Greedy Approach)](#5-example-3-coin-change-greedy-approach)
6. [Example 4: Huffman Coding](#6-example-4-huffman-coding)
7. [When Greedy Works vs When It Fails](#7-when-greedy-works-vs-when-it-fails)
8. [Proving Greedy Correctness](#8-proving-greedy-correctness)
9. [Greedy vs Dynamic Programming](#9-greedy-vs-dynamic-programming)
10. [Complete Example Programs](#complete-example-programs)
11. [Common Mistakes](#common-mistakes)
12. [Practice Problems](#practice-problems)
13. [Key Takeaways](#key-takeaways)

---

## Introduction: The Optimal Choice Problem

You're at a buffet with limited plate space. Two strategies:

**Strategy 1: Greedy**
```
Look at all food → Pick what looks best right now → Repeat
Simple, fast, but might miss better combinations!
```

**Strategy 2: Consider Everything**
```
Calculate all possible plate combinations → Pick optimal
Guaranteed best, but takes forever!
```

**Question**: When can we trust the greedy approach?

**Answer**: When the **best local choice always leads to the best global solution**!

---

## 1. What is a Greedy Algorithm?

A **greedy algorithm** makes the **locally optimal choice** at each step, hoping to find a **global optimum**.

**Think of it like**:
- **Cashier making change**: Always give largest coin that fits
- **Climbing a hill**: Always take steepest path upward
- **Packing boxes**: Put largest item first

**Visual - Greedy vs Optimal**:
```
Finding maximum sum path (greedy might fail):

        10
       /  \
      5    2
     / \  / \
    1  1 20 1

Greedy picks: 10 → 5 → 1 = 16 ✗
Optimal is:   10 → 2 → 20 = 32 ✓

But in some problems, greedy = optimal!
```

---

## 2. Greedy Choice Property

**Greedy choice property**: A global optimum can be arrived at by making **locally optimal choices**.

**Requirements for greedy to work**:
1. **Greedy choice property**: Local optimum → global optimum
2. **Optimal substructure**: Optimal solution contains optimal subsolutions

**Example where greedy works**: Making change with standard coins
```
Amount: 63 cents
Coins: [25, 10, 5, 1]

Greedy:
- 2 × 25¢ = 50¢ (always pick largest)
- 1 × 10¢ = 10¢
- 0 × 5¢  = 0¢
- 3 × 1¢  = 3¢
Total: 6 coins (OPTIMAL!)
```

**Example where greedy fails**: Coins [25, 10, 1] for 30 cents
```
Greedy:
- 1 × 25¢ = 25¢ (largest)
- 0 × 10¢ = 0¢
- 5 × 1¢  = 5¢
Total: 6 coins ✗

Optimal:
- 3 × 10¢ = 30¢
Total: 3 coins ✓
```

---

## 3. Example 1: Activity Selection

**Problem**: Given N activities with start and end times, select maximum number of non-overlapping activities.

**Example**:
```
Activity    Start    End
   A1         1       4
   A2         3       5
   A3         0       6
   A4         5       7
   A5         3       9
   A6         5       9
   A7         6      10
   A8         8      11
   A9         8      12
   A10        2      14
   A11       12      16
```

**Visual - Timeline**:
```
Time: 0    2    4    6    8   10   12   14   16
A1:      |---|
A2:          |---|
A3:   |---------|
A4:              |---|
A5:          |-----------|
A6:              |-------|
A7:                  |---|
A8:                      |---|
A9:                      |-------|
A10:     |----------------------|
A11:                            |-----|

Greedy picks: A1, A4, A8, A11 (4 activities)
```

**Greedy strategy**: Always pick activity that **ends earliest** (leaves most room for others)!

**Implementation**:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Activity {
    int start, end;
    string name;
};

// Sort by end time (greedy choice!)
bool compare(Activity a, Activity b) {
    return a.end < b.end;
}

vector<Activity> selectActivities(vector<Activity>& activities) {
    // Sort by end time
    sort(activities.begin(), activities.end(), compare);

    vector<Activity> selected;
    selected.push_back(activities[0]);  // First activity always selected

    int lastEnd = activities[0].end;

    // Greedy: pick next activity that starts after last one ends
    for (int i = 1; i < activities.size(); i++) {
        if (activities[i].start >= lastEnd) {
            selected.push_back(activities[i]);
            lastEnd = activities[i].end;
        }
    }

    return selected;
}

int main() {
    vector<Activity> activities = {
        {1, 4, "A1"},  {3, 5, "A2"},  {0, 6, "A3"},
        {5, 7, "A4"},  {3, 9, "A5"},  {5, 9, "A6"},
        {6, 10, "A7"}, {8, 11, "A8"}, {8, 12, "A9"},
        {2, 14, "A10"}, {12, 16, "A11"}
    };

    vector<Activity> selected = selectActivities(activities);

    cout << "Maximum activities: " << selected.size() << endl;
    cout << "Selected activities:" << endl;
    for (auto& a : selected) {
        cout << a.name << " [" << a.start << ", " << a.end << "]" << endl;
    }

    return 0;
}
```

**Output**:
```
Maximum activities: 4
Selected activities:
A1 [1, 4]
A4 [5, 7]
A8 [8, 11]
A11 [12, 16]
```

**Why greedy works here?**
- Picking earliest-ending activity leaves maximum time for remaining activities
- Optimal substructure: If A1 is in optimal solution, rest is optimal for remaining time

**Time Complexity**: O(n log n) - sorting dominates
**Space Complexity**: O(1) - ignoring output

---

## 4. Example 2: Fractional Knapsack

**Problem**: Given weights and values, and capacity W, maximize total value (can take fractions!).

**Example**:
```
Item    Weight    Value    Value/Weight
  1       10        60         6.0
  2       20       100         5.0
  3       30       120         4.0

Capacity: 50
```

**Greedy strategy**: Pick items with **highest value-to-weight ratio** first!

**Visual**:
```
Ratio: 6.0      5.0      4.0
Item:   1        2        3

Take all of 1:  10 kg, value = 60   (40 capacity left)
Take all of 2:  20 kg, value = 100  (20 capacity left)
Take 2/3 of 3:  20 kg, value = 80   (0 capacity left)

Total: 60 + 100 + 80 = 240 ✓ OPTIMAL!
```

**Implementation**:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Item {
    int weight, value;
    string name;
};

// Sort by value/weight ratio (descending)
bool compare(Item a, Item b) {
    double ratioA = (double)a.value / a.weight;
    double ratioB = (double)b.value / b.weight;
    return ratioA > ratioB;
}

double fractionalKnapsack(vector<Item>& items, int capacity) {
    // Sort by value/weight ratio
    sort(items.begin(), items.end(), compare);

    double totalValue = 0.0;
    int remainingCapacity = capacity;

    for (auto& item : items) {
        if (remainingCapacity >= item.weight) {
            // Take whole item
            totalValue += item.value;
            remainingCapacity -= item.weight;
            cout << "Take all of " << item.name << ": value = "
                 << item.value << endl;
        } else {
            // Take fraction
            double fraction = (double)remainingCapacity / item.weight;
            double valueAdded = item.value * fraction;
            totalValue += valueAdded;
            cout << "Take " << fraction << " of " << item.name
                 << ": value = " << valueAdded << endl;
            remainingCapacity = 0;
            break;
        }
    }

    return totalValue;
}

int main() {
    vector<Item> items = {
        {10, 60, "Item1"},
        {20, 100, "Item2"},
        {30, 120, "Item3"}
    };
    int capacity = 50;

    cout << "Items (sorted by value/weight):" << endl;
    for (auto& item : items) {
        cout << item.name << ": weight=" << item.weight
             << ", value=" << item.value
             << ", ratio=" << (double)item.value/item.weight << endl;
    }

    cout << "\nPacking knapsack (capacity " << capacity << "):" << endl;
    double maxValue = fractionalKnapsack(items, capacity);

    cout << "\nMaximum value: " << maxValue << endl;

    return 0;
}
```

**Why greedy works here?**
- Fractions allowed → can always take best ratio items
- No dependencies between items

**Important**: For **0/1 Knapsack** (no fractions), greedy FAILS! Need Dynamic Programming.

**Time Complexity**: O(n log n)
**Space Complexity**: O(1)

---

## 5. Example 3: Coin Change (Greedy Approach)

**Problem**: Make change using minimum coins.

**Greedy strategy**: Always use **largest coin** that fits!

**Implementation**:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

vector<int> coinChangeGreedy(vector<int>& coins, int amount) {
    sort(coins.rbegin(), coins.rend());  // Sort descending

    vector<int> result;

    for (int coin : coins) {
        while (amount >= coin) {
            result.push_back(coin);
            amount -= coin;
        }
    }

    return result;
}

int main() {
    vector<int> coins = {1, 5, 10, 25};
    int amount = 63;

    cout << "Coins: ";
    for (int c : coins) cout << c << " ";
    cout << "\nAmount: " << amount << endl;

    vector<int> result = coinChangeGreedy(coins, amount);

    cout << "\nGreedy solution:" << endl;
    cout << "Coins used: " << result.size() << endl;
    cout << "Coins: ";
    for (int c : result) cout << c << " ";
    cout << endl;

    // Count each coin
    cout << "\nBreakdown:" << endl;
    sort(coins.rbegin(), coins.rend());
    for (int coin : coins) {
        int count = 0;
        for (int c : result) {
            if (c == coin) count++;
        }
        if (count > 0) {
            cout << count << " × " << coin << "¢" << endl;
        }
    }

    return 0;
}
```

**When greedy works**: Standard US coins [1, 5, 10, 25]
```
Amount: 63¢
- 2 × 25¢ = 50¢
- 1 × 10¢ = 10¢
- 0 × 5¢  = 0¢
- 3 × 1¢  = 3¢
Total: 6 coins ✓ OPTIMAL!
```

**When greedy fails**: Coins [1, 3, 4] for amount 6
```
Greedy:
- 1 × 4 = 4
- 0 × 3 = 0
- 2 × 1 = 2
Total: 3 coins ✗

Optimal:
- 2 × 3 = 6
Total: 2 coins ✓

Need Dynamic Programming!
```

---

## 6. Example 4: Huffman Coding

**Problem**: Compress text by assigning shorter codes to frequent characters.

**Example**:
```
Text: "ABRACADABRA"
Frequency: A=5, B=2, R=2, C=1, D=1

Fixed-length encoding: 3 bits/char × 11 chars = 33 bits
Huffman encoding: ~23 bits (save 30%!)
```

**Greedy strategy**: Build tree bottom-up, always merge **two lowest frequencies**!

**Visual - Building Huffman Tree**:
```
Step 1: Frequencies
C:1  D:1  B:2  R:2  A:5

Step 2: Merge C and D (lowest)
   2
  / \
 C   D

Step 3: Merge B and R
   2       2
  / \     / \
 C   D   B   R

Step 4: Merge two 2s
       4
      / \
     /   \
    2     2
   / \   / \
  C   D B   R

Step 5: Merge with A
           9
          / \
         /   \
        4     A
       / \
      /   \
     2     2
    / \   / \
   C   D B   R

Codes:
A: 1      (1 bit)
B: 010    (3 bits)
R: 011    (3 bits)
C: 000    (3 bits)
D: 001    (3 bits)
```

**Implementation** (simplified):

```cpp
#include <iostream>
#include <queue>
#include <unordered_map>
#include <vector>
using namespace std;

struct Node {
    char ch;
    int freq;
    Node *left, *right;

    Node(char c, int f) : ch(c), freq(f), left(nullptr), right(nullptr) {}
};

struct Compare {
    bool operator()(Node* a, Node* b) {
        return a->freq > b->freq;  // Min heap
    }
};

void printCodes(Node* root, string code, unordered_map<char, string>& codes) {
    if (!root) return;

    if (!root->left && !root->right) {
        codes[root->ch] = code;
    }

    printCodes(root->left, code + "0", codes);
    printCodes(root->right, code + "1", codes);
}

unordered_map<char, string> huffmanCoding(unordered_map<char, int>& freq) {
    priority_queue<Node*, vector<Node*>, Compare> pq;

    // Create leaf nodes
    for (auto& p : freq) {
        pq.push(new Node(p.first, p.second));
    }

    // Build tree (greedy: merge lowest frequencies)
    while (pq.size() > 1) {
        Node* left = pq.top(); pq.pop();
        Node* right = pq.top(); pq.pop();

        // Create internal node
        Node* parent = new Node('\0', left->freq + right->freq);
        parent->left = left;
        parent->right = right;

        pq.push(parent);
    }

    // Generate codes
    unordered_map<char, string> codes;
    printCodes(pq.top(), "", codes);

    return codes;
}

int main() {
    string text = "ABRACADABRA";
    unordered_map<char, int> freq;

    // Count frequencies
    for (char c : text) {
        freq[c]++;
    }

    cout << "Text: " << text << endl;
    cout << "\nCharacter frequencies:" << endl;
    for (auto& p : freq) {
        cout << p.first << ": " << p.second << endl;
    }

    // Generate Huffman codes
    unordered_map<char, string> codes = huffmanCoding(freq);

    cout << "\nHuffman codes:" << endl;
    for (auto& p : codes) {
        cout << p.first << ": " << p.second << endl;
    }

    // Calculate bits
    int fixedBits = text.length() * 3;  // 3 bits for 5 chars
    int huffmanBits = 0;
    for (char c : text) {
        huffmanBits += codes[c].length();
    }

    cout << "\nFixed-length encoding: " << fixedBits << " bits" << endl;
    cout << "Huffman encoding: " << huffmanBits << " bits" << endl;
    cout << "Savings: " << (fixedBits - huffmanBits) << " bits ("
         << (100.0 * (fixedBits - huffmanBits) / fixedBits) << "%)" << endl;

    return 0;
}
```

**Why greedy works?**
- Merging lowest frequencies ensures frequent chars get shorter codes
- Optimal substructure: Tree built from optimal subtrees

**Time Complexity**: O(n log n) - heap operations
**Space Complexity**: O(n) - tree storage

---

## 7. When Greedy Works vs When It Fails

### ✅ Greedy Works

**1. Activity Selection**
- Greedy: Pick earliest ending
- Always optimal!

**2. Fractional Knapsack**
- Greedy: Pick highest value/weight
- Always optimal!

**3. Huffman Coding**
- Greedy: Merge lowest frequencies
- Always optimal!

**4. Standard Coin Change**
- Greedy: Pick largest coin
- Optimal for standard denominations!

**5. Minimum Spanning Tree (Prim's, Kruskal's)**
- Greedy: Pick shortest edge
- Always optimal!

### ❌ Greedy Fails

**1. 0/1 Knapsack**
```
Items: [(weight=1, value=2), (weight=2, value=3)]
Capacity: 2

Greedy (by value): Take item 2 → value = 3 ✗
Optimal: Take item 1 twice → value = 4 ✓
Wait, can't take twice in 0/1!

Actually: Take item 2 → value = 3 ✓ (greedy works here)
But in general, greedy fails for 0/1 knapsack!
```

**2. Non-standard Coins**
```
Coins: [1, 3, 4], Amount: 6
Greedy: 4 + 1 + 1 = 3 coins ✗
Optimal: 3 + 3 = 2 coins ✓
```

**3. Longest Path in Graph**
```
Greedy picks heaviest edge → might miss longer path
Need Dynamic Programming or exhaustive search!
```

**4. Traveling Salesman Problem (TSP)**
```
Greedy picks nearest city → often suboptimal
Need dynamic programming or approximation algorithms!
```

---

## 8. Proving Greedy Correctness

To prove greedy works, show:

**1. Greedy Choice Property**
- Prove: Local optimal choice → global optimal solution

**2. Optimal Substructure**
- Prove: Optimal solution contains optimal subsolutions

**Example: Activity Selection**

**Claim**: Picking earliest-ending activity is always part of some optimal solution.

**Proof**:
1. Let OPT be optimal solution with different first activity A'
2. Replace A' with earliest-ending activity A
3. Since A ends earlier, all activities after A' still fit
4. New solution has same or better count
5. Therefore, greedy choice is safe! ✓

---

## 9. Greedy vs Dynamic Programming

| Feature | Greedy | Dynamic Programming |
|---------|--------|---------------------|
| Approach | Make local best choice | Solve all subproblems |
| When to use | Greedy choice property holds | Overlapping subproblems + optimal substructure |
| Time | Usually faster (O(n log n)) | Usually slower (O(n²) or worse) |
| Correctness | Must prove greedy works | Always correct if formulated properly |
| Examples | Activity selection, Huffman | Knapsack 0/1, LCS, edit distance |

**Rule of thumb**: Try greedy first (if it works, great!), fall back to DP if needed.

---

## Complete Example Programs

### Example: Job Sequencing

**Problem**: Jobs with deadlines and profits. Maximize profit.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Job {
    char id;
    int deadline, profit;
};

bool compare(Job a, Job b) {
    return a.profit > b.profit;  // Sort by profit (descending)
}

vector<char> jobSequencing(vector<Job>& jobs, int maxDeadline) {
    sort(jobs.begin(), jobs.end(), compare);

    vector<char> result(maxDeadline, '-');
    vector<bool> slot(maxDeadline, false);

    // Greedy: For each job, find latest available slot before deadline
    for (auto& job : jobs) {
        for (int j = min(maxDeadline, job.deadline) - 1; j >= 0; j--) {
            if (!slot[j]) {
                result[j] = job.id;
                slot[j] = true;
                break;
            }
        }
    }

    return result;
}

int main() {
    vector<Job> jobs = {
        {'A', 2, 100},
        {'B', 1, 19},
        {'C', 2, 27},
        {'D', 1, 25},
        {'E', 3, 15}
    };
    int maxDeadline = 3;

    cout << "Jobs (profit, deadline):" << endl;
    for (auto& j : jobs) {
        cout << j.id << ": profit=" << j.profit
             << ", deadline=" << j.deadline << endl;
    }

    vector<char> schedule = jobSequencing(jobs, maxDeadline);

    cout << "\nSchedule:" << endl;
    int totalProfit = 0;
    for (int i = 0; i < schedule.size(); i++) {
        cout << "Slot " << (i + 1) << ": ";
        if (schedule[i] != '-') {
            cout << "Job " << schedule[i];
            // Find profit
            for (auto& j : jobs) {
                if (j.id == schedule[i]) {
                    cout << " (profit=" << j.profit << ")";
                    totalProfit += j.profit;
                    break;
                }
            }
        } else {
            cout << "Empty";
        }
        cout << endl;
    }

    cout << "\nTotal profit: " << totalProfit << endl;

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Using Greedy When It Doesn't Work

```cpp
// 0/1 Knapsack with greedy = WRONG!
// Must use Dynamic Programming
```

### Mistake 2: Wrong Greedy Choice

```cpp
// Activity selection: sorting by start time = WRONG!
// Must sort by END time!
```

### Mistake 3: Not Proving Correctness

```cpp
// Always ask: Does greedy choice property hold?
// If unsure, greedy might fail!
```

### Mistake 4: Forgetting Edge Cases

```cpp
// What if no items fit?
// What if all deadlines are same?
// Test edge cases!
```

---

## Practice Problems

### Problem 1: Minimum Platforms
Given train arrival/departure times, find minimum platforms needed.

### Problem 2: Maximum Meetings
Schedule maximum meetings in one room.

### Problem 3: Candy Distribution
Minimum candies to distribute based on ratings (LeetCode).

### Problem 4: Jump Game
Can you reach the end? (Greedy vs DP approaches).

### Problem 5: Gas Station
Can you complete circular route? (Greedy approach).

---

## Key Takeaways

✅ **Greedy = make locally optimal choice**
✅ **Works when**: Greedy choice property + optimal substructure
✅ **Faster than DP** but only works for specific problems
✅ **Common uses**: Activity selection, Huffman, fractional knapsack, MST
✅ **Fails for**: 0/1 knapsack, TSP, longest path
✅ **Always sort first** (by end time, value/weight, etc.)
✅ **Must prove correctness** - greedy intuition can be wrong!
✅ **When in doubt**: Try greedy first, fall back to DP if needed

---

## Greedy Algorithm Checklist

Before using greedy, verify:
- [ ] Does greedy choice property hold?
- [ ] What's the optimal local choice?
- [ ] Can I prove this leads to global optimum?
- [ ] Are there counterexamples?
- [ ] Should I use DP instead?

---

## What's Next?

In **[Chapter 26](chapter-26-dynamic-programming-intro.md)**, you'll learn about **Dynamic Programming** - the ultimate technique for optimization problems when greedy fails, using memoization and tabulation to solve overlapping subproblems efficiently!

---

**Chapter Progress**: ✅ Chapters 1-25 Complete | 📖 Next: Chapter 26 (Final Chapter!)
