# 100 Days of Python (CampusX) — Video 17: Built-in Functions

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: the most useful ready-made functions that Python gives you without any `import`.

---

## 1. What is a function? What are built-ins?

> A **function** is a block of code with a single purpose: you give it some **input** and it gives you some **output**.

- Example: `print()` takes something and shows it on screen.
- **Built-in functions** are functions that Python already provides. You don't import or define anything.
- Python has many (roughly 70 documented built-ins). The video covers about **15 of the most frequently used**, which handles ~90% of everyday needs.

---

## 2. Functions already seen

| Function | Purpose | Example |
|----------|---------|---------|
| `print()` | Show output on screen | `print("Hello")` |
| `input()` | Read from the user (**always returns a string**) | `name = input("Name: ")` |
| `type()` | Tell the data type of a value | `type(3)` → `<class 'int'>` |
| `int()`, `float()`, `str()`, `bool()`, `list()`, `tuple()`, ... | Type conversion | `int("5")` → `5` |

---

## 3. New functions

### 3.1 `abs()`: absolute value (the modulus)
```python
abs(4)      # 4
abs(-4)     # 4
```
Used often (especially in machine learning / maths work).

### 3.2 `pow()`: power
```python
pow(2, 3)    # 8      (2 ** 3)
pow(2, -3)   # 0.125  (2 ** -3 = 1/8)
```

### 3.3 `min()` and `max()`
Smallest and largest of the items you pass (works on a list or on several arguments).
```python
max([1, 5, 3])     # 5
min([1, 5, 3])     # 1
```
- Works with strings too: `max(["Kolkata", "Delhi", "Mumbai"])` → compares **alphabetically** (dictionary order), so it gives `'Mumbai'`, and `min` gives `'Delhi'`.
- The same applies to tuples, sets, etc. (Iterables get a full video later.)

### 3.4 `round()`: rounding
```python
round(2/3, 2)    # 0.67   → 2 decimal places
round(2/3, 3)    # 0.667  → 3 decimal places
round(2/3)       # 1      → no second argument: nearest integer
```
Useful when you want to print a limited number of decimals.

### 3.5 `divmod()`: quotient and remainder together
```python
divmod(5, 2)     # (2, 1)   → 5 // 2 = 2 and 5 % 2 = 1
```
Returns a **tuple**: `(quotient, remainder)`.

### 3.6 `bin()`, `oct()`, `hex()`: number system conversion
```python
bin(4)      # '0b100'    binary
oct(8)      # '0o10'     octal
hex(255)    # '0xff'     hexadecimal
```
Give you the binary / octal / hexadecimal form of a decimal number.

### 3.7 `id()`: identity (memory address)
```python
a = 3
print(id(a))     # a large number: where the object lives in memory
```
- The value differs on every computer/run.
- Used to check which variables refer to the same object (the idea behind the `is` operator from Video 9). A full video on this comes later.

### 3.8 `ord()`: character → code
```python
ord("A")    # 65
ord("C")    # 67
ord("a")    # 97
```
Returns the **Unicode/ASCII code** of a single character. Handy in programs that work with character codes.

### 3.9 `len()`: length
```python
len("Kolkata")        # 7
len([1, 2, 3])        # 3
```
Works for strings, lists, tuples, sets, dictionaries, etc. You'll use it **a lot**.

### 3.10 `sum()`: total of an iterable's items
```python
sum([1, 2, 3, 4])     # 10
sum((1, 2, 3))        # 6
```
Works on lists, tuples, sets, etc. (numbers only).

### 3.11 `help()`: documentation
```python
help(print)
help(len)
```
Prints the documentation of any function. If you don't understand a function, `help()` is the first place to look.

---

## 4. Quick summary

| Need | Function |
|------|----------|
| Absolute value | `abs(x)` |
| Power | `pow(a, b)` |
| Smallest / largest | `min(...)`, `max(...)` |
| Round | `round(x, n)` |
| Quotient + remainder | `divmod(a, b)` |
| Binary / octal / hex | `bin()`, `oct()`, `hex()` |
| Memory identity | `id(x)` |
| Character code | `ord(c)` |
| Length | `len(x)` |
| Total | `sum(x)` |
| Documentation | `help(f)` |

---

## Extra Knowledge (not in the video)

### Inverse and related functions
- `chr(65)` → `'A'` (the reverse of `ord`).
- `int("ff", 16)` → `255`, `int("101", 2)` → `5` (reverse of `hex`/`bin`).
- Without the `0b` / `0x` prefix: `format(255, "x")` → `'ff'`, `format(5, "b")` → `'101'`, or `f"{255:x}"`.

### `round()` details (a classic trap)
```python
round(2.5)       # 2     ← "banker's rounding": ties go to the nearest EVEN number
round(3.5)       # 4
round(2.675, 2)  # 2.67  ← floating-point representation, not a bug
round(1234, -2)  # 1200  ← negative digits round to tens/hundreds
```
For exact decimal rounding use the `decimal` module.

### `min()` / `max()` upgrades
```python
max(["apple", "fig", "banana"], key=len)     # 'banana' (longest)
max([], default=0)                            # 0  (avoid ValueError on empty input)
min(3, 1, 2)                                  # 1  (works with separate arguments)
```

### `sum()` upgrades
```python
sum([1, 2, 3], 10)        # 16 → optional start value
sum(["a", "b"])           # ❌ TypeError: for strings use "".join(list)
sum([0.1] * 10)           # 0.9999999999999999
import math; math.fsum([0.1] * 10)   # 1.0  → exact-ish sum of floats
```
`sum` on a dictionary adds its **keys**.

### `pow()` with 3 arguments: fast modular exponent
```python
pow(2, 10, 1000)     # 24  → (2**10) % 1000, efficient even for huge numbers
```
`abs()` on a complex number gives its **magnitude**: `abs(3+4j)` → `5.0`.

### `divmod` with negatives
`divmod(-7, 2)` → `(-4, 1)` (follows floor division, see Video 9).

### `id()`
- In CPython, `id()` is the object's **memory address**. It's only guaranteed unique among objects alive at the same time.
- `a = 5; b = 5; id(a) == id(b)` → `True` (small ints are cached).

### `len()`
- Runs in **O(1)** time for built-in containers (the size is stored).
- Not valid for numbers: `len(123)` → `TypeError`.

### More must-know built-ins (not in the video)
| Function | Use |
|----------|-----|
| `sorted(x)` | Returns a new sorted list |
| `reversed(x)` | Iterator in reverse order |
| `enumerate(x)` | Index + item pairs |
| `zip(a, b)` | Pair items from several iterables |
| `range()` | Number sequences (Video 14) |
| `any(x)` / `all(x)` | True if any / all items are truthy |
| `isinstance(x, T)` | Type check (preferred over `type(x) ==`) |
| `dir(x)` | List attributes/methods of an object |
| `map()`, `filter()` | Apply / filter over an iterable |
| `open()` | Open a file (File Handling later) |
| `callable(x)` | Can `x` be called like a function? |

### Be careful with
- **`eval()` / `exec()`** run arbitrary code → never pass untrusted input.
- Don't name your variables after built-ins (`sum = 0`, `max = 5`, `list = []`, `id = 1`): it shadows the function and breaks later code (Video 5).
- Use `dir(__builtins__)` (or `import builtins; dir(builtins)`) to see everything built in; `help("builtins")` for docs.

---

## Tricky Parts / Gotchas

1. `round(2.5)` is **2**, not 3.
2. `round(x)` with no digits returns an **int**; `round(x, 2)` returns a **float**.
3. `sum()` doesn't work on lists of strings.
4. `max()`/`min()` on an **empty** sequence raises `ValueError` (use `default=`).
5. Mixed types in `max`: `max(3, "a")` → `TypeError`.
6. `len()` counts items/characters, not digits of a number. Convert first: `len(str(12345))` → `5`.
7. `ord()` takes exactly **one** character: `ord("AB")` → `TypeError`.
8. `bin()`, `oct()`, `hex()` return **strings**, not numbers.
9. `pow(2, -3)` returns a **float** (`0.125`).

---

## Interview Questions

1. **What is a built-in function? Name a few.** Predefined functions available without import: `print`, `len`, `type`, `sum`, `max`...
2. **`round(2.5)` output? Why?** `2`, because of banker's rounding (half to even).
3. **`divmod(17, 5)`?** `(3, 2)`.
4. **How do you get the ASCII code of a character and back?** `ord()` and `chr()`.
5. **What's the difference between `pow(a, b)` and `a ** b`?** Same result for two args; `pow` also supports a third modulus argument.
6. **How do you find the longest string in a list?** `max(items, key=len)`.
7. **How can you see documentation of a function inside Python?** `help(func)`.
8. **`id()` use?** Identify the object's identity/memory address.
9. **Why shouldn't you name a variable `list` or `sum`?** It shadows the built-in.

---

### Practice ideas
1. Take a list of numbers and print the min, max, sum and average (`sum / len`).
2. Print `ord()` of every character in a word, then rebuild the word with `chr()`.
3. Convert 255 to binary, octal and hex.
4. Use `divmod()` to convert 500 seconds into minutes and seconds.
5. Round `22/7` to 0, 2 and 4 decimals.
6. Try `help(round)` and `help(max)` and read the docs.

**Next video:** Modules.
