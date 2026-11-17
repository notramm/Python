# 100 Days of Python (CampusX) — Video 7: Type Conversion

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: implicit vs explicit type conversion, conversion functions, and fixing the "add two numbers" program.

---

## 1. Recap: the problem

In the last video we found that `input()` always returns a **string**, so `"56" + "76"` gave `"5676"` instead of `132`. The solution is **type conversion**.

---

## 2. What is type conversion?

> A process that converts **one data type into another**.

- The types must be **compatible** (convertible).
  - You can't convert `"Kolkata"` to a number.
  - You can't convert a non-numeric string like `"abc"` to an integer.
- The conversion only happens when it's actually possible.

### Two kinds

| | Implicit | Explicit |
|---|----------|----------|
| Who does it? | **Python**, automatically | **You**, using conversion functions |
| When? | When types can be mixed safely | When Python can't or shouldn't guess |

---

## 3. Implicit type conversion

Python converts automatically, with no action from you:

```python
print(4 + 5.5)          # 9.5       (int + float → float)
print(5 + 6j)           # (5+6j)    
print(4 + (5 + 6j))     # (9+6j)    (int + complex → complex)
print(4.5 + (5 + 6j))   # (9.5+6j)  (float + complex → complex)
```

Python is smart enough to work out how to combine the different types, so you never write the conversion yourself.

---

## 4. Explicit type conversion

Used when Python can't guess, e.g. `"56" + "76"`: both are strings, so Python concatenates them. It cannot know you meant math. You must tell it.

### Conversion functions (one per data type)

```python
int(4.5)          # 4        float → int (decimal part dropped)
int("4")          # 4        numeric string → int
int("4.5")        # ValueError  ← a decimal string can't go straight to int

float(4)          # 4.0
str(4)            # '4'
bool(4)           # True
complex(4, 2)     # (4+2j)
list("hello")     # ['h', 'e', 'l', 'l', 'o']
# also: tuple(), set(), dict()
```

- Every data type has its own conversion function: `int()`, `float()`, `str()`, `bool()`, `complex()`, `list()`, `tuple()`, `set()`, `dict()`.
- You can only convert things that make sense: `int("Kolkata")` is invalid, as expected.

### The conversion is NOT permanent

```python
a = 4.5
print(int(a))    # 4
print(a)         # 4.5  ← original unchanged
```
`int(a)` produces a **new** integer value. The original variable keeps its value (unless you reassign: `a = int(a)`).

> The instructor distinguishes "type casting" (C/Java; changes the value in place) from "type conversion" (Python; makes a new value). In everyday Python talk the two terms are used loosely.

---

## 5. Fixing our program

```python
first_number = int(input("Enter the first number: "))
second_number = int(input("Enter the second number: "))

result = first_number + second_number
print(result)        # 56 and 76 → 132
```

**Best practice:** convert **at the point of taking input**. If you left them as strings, you'd have to convert again every time you do math on them. Converting once at input time means the rest of the program can use the numbers freely.

---

## 6. Quick summary

- **Implicit:** Python does it (`int + float → float`).
- **Explicit:** you do it with `int()`, `float()`, `str()`, `bool()`, etc.
- Only compatible values convert.
- Conversion returns a **new** value; the original isn't modified.
- Convert user input immediately: `int(input(...))`.

---

## Extra Knowledge (not in the video)

### Implicit conversion hierarchy (numeric "promotion")
```
bool → int → float → complex
```
Python promotes to the "wider" type. `True + 2` → `3` (bool becomes int), `3 + 1.5` → `4.5`.

### How `int()` handles different inputs
| Expression | Result | Note |
|------------|--------|------|
| `int(4.9)` | `4` | **Truncates toward zero**, does not round |
| `int(-4.9)` | `-4` | Toward zero (not -5) |
| `int(True)` | `1` | |
| `int("  42 ")` | `42` | Surrounding whitespace is OK |
| `int("1_000")` | `1000` | Underscores allowed (3.6+) |
| `int("4.5")` | ❌ `ValueError` | Do `int(float("4.5"))` |
| `int("101", 2)` | `5` | Second arg = base |
| `int("ff", 16)` | `255` | |
| `int("abc")` | ❌ `ValueError` | |

### Rounding vs truncating
```python
int(4.7)       # 4   (truncate)
round(4.7)     # 5
round(4.5)     # 4   ← banker's rounding (rounds half to even)
round(5.5)     # 6
import math
math.floor(-4.7)   # -5
math.ceil(4.2)     # 5
```

### `bool()` conversion: truthy and falsy
Falsy values: `0`, `0.0`, `0j`, `""`, `[]`, `()`, `{}`, `set()`, `None`, `False`. Everything else is truthy.
```python
bool("")        # False
bool("False")   # True   ← non-empty string is truthy!
bool([0])       # True   (non-empty list)
```

### Other conversion facts
- `str(None)` → `'None'`, `str([1,2])` → `'[1, 2]'`.
- `list("hello")` splits into characters; `set([1,1,2])` → `{1, 2}` (removes duplicates, handy trick); `dict([("a",1),("b",2)])` → `{'a':1,'b':2}`.
- `float("inf")`, `float("nan")` are valid.
- **Strong typing:** Python never implicitly converts between `str` and numbers. `"5" + 5` → `TypeError`. Use `int("5") + 5` or `"5" + str(5)`.
- `"5" * 3` → `"555"` (string repetition, not math).
- Floating-point caveat: `0.1 + 0.2 == 0.3` is `False` (`0.30000000000000004`). Use `round()` or `math.isclose()`.

---

## Tricky Parts / Gotchas

1. `int("4.5")` fails, but `int(4.5)` works. String → int needs a whole number string.
2. `bool("False")` is `True`. Don't use `bool()` to parse text like `"False"`.
3. `int()` truncates, it doesn't round: `int(2.99)` is `2`.
4. `int(input())` raises `ValueError` on bad input. Real programs wrap it in `try/except`.
5. Conversion doesn't change the original variable; you must assign the result.
6. `"10" + 5` fails, but `"10" + str(5)` and `int("10") + 5` both work, with different results (`"105"` vs `15`).

---

## Interview Questions

1. **Implicit vs explicit type conversion?** Automatic (by Python) vs manual (using functions).
2. **What happens with `5 + "5"`?** `TypeError`. Python doesn't auto-convert between `int` and `str`.
3. **Does `int(3.9)` give 3 or 4?** 3 (truncation).
4. **What's `bool("0")`?** `True`: any non-empty string is truthy.
5. **How do you convert `"4.5"` to an integer?** `int(float("4.5"))`.
6. **Is type conversion permanent?** The function returns a new value; the original is unchanged.
7. **What is the numeric promotion order?** `bool → int → float → complex`.

---

### Practice ideas
1. Predict then verify: `int(-3.7)`, `round(2.5)`, `bool("0")`, `int("  7 ")`, `str(3) + str(4)`.
2. Write a program that reads two numbers and prints their sum, difference, product and quotient.
3. Convert a list with duplicates to a set and back to a list to remove duplicates.
4. Convert the string `"1010"` to an integer using base 2.

**Next video:** Literals.
