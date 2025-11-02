# Chapter 3: Control Structures (Making Decisions)

**What You'll Learn**: How to make your program take different paths based on conditions using if/else and switch statements.

## Table of Contents

1. [Introduction: Why Control Structures?](#introduction-why-control-structures)
2. [The if Statement (Simple Decision)](#1-the-if-statement-simple-decision)
3. [The if-else Statement (Two Choices)](#2-the-if-else-statement-two-choices)
4. [The if-else if-else Ladder (Multiple Choices)](#3-the-if-else-if-else-ladder-multiple-choices)
5. [Nested if Statements (if Inside if)](#4-nested-if-statements-if-inside-if)
6. [The Ternary Operator (Short if-else)](#5-the-ternary-operator-short-if-else)
7. [The switch Statement (Multiple Exact Values)](#6-the-switch-statement-multiple-exact-values)
8. [When to Use What?](#7-when-to-use-what)
9. [Complete Example Programs](#complete-example-programs)
10. [Common Mistakes](#common-mistakes)
11. [Practice Problems](#practice-problems)
12. [Key Takeaways](#key-takeaways)
13. [What's Next?](#whats-next)

---

## Introduction: Why Control Structures?

Until now, our programs run **straight through** from top to bottom:

```
Start
  ↓
Step 1
  ↓
Step 2
  ↓
Step 3
  ↓
End
```

But real programs need to **make decisions**:

```
Start
  ↓
Is it raining?
  ├─ YES → Take umbrella
  └─ NO  → Don't take umbrella
  ↓
Continue...
```

**Control structures** let your program choose different paths based on conditions!

---

## 1. The if Statement (Simple Decision)

**Syntax**:

```cpp
if (condition) {
    // Code runs ONLY if condition is true
}
```

**Think of it like**:
```
IF it's raining:
    Take umbrella
```

### Example 1: Check if Adult

```cpp
int age = 20;

if (age >= 18) {
    cout << "You are an adult!" << endl;
}
```

**What happens**:
1. Check: Is `age >= 18`?
2. Yes (20 >= 18 is true)
3. Execute the code inside `{}`
4. Print "You are an adult!"

**Visual Flow**:

```
Start
  ↓
age >= 18?
  ├─ YES → Print "You are an adult!"
  ↓
Continue...
```

---

### Example 2: Check if Passing

```cpp
int marks = 75;

if (marks >= 50) {
    cout << "You passed!" << endl;
}

cout << "End of program" << endl;
```

**Output**:
```
You passed!
End of program
```

**If marks were 40**:
```
End of program
```
(The if block is skipped)

---

## 2. The if-else Statement (Two Choices)

**Syntax**:

```cpp
if (condition) {
    // Code if condition is TRUE
} else {
    // Code if condition is FALSE
}
```

**Think of it like**:
```
IF it's raining:
    Take umbrella
ELSE:
    Leave umbrella at home
```

---

### Example 1: Even or Odd

```cpp
int num = 7;

if (num % 2 == 0) {
    cout << num << " is EVEN" << endl;
} else {
    cout << num << " is ODD" << endl;
}
```

**Visual Flow**:

```
Start
  ↓
num % 2 == 0?
  ├─ YES (TRUE)  → Print "EVEN"
  └─ NO (FALSE)  → Print "ODD"
  ↓
Continue...
```

**Output**: `7 is ODD`

---

### Example 2: Pass or Fail

```cpp
int marks;
cout << "Enter your marks: ";
cin >> marks;

if (marks >= 50) {
    cout << "You PASSED!" << endl;
} else {
    cout << "You FAILED" << endl;
}
```

**Test Runs**:

Input: 75 → Output: `You PASSED!`
Input: 40 → Output: `You FAILED`

---

## 3. The if-else if-else Ladder (Multiple Choices)

**Syntax**:

```cpp
if (condition1) {
    // Code if condition1 is TRUE
} else if (condition2) {
    // Code if condition1 is FALSE and condition2 is TRUE
} else if (condition3) {
    // Code if condition1 and condition2 are FALSE, condition3 is TRUE
} else {
    // Code if ALL conditions are FALSE
}
```

**Think of it like**:
```
IF temperature > 30:
    "It's HOT"
ELSE IF temperature > 20:
    "It's WARM"
ELSE IF temperature > 10:
    "It's COOL"
ELSE:
    "It's COLD"
```

---

### Example 1: Grade System

```cpp
int marks;
cout << "Enter marks: ";
cin >> marks;

if (marks >= 90) {
    cout << "Grade: A (Excellent!)" << endl;
} else if (marks >= 80) {
    cout << "Grade: B (Good)" << endl;
} else if (marks >= 70) {
    cout << "Grade: C (Satisfactory)" << endl;
} else if (marks >= 60) {
    cout << "Grade: D (Passing)" << endl;
} else {
    cout << "Grade: F (Failing)" << endl;
}
```

**Visual Flow**:

```
marks >= 90?
  ├─ YES → Grade A
  └─ NO
      ↓
   marks >= 80?
      ├─ YES → Grade B
      └─ NO
          ↓
       marks >= 70?
          ├─ YES → Grade C
          └─ NO
              ↓
           marks >= 60?
              ├─ YES → Grade D
              └─ NO → Grade F
```

**Test Runs**:

- Input: 95 → `Grade: A (Excellent!)`
- Input: 75 → `Grade: C (Satisfactory)`
- Input: 45 → `Grade: F (Failing)`

---

### Important: Order Matters!

```cpp
// WRONG ORDER:
if (marks >= 60) {           // This will catch 90!
    cout << "Grade: D" << endl;
} else if (marks >= 90) {    // Never reaches here if marks = 90
    cout << "Grade: A" << endl;
}

// CORRECT ORDER (most restrictive first):
if (marks >= 90) {
    cout << "Grade: A" << endl;
} else if (marks >= 60) {
    cout << "Grade: D" << endl;
}
```

**Rule**: Start with the **most specific/restrictive** condition!

---

## 4. Nested if Statements (if Inside if)

You can put if statements **inside** other if statements:

```cpp
if (outer_condition) {
    // Some code
    if (inner_condition) {
        // Code if BOTH outer AND inner conditions are true
    }
}
```

---

### Example: Eligibility to Drive

```cpp
int age;
bool hasLicense;

cout << "Enter age: ";
cin >> age;
cout << "Have driver's license? (1=yes, 0=no): ";
cin >> hasLicense;

if (age >= 18) {
    // Only check license if age is OK
    if (hasLicense) {
        cout << "You CAN drive!" << endl;
    } else {
        cout << "You need to get a license first" << endl;
    }
} else {
    cout << "You are too young to drive" << endl;
}
```

**Visual Flow**:

```
age >= 18?
  ├─ YES → hasLicense?
  │          ├─ YES → "You CAN drive!"
  │          └─ NO  → "Get a license"
  └─ NO  → "Too young"
```

**Test Runs**:

- age=20, license=1 → `You CAN drive!`
- age=20, license=0 → `You need to get a license first`
- age=15, license=1 → `You are too young to drive`

---

### Can Be Rewritten with && (Cleaner):

```cpp
if (age >= 18 && hasLicense) {
    cout << "You CAN drive!" << endl;
} else if (age >= 18 && !hasLicense) {
    cout << "You need to get a license first" << endl;
} else {
    cout << "You are too young to drive" << endl;
}
```

**Both work!** Choose what's clearer for you.

---

## 5. The Ternary Operator (Short if-else)

For simple if-else, you can use a **shortcut**:

**Syntax**:

```cpp
condition ? value_if_true : value_if_false
```

**Think of it as**: "If condition, then this, otherwise that"

---

### Example 1: Find Maximum

```cpp
int a = 10, b = 20;
int max = (a > b) ? a : b;

cout << "Maximum: " << max << endl;  // 20
```

**Expanded form**:

```cpp
int max;
if (a > b) {
    max = a;
} else {
    max = b;
}
```

The ternary is shorter!

---

### Example 2: Even or Odd

```cpp
int num = 7;
string result = (num % 2 == 0) ? "even" : "odd";

cout << num << " is " << result << endl;  // 7 is odd
```

---

### Example 3: Nested Ternary (Gets Messy!)

```cpp
int num = 0;
string result = (num > 0) ? "positive" : (num < 0) ? "negative" : "zero";

cout << "Number is " << result << endl;
```

**Warning**: Too many nested ternaries become **unreadable**. Use regular if-else for complex logic!

---

## 6. The switch Statement (Multiple Exact Values)

When you're checking **one variable** against **multiple exact values**, use switch:

**Syntax**:

```cpp
switch (variable) {
    case value1:
        // Code if variable == value1
        break;
    case value2:
        // Code if variable == value2
        break;
    case value3:
        // Code if variable == value3
        break;
    default:
        // Code if no case matches
}
```

---

### Example 1: Day of Week

```cpp
int day;
cout << "Enter day number (1-7): ";
cin >> day;

switch (day) {
    case 1:
        cout << "Monday" << endl;
        break;
    case 2:
        cout << "Tuesday" << endl;
        break;
    case 3:
        cout << "Wednesday" << endl;
        break;
    case 4:
        cout << "Thursday" << endl;
        break;
    case 5:
        cout << "Friday" << endl;
        break;
    case 6:
        cout << "Saturday" << endl;
        break;
    case 7:
        cout << "Sunday" << endl;
        break;
    default:
        cout << "Invalid day!" << endl;
}
```

**Test Runs**:

- Input: 3 → `Wednesday`
- Input: 7 → `Sunday`
- Input: 9 → `Invalid day!`

---

### Why the `break` Statement?

Without `break`, execution **falls through** to the next case!

```cpp
int x = 2;

switch (x) {
    case 1:
        cout << "One" << endl;
        // No break!
    case 2:
        cout << "Two" << endl;
        // No break!
    case 3:
        cout << "Three" << endl;
        break;
}
```

**Output**:
```
Two
Three
```

It prints **both** "Two" and "Three" because there's no break after case 2!

**Visual Flow**:

```
x == 1? NO
x == 2? YES → Print "Two" → No break → Continue to case 3
              Print "Three" → break → Exit switch
```

---

### Example 2: Calculator with Switch

```cpp
double num1, num2;
char op;

cout << "Enter first number: ";
cin >> num1;
cout << "Enter operator (+, -, *, /): ";
cin >> op;
cout << "Enter second number: ";
cin >> num2;

double result;

switch (op) {
    case '+':
        result = num1 + num2;
        cout << "Result: " << result << endl;
        break;
    case '-':
        result = num1 - num2;
        cout << "Result: " << result << endl;
        break;
    case '*':
        result = num1 * num2;
        cout << "Result: " << result << endl;
        break;
    case '/':
        if (num2 != 0) {
            result = num1 / num2;
            cout << "Result: " << result << endl;
        } else {
            cout << "Error: Division by zero!" << endl;
        }
        break;
    default:
        cout << "Invalid operator!" << endl;
}
```

---

### Example 3: Grouping Cases (Intentional Fall-Through)

```cpp
char grade;
cout << "Enter your grade (A-F): ";
cin >> grade;

switch (grade) {
    case 'A':
    case 'a':
        cout << "Excellent!" << endl;
        break;
    case 'B':
    case 'b':
        cout << "Good!" << endl;
        break;
    case 'C':
    case 'c':
        cout << "Satisfactory" << endl;
        break;
    case 'D':
    case 'd':
        cout << "Passing" << endl;
        break;
    case 'F':
    case 'f':
        cout << "Failing" << endl;
        break;
    default:
        cout << "Invalid grade!" << endl;
}
```

Here, `case 'A'` and `case 'a'` both execute the same code (handles both uppercase and lowercase).

---

## 7. When to Use What?

| Situation | Use |
|-----------|-----|
| Single condition | `if` |
| Two choices | `if-else` |
| Multiple conditions (ranges) | `if-else if-else` |
| Multiple **exact** values | `switch` |
| Simple true/false assignment | Ternary `? :` |

**Examples**:

```cpp
// Range checking → use if-else if
if (age < 13) { ... }
else if (age < 18) { ... }
else { ... }

// Exact values → use switch
switch (day) {
    case 1: ... break;
    case 2: ... break;
}

// Simple assignment → use ternary
int max = (a > b) ? a : b;
```

---

## Complete Example Programs

### Example 1: BMI Calculator

```cpp
#include <iostream>
using namespace std;

int main() {
    double weight, height, bmi;

    cout << "Enter weight (kg): ";
    cin >> weight;
    cout << "Enter height (m): ";
    cin >> height;

    bmi = weight / (height * height);

    cout << "Your BMI: " << bmi << endl;

    if (bmi < 18.5) {
        cout << "Category: Underweight" << endl;
    } else if (bmi < 25) {
        cout << "Category: Normal weight" << endl;
    } else if (bmi < 30) {
        cout << "Category: Overweight" << endl;
    } else {
        cout << "Category: Obese" << endl;
    }

    return 0;
}
```

---

### Example 2: Ticket Price Calculator

```cpp
#include <iostream>
using namespace std;

int main() {
    int age;
    double price;

    cout << "Enter your age: ";
    cin >> age;

    if (age < 3) {
        price = 0;  // Free for toddlers
    } else if (age < 12) {
        price = 5;  // Child price
    } else if (age < 60) {
        price = 10; // Adult price
    } else {
        price = 7;  // Senior discount
    }

    cout << "Ticket price: $" << price << endl;

    return 0;
}
```

---

### Example 3: Menu-Driven Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int choice;

    cout << "=== Menu ===" << endl;
    cout << "1. Say Hello" << endl;
    cout << "2. Show Date" << endl;
    cout << "3. Exit" << endl;
    cout << "Enter choice: ";
    cin >> choice;

    switch (choice) {
        case 1:
            cout << "Hello, World!" << endl;
            break;
        case 2:
            cout << "Today is a great day!" << endl;
            break;
        case 3:
            cout << "Goodbye!" << endl;
            break;
        default:
            cout << "Invalid choice!" << endl;
    }

    return 0;
}
```

---

## Common Mistakes ⚠️

### Mistake 1: Forgetting Braces for Multiple Statements

```cpp
// WRONG: Only first statement is in the if!
if (x > 5)
    cout << "x is big" << endl;
    cout << "This always prints!" << endl;  // Not part of if!

// CORRECT:
if (x > 5) {
    cout << "x is big" << endl;
    cout << "This only prints if x > 5" << endl;
}
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

### Mistake 3: Forgetting break in switch

```cpp
switch (x) {
    case 1:
        cout << "One";
        // Forgot break! Falls through to case 2
    case 2:
        cout << "Two";
        break;
}

// Input: 1 → Output: OneTwo (Wrong!)
```

### Mistake 4: Semicolon After if

```cpp
if (x > 5);  // WRONG! Semicolon ends the if statement
{
    cout << "This always executes!" << endl;
}

// Correct:
if (x > 5) {
    cout << "This only executes if x > 5" << endl;
}
```

---

## Practice Problems

### Problem 1: Triangle Type
Read three sides of a triangle (a, b, c).
- Check if it's a valid triangle (sum of any two sides > third side)
- If valid, determine type:
  - Equilateral: all sides equal
  - Isosceles: two sides equal
  - Scalene: no sides equal

### Problem 2: Electricity Bill
Calculate electricity bill based on units consumed:
- First 50 units: $0.50/unit
- Next 100 units (51-150): $0.75/unit
- Next 100 units (151-250): $1.20/unit
- Above 250: $1.50/unit
- Add 20% surcharge if bill > $400

### Problem 3: Leap Year
Check if a year is a leap year:
- Divisible by 4
- BUT NOT divisible by 100
- UNLESS also divisible by 400

Examples:
- 2000: leap (divisible by 400)
- 1900: not leap (divisible by 100 but not 400)
- 2024: leap (divisible by 4, not by 100)

### Problem 4: Rock, Paper, Scissors
Two players choose rock (1), paper (2), or scissors (3).
Determine winner:
- Rock beats Scissors
- Scissors beats Paper
- Paper beats Rock
- Same choice = Draw

### Problem 5: Quadratic Equation Solver
For ax² + bx + c = 0:
- Calculate discriminant: D = b² - 4ac
- If D > 0: Two real roots
- If D = 0: One real root
- If D < 0: No real roots
Display appropriate message.

---

## Key Takeaways

✅ **if**: Execute code only if condition is true
✅ **if-else**: Choose between two paths
✅ **if-else if-else**: Choose from multiple paths
✅ **Nested if**: if inside another if
✅ **Ternary ?:**: Short form for simple if-else
✅ **switch**: Check exact values (don't forget break!)
✅ **Always use {}**: Even for single statements (safer)
✅ **Watch out**: Don't use = when you mean ==

---

## What's Next?

In **Chapter 4**, you'll learn about loops - how to repeat code multiple times without copy-pasting!

---

**Chapter Progress**: ✅ Chapters 1-3 Complete | 📖 Next: Chapter 4
