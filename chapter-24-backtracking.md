# Chapter 24: Backtracking

**What You'll Learn**: What backtracking is, WHY it exists, the choose-explore-unchoose pattern, solving N-Queens and Sudoku, generating permutations/combinations, and when to use backtracking.

## Table of Contents

1. [What is Backtracking?](#1-what-is-backtracking)
2. [The Backtracking Pattern](#2-the-backtracking-pattern)
3. [Example 1: Generate All Permutations](#3-example-1-generate-all-permutations)
4. [Example 2: Generate Combinations](#4-example-2-generate-combinations)
5. [Example 3: N-Queens Problem](#5-example-3-n-queens-problem)
6. [Example 4: Sudoku Solver](#6-example-4-sudoku-solver)
7. [When to Use Backtracking](#7-when-to-use-backtracking)
8. [Backtracking vs Other Techniques](#8-backtracking-vs-other-techniques)
9. [Optimization Techniques](#9-optimization-techniques)
10. [Complete Example Programs](#complete-example-programs)
11. [Common Mistakes](#common-mistakes)
12. [Practice Problems](#practice-problems)
13. [Key Takeaways](#key-takeaways)

---

## Introduction: The Exploration Problem

Imagine you're in a maze trying to find the exit:

**Wrong approach**:
```
1. Pick a random path
2. If it's wrong, give up
```

**Smart approach (Backtracking)**:
```
1. Try a path
2. If it leads to exit → Done!
3. If it's a dead end → Go back and try another path
4. Repeat until you find exit or exhaust all paths
```

**Problem**: How do you **systematically explore ALL possibilities** without getting lost or repeating work?

**Solution**: **Backtracking** - a systematic way to try all possibilities by exploring, and when stuck, going back (backtracking) to try a different choice.

---

## 1. What is Backtracking?

**Backtracking** is an algorithmic technique for solving problems recursively by trying to build a solution **incrementally**, abandoning solutions ("backtracking") as soon as it determines the solution cannot work.

**Think of it like**:
- **Maze exploration**: Try path, if blocked, return and try different path
- **Sudoku puzzle**: Fill cell, if contradiction, erase and try different number
- **Decision tree**: Explore branch, if fails, backtrack to parent and try sibling

**Visual - Decision Tree**:
```
                    Start
                   /  |  \
                  /   |   \
                 1    2    3  ← Choose
                / \   |   / \
               /   \  |  /   \
              1a   1b 2a 3a  3b  ← Explore deeper
              |    ✗  ✗  ✓   ✗   ← Check if valid
            Goal!

✓ = Valid solution
✗ = Dead end → Backtrack!
```

---

## 2. The Backtracking Pattern

Every backtracking problem follows this **3-step pattern**:

```cpp
void backtrack(state, choices) {
    // Base case: found solution or exhausted options
    if (isSolution(state)) {
        processSolution(state);
        return;
    }

    // Recursive case
    for (each choice in choices) {
        // 1. CHOOSE: Make a choice
        makeChoice(state, choice);

        // 2. EXPLORE: Recurse with new state
        backtrack(state, remainingChoices);

        // 3. UNCHOOSE: Undo the choice (backtrack)
        undoChoice(state, choice);
    }
}
```

**The Three Steps**:
1. **CHOOSE**: Add element to current solution
2. **EXPLORE**: Recursively try to complete solution
3. **UNCHOOSE**: Remove element from solution (backtrack)

**Why unchoose?** To restore state for trying next choice!

---

## 3. Example 1: Generate All Permutations

**Problem**: Generate all permutations of [1, 2, 3]

**Output**: [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]

**Visual - Decision Tree**:
```
                        []
                    /   |   \
                   1    2    3      ← Choose first number
                  / \   |   / \
                 2   3  1  1   2    ← Choose second number
                 |   |  |  |   |
                 3   2  3  2   1    ← Choose third number

Result: [1,2,3] [1,3,2] [2,1,3] [2,3,1] [3,1,2] [3,2,1]
```

**Implementation**:

```cpp
#include <iostream>
#include <vector>
using namespace std;

void backtrack(vector<int>& current, vector<bool>& used,
               vector<int>& nums, vector<vector<int>>& result) {
    // Base case: permutation is complete
    if (current.size() == nums.size()) {
        result.push_back(current);
        return;
    }

    // Try each number
    for (int i = 0; i < nums.size(); i++) {
        // Skip if already used
        if (used[i]) continue;

        // 1. CHOOSE: Add number to permutation
        current.push_back(nums[i]);
        used[i] = true;

        // 2. EXPLORE: Recurse
        backtrack(current, used, nums, result);

        // 3. UNCHOOSE: Remove number (backtrack)
        current.pop_back();
        used[i] = false;
    }
}

vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> current;
    vector<bool> used(nums.size(), false);

    backtrack(current, used, nums, result);
    return result;
}

int main() {
    vector<int> nums = {1, 2, 3};
    vector<vector<int>> perms = permute(nums);

    cout << "All permutations:" << endl;
    for (auto& perm : perms) {
        cout << "[";
        for (int i = 0; i < perm.size(); i++) {
            cout << perm[i];
            if (i < perm.size() - 1) cout << ", ";
        }
        cout << "]" << endl;
    }

    return 0;
}
```

**How it works**:
1. Start with empty permutation
2. Try adding each unused number
3. Recursively build rest of permutation
4. When complete, save result
5. Backtrack (remove number) to try next option

**Time Complexity**: O(n × n!) - n! permutations, each takes O(n) to build
**Space Complexity**: O(n) - recursion depth

---

## 4. Example 2: Generate Combinations

**Problem**: Generate all combinations of k numbers from [1, 2, 3, 4]

**Example**: k=2 → [1,2], [1,3], [1,4], [2,3], [2,4], [3,4]

**Key difference from permutations**: Order doesn't matter! [1,2] = [2,1]

**Visual - Decision Tree**:
```
                        []
                    /   |   \  \
                   1    2    3  4    ← Choose first (start from any)
                  /|\   |\   |
                 2 3 4  3 4  4       ← Choose second (only higher numbers!)

Result: [1,2] [1,3] [1,4] [2,3] [2,4] [3,4]
```

**Implementation**:

```cpp
#include <iostream>
#include <vector>
using namespace std;

void backtrack(int start, int k, vector<int>& current,
               int n, vector<vector<int>>& result) {
    // Base case: combination is complete
    if (current.size() == k) {
        result.push_back(current);
        return;
    }

    // Try each number from start to n
    for (int i = start; i <= n; i++) {
        // 1. CHOOSE: Add number
        current.push_back(i);

        // 2. EXPLORE: Recurse (start from i+1 to avoid duplicates)
        backtrack(i + 1, k, current, n, result);

        // 3. UNCHOOSE: Remove number
        current.pop_back();
    }
}

vector<vector<int>> combine(int n, int k) {
    vector<vector<int>> result;
    vector<int> current;

    backtrack(1, k, current, n, result);
    return result;
}

int main() {
    int n = 4, k = 2;
    vector<vector<int>> combs = combine(n, k);

    cout << "All combinations of " << k << " from 1.." << n << ":" << endl;
    for (auto& comb : combs) {
        cout << "[";
        for (int i = 0; i < comb.size(); i++) {
            cout << comb[i];
            if (i < comb.size() - 1) cout << ", ";
        }
        cout << "]" << endl;
    }

    return 0;
}
```

**Key insight**: Start from `i+1` to avoid duplicates and maintain order

---

## 5. Example 3: N-Queens Problem

**Problem**: Place N queens on N×N chessboard so no two queens attack each other.

**Rules**: Queens attack same row, column, or diagonal.

**Example** (4-Queens):
```
Solution 1:        Solution 2:
. Q . .            . . Q .
. . . Q            Q . . .
Q . . .            . . . Q
. . Q .            . Q . .
```

**Visual - Decision Tree** (N=4):
```
                    []
                 / | | \
    Q in row 0: 0 1 2 3    ← Try placing queen in each column
                |
           Q in row 1       ← Can't place in column 0,1,2 (attacks!)
                |
           Q in row 2       ← Try valid positions
                |
           Q in row 3       ← Complete board or backtrack
```

**Implementation**:

```cpp
#include <iostream>
#include <vector>
using namespace std;

bool isSafe(vector<int>& board, int row, int col) {
    // Check if placing queen at (row, col) is safe

    // Check previous rows
    for (int i = 0; i < row; i++) {
        int prevCol = board[i];

        // Same column?
        if (prevCol == col) return false;

        // Same diagonal?
        if (abs(i - row) == abs(prevCol - col)) return false;
    }

    return true;
}

void backtrack(int row, int n, vector<int>& board,
               vector<vector<string>>& result) {
    // Base case: All queens placed
    if (row == n) {
        // Convert board to string format
        vector<string> solution;
        for (int i = 0; i < n; i++) {
            string rowStr(n, '.');
            rowStr[board[i]] = 'Q';
            solution.push_back(rowStr);
        }
        result.push_back(solution);
        return;
    }

    // Try placing queen in each column of current row
    for (int col = 0; col < n; col++) {
        if (isSafe(board, row, col)) {
            // 1. CHOOSE: Place queen
            board[row] = col;

            // 2. EXPLORE: Try next row
            backtrack(row + 1, n, board, result);

            // 3. UNCHOOSE: Remove queen (implicit - will be overwritten)
        }
    }
}

vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> result;
    vector<int> board(n, -1);  // board[row] = column of queen in that row

    backtrack(0, n, board, result);
    return result;
}

void printBoard(vector<string>& board) {
    for (string& row : board) {
        cout << row << endl;
    }
    cout << endl;
}

int main() {
    int n = 4;
    vector<vector<string>> solutions = solveNQueens(n);

    cout << "Solutions for " << n << "-Queens:" << endl;
    cout << "Total: " << solutions.size() << " solutions\n" << endl;

    for (int i = 0; i < solutions.size(); i++) {
        cout << "Solution " << (i + 1) << ":" << endl;
        printBoard(solutions[i]);
    }

    return 0;
}
```

**How it works**:
1. Try placing queen in each column of current row
2. Check if placement is safe (no attacks)
3. If safe, recursively try next row
4. If can't place in any column, backtrack
5. When all rows filled, found solution!

**Optimization**: Store board as `board[row] = col` instead of 2D array

**Time Complexity**: O(n!) in worst case
**Space Complexity**: O(n) - recursion depth

---

## 6. Example 4: Sudoku Solver

**Problem**: Fill 9×9 grid following Sudoku rules:
- Each row has 1-9 exactly once
- Each column has 1-9 exactly once
- Each 3×3 box has 1-9 exactly once

**Visual**:
```
Before:              After:
5 3 . | . 7 . | . . .    5 3 4 | 6 7 8 | 9 1 2
6 . . | 1 9 5 | . . .    6 7 2 | 1 9 5 | 3 4 8
. 9 8 | . . . | . 6 .    1 9 8 | 3 4 2 | 5 6 7
------+-------+------    ------+-------+------
8 . . | . 6 . | . . 3    8 5 9 | 7 6 1 | 4 2 3
4 . . | 8 . 3 | . . 1    4 2 6 | 8 5 3 | 7 9 1
7 . . | . 2 . | . . 6    7 1 3 | 9 2 4 | 8 5 6
------+-------+------    ------+-------+------
. 6 . | . . . | 2 8 .    9 6 1 | 5 3 7 | 2 8 4
. . . | 4 1 9 | . . 5    2 8 7 | 4 1 9 | 6 3 5
. . . | . 8 . | . 7 9    3 4 5 | 2 8 6 | 1 7 9
```

**Implementation**:

```cpp
#include <iostream>
#include <vector>
using namespace std;

bool isValid(vector<vector<char>>& board, int row, int col, char num) {
    // Check row
    for (int i = 0; i < 9; i++) {
        if (board[row][i] == num) return false;
    }

    // Check column
    for (int i = 0; i < 9; i++) {
        if (board[i][col] == num) return false;
    }

    // Check 3×3 box
    int boxRow = (row / 3) * 3;
    int boxCol = (col / 3) * 3;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (board[boxRow + i][boxCol + j] == num) return false;
        }
    }

    return true;
}

bool backtrack(vector<vector<char>>& board) {
    // Find next empty cell
    for (int row = 0; row < 9; row++) {
        for (int col = 0; col < 9; col++) {
            if (board[row][col] == '.') {
                // Try each number 1-9
                for (char num = '1'; num <= '9'; num++) {
                    if (isValid(board, row, col, num)) {
                        // 1. CHOOSE: Place number
                        board[row][col] = num;

                        // 2. EXPLORE: Try to solve rest
                        if (backtrack(board)) {
                            return true;  // Solution found!
                        }

                        // 3. UNCHOOSE: Remove number (backtrack)
                        board[row][col] = '.';
                    }
                }

                // No valid number found → dead end
                return false;
            }
        }
    }

    // No empty cells → solved!
    return true;
}

void solveSudoku(vector<vector<char>>& board) {
    backtrack(board);
}

void printBoard(vector<vector<char>>& board) {
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            cout << board[i][j] << " ";
            if (j == 2 || j == 5) cout << "| ";
        }
        cout << endl;
        if (i == 2 || i == 5) {
            cout << "------+-------+------" << endl;
        }
    }
}

int main() {
    vector<vector<char>> board = {
        {'5','3','.','.','7','.','.','.','.'},
        {'6','.','.','1','9','5','.','.','.'},
        {'.','9','8','.','.','.','.','6','.'},
        {'8','.','.','.','6','.','.','.','3'},
        {'4','.','.','8','.','3','.','.','1'},
        {'7','.','.','.','2','.','.','.','6'},
        {'.','6','.','.','.','.','2','8','.'},
        {'.','.','.','4','1','9','.','.','5'},
        {'.','.','.','.','8','.','.','7','9'}
    };

    cout << "Original Sudoku:" << endl;
    printBoard(board);

    solveSudoku(board);

    cout << "\nSolved Sudoku:" << endl;
    printBoard(board);

    return 0;
}
```

**How it works**:
1. Find first empty cell
2. Try numbers 1-9
3. Check if number is valid (row, column, box)
4. If valid, place it and recursively solve rest
5. If rest can't be solved, backtrack (try next number)
6. If no number works, return false (dead end)

**Optimization**: Can optimize by choosing cell with fewest possibilities

---

## 7. When to Use Backtracking

**Use backtracking when**:
✅ Need to **explore all possibilities**
✅ Problem has **constraints** (rules to follow)
✅ Can **build solution incrementally**
✅ Can **detect invalid states early**
✅ Solution involves **choices** at each step

**Common problem types**:
- **Permutations/Combinations**: Generate all arrangements
- **Constraint satisfaction**: Sudoku, N-Queens, graph coloring
- **Path finding**: Maze, word search
- **Subset problems**: Subset sum, partition

**When NOT to use**:
❌ Need **optimal solution** with overlapping subproblems → Use Dynamic Programming
❌ Need **single shortest path** → Use BFS
❌ Need **best choice at each step** → Use Greedy
❌ Too many possibilities → Exponential time (may be infeasible)

---

## 8. Backtracking vs Other Techniques

| Feature | Backtracking | Dynamic Programming | Greedy |
|---------|--------------|---------------------|--------|
| Approach | Try all paths, backtrack on failure | Solve subproblems, reuse results | Make best local choice |
| Use case | Constraint satisfaction | Optimization with overlapping subproblems | Optimization with greedy choice property |
| Complexity | Exponential (often) | Polynomial (often) | Polynomial (usually) |
| Examples | N-Queens, Sudoku | Knapsack, LCS | Activity selection |

---

## 9. Optimization Techniques

**1. Pruning**: Stop early when solution can't work
```cpp
if (currentSum > target) return;  // No point exploring further
```

**2. Constraint propagation**: Use constraints to eliminate choices
```cpp
if (isValid(choice)) {  // Check constraints before recursing
    explore(choice);
}
```

**3. Early termination**: Stop when first solution found
```cpp
if (foundSolution) return true;  // Don't need all solutions
```

**4. Memoization**: Cache results (if subproblems overlap)
```cpp
if (memo.count(state)) return memo[state];
```

---

## Complete Example Programs

### Example: Word Search

**Problem**: Find if word exists in grid (can move up/down/left/right)

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

bool backtrack(vector<vector<char>>& board, string& word,
               int row, int col, int index) {
    // Base case: found whole word
    if (index == word.length()) return true;

    // Out of bounds or wrong character
    if (row < 0 || row >= board.size() ||
        col < 0 || col >= board[0].size() ||
        board[row][col] != word[index]) {
        return false;
    }

    // 1. CHOOSE: Mark cell as visited
    char temp = board[row][col];
    board[row][col] = '#';

    // 2. EXPLORE: Try all 4 directions
    bool found = backtrack(board, word, row + 1, col, index + 1) ||
                 backtrack(board, word, row - 1, col, index + 1) ||
                 backtrack(board, word, row, col + 1, index + 1) ||
                 backtrack(board, word, row, col - 1, index + 1);

    // 3. UNCHOOSE: Restore cell
    board[row][col] = temp;

    return found;
}

bool exist(vector<vector<char>>& board, string word) {
    for (int i = 0; i < board.size(); i++) {
        for (int j = 0; j < board[0].size(); j++) {
            if (backtrack(board, word, i, j, 0)) {
                return true;
            }
        }
    }
    return false;
}

int main() {
    vector<vector<char>> board = {
        {'A', 'B', 'C', 'E'},
        {'S', 'F', 'C', 'S'},
        {'A', 'D', 'E', 'E'}
    };

    string word1 = "ABCCED";
    string word2 = "SEE";
    string word3 = "ABCB";

    cout << "Board:" << endl;
    for (auto& row : board) {
        for (char c : row) cout << c << " ";
        cout << endl;
    }

    cout << "\nWord '" << word1 << "': " << (exist(board, word1) ? "Found" : "Not found") << endl;
    cout << "Word '" << word2 << "': " << (exist(board, word2) ? "Found" : "Not found") << endl;
    cout << "Word '" << word3 << "': " << (exist(board, word3) ? "Found" : "Not found") << endl;

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Forgetting to Backtrack

```cpp
// BAD: No unchoose step!
void backtrack(vector<int>& current) {
    current.push_back(value);
    backtrack(current);
    // FORGOT: current.pop_back();
}
```

**Fix**: Always undo changes after recursion!

### Mistake 2: Modifying Shared State Incorrectly

```cpp
// BAD: Modifying parameter without restoration
void backtrack(vector<int> current) {  // Pass by value (inefficient)
    current.push_back(value);
    backtrack(current);
}
```

**Fix**: Pass by reference and restore state, or pass by value (less efficient)

### Mistake 3: Not Checking Constraints Early

```cpp
// BAD: Exploring invalid paths
for (each choice) {
    backtrack(choice);  // Should check if valid first!
}

// GOOD: Prune early
for (each choice) {
    if (isValid(choice)) {
        backtrack(choice);
    }
}
```

### Mistake 4: Infinite Recursion

```cpp
// BAD: No base case or wrong base case
void backtrack() {
    backtrack();  // Never stops!
}

// GOOD: Clear base case
void backtrack() {
    if (done) return;
    // ...
}
```

---

## Practice Problems

### Problem 1: Letter Combinations of Phone Number
Given digit string "23", return all letter combinations (like phone keypad).

### Problem 2: Palindrome Partitioning
Partition string into all possible palindromic substrings.

### Problem 3: Generate Parentheses
Generate all combinations of n pairs of balanced parentheses.

### Problem 4: Combination Sum
Find all combinations that sum to target (can reuse numbers).

### Problem 5: Restore IP Addresses
Generate all valid IP addresses from a string of digits.

---

## Key Takeaways

✅ **Backtracking = try all possibilities systematically**
✅ **Pattern**: Choose → Explore → Unchoose
✅ **Base case**: Solution found or no more choices
✅ **Pruning**: Stop early when constraints violated
✅ **State restoration**: Always undo changes (backtrack)
✅ **Use when**: Need all solutions, constraint satisfaction
✅ **Time complexity**: Often exponential (but necessary!)
✅ **Common uses**: Permutations, N-Queens, Sudoku, path finding

---

## Backtracking Checklist

Before implementing backtracking, ask:
- [ ] What are my **choices** at each step?
- [ ] What's my **goal** (base case)?
- [ ] What **constraints** must I satisfy?
- [ ] How do I **choose** (add to solution)?
- [ ] How do I **unchoose** (restore state)?
- [ ] Can I **prune** invalid paths early?

---

## What's Next?

In **[Chapter 25](chapter-25-greedy-algorithms.md)**, you'll learn about **Greedy Algorithms** - making the locally optimal choice at each step, and when this simple strategy actually works!

---

**Chapter Progress**: ✅ Chapters 1-24 Complete | 📖 Next: Chapter 25
