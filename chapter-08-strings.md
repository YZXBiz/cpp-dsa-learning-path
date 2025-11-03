# Chapter 8: Strings

**What You'll Learn**: What strings are, WHY C++ has two types of strings, how they're stored in memory, and common string operations.

## Table of Contents

1. [Two Types of Strings in C++](#1-two-types-of-strings-in-c)
2. [C-Style Strings (Character Arrays)](#2-c-style-strings-character-arrays)
3. [std::string (The Modern Way)](#3-stdstring-the-modern-way)
4. [String Input/Output](#4-string-inputoutput)
5. [String Memory Layout](#5-string-memory-layout)
6. [String Iteration](#6-string-iteration)
7. [Character Functions (from `<cctype>`)](#7-character-functions-from-cctype)
8. [String Number Conversion](#8-string-number-conversion)
9. [Common String Mistakes](#common-string-mistakes-)
10. [Practice Problems](#practice-problems)
11. [Key Takeaways](#key-takeaways)

---

## Introduction: The Text Problem

Imagine you need to store someone's name:

**Bad approach (separate characters)**:
```cpp
char c1 = 'J';
char c2 = 'o';
char c3 = 'h';
char c4 = 'n';
// What if name has 20 letters? 😱
```

**Problems**:
- ❌ Need separate variable for each letter
- ❌ Can't easily compare names
- ❌ Can't pass name to a function easily
- ❌ No way to know where name ends

**With a string**:
```cpp
string name = "John";  // Easy!
```

**Strings are like a bracelet of beads** - individual characters linked together with a label!

---

## 1. Two Types of Strings in C++

C++ has **TWO** ways to work with strings:

### Type 1: C-Style Strings (Old Way)
```cpp
char name[20] = "John";
```
- From the C language (C++ inherited it)
- Just an array of characters
- Ends with `'\0'` (null terminator)
- More manual, less safe

### Type 2: std::string (Modern Way)
```cpp
string name = "John";
```
- From C++ Standard Library
- More features, easier to use
- Automatically manages memory
- **Recommended for beginners!**

**Think of it like**: C-strings are manual cars, std::string is automatic!

---

## 2. C-Style Strings (Character Arrays)

### The Null Terminator

C-strings **must** end with `'\0'` (null character):

```cpp
char name[5] = "John";
```

**In memory**:
```
Index:   0    1    2    3    4
Value:  'J'  'o'  'h'  'n'  '\0'
         ↑                   ↑
       Start                End marker
```

**Why `'\0'`?** It tells functions "string ends here!"

Without it, functions don't know where to stop:
```cpp
char broken[5] = {'J', 'o', 'h', 'n', '!'};  // No '\0'!
cout << broken;  // Prints garbage after '!' until it finds '\0'
```

---

### Declaration and Initialization

**Method 1: With size (automatic null terminator)**:
```cpp
char name[20] = "John";  // Compiler adds '\0' automatically
```

**Method 2: Character by character (manual null)**:
```cpp
char name[5];
name[0] = 'J';
name[1] = 'o';
name[2] = 'h';
name[3] = 'n';
name[4] = '\0';  // MUST add this!
```

**Method 3: Size calculated automatically**:
```cpp
char name[] = "John";  // Size is 5 (including '\0')
```

---

### Common C-String Functions

Need to `#include <cstring>`:

```cpp
#include <cstring>

char str1[20] = "Hello";
char str2[20] = "World";

// Length (not counting '\0')
int len = strlen(str1);  // 5

// Copy
strcpy(str1, "New");     // str1 is now "New"

// Concatenate (append)
strcat(str1, str2);      // str1 is now "NewWorld"

// Compare
int result = strcmp(str1, str2);
// result < 0: str1 comes before str2
// result = 0: equal
// result > 0: str1 comes after str2
```

---

### Problems with C-Strings ⚠️

**Problem 1: Buffer overflow**:
```cpp
char name[5] = "John";
strcpy(name, "Alexander");  // DISASTER! Doesn't fit!
// Overwrites memory beyond array!
```

**Problem 2: Manual memory management**:
```cpp
char* name = new char[100];
// ... use it ...
delete[] name;  // Must remember to free!
```

**Problem 3: Comparison doesn't work intuitively**:
```cpp
char str1[] = "Hello";
char str2[] = "Hello";

if (str1 == str2) {  // WRONG! Compares addresses, not content
    cout << "Equal";
}

// Must use:
if (strcmp(str1, str2) == 0) {
    cout << "Equal";
}
```

---

## 3. std::string (The Modern Way)

**Include**:
```cpp
#include <string>
using namespace std;
```

### Why std::string is Better

✅ **Automatic memory management** (grows as needed)
✅ **Safe** (no buffer overflows)
✅ **Intuitive operators** (`+`, `==`, `<`, etc.)
✅ **Rich functionality** (find, replace, substr, etc.)
✅ **Easy to use**

---

### Declaration and Initialization

```cpp
string name = "John";          // Most common
string greeting("Hello");      // Constructor style
string empty;                  // Empty string ""
string copy = name;            // Copy of name
string repeated(5, 'A');       // "AAAAA"
```

---

### Basic Operations

**Length**:
```cpp
string name = "John";
cout << name.length();  // 4
cout << name.size();    // 4 (same as length)
```

**Concatenation** (joining):
```cpp
string first = "Hello";
string second = "World";

string result = first + " " + second;  // "Hello World"

first += "!";  // first is now "Hello!"
```

**Comparison** (works naturally!):
```cpp
string s1 = "Apple";
string s2 = "Banana";

if (s1 == s2) { }      // Check equal
if (s1 < s2) { }       // Alphabetical order
if (s1 != s2) { }      // Not equal
```

**Access characters**:
```cpp
string name = "John";

cout << name[0];    // 'J' (first character)
cout << name[3];    // 'n' (last character)

name[0] = 'T';      // name is now "Tohn"
```

---

### Common String Methods

**1. Append**:
```cpp
string s = "Hello";
s.append(" World");  // "Hello World"
```

**2. Insert**:
```cpp
string s = "Hello World";
s.insert(6, "Beautiful ");  // "Hello Beautiful World"
//         ↑ position
```

**3. Erase**:
```cpp
string s = "Hello World";
s.erase(5, 6);  // "Hello" (erase 6 chars starting at index 5)
//      ↑  ↑
//    start length
```

**4. Substring**:
```cpp
string s = "Hello World";
string sub = s.substr(0, 5);  // "Hello"
//                    ↑  ↑
//                  start length
```

**5. Find**:
```cpp
string s = "Hello World";
int pos = s.find("World");  // 6 (index where "World" starts)

if (s.find("xyz") == string::npos) {
    cout << "Not found";  // string::npos means "not found"
}
```

**6. Replace**:
```cpp
string s = "Hello World";
s.replace(0, 5, "Hi");  // "Hi World"
//        ↑  ↑   ↑
//      start len new
```

**7. Clear & Empty**:
```cpp
string s = "Hello";
s.clear();           // s is now ""
if (s.empty()) {     // true
    cout << "Empty";
}
```

**8. Convert to C-string**:
```cpp
string s = "Hello";
const char* cstr = s.c_str();  // For legacy functions
```

---

## 4. String Input/Output

### Output (Easy)

```cpp
string name = "John";
cout << name;  // Prints: John
```

---

### Input (Be Careful!)

**Problem with `cin`**:
```cpp
string name;
cout << "Enter name: ";
cin >> name;  // ONLY reads until SPACE!

// Input: "John Doe"
// name = "John"  (stops at space!)
```

**Solution: Use `getline`**:
```cpp
string name;
cout << "Enter full name: ";
getline(cin, name);  // Reads entire line including spaces

// Input: "John Doe"
// name = "John Doe"  ✓
```

**Note**: After using `cin >>`, you may need to clear the buffer:
```cpp
int age;
string name;

cin >> age;
cin.ignore();  // Clear the newline left by cin
getline(cin, name);
```

---

## 5. String Memory Layout

**std::string** is actually a class that manages:
1. **Pointer** to character array (on heap)
2. **Length** of string
3. **Capacity** (allocated space)

**Conceptual diagram**:
```
string s = "Hello";

Stack:
┌──────────────────┐
│ string object s  │
│ - ptr: 0x2000    │───┐
│ - length: 5      │   │
│ - capacity: 15   │   │
└──────────────────┘   │
                       │
Heap:                  │
┌──────────────────┐   │
│ 0x2000:          │◄──┘
│ H e l l o \0     │
└──────────────────┘
```

**This is WHY string can grow** - it manages heap memory automatically!

---

## 6. String Iteration

### Method 1: Index Loop

```cpp
string s = "Hello";

for (int i = 0; i < s.length(); i++) {
    cout << s[i] << " ";
}
// Output: H e l l o
```

---

### Method 2: Range-Based Loop (Modern!)

```cpp
string s = "Hello";

for (char c : s) {
    cout << c << " ";
}
// Output: H e l l o
```

**Modify characters** (use reference):
```cpp
string s = "hello";

for (char& c : s) {  // Note: &
    c = toupper(c);
}
// s is now "HELLO"
```

---

## 7. Character Functions (from `<cctype>`)

Useful for working with individual characters:

```cpp
#include <cctype>

char c = 'a';

toupper(c);     // 'A'
tolower(c);     // 'a'
isalpha(c);     // true (is it a letter?)
isdigit(c);     // false (is it a digit 0-9?)
isspace(c);     // false (is it whitespace?)
isalnum(c);     // true (alphanumeric: letter or digit?)
```

**Example: Convert to uppercase**:
```cpp
string s = "Hello World";

for (char& c : s) {
    c = toupper(c);
}
// s is now "HELLO WORLD"
```

---

## 8. String Number Conversion

### String to Number

```cpp
#include <string>

string numStr = "123";
int num = stoi(numStr);        // String to int: 123

string floatStr = "3.14";
double pi = stod(floatStr);    // String to double: 3.14

long bigNum = stol("9999999"); // String to long
```

---

### Number to String

```cpp
int num = 123;
string s = to_string(num);  // "123"

double pi = 3.14159;
string piStr = to_string(pi);  // "3.141590"
```

---

## Complete Example Programs

### Example 1: Palindrome Checker

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string str;
    cout << "Enter a string: ";
    getline(cin, str);

    int left = 0;
    int right = str.length() - 1;
    bool isPalindrome = true;

    while (left < right) {
        if (str[left] != str[right]) {
            isPalindrome = false;
            break;
        }
        left++;
        right--;
    }

    if (isPalindrome) {
        cout << "\"" << str << "\" is a palindrome!" << endl;
    } else {
        cout << "\"" << str << "\" is NOT a palindrome." << endl;
    }

    return 0;
}
```

---

### Example 2: Count Vowels and Consonants

```cpp
#include <iostream>
#include <string>
#include <cctype>
using namespace std;

int main() {
    string text;
    cout << "Enter text: ";
    getline(cin, text);

    int vowels = 0, consonants = 0;

    for (char c : text) {
        c = tolower(c);  // Convert to lowercase
        if (isalpha(c)) {
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                vowels++;
            } else {
                consonants++;
            }
        }
    }

    cout << "Vowels: " << vowels << endl;
    cout << "Consonants: " << consonants << endl;

    return 0;
}
```

---

### Example 3: Word Frequency Counter

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string text;
    cout << "Enter text: ";
    getline(cin, text);

    int wordCount = 0;
    bool inWord = false;

    for (char c : text) {
        if (c == ' ' || c == '\t' || c == '\n') {
            inWord = false;
        } else if (!inWord) {
            inWord = true;
            wordCount++;
        }
    }

    cout << "Word count: " << wordCount << endl;

    return 0;
}
```

---

### Example 4: Reverse Words in String

```cpp
#include <iostream>
#include <string>
using namespace std;

string reverseWords(string s) {
    string result = "";
    string word = "";

    for (int i = 0; i < s.length(); i++) {
        if (s[i] != ' ') {
            word += s[i];
        } else {
            if (word != "") {
                result = word + " " + result;
                word = "";
            }
        }
    }

    // Add last word
    if (word != "") {
        result = word + " " + result;
    }

    // Remove trailing space
    if (result.back() == ' ') {
        result.pop_back();
    }

    return result;
}

int main() {
    string s = "Hello World from C++";
    cout << "Original: " << s << endl;
    cout << "Reversed: " << reverseWords(s) << endl;
    // Output: C++ from World Hello

    return 0;
}
```

---

## Common String Mistakes ⚠️

### Mistake 1: Forgetting `getline` for Multi-Word Input

```cpp
string name;
cin >> name;  // WRONG for "John Doe"

// FIX:
getline(cin, name);
```

---

### Mistake 2: Using C-String Functions on std::string

```cpp
string s = "Hello";
strlen(s);  // ERROR! strlen is for C-strings

// FIX:
s.length();  // Correct for std::string
```

---

### Mistake 3: Index Out of Bounds

```cpp
string s = "Hi";
cout << s[5];  // CRASH! Index 5 doesn't exist (only 0-1)

// FIX: Check length first
if (index < s.length()) {
    cout << s[index];
}
```

---

### Mistake 4: Mixing cin and getline

```cpp
int age;
string name;

cin >> age;
getline(cin, name);  // PROBLEM! Gets empty string

// FIX: Clear buffer
cin >> age;
cin.ignore();  // Clear the newline
getline(cin, name);
```

---

## Practice Problems

### Problem 1: String Compression
Compress string "aaabbcccc" → "a3b2c4"

### Problem 2: Anagram Checker
Check if two strings are anagrams (e.g., "listen" and "silent")

### Problem 3: Remove Duplicates
Remove duplicate characters from a string.

### Problem 4: Longest Word
Find the longest word in a sentence.

### Problem 5: Caesar Cipher
Shift each letter by k positions (e.g., "abc" with k=1 → "bcd")

### Problem 6: Valid Parentheses
Check if string has valid parentheses: "(())" = valid, "(()" = invalid

---

## Key Takeaways

✅ **C-strings** = character arrays ending with `'\0'`
✅ **std::string** = modern, safe, easy (use this!)
✅ **getline()** for input with spaces
✅ **String concatenation** with `+` operator
✅ **Natural comparisons** with `==`, `<`, etc.
✅ **Rich methods**: substr, find, replace, erase, insert
✅ **Character functions**: toupper, tolower, isalpha, isdigit
✅ **Conversions**: stoi, stod, to_string

---

## C-Strings vs std::string

| Feature | C-String | std::string |
|---------|----------|-------------|
| Memory | Fixed size | Dynamic (grows) |
| Safety | Unsafe (buffer overflow) | Safe |
| Operators | Manual (strcmp, etc.) | Natural (==, +) |
| Ease | Hard | Easy |
| Use case | Legacy code | Modern C++ ✓ |

**Recommendation**: Use `std::string` unless working with legacy C code!

---

## What's Next?

In **[Chapter 9](chapter-09-classes-and-oop.md)**, you'll learn about Classes and Object-Oriented Programming - the foundation of organizing complex programs!

---

**Chapter Progress**: ✅ Chapters 1-8 Complete | 📖 Next: Chapter 9
