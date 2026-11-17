# Python Programming Basics — Study Notes

> Source: a Hinglish Python series on YouTube (8 videos). The speech-to-text was rough, so I fixed the obvious errors.
> Order below follows the series: Variables → Input → Type conversion → Literals → Operators → Indentation → If/else → While loops.
> Not included: the "Keywords and Identifiers" video (it was mentioned as next, but was not in the files).

---

## Table of Contents
1. [Variables](#1-variables)
2. [User input](#2-user-input)
3. [Type conversion](#3-type-conversion)
4. [Literals](#4-literals)
5. [Operators](#5-operators)
6. [Indentation](#6-indentation)
7. [If / elif / else](#7-if--elif--else)
8. [Loops (while loop)](#8-loops-while-loop)
9. [Quick cheat sheet](#9-quick-cheat-sheet)

---

## 1. Variables

### What is a variable?
- A **container** that holds a value you don't know yet (it will be filled in the future).
- Example: a website shows a user's name on the profile page. You don't write one page per user. You write one page and put a **variable** where the name goes.
- Example: an "add two numbers" program. You don't know the numbers in advance, so you use two variables.

### Creating variables in Python
```python
name = "Shubham"
a = 5
```
- **No declaration needed.** You don't say "I will use this variable" beforehand.
- **No data type needed.** Python figures it out by itself.

### Dynamic typing
- Python **decides the data type automatically** from the value you assign.
- Opposite: **static typing** (C, C++, Java), where you must write the type (`int a = 5;`).
- PHP also has dynamic typing.

### Dynamic binding
- One variable can hold **different data types at different times**.
```python
name = "Hello World"
name = 4        # now an int
name = True     # now a bool
```
- Opposite: **static binding** (C, C++, Java). Once a variable is `int`, it stays `int` for the whole program.
- Tip from the video: C/Java programmers may worry about errors, but in practice it is rarely a problem.

### Useful ways to create variables
```python
# 1. Multiple variables on one line (separated by ;)
a = 5; b = 6; c = 7

# 2. Multiple assignment (comma style)
a, b, c = 5, 6, 7

# 3. Same value for many variables
a = b = c = 6
```

### Gotcha
- A string value needs quotes. Writing `name = Hello` (no quotes) gives an error because Python thinks `Hello` is a variable name that doesn't exist.

---

## 2. User input

### Two kinds of software
| Type | Meaning | Examples |
|---|---|---|
| **Static** | Software talks to you; you can't talk back | Alarm clock, calendar, college website, blogs |
| **Dynamic** | You and the software talk both ways | YouTube, WhatsApp, food apps |

- Almost all modern apps are **dynamic**. To build them you must **take input from the user**.
- Every language has a way to do this (C `scanf`, C++ `cin`, Java `System.in`). Python has `input()`.

### The `input()` function
```python
name = input("Enter your name: ")
```
- The text inside is the **prompt**. Always add one, or the user won't know what to type.
- `input()` works like `print()`: a built-in function, used everywhere.

### Very important rule
> **`input()` always returns a string**, even if the user types a number.

```python
first_num = input("Enter the first number: ")    # user types 56
second_num = input("Enter the second number: ")  # user types 7
result = first_num + second_num
print(result)        # 567   (NOT 63!)
```
- Both values are strings, so `+` just **joins** them (concatenation) instead of adding.

### Why does Python return a string?
- A string is the **universal format**. A number can be stored as a string, and so can text.
- The opposite doesn't work: you cannot store "Kolkata" as an integer.

### `type()` function
```python
type(4)        # <class 'int'>
type(4.5)      # <class 'float'>
type("hi")     # <class 'str'>
type(True)     # <class 'bool'>
type([1, 2])   # <class 'list'>
type(first_num)  # <class 'str'>   <- proves input gives a string
```

**Fix:** convert the strings to integers (next section).

---

## 3. Type conversion

- Changing one data type into another. It only works if the value is **convertible**.
  - `int("45")` works. `int("Kolkata")` gives an error.

### Two types
| Type | Meaning |
|---|---|
| **Implicit** | Python converts **automatically** |
| **Explicit** | **You** tell Python to convert, using a function |

### Implicit examples
```python
4 + 5.5        # 9.5     (int + float -> float)
5 + 6 + 7.2    # 18.2
4.5 + 5        # 9.5
# int + complex -> complex, float + complex -> complex
```
- Python is smart enough to do these without help.

### When Python can't guess (explicit)
```python
"4" + "5"    # '45'  (string join, not math)
```
Here you must convert yourself.

### Conversion functions
| Function | Converts to | Example |
|---|---|---|
| `int()` | integer | `int(4.5)` → `4` (cuts the decimal part) |
| `float()` | float | `float(4)` → `4.0` |
| `bool()` | boolean | `bool(...)` |
| `complex()` | complex | `complex(4, 2)` → `(4+2j)` |
| `str()` | string | `str(4)` → `'4'` |
| `list()` | list | `list("Hello")` → `['H','e','l','l','o']` |
| `tuple()`, `set()`, `dict()` | same idea | |

- `int("Kolkata")` → **ValueError** (not convertible).
- `int("4.5")` also fails; convert to `float` first.

### Not permanent
```python
x = 4.5
int(x)      # 4
print(x)    # 4.5  <- original is unchanged
```
- Conversion creates a **new value**. The original variable stays the same unless you assign the result.

### Fixing the add-two-numbers program
```python
first_num = int(input("Enter the first number: "))
second_num = int(input("Enter the second number: "))
print(first_num + second_num)    # 63
```
**Best practice:** convert **at the moment you take the input**. Then you don't need to convert again every time you do maths.

---

## 4. Literals

- A **literal** is a raw value you give to a variable: `a = 10` (`10` is the literal).
- Four categories: **numeric, string, boolean, special**.

### Numeric literals
```python
a = 10                 # decimal (default)
b = 0b1010             # binary  -> 10
c = 0o12               # octal   -> 10
d = 0xA                # hexadecimal -> 10
```
- Binary/octal/hex are mostly used in hardware work (e.g. Raspberry Pi boards), not much in web or app work.
- Printing always shows the decimal value.

**Float / scientific notation** (very big or very small numbers):
```python
x = 1.5e2     # 150.0   (1.5 x 10^2)
y = 1.5e-2    # 0.015   (1.5 x 10^-2)
```
Useful for space or biology/lab software.

**Complex numbers:**
```python
z = 3 + 4j
z.real    # 3.0
z.imag    # 4.0
```

### String literals
```python
'Hello'        # single quotes (most common)
"Hello"        # double quotes (same thing)
'''
Multi-line
text
'''            # triple quotes for many lines
```
- Python has **no separate "char" type**. A single character is also a string.
- **Unicode / emojis:** use `\u` followed by the code, e.g. `"\u263A"` for a smiley.
- **Raw strings:** put `r` before the string so backslashes are not treated as special.
  ```python
  path = r"C:\new\folder"
  ```

### Boolean literals
- `True` and `False`.
- Python treats them as **1 and 0** in maths: `True + 4` → `5`, `False + 4` → `4`.

### Special literal: `None`
```python
x = None
print(x)    # None
```
- Means "nothing / no value".
- **Why use it:** Python has no variable declaration. If a variable must exist but you have no value yet, give it `None`.
  ```python
  print(a)   # NameError: name 'a' is not defined
  ```
  A variable can't exist without a value, so `x = None` is the way to "declare" it. (Using `0` might break your logic.)

---

## 5. Operators

Operators do operations on variables and values. Python has 7 groups.

### 1) Arithmetic
```python
x, y = 5, 2
x + y     # 7
x - y     # 3
x * y     # 10
x / y     # 2.5    (true division, always float)
x % y     # 1      (remainder, "modulus")
x ** y    # 25     (power)
x // y    # 2      (floor/integer division: cuts the decimal part)
```

### 2) Comparison (relational)
`>`, `<`, `>=`, `<=`, `==` (equal), `!=` (not equal). They return `True` or `False`.

### 3) Logical
| Operator | Meaning |
|---|---|
| `and` | True only if **both** are true |
| `or` | True if **at least one** is true |
| `not` | Flips the result |

- Note from the video: `and`/`or` can return an actual value, not just True/False (e.g. `2 and 4` → `4`).

### 4) Bitwise
Work on the **binary** form of numbers.
```python
x, y = 5, 4
x & y     # 4    AND  (101 & 100 = 100)
x | y     # 5    OR
x ^ y     # 1    XOR
~x        # complement
x >> 1    # right shift
x << 1    # left shift
```
- Used mostly in image processing, hardware, low-level work. Not common in websites.

### 5) Assignment
```python
a = 5
a += 3     # a = a + 3  -> 8
a -= 3     # a = a - 3
a *= 3     # a = a * 3
```
- Works with any arithmetic operator.
- **Python has no `++` or `--`.** Use `a += 1` or `a = a + 1`.

### 6) Identity: `is`, `is not`
- Checks whether two variables point to the **same memory location** (the same object), not just the same value.
```python
a = 3
b = 3
a is b          # True  (small ints share the object)

a = [1, 2, 3]
b = [1, 2, 3]
a == b          # True  (same values)
a is b          # False (different objects in memory)
```
- Same value does **not** always mean same memory location. For strings and numbers, Python may or may not reuse the object, so don't rely on `is` to compare values. Use `==` for values.

### 7) Membership: `in`, `not in`
- Checks whether something is **inside** something else.
```python
x = "Delhi"
"D" in x           # True
"z" in x           # False
1 in [1, 2, 3]     # True
"z" not in x       # True
```
- Works with strings, lists, tuples, sets and dictionaries. Many other languages need a loop to do this.

---

## 6. Indentation

### The problem it solves
- In C/Java, a block of code is marked with `{ }` and each line ends with `;`.
- **Python has neither.** It uses **indentation** (the space at the start of a line) to show which code belongs to which block.

```python
if name == "xyz":
    print("line 1")     # inside the if
    print("line 2")     # inside the if
else:
    print("line 3")     # inside the else
```
- An `if` block runs only when its condition is True, and the `else` block runs only when it is False. They never run together.

### How to indent
- Press **Tab** once per level. (Don't press spacebar many times, as the video warns.)
- After a line ending with `:`, press Enter. The editor usually indents automatically.
- To leave a block, press **Backspace/Shift+Tab** to go back.

### Rules
- Indentation is **mandatory**. Wrong indentation → `IndentationError` ("unexpected indent" / "block misplaced").
- Nested blocks need **one more Tab** each:
```python
if a == 2:
    print("level 1")
    if b == 3:
        print("level 2")    # two tabs
```
- Every line of the same block must have the **same** indentation.
- Mistake to avoid: putting a statement at the wrong level changes when it runs (e.g. it may run even when the condition is False).

### Why Python does this
- The creators believe in **readability**. Indentation makes it clear what code is inside what, which also helps debugging.

---

## 7. If / elif / else

### Why we need it
- A program normally runs top to bottom. Sometimes the logic must **branch**: do one thing in one case, another thing in another. Example: login success vs. login failure.

### Syntax
```python
if condition:
    # runs if condition is True
elif other_condition:
    # runs if the first was False and this is True
else:
    # runs if none of the above were True
```
- No brackets around the condition, and a **colon `:`** at the end of the line.
- `elif` is Python's short for "else if". It is the **middle** case; you can have **many** `elif` blocks.
- Order: first case is `if`, last case is `else`, in-between cases are `elif`.
- `else` doesn't need a condition.

### Example 1: simple login
```python
email = input("Enter your email: ")
password = input("Enter your password: ")

if email == "xyz@gmail.com" and password == "1234":
    print("Welcome")
else:
    print("Incorrect credentials")
```
- `==` compares values. `and` means **both** must be true.

### Example 2: second chance (using `elif` and a nested `if`)
Rule: if the email is right but the password is wrong, give one more try.

```python
email = input("Enter your email: ")
password = input("Enter your password: ")

if email == "xyz@gmail.com" and password == "1234":
    print("Welcome")

elif email == "xyz@gmail.com" and not (password == "1234"):
    print("Password incorrect")
    password = input("Enter your password again: ")
    if password == "1234":      # nested if (extra indentation)
        print("Welcome")
    else:
        print("Incorrect")

else:
    print("Incorrect credentials")
```
- You can put `if/elif/else` **inside** another `if/elif/else` (this is called nesting), as long as the indentation is correct.

### Extra check example (email format)
```python
if "@" not in email:
    print("Email is wrong")
else:
    # continue with password checks
    ...
```
- Putting this check first avoids asking for the password if the email is obviously invalid.

---

## 8. Loops (while loop)

### What is a loop?
- Used to do the **same work again and again**. You put the work inside a loop.
- Also called an **iteration / control block**.

### Where loops are used (real-world example)
- A shopping page (like Flipkart) shows many products. Each product box (image + description + price) is a **container**.
  - Each container has **different data** but the **same layout**.
  - Developers do **not** create 24 containers by hand. They create **one container** and put it in a **loop**.
  - The loop runs **once per product in the database**, printing the container each time.
- Same idea in Zomato (restaurants), OLX, Uber, news feeds, etc.
- Interview tip: this practical example shows you understand where loops are used.

### Loops in Python
- Python has only **two** loops: **`while`** and **`for`**.
  (Other languages also have `do-while`, `for-each`, etc.)
- This video: `while`. Next video: `for`.

### while syntax
```python
while condition:
    # code (indented)
```
- No brackets around the condition. Use a colon. Indent the body.
- The body keeps running **as long as the condition is True**.

### Example: multiplication table
```python
number = int(input("Enter the number: "))   # convert input to int
i = 1                                       # start value

while i < 11:                               # stop when i reaches 11
    print(number, "x", i, "=", number * i)
    i = i + 1                               # increase i, or the loop never ends!
```
Output (for 11): `11 x 1 = 11`, `11 x 2 = 22`, … `11 x 10 = 110`.

### How it runs (step by step)
1. `i = 1`. Is `1 < 11`? Yes → print `number * 1`, then `i` becomes 2.
2. `i = 2`. `2 < 11`? Yes → print `number * 2`, then `i` becomes 3.
3. … continues until `i = 10` is printed and `i` becomes 11.
4. `11 < 11`? **False** → the loop ends.

### Important
- You must **change the variable** (here `i = i + 1`) so the condition eventually becomes False. If you forget, you get an **infinite loop**.
- You can make the output prettier with formatting, e.g. `print(f"{number} x {i} = {number * i}")`.

---

## 9. Quick cheat sheet

### Core ideas
| Topic | Key point |
|---|---|
| Variable | Container for a value; no declaration, no type needed |
| Dynamic typing | Python picks the type automatically |
| Dynamic binding | A variable can change type any time |
| `input()` | **Always returns a string** |
| `type(x)` | Shows the data type |
| Implicit conversion | Python does it (int + float → float) |
| Explicit conversion | You do it: `int()`, `float()`, `str()`, `list()` … |
| Conversion is not permanent | Original value is unchanged |
| `None` | "No value"; used to "declare" an empty variable |
| Indentation | Required; marks blocks (no `{ }` or `;`) |
| `elif` | Python's "else if" |
| Loops | Only `while` and `for` |

### Operator groups
```
Arithmetic:   + - * / % ** //
Comparison:   == != > < >= <=
Logical:      and  or  not
Bitwise:      &  |  ^  ~  >>  <<
Assignment:   =  +=  -=  *= ...   (no ++ / --)
Identity:     is  is not
Membership:   in  not in
```

### Common mistakes
- Forgetting to convert `input()` to `int`/`float` before doing maths.
- Wrong or mixed indentation (use Tab; keep the same level in a block).
- Forgetting the colon `:` after `if`, `elif`, `else`, `while`.
- Forgetting to update the loop variable → infinite loop.
- Using `is` to compare values (use `==`).
- Using `++` (not valid in Python).
- Writing text without quotes (Python thinks it's a variable name).

---

*End of notes.*
