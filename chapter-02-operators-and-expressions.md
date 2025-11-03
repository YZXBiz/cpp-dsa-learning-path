# Chapter 2: Operators and Expressions

**What You'll Learn**: How to perform calculations, compare values, combine conditions, and understand operator precedence.

## Table of Contents

1. [Introduction: What are Operators?](#introduction-what-are-operators)
2. [Arithmetic Operators (Math Operations)](#1-arithmetic-operators-math-operations)
3. [Assignment Operators](#2-assignment-operators)
4. [Comparison Operators (Relational)](#3-comparison-operators-relational)
5. [Logical Operators (Combining Conditions)](#4-logical-operators-combining-conditions)
6. [Operator Precedence (Order of Operations)](#5-operator-precedence-order-of-operations)
7. [Type Conversion (Casting)](#6-type-conversion-casting)
8. [Complete Example Programs](#complete-example-programs)
9. [Common Mistakes](#common-mistakes)
10. [Practice Problems](#practice-problems)
11. [Key Takeaways](#key-takeaways)
12. [What's Next?](#whats-next)

---

## Introduction: What are Operators?

**Operators** are symbols that tell the computer to perform specific operations on values.

Think of operators like **math symbols** you learned in school:
- `+` means "add these numbers"
- `-` means "subtract

"
- `>` means "is greater than"

In programming, we have many more operators to work with!

---

## 1. Arithmetic Operators (Math Operations)

These perform basic math calculations:

| Operator | Name | Example | Result |
|----------|------|---------|--------|
| `+` | Addition | `5 + 3` | `8` |
| `-` | Subtraction | `5 - 3` | `2` |
| `*` | Multiplication | `5 * 3` | `15` |
| `/` | Division | `6 / 3` | `2` |
| `%` | Modulo (Remainder) | `7 % 3` | `1` |

### Addition and Subtraction

```cpp
int a = 10;
int b = 5;

int sum = a + b;        // 15
int difference = a - b;  // 5

cout << "Sum: " << sum << endl;
cout << "Difference: " << difference << endl;
```

**Simple!** Works just like normal math.

---

### Multiplication

```cpp
int length = 5;
int width = 3;
int area = length * width;  // 15

cout << "Area: " << area << endl;
```

**Note**: We use `*` (asterisk) instead of `×` because keyboards don't have ×.

---

### Division - The Tricky One!

Division behaves differently based on types:

#### Integer Division (Both numbers are integers)

```cpp
int a = 7;
int b = 2;
int result = a / b;

cout << result;  // Prints: 3 (NOT 3.5!)
```

**Why?** When you divide two integers, C++ gives you an **integer result** and **throws away the decimal part**.

Think of it like: "How many complete times does 2 fit into 7?"
- 2 fits into 7 exactly **3 complete times** (with 1 left over)

```
7 ÷ 2 = 3 remainder 1
        ↑
    Only this part is kept!
```

#### Decimal Division (At least one number is a decimal)

```cpp
double a = 7.0;  // Note: decimal
double b = 2.0;
double result = a / b;

cout << result;  // Prints: 3.5
```

**or**:

```cpp
int a = 7;
int b = 2;
double result = (double)a / b;  // Convert to double first

cout << result;  // Prints: 3.5
```

---

### Modulo % - The Remainder Operator

The `%` operator gives you the **remainder** after division:

```cpp
cout << 7 % 2;   // 1 (7 ÷ 2 = 3 remainder 1)
cout << 10 % 3;  // 1 (10 ÷ 3 = 3 remainder 1)
cout << 15 % 4;  // 3 (15 ÷ 4 = 3 remainder 3)
cout << 8 % 2;   // 0 (8 ÷ 2 = 4 remainder 0 - evenly divisible!)
```

**Visual**:

```
7 ÷ 2:
  ___
2 | 7
  - 6    (2 × 3 = 6)
  ---
    1    ← This is 7 % 2
```

**Common Uses**:

**Check if number is even or odd**:
```cpp
if (num % 2 == 0) {
    cout << "Even";
} else {
    cout << "Odd";
}
```

**Get last digit of a number**:
```cpp
int num = 12345;
int lastDigit = num % 10;  // 5
```

**Cycle through values** (like clock):
```cpp
int hour = 25;
int displayHour = hour % 24;  // 1 (wraps around after 24)
```

---

## 2. Assignment Operators

### Basic Assignment `=`

```cpp
int x = 10;  // Assign 10 to x
```

**Read it as**: "x gets the value 10" (not "x equals 10")

```
Memory:
┌─────────────┐
│ Variable: x │
│ Value: 10   │
└─────────────┘
```

### Compound Assignment Operators

These are shortcuts for common operations:

| Operator | Long Form | Short Form | Example |
|----------|-----------|------------|---------|
| `+=` | `x = x + 5` | `x += 5` | Add and assign |
| `-=` | `x = x - 3` | `x -= 3` | Subtract and assign |
| `*=` | `x = x * 2` | `x *= 2` | Multiply and assign |
| `/=` | `x = x / 4` | `x /= 4` | Divide and assign |
| `%=` | `x = x % 3` | `x %= 3` | Modulo and assign |

**Example**:

```cpp
int score = 100;

score += 50;   // score = score + 50; → score is now 150
score -= 20;   // score = score - 20; → score is now 130
score *= 2;    // score = score * 2;  → score is now 260
score /= 4;    // score = score / 4;  → score is now 65
```

---

### Increment and Decrement

**Super common** shortcuts for adding or subtracting 1:

| Operator | Meaning | Example |
|----------|---------|---------|
| `++` | Add 1 | `x++` or `++x` |
| `--` | Subtract 1 | `x--` or `--x` |

```cpp
int count = 5;

count++;  // count = count + 1; → count is now 6
count--;  // count = count - 1; → count is now 5
```

**Difference between `x++` and `++x`**:

```cpp
int x = 5;
int a = x++;  // a = 5, then x = 6 (use old value, then increment)
int b = ++x;  // x = 7, then b = 7 (increment first, then use new value)
```

**Visual**:

```
x++ (Post-increment):
1. Copy x (5)
2. Return the copy (5)
3. Increment x (x becomes 6)

++x (Pre-increment):
1. Increment x (x becomes 6)
2. Return x (6)
```

**When does it matter?** Only when you're using the value immediately:

```cpp
int x = 5;
cout << x++;  // Prints: 5 (old value), x is now 6
cout << x;    // Prints: 6

int y = 5;
cout << ++y;  // Prints: 6 (new value), y is now 6
```

**In most cases**: Just use `x++` (more common)

---

## 3. Comparison Operators (Relational)

These compare two values and return `true` or `false`:

| Operator | Meaning | Example | Result |
|----------|---------|---------|--------|
| `==` | Equal to | `5 == 5` | `true` |
| `!=` | Not equal to | `5 != 3` | `true` |
| `>` | Greater than | `5 > 3` | `true` |
| `<` | Less than | `5 < 3` | `false` |
| `>=` | Greater than or equal | `5 >= 5` | `true` |
| `<=` | Less than or equal | `5 <= 3` | `false` |

**Examples**:

```cpp
int age = 18;

bool isAdult = (age >= 18);        // true
bool isChild = (age < 13);         // false
bool isTeenager = (age >= 13 && age < 20);  // true
```

### Common Mistake: Using `=` Instead of `==`

```cpp
// WRONG:
if (x = 5) {  // This ASSIGNS 5 to x, doesn't compare!
    cout << "x is 5";
}

// CORRECT:
if (x == 5) {  // This COMPARES x with 5
    cout << "x is 5";
}
```

**Remember**:
- `=` is **assignment** ("make x equal to 5")
- `==` is **comparison** ("is x equal to 5?")

---

## 4. Logical Operators (Combining Conditions)

These combine multiple true/false conditions:

| Operator | Name | Meaning | Example |
|----------|------|---------|---------|
| `&&` | AND | Both must be true | `(a > 5 && b < 10)` |
| `\|\|` | OR | At least one must be true | `(a > 5 \|\| b < 10)` |
| `!` | NOT | Inverts true/false | `!(a > 5)` |

### AND Operator `&&`

**Both conditions must be true** for the result to be true:

```cpp
int age = 20;
bool hasLicense = true;

// Can drive if: age >= 18 AND has license
if (age >= 18 && hasLicense) {
    cout << "You can drive!";
}
```

**Truth Table for AND**:

| Condition A | Condition B | A && B |
|-------------|-------------|--------|
| true | true | **true** ✓ |
| true | false | false |
| false | true | false |
| false | false | false |

**Think of it like**:
- Both doors must be open to pass through
- Need BOTH key AND password to access

---

### OR Operator `||`

**At least one condition must be true** for the result to be true:

```cpp
int age = 70;
bool isStudent = false;

// Get discount if: senior (age >= 65) OR student
if (age >= 65 || isStudent) {
    cout << "You get a discount!";
}
```

**Truth Table for OR**:

| Condition A | Condition B | A \|\| B |
|-------------|-------------|----------|
| true | true | **true** ✓ |
| true | false | **true** ✓ |
| false | true | **true** ✓ |
| false | false | false |

**Think of it like**:
- You can enter through door A OR door B
- Accepted if you have degree OR experience

---

### NOT Operator `!`

**Inverts** the true/false value:

```cpp
bool isRaining = false;

if (!isRaining) {  // If NOT raining
    cout << "Let's go outside!";
}
```

**Truth Table for NOT**:

| Condition | !Condition |
|-----------|------------|
| true | **false** |
| false | **true** |

---

### Combining Multiple Conditions

You can combine operators with parentheses:

```cpp
int age = 25;
bool hasLicense = true;
bool hasCar = false;

// Can deliver if: adult AND (has license OR has car)
if (age >= 18 && (hasLicense || hasCar)) {
    cout << "You can be a delivery driver!";
}
```

**Order of evaluation**:
1. Parentheses first: `(hasLicense || hasCar)` → true
2. Then AND: `age >= 18 && true` → true

---

## 5. Operator Precedence (Order of Operations)

Just like math (PEMDAS/BODMAS), C++ has order of operations:

**Priority (High to Low)**:

1. **Parentheses** `()`
2. **Unary** `!`, `++`, `--`
3. **Multiplication/Division** `*`, `/`, `%`
4. **Addition/Subtraction** `+`, `-`
5. **Comparison** `<`, `>`, `<=`, `>=`
6. **Equality** `==`, `!=`
7. **Logical AND** `&&`
8. **Logical OR** `||`
9. **Assignment** `=`, `+=`, etc.

**Examples**:

```cpp
int result = 5 + 3 * 2;
// 1. 3 * 2 = 6 (multiplication first)
// 2. 5 + 6 = 11
// result = 11

int result2 = (5 + 3) * 2;
// 1. (5 + 3) = 8 (parentheses first)
// 2. 8 * 2 = 16
// result2 = 16
```

**Best Practice**: **Use parentheses** to make your intention clear!

```cpp
// Unclear:
if (a > 5 && b < 10 || c == 3)

// Clear:
if ((a > 5 && b < 10) || (c == 3))
```

---

## 6. Type Conversion (Casting)

Sometimes you need to convert between types:

### Implicit Conversion (Automatic)

```cpp
int x = 5;
double y = x;  // int automatically converts to double
// y = 5.0
```

### Explicit Conversion (Manual)

```cpp
double pi = 3.14159;
int wholePart = (int)pi;  // Cast to int
// wholePart = 3 (decimal part removed)

int a = 7;
int b = 2;
double result = (double)a / b;  // Cast a to double
// result = 3.5
```

**Why explicit?** Because you might lose information:

```cpp
double x = 3.9;
int y = (int)x;  // y = 3 (loses 0.9!)
```

---

## Complete Example Programs

### Example 1: Calculator

```cpp
#include <iostream>
using namespace std;

int main() {
    double num1, num2;
    char operation;

    cout << "Enter first number: ";
    cin >> num1;

    cout << "Enter operation (+, -, *, /): ";
    cin >> operation;

    cout << "Enter second number: ";
    cin >> num2;

    double result;

    if (operation == '+') {
        result = num1 + num2;
    } else if (operation == '-') {
        result = num1 - num2;
    } else if (operation == '*') {
        result = num1 * num2;
    } else if (operation == '/') {
        if (num2 != 0) {
            result = num1 / num2;
        } else {
            cout << "Error: Division by zero!" << endl;
            return 1;
        }
    } else {
        cout << "Invalid operation!" << endl;
        return 1;
    }

    cout << "Result: " << result << endl;

    return 0;
}
```

### Example 2: Even or Odd

```cpp
#include <iostream>
using namespace std;

int main() {
    int num;

    cout << "Enter a number: ";
    cin >> num;

    if (num % 2 == 0) {
        cout << num << " is EVEN" << endl;
    } else {
        cout << num << " is ODD" << endl;
    }

    return 0;
}
```

### Example 3: Age Category

```cpp
#include <iostream>
using namespace std;

int main() {
    int age;

    cout << "Enter your age: ";
    cin >> age;

    if (age < 0) {
        cout << "Invalid age!" << endl;
    } else if (age < 13) {
        cout << "You are a CHILD" << endl;
    } else if (age < 18) {
        cout << "You are a TEENAGER" << endl;
    } else if (age < 60) {
        cout << "You are an ADULT" << endl;
    } else {
        cout << "You are a SENIOR" << endl;
    }

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Forgetting Integer Division

```cpp
int result = 7 / 2;  // result = 3 (not 3.5!)

// Fix:
double result = 7.0 / 2.0;  // result = 3.5
```

### Mistake 2: Using = Instead of ==

```cpp
if (x = 5) {  // WRONG! Assigns 5 to x
    ...
}

if (x == 5) {  // CORRECT! Compares x with 5
    ...
}
```

### Mistake 3: Confusing && and ||

```cpp
// Want: "Between 10 and 20"
if (x > 10 || x < 20) {  // WRONG! Always true

if (x > 10 && x < 20) {  // CORRECT!
```

### Mistake 4: Not Using Parentheses

```cpp
if (a > 5 && b < 10 || c == 3)  // Unclear!

if ((a > 5 && b < 10) || (c == 3))  // Clear!
```

---

## Practice Problems

### Problem 1: Temperature Converter
Write a program that:
- Reads temperature in Celsius
- Converts to Fahrenheit: F = (C × 9/5) + 32
- Converts to Kelvin: K = C + 273.15
- Prints all three values

### Problem 2: Discount Calculator
Write a program that:
- Reads item price
- If price > $100, give 20% discount
- If price > $50, give 10% discount
- Otherwise, no discount
- Print final price

### Problem 3: Leap Year Checker
A year is a leap year if:
- Divisible by 4 AND
- (NOT divisible by 100 OR divisible by 400)

Write a program to check if a year is a leap year.

### Problem 4: Grade Calculator
Read marks (0-100) and print grade:
- 90-100: A
- 80-89: B
- 70-79: C
- 60-69: D
- Below 60: F

### Problem 5: Compound Interest
Calculate compound interest:
- Formula: A = P(1 + r/n)^(nt)
- P = principal, r = rate, n = compounds per year, t = years
- Read all values and calculate final amount

---

## Key Takeaways

✅ **Arithmetic operators**: +, -, *, /, % (watch out for integer division!)
✅ **Assignment operators**: =, +=, -=, *=, /=, ++, --
✅ **Comparison operators**: ==, !=, <, >, <=, >=
✅ **Logical operators**: && (AND), || (OR), ! (NOT)
✅ **Operator precedence**: Use parentheses to be clear!
✅ **Type conversion**: Be careful when converting types
✅ **Common mistake**: Don't confuse = (assign) with == (compare)

---

## What's Next?

In **[Chapter 3](chapter-03-control-structures.md)**, you'll learn about control structures - how to make decisions with if/else and switch statements!

---

**Chapter Progress**: ✅ Chapters 1-2 Complete | 📖 Next: Chapter 3
