# 100 Days of Python (CampusX) — Video 8: Literals

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.

---

## 1. What is a literal?

> A **literal** is the raw data (the actual value) that you give to a variable.

```python
a = 10          # 10 is the literal, a is the variable
name = "Ram"    # "Ram" is the literal
```

Python has **four** kinds of literals:

1. **Numeric** literals (int, float, complex)
2. **String** literals
3. **Boolean** literals
4. **Special** literal (`None`)

---

## 2. Numeric literals

### 2.1 Integer literals in different number systems

```python
a = 10          # decimal (default)
b = 0b1010      # binary  (prefix 0b)   → 10
c = 0o17        # octal   (prefix 0o)   → 15
d = 0x1F        # hexadecimal (0x)      → 31

print(a, b, c, d)    # 10 10 15 31   ← always printed in decimal
```

- The prefix tells Python which number system you're using. Python then treats the value as an ordinary integer.
- **Printing always shows decimal.**
- Where it's used: electronics boards (e.g. Raspberry Pi, where you control pins using binary), low-level/hardware work. For websites or Android apps it's rarely used. 90% of the time you just use plain decimals.

### 2.2 Float literals

```python
x = 10.5
```

**Scientific notation** for very large or very small numbers (useful in e.g. NASA-style or virology/lab software):

```python
print(1.5e2)     # 150.0      (1.5 × 10²)
print(1.5e-2)    # 0.015      (1.5 × 10⁻²)
```

### 2.3 Complex literals

```python
z = 3 + 4j
print(z.real)    # 3.0   ← real part
print(z.imag)    # 4.0   ← imaginary part
```
- Write the imaginary part directly with `j`.
- Extract the parts with `.real` and `.imag`.

---

## 3. String literals

### Ways to write a string

```python
'Hello'            # single quotes (most common)
"Hello"            # double quotes
'''Hello'''        # triple single quotes
"""Hello"""        # triple double quotes
```

- Python has **no separate `char` type**: even a single character is a string (`'a'`).
- Single vs double quotes: no difference.
- **Triple quotes** are needed for **multi-line strings** (e.g. a blog post typed by a user):

```python
text = """This is line 1
This is line 2
This is line 3"""
```

### Unicode strings (emojis etc.)
You can use Unicode characters such as smileys:

```python
print("\U0001F600")      # 😀 (Unicode escape by code point)
print("😀")              # directly typing the emoji also works in Python 3
```
(The video uses a `u` prefix; in Python 3 strings are already Unicode, so `u"..."` is optional.)

### Raw strings
Prefix `r` makes backslashes literal, useful for things like HTML or file paths:

```python
print(r"C:\new\table")    # C:\new\table  (no \n or \t interpretation)
print("C:\new\table")     # breaks across lines and adds a tab!
```

---

## 4. Boolean literals

```python
True
False
```

- Python internally treats `True` as **1** and `False` as **0**, so you can do math with them:

```python
print(True + 4)     # 5
print(False + 4)    # 4
```
(This is just implicit type conversion from Video 7.)

---

## 5. Special literal: `None`

```python
a = None
print(a)        # None
```

- `None` means "**nothing / no value**".

### Why do we need it?
In Python you don't declare variables up front. But sometimes (e.g. a manager asks you to "declare a variable to use later") you want a variable to **exist without a real value**.

```python
x            # NameError: name 'x' is not defined  ← can't just "declare" it
x = None     # ✅ the variable exists; value is "nothing yet"
```

So `None` is the standard placeholder for "no value yet / intentionally empty".

---

## 6. Quick summary

| Literal type | Examples |
|--------------|----------|
| Integer | `10`, `0b1010`, `0o17`, `0x1F` |
| Float | `10.5`, `1.5e2`, `1.5e-2` |
| Complex | `3+4j` |
| String | `'a'`, `"abc"`, `'''multi\nline'''` |
| Boolean | `True`, `False` |
| Special | `None` |

---

## Extra Knowledge (not in the video)

### Handy literal features
- **Underscores for readability** (3.6+): `1_000_000`, `0xFF_FF`, `1_000.50`.
- Float shortcuts: `.5` = `0.5`, `5.` = `5.0`.
- **Convert between number systems:** `bin(10)` → `'0b1010'`, `oct(15)` → `'0o17'`, `hex(31)` → `'0x1f'`. Reverse: `int("1010", 2)`.
- **Bytes literals:** `b"abc"` (raw bytes, not text).
- **f-string literals:** `f"Name: {name}"` (very common; formatting).
- **Escape sequences:** `\n` (new line), `\t` (tab), `\\` (backslash), `\'`, `\"`.
- **Container literals:** `[1,2]` (list), `(1,2)` (tuple), `{1,2}` (set), `{"a":1}` (dict).
- Python integers have **no size limit** (limited only by memory), unlike floats (~1.8 × 10³⁰⁸ max).
- `0777` (old-style octal) is a **SyntaxError** in Python 3; use `0o777`.

### About `None`
- `None` is a **singleton**: there's exactly one `None` object. Its type is `NoneType`.
- Compare with `is`, not `==`:
  ```python
  if x is None:       # ✅ preferred
  if x == None:       # works, but not recommended
  ```
- A function with no `return` returns `None` automatically.
- `bool(None)` is `False`.
- Don't confuse `None` with `0`, `""` or `False`. They're all "falsy", but different.
- Commonly used as a default argument: `def f(x=None):`.

### About booleans
- `bool` is a **subclass of `int`**: `isinstance(True, int)` → `True`.
- `True` and `False` are keywords (capitalised).

---

## Tricky Parts / Gotchas

1. `True == 1` → `True`, and `False == 0` → `True`. But `True is 1` → `False`.
2. Dictionary keys: `{True: "a", 1: "b"}` → `{True: "b"}`. `True` and `1` hash the same, so they collide.
3. `sum([True, True, False])` → `2` (handy to count how many conditions are true).
4. `"C:\new"` contains a newline; use a raw string or `"C:\\new"`.
5. `0.1 + 0.2` is not exactly `0.3` (floating-point representation).
6. `print(0b1010)` prints `10`, not `0b1010`. Use `bin()` to see binary.
7. Strings like `'5'` and number `5` are different literals; `"5" == 5` is `False`.

---

## Interview Questions

1. **What is a literal?** A fixed value written directly in code and assigned to a variable.
2. **What kinds of literals does Python have?** Numeric, string, boolean, special (`None`), plus collection literals.
3. **How do you write binary / octal / hex numbers?** `0b`, `0o`, `0x` prefixes.
4. **What is `None`? How is it different from `0` or `False`?** A singleton meaning "no value"; different type (`NoneType`).
5. **How do you check whether a variable is `None`?** `x is None`.
6. **Is `True + True` valid?** Yes, it's `2`, because `bool` is a subclass of `int`.
7. **How do you write a multi-line string?** Triple quotes.
8. **What's a raw string and when do you use it?** `r"..."`: backslashes aren't escapes; used for regex, file paths.

---

### Practice ideas
1. Print `0b1111`, `0o17`, `0xFF` and confirm all print in decimal.
2. Write `6.02e23` and `1.6e-19` and print them.
3. Create a complex number and print its real and imaginary parts.
4. Print a path like `C:\temp\new` using a raw string.
5. Create a variable with `None`, then check it using `is None`.

**Next video:** Operators.
