# Chapter 1: Variables and Data Types

**What You'll Learn**: How computers store data, why int goes from -2³¹ to 2³¹-1, and how to choose the right data type.

## Table of Contents

1. [Introduction: What is a Variable?](#introduction-what-is-a-variable)
2. [Understanding Computer Memory: The Apartment Building Analogy](#understanding-computer-memory-the-apartment-building-analogy)
3. [What is a BIT?](#what-is-a-bit)
4. [What is a BYTE?](#what-is-a-byte)
5. [Common Data Types](#common-data-types)
6. [Why So Many Types?](#why-so-many-types)
7. [Code Examples](#code-examples)
8. [Common Mistakes](#common-mistakes)
9. [Practice Problems](#practice-problems)
10. [Key Takeaways](#key-takeaways)
11. [What's Next?](#whats-next)

---

## Introduction: What is a Variable?

A **variable** is like a labeled box where you store information. When you write:

```cpp
int age = 25;
```

You're telling the computer:
> "Hey, find me a box, label it 'age', and put the number 25 inside."

Think of your computer's memory like a giant warehouse with millions of boxes. Each box can hold data, and you give them names so you can find them later.

---

## Understanding Computer Memory: The Apartment Building Analogy

Your computer's RAM (memory) is like a **massive apartment building** with millions of tiny rooms:

```
Memory (The Apartment Building):

Floor 1000234:  [Room][Room][Room][Room][Room] ...
Floor 1000233:  [Room][Room][Room][Room][Room] ...
Floor 1000232:  [Room][Room][Room][Room][Room] ...
    ...
Floor 2:        [Room][Room][Room][Room][Room] ...
Floor 1:        [Room][Room][Room][Room][Room] ...
```

- Each **room** = 1 **BYTE** of memory
- Each room has an **address** (like apartment number)
- Inside each room are **8 light switches** called **BITS**
- Each switch can be **ON (1)** or **OFF (0)**

When you create `int age = 25`, you're renting 4 rooms and storing 25 in them!

---

## What is a BIT?

A **bit** is the smallest piece of information a computer understands:

```
OFF = 0
ON  = 1
```

That's it! Everything on your computer (games, videos, this text) is made of billions of 1s and 0s.

**Why only 1 and 0?** Because computers use electricity:
- Electricity flowing = 1 (ON)
- No electricity = 0 (OFF)

It's like a light switch - on or off, nothing in between.

---

## What is a BYTE?

A **byte** is **8 bits** grouped together:

```
One byte = 8 light switches in a row:
[0][1][0][1][1][0][0][1]
```

**Why 8?** Historically, 8 bits could represent:
- All English letters (A-Z, a-z)
- All digits (0-9)
- Common symbols (!, @, #, etc.)

So it became the standard "unit of storage."

**How many patterns?** With 8 switches that can be ON or OFF:
- 2 × 2 × 2 × 2 × 2 × 2 × 2 × 2 = 2⁸ = **256 different patterns**

This means 1 byte can store values from **0 to 255** (256 different numbers).

---

## Common Data Types

### 1. Integer Types (Whole Numbers)

#### `int` - The Standard Integer (4 bytes)

```cpp
int age = 25;
int score = -10;
int year = 2024;
```

**Memory**: 4 bytes = 32 bits

```
Memory addresses:  1000    1001    1002    1003
                  [byte] [byte] [byte] [byte]  ← Your int lives here
```

**Range**: -2,147,483,648 to 2,147,483,647
- That's about **-2 billion to +2 billion**

**Use for**: Ages, scores, counts, years, most everyday numbers

---

#### Why -2³¹ to 2³¹-1? The Mystery Explained! 🔍

You might wonder: "If I have 4 bytes × 8 bits = 32 bits, that's 2³² patterns (4 billion). Why only -2 billion to +2 billion?"

**Answer**: One bit is reserved for the **sign** (positive or negative)!

```
32 bits total:
[Sign bit][31 bits for the actual number]
    ↑
    0 = positive
    1 = negative
```

So we have only **31 bits** for the number value:
- 2³¹ = 2,147,483,648 different values

Split between positive and negative:
- **Positive**: 0 to 2,147,483,647 (that's 2³¹ - 1)
- **Negative**: -1 to -2,147,483,648 (that's -2³¹)

**Why -1 extra on negative side?** Because 0 is considered positive, so negative side gets one extra slot!

---

#### Visual: How Numbers Are Stored in Binary

**Example: Storing the number 5**

```
Decimal (what we see): 5

Binary (what computer stores):
00000000 00000000 00000000 00000101
↑                               ↑↑↑
Sign bit (0 = positive)         101 in binary = 5
```

**Example: Storing -5**

The computer uses "Two's Complement" (don't worry about details):

```
Binary:
11111111 11111111 11111111 11111011
↑
Sign bit (1 = negative)

Decimal: -5
```

---

#### Other Integer Types

```cpp
// char - 1 byte (very small numbers)
char smallNum = 100;
// Range: -128 to 127

// short - 2 bytes (small numbers)
short temperature = -15;
// Range: -32,768 to 32,767

// long long - 8 bytes (HUGE numbers)
long long population = 8000000000;
// Range: -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
```

**Comparison Table**:

| Type | Bytes | Bits | Range | When to Use |
|------|-------|------|-------|-------------|
| `char` | 1 | 8 | -128 to 127 | Very small numbers, characters |
| `short` | 2 | 16 | -32K to +32K | Small numbers like age |
| `int` | 4 | 32 | -2B to +2B | Most common numbers |
| `long long` | 8 | 64 | -9 quintillion to +9 quintillion | Huge numbers |

---

#### `unsigned` Types - Only Positive Numbers

If you **never need negative numbers**, use `unsigned`:

```cpp
unsigned int count = 100;
// Range: 0 to 4,294,967,295 (0 to 4 billion)

unsigned char age = 25;
// Range: 0 to 255
```

Since you don't need the sign bit, you get **all bits for the number**!

**When to use**:
- Counters (never negative)
- Array sizes (never negative)
- IDs (never negative)
- Age (never negative)

---

### 2. Floating Point Types (Decimal Numbers)

#### `float` - Single Precision (4 bytes)

```cpp
float price = 19.99f;  // Note the 'f' at the end
float pi = 3.14f;
```

**Precision**: About 6-7 decimal digits
**Range**: ±10³⁸ (very large!)

**Use for**: Prices, measurements when exact precision isn't critical

---

#### `double` - Double Precision (8 bytes)

```cpp
double pi = 3.14159265359;
double distance = 149597870.7;  // Distance to sun in km
```

**Precision**: About 15-16 decimal digits
**Range**: ±10³⁰⁸ (EXTREMELY large!)

**Use for**: Scientific calculations, precise measurements

---

#### Important: Floating Point Isn't Perfect!

```cpp
double a = 0.1;
double b = 0.2;
cout << (a + b);  // Might print: 0.30000000000000004 (not exactly 0.3!)
```

**Why?** Computers store decimals in binary, and some decimal numbers (like 0.1) can't be represented exactly in binary - just like 1/3 = 0.333... goes on forever in decimal.

**Lesson**: Don't use `==` with floats/doubles. Use a range:

```cpp
// Bad:
if (a + b == 0.3) { ... }

// Good:
if (abs((a + b) - 0.3) < 0.0001) { ... }
```

---

### 3. Character Type

#### `char` - Single Character (1 byte)

```cpp
char grade = 'A';
char initial = 'J';
char symbol = '@';
```

**Characters are actually numbers!** The computer uses **ASCII codes**:

```
'A' = 65
'B' = 66
'a' = 97 (lowercase different from uppercase!)
'0' = 48 (the character zero, not number zero)
' ' = 32 (space character)
```

**Proof**:

```cpp
char letter = 'A';
cout << letter;         // Prints: A
cout << (int)letter;    // Prints: 65 (the ASCII code)

char num = 65;
cout << num;            // Prints: A (interprets as character)
```

---

### 4. Boolean Type

#### `bool` - True or False (1 byte)

```cpp
bool isStudent = true;
bool hasLicense = false;
```

**Values**:
- `true` (internally stored as 1)
- `false` (internally stored as 0)

**Use for**: Flags, conditions, yes/no questions

```cpp
bool isAdult = (age >= 18);
if (isAdult) {
    cout << "You can vote!";
}
```

---

### 5. String Type

#### `string` - Multiple Characters

```cpp
#include <string>  // Need this!

string name = "Alice";
string message = "Hello, World!";
```

**Behind the scenes**: A string is actually a **collection of chars**:

```
"Hello" is stored as:
['H']['e']['l']['l']['o']['\0']
 ↑                        ↑
char char char char char  null terminator
```

**Common operations**:

```cpp
string name = "Alice";
cout << name.length();        // 5
cout << name[0];              // 'A' (first character)
name = name + " Smith";       // "Alice Smith" (concatenation)
```

---

## Why So Many Types? 🤔

**Question**: "Why not just use `long long` and `double` for everything?"

**Answer**: **Memory and speed!**

### Example: Storing Ages of 1 Million People

```
Using int (4 bytes each):
1,000,000 × 4 = 4,000,000 bytes = 4 MB

Using long long (8 bytes each):
1,000,000 × 8 = 8,000,000 bytes = 8 MB

Using char (1 byte each):
1,000,000 × 1 = 1,000,000 bytes = 1 MB
```

For ages (0-120), `char` is enough and **saves 75% memory**!

**Golden Rule**: Use the **smallest type that fits your needs**.

---

## Code Examples

### Example 1: Basic Variables

```cpp
#include <iostream>
using namespace std;

int main() {
    // Integer types
    int age = 25;
    short year = 2024;
    long long worldPopulation = 8000000000;

    // Floating point
    float price = 19.99f;
    double pi = 3.14159265359;

    // Character and boolean
    char grade = 'A';
    bool isPassing = true;

    // String
    string name = "Alice";

    // Print everything
    cout << "Age: " << age << endl;
    cout << "Year: " << year << endl;
    cout << "Population: " << worldPopulation << endl;
    cout << "Price: $" << price << endl;
    cout << "Pi: " << pi << endl;
    cout << "Grade: " << grade << endl;
    cout << "Passing: " << isPassing << endl;
    cout << "Name: " << name << endl;

    return 0;
}
```

### Example 2: Size of Types

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Size of char: " << sizeof(char) << " bytes" << endl;
    cout << "Size of short: " << sizeof(short) << " bytes" << endl;
    cout << "Size of int: " << sizeof(int) << " bytes" << endl;
    cout << "Size of long long: " << sizeof(long long) << " bytes" << endl;
    cout << "Size of float: " << sizeof(float) << " bytes" << endl;
    cout << "Size of double: " << sizeof(double) << " bytes" << endl;
    cout << "Size of bool: " << sizeof(bool) << " bytes" << endl;

    return 0;
}
```

### Example 3: Integer Overflow

```cpp
#include <iostream>
using namespace std;

int main() {
    int maxInt = 2147483647;  // Maximum int value
    cout << "Max int: " << maxInt << endl;

    maxInt = maxInt + 1;  // Overflow!
    cout << "Max int + 1: " << maxInt << endl;  // Wraps to -2147483648!

    // Like a car odometer:
    // 99999 + 1 = 00000 (rolls over)

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Using Uninitialized Variables

```cpp
int x;           // x contains garbage (random value)
cout << x;       // BAD! Could print anything

// Always initialize:
int x = 0;       // Good
```

### Mistake 2: Integer Division

```cpp
int a = 7;
int b = 2;
cout << a / b;   // Prints: 3 (not 3.5!)

// Solution: Use double
double result = 7.0 / 2.0;
cout << result;  // Prints: 3.5
```

### Mistake 3: Comparing Floats with ==

```cpp
double x = 0.1 + 0.2;
if (x == 0.3) {  // Might be false due to precision!
    cout << "Equal";
}

// Better:
if (abs(x - 0.3) < 0.0001) {  // Check if very close
    cout << "Equal";
}
```

---

## Practice Problems

### Problem 1: Person Information
Create variables to store:
- First name (string)
- Age (int)
- Height in meters (float)
- Is student (bool)
- Grade (char)

Print all information in a formatted way.

### Problem 2: Temperature Converter
- Store a temperature in Celsius (double)
- Convert to Fahrenheit: F = (C × 9/5) + 32
- Print both values

### Problem 3: Character ASCII
- Create a char variable with your initial
- Print the character
- Print its ASCII code (hint: cast to int)
- Try with uppercase and lowercase

### Problem 4: Variable Sizes
- Calculate how much memory is needed to store:
  - 100 int values
  - 100 char values
  - 100 double values
- Print the results

### Problem 5: Type Limits
- Create an int with the maximum value (2147483647)
- Add 1 to it
- Observe what happens (overflow)
- Try the same with unsigned int

---

## Key Takeaways

✅ **Memory is made of bytes** (8 bits each)
✅ **Bits are 0s and 1s** (ON/OFF switches)
✅ **int uses 4 bytes (32 bits)**: 1 for sign, 31 for value → range -2³¹ to 2³¹-1
✅ **Choose the smallest type that fits** to save memory
✅ **Characters are numbers** (ASCII codes)
✅ **Floating point isn't exact** (avoid == comparisons)
✅ **Always initialize variables** before using them

---

## What's Next?

In **[Chapter 2](chapter-02-operators-and-expressions.md)**, you'll learn about operators and expressions - how to perform calculations, comparisons, and logical operations with your variables!

---

**Chapter Progress**: ✅ Chapter 1 Complete | 📖 Next: Chapter 2
