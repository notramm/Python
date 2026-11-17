# 100 Days of Python (CampusX) — Video 18: Modules

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: what modules are, how to import and use them, and a tour of `math`, `random`, `time` and `os`.

---

## 1. What is a module?

> A **module** is a Python **file** (`.py`) that contains ready-made **functions** (and variables/classes) you can **import and reuse**.

- In other languages the same idea is called a **library**.
- Why use modules? **Reusability.** If your project needs lots of mathematical functions, you don't write them yourself. You import a module and use them, saving time.
- Python has a huge number of **built-in (standard library) modules**. You can also install third-party ones and write your own (custom modules come later, after functions).

### Seeing all modules available on your system
```python
help("modules")
```
Lists every module available in your installation (a much longer list on a full Anaconda install than on plain Python). You can't, and needn't, learn them all. Once you know how to use *one* module, you can use any.

---

## 2. Importing a module

```python
import math
```
- Python syntax: the `import` keyword + the module's name (no `#include`/`.h` like C).
- If it runs with no output, the import worked.
- Wrong name → `ModuleNotFoundError: No module named '...'`.

### Exploring what's inside
Type the module name, put a **dot** and press **Tab** (in Jupyter/IDEs) to see all its functions:
```python
math.      # ← press Tab
```
You can also use `dir(math)`, or `help(math.sqrt)` for a function's documentation.

> Best way to learn a module: **experiment**. Try the functions that look useful or interesting.

---

## 3. The `math` module

```python
import math

math.pi            # 3.141592653589793
math.e             # 2.718281828459045
math.factorial(5)  # 120      (no need to write your own function!)
math.ceil(6.3)     # 7        (round UP)
math.floor(6.9)    # 6        (round DOWN)
math.sqrt(16)      # 4.0      (square root)
```
Also contains trigonometric functions (`sin`, `cos`, `tan`), logarithms (`log`, `log10`) and more, covering most formulas from school/college maths.

---

## 4. The `random` module

```python
import random

random.randint(1, 100)       # random integer from 1 to 100 (both inclusive)
```
- Different result each run. (Used in the guessing game, Video 13.)

### `shuffle()`: shuffle a list
```python
import random

cards = [1, 2, 3, 4, 5]
random.shuffle(cards)
print(cards)                 # e.g. [3, 1, 5, 2, 4]  (different order each run)
```
- Works like shuffling a deck of cards. It shuffles the list itself (**in place**) rather than returning a new one.
- Run it repeatedly and you get a different order every time.

---

## 5. The `time` module

```python
import time

time.time()      # e.g. 1731831234.56  (seconds since the "epoch")
```
- `time.time()` returns the number of **seconds passed since 1 January 1970, 00:00:00 UTC** (the "epoch" / Unix time). Call it repeatedly and it keeps increasing.
- `time.ctime()` gives a human-readable current date and time string.
- (A separate video on date and time handling will come later.)

### `time.sleep()`: pause the program
```python
import time

print("Hello")
time.sleep(2)          # wait 2 seconds
print("World")
```
"Hello" appears, then after 2 seconds "World".

---

## 6. The `os` module

For interacting with the operating system: files, folders and paths.

```python
import os

os.getcwd()        # current working directory ("where am I?")
os.listdir()       # list the files/folders in the current directory
```
- `getcwd()` = **get current working directory**.
- `listdir()` lists the files in your current folder (for example your notebook and auto-generated files).
- Widely used in file-handling work.

---

## 7. Summary

- A module = a Python file of ready-made functions; equivalent to a **library** in other languages.
- `import module_name`, then use `module_name.function()`.
- Don't try to learn every module; learn **how to import and explore** them.
- Import what your project needs: `random` for randomness, `math` for maths, `time` for time, `os` for files/system.
- You can also create **your own modules** (taught later with functions).

---

## Extra Knowledge (not in the video)

### Different ways to import
```python
import math                    # use as math.sqrt(16)
import math as m               # alias → m.sqrt(16)
from math import sqrt, pi      # import specific names → sqrt(16)
from math import *             # ❌ avoid: pollutes your namespace, hides name clashes
```

### Module vs package vs library
| Term | Meaning |
|------|---------|
| **Module** | A single `.py` file |
| **Package** | A folder of modules (contains `__init__.py` in the classic layout) |
| **Library / framework** | A collection of packages (e.g. NumPy, Django); the word "library" is used loosely |
| **Standard library** | Modules shipped with Python (`math`, `random`, `os`, `datetime`, `json`...) |
| **Third-party** | Installed with `pip` (e.g. `pip install requests`) |

### Where Python looks for modules
Python searches in order: the current script's folder → standard library locations → installed packages (`site-packages`). You can see the list with `import sys; print(sys.path)`.

### Don't name your own file after a module!
If you save a file as `random.py` or `math.py`, then `import random` imports **your file** instead of the real module → confusing errors.

### Modern recommendations (up to date)
- **Paths and files:** prefer **`pathlib`** over manual `os` string handling:
  ```python
  from pathlib import Path
  Path.cwd()                   # current directory
  list(Path(".").iterdir())    # files in the current directory
  ```
  `os.getcwd()` and `os.listdir()` still work and are widely used.
- **Timing code:** use `time.perf_counter()` (not `time.time()`) to measure how long something takes.
  ```python
  start = time.perf_counter()
  # ... work ...
  print(time.perf_counter() - start)
  ```
- **Don't use `time.clock()`**: it was removed in Python 3.8.
- **Dates and times:** use the `datetime` module for readable dates (`datetime.now()`).
- **Security-sensitive randomness** (passwords, tokens, OTPs): use **`secrets`**, not `random`.
- **More maths:** `math.isqrt(n)` (exact integer square root), `math.comb(n, k)`, `math.prod(iterable)`, `math.gcd(a, b)`, `math.isclose(a, b)` for float comparison; `statistics.mean()` for averages.

### More about `random`
```python
random.random()                     # float in [0.0, 1.0)
random.choice(["a", "b", "c"])      # one random item
random.sample(range(1, 50), 6)      # 6 unique random items (like a lottery)
random.randrange(0, 10, 2)          # random even number below 10 (stop excluded)
random.seed(42)                     # same sequence every run → reproducible tests
```
- `random.shuffle(x)` **returns `None`** and works on **lists only** (mutable). It fails on tuples/strings. Use `random.sample(seq, len(seq))` to get a shuffled copy of any sequence.
  ```python
  cards = random.shuffle(cards)     # ❌ cards becomes None!
  ```

### More about `time`
- `time.time()` returns a **float** (seconds). Convert it with `time.ctime(ts)` or `datetime.fromtimestamp(ts)`.
- `time.sleep()` blocks the whole program. Fine for scripts and demos; real applications use async/threads/schedulers.
- `time.localtime()` and `time.strftime("%H:%M:%S")` give formatted time.

### More about `math`
- `math.sqrt(16)` returns `4.0` (a float). Use `math.isqrt(16)` for the int `4`.
- `math.ceil` / `math.floor` return ints; compare with `round()` (banker's rounding) and `int()` (truncates).
- `math.factorial(-1)` → `ValueError`; `math.sqrt(-1)` → `ValueError` (use `cmath` for complex results).
- Large factorials are fine in Python since integers have no size limit.

### Handy introspection
```python
dir(math)                # names inside a module
help(math.factorial)     # docs
math.__name__, math.__file__
```
And in your own scripts: `if __name__ == "__main__":` runs code only when the file is executed directly, not when it's imported (covered later).

---

## Tricky Parts / Gotchas

1. **`import Math`** fails: module names are **case-sensitive** (`math`).
2. `random.randint(1, 100)` includes 100; `random.randrange(1, 100)` doesn't.
3. `random.shuffle()` returns `None` (changes the list in place).
4. `import random` then calling `randint(1, 10)` directly → `NameError`. You need `random.randint` or `from random import randint`.
5. Naming your own file `random.py` / `math.py` / `time.py` shadows the real module.
6. `time.time()` is the **current time in seconds since 1970**, not a readable date.
7. `os.getcwd()` shows where the script is **run from**, which may not be where the script file lives.
8. Importing a module executes its top-level code once; later imports reuse the cached module (`sys.modules`).

---

## Interview Questions

1. **What is a module?** A Python file with functions/classes/variables that can be imported and reused.
2. **Module vs package vs library?** File vs folder of modules vs a collection of packages.
3. **Different ways to import?** `import x`, `import x as y`, `from x import a, b`, `from x import *` (avoid).
4. **How do you see everything inside a module?** `dir(module)`, `help(module)`.
5. **How do you list all installed modules?** `help("modules")`.
6. **What does `time.time()` return?** Seconds since the epoch (1 Jan 1970 UTC).
7. **How is `random.shuffle` different from `random.sample`?** `shuffle` reorders a list in place and returns `None`; `sample` returns a new list of chosen items.
8. **What is `__name__ == "__main__"` for?** To run code only when the file is executed directly.
9. **How do you install a third-party module?** `pip install package_name`.
10. **What happens if two modules have the same name?** The first found on `sys.path` wins (often your local file), which can shadow the standard one.

---

### Practice ideas
1. Compute `math.factorial(10)`, `math.sqrt(144)`, `math.ceil(4.1)`, `math.floor(4.9)`.
2. Create a list of 10 numbers, shuffle it, and print it three times.
3. Print "Starting...", wait 3 seconds with `time.sleep`, then print "Done!".
4. Print your current directory and the number of items in it using `os`.
5. Use `help("modules")` and pick two modules you've never heard of; explore them with `dir()`.
6. Repeat the guessing game (Video 13) using `from random import randint`.

**Next:** Functions (and creating your own modules).
