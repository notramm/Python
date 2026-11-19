# 100 Days of Python (CampusX) — Video 29: Functions

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: why functions exist, how to create and call them, arguments, scope, nested functions, and functions as objects.

---

## 1. What is a function? Why do we need it?

> A **function** is a **block of code** that performs one task: you give it **input** and it gives back **output**. Think of it as a **black box**.

You've been using functions all along: `print()`, `input()`, `len()`, `type()`...
- You don't know (or need to know) what code is inside `print()`. You just give it something and it shows it.
- If functions didn't exist, you'd have to write all that code yourself.

### Two core principles
1. **Abstraction:** hide the complexity. A user only needs to know *what a function does*, not *how*. (You use `print` without knowing its internals.)
2. **Decomposition:** break a big problem into **small functions**, then combine them. E.g. a website = a `login` function + a `register` function + a `logout` function + ... Putting those together gives the full application.

---

## 2. Creating a function

### Syntax
```python
def is_even(number):
    """This function returns whether a number is even or odd.
    Input: any valid integer
    Output: 'even' or 'odd'
    Created by: <author>   Date: <date>
    """
    if number % 2 == 0:
        return "even"
    else:
        return "odd"
```

### Parts of a function
| Part | Example | Meaning |
|------|---------|---------|
| `def` keyword | `def` | Starts a function definition |
| **Function name** | `is_even` | An identifier (follows the naming rules, Video 5) |
| **Parameters** | `(number)` | Inputs the function expects, inside round brackets |
| Colon `:` | `:` | Starts the body |
| **Docstring** | `""" ... """` | Documentation text |
| **Body** | the indented code | The logic (indentation matters, Video 11) |
| **`return`** | `return "even"` | The output sent back to the caller |

### Calling (using) a function
Defining a function does nothing by itself. It only runs when you **call** it:
```python
print(is_even(5))          # odd
print(is_even(10))         # even

for i in range(1, 11):
    print(is_even(i))
```
- The value you pass in (`5`) is the **argument**; the variable in the definition (`number`) is the **parameter**.

---

## 3. Docstrings: documentation for your function

The triple-quoted string right after the `def` line is the **docstring**.

```python
print(is_even.__doc__)     # prints the docstring
help(is_even)              # shows nicely formatted documentation
```
- Why write it? In a company, other developers will use your function. They can read the docstring to learn what it does, what input it expects, what it returns, who wrote it, etc. instead of reading all your code or asking you.
- It's the same documentation you get when you press **Shift+Tab** (or use `help()`) on built-in functions like `print`.
- Typical contents: one-line summary, inputs, outputs, author, date.

---

## 4. Two points of view: function creator vs function user

- The **creator** (e.g. a senior developer) writes the function.
- The **user** (e.g. a junior) just calls it.
- The code that **defines** a function and the code that **uses** it are often in **different files**.

```python
# file: demo.py  (creator)
def is_even(number):
    ...

# file: main.py  (user)
from demo import is_even
print(is_even(10))
```
Any `.py` file can be imported like a module (Video 18); a file in the same folder works directly.

### Who is responsible when the function crashes?
```python
is_even("hello")        # crashes (TypeError)
```
It is the **creator's** responsibility to make the function **robust**: think about every kind of input a user might give and **handle it** so a user never sees a crash.
```python
def is_even(number):
    if type(number) == int:        # (isinstance(number, int) is the modern check)
        return "even" if number % 2 == 0 else "odd"
    else:
        return "Dude, I can only handle integers"
```
Meanwhile the **user's** responsibility is to read the docstring and pass proper input.

---

## 5. What happens in memory when a function runs

- Every time you **call** a function, Python creates a **new temporary workspace ("room"/frame)** for it, with its own variables.
- The function's code runs inside that frame.
- When the function **returns**, that frame is **destroyed** along with all its local variables.
- The main program has its own workspace (the *global* frame).
- To *see* this happen, paste your code into the free visualiser **pythontutor.com** and step through it. This makes tricky questions much easier.

### `return`
- `return` sends a value back **to the code that called the function** and **ends the function** immediately (lines after it don't run).
- If a function has **no `return`**, it automatically returns **`None`**:
```python
def f(x):
    print(x * 2)          # prints, but returns nothing

result = f(5)             # prints 10
print(result)             # None
```
- `print` shows something on screen; `return` hands a value back. They are different things.

---

## 6. Parameters vs arguments, and types of arguments

- **Parameter:** the variable in the function *definition* (`def power(a, b)` → `a`, `b`).
- **Argument:** the actual value passed when *calling* (`power(2, 3)` → `2`, `3`).

If the number of arguments doesn't match what's expected, Python raises a `TypeError` (the function "crashes").

### 6.1 Default arguments
Give a parameter a **default value** so callers may leave it out:
```python
def power(a=1, b=1):
    return a ** b

power(2, 3)      # 8
power(2)         # 2   (b uses its default, 1)
power()          # 1   (both defaults)
```
Real-life idea: a game's difficulty level defaults to "medium" unless the user picks one; screen brightness has a default value.

### 6.2 Positional arguments
Values are matched to parameters **by position (order)**:
```python
power(2, 3)      # a=2, b=3  → 8
power(3, 2)      # a=3, b=2  → 9
```

### 6.3 Keyword arguments
Pass values **by parameter name**; the order no longer matters, and it **overrides position**:
```python
power(b=2, a=3)      # 9
power(2, b=5)        # 32
```
Useful when a function has many parameters and you don't want to remember their order.

### 6.4 `*args`: any number of arguments
How does `print("a", "b", "c", ...)` accept any number of inputs? With `*args`:
```python
def multiply(*numbers):
    product = 1
    for i in numbers:
        product = product * i
    return product

multiply(1, 2, 3, 4)       # 24
multiply(5)                # 5
multiply()                 # 1
print(type(numbers))       # inside the function it's a tuple
```
- The `*` collects **all positional arguments into a tuple**.
- (There's a sibling `**kwargs`, which collects **keyword** arguments into a **dictionary**.)

---

## 7. Scope: local vs global variables

- **Global variable:** defined in the main program (outside any function). Visible everywhere.
- **Local variable:** created **inside** a function. Exists only while that function runs and is destroyed when it ends.

### The three rules
**Rule 1: A function can *read* a global variable** if it has no local variable of that name.
```python
x = 5
def f():
    print(x)        # 5 → uses the global x
f()
```

**Rule 2: A local variable with the same name is separate** from the global one:
```python
x = 5
def g(y):
    x = 10          # a NEW local x (the global x is untouched)
    return x

g(1)
print(x)            # 5
```
Function variables and global variables can safely share names, since each lives in its own space.

**Rule 3: A function can *read* but not *modify* a global variable:**
```python
x = 5
def h():
    x = x + 1       # ❌ UnboundLocalError
h()
```
Python sees an assignment to `x`, so it treats `x` as *local*, but it has no value yet. This is intentional: if any function could change a global, it could break other parts of the program.

### Modifying a global explicitly: `global`
```python
x = 5
def h():
    global x
    x = x + 1
h()
print(x)            # 6
```
- It works, but **avoid it**. If you use `global` frequently, you're probably not using functions properly. Prefer passing values in and returning results.

---

## 8. Nested functions (a function inside a function)

```python
def f():
    print("inside f")

    def g():
        print("inside g")

    g()             # f must call g itself

f()                 # inside f / inside g
g()                 # ❌ NameError: name 'g' is not defined
```
- `g` is **local to `f`**: the main program can't see or call it.
- Use it to hide helper code inside a function.
- Inner functions can **read** the outer function's variables. To *modify* them use `nonlocal`.

### Infinite calls
```python
def f():
    def g():
        f()        # g calls f, f calls g ...
    g()
f()                # ❌ RecursionError: maximum recursion depth exceeded
```
Python caps the call depth (default about 1000) so a runaway program stops rather than crashing the machine.

---

## 9. Functions are objects ("first-class citizens")

> **In Python, everything is an object**: integers, strings, lists and *functions* too.

So you can treat a function like any other value:

```python
def square(x):
    return x * x
```

**1. Check its type**
```python
print(type(square))        # <class 'function'>
```

**2. Assign it to another variable** (a second reference to the same function)
```python
y = square
print(y(4))                # 16
print(id(square) == id(y)) # True
```

**3. Delete the original name**
```python
del square
print(y(4))                # 16  ← still works, since y still refers to the function object
```

**4. Store it in a list/dictionary**
```python
L = [1, 2, 3, y]
print(L[-1](5))            # 25
```

**5. Pass a function as an argument**
```python
def apply(func, value):
    return func(value)

print(apply(y, 3))         # 9
```

**6. Return a function from a function**
```python
def make_adder():
    def add(a, b):
        return a + b
    return add

adder = make_adder()
print(adder(2, 3))         # 5
print(make_adder()(2, 3))  # 5   (two-step call)
```
These ideas power **higher-order functions**, **closures** and **decorators** (coming up).

> Tip: Exams love combining these tricks into confusing code. Practise by running such snippets and tracing them in **pythontutor.com**.

---

## 10. Benefits of functions

1. **Code modularity:** split a big application into logical pieces (login, register, ...).
2. **Code reuse (DRY, "Don't Repeat Yourself"):** write once, call many times. If a bug needs fixing, change it in one place.
3. **Code readability and organisation:** easier to understand and debug (a login bug → look in the login function).

---

## 11. Quick summary

| Concept | Remember |
|---------|----------|
| Define | `def name(params):` + docstring + body |
| Output | `return` (default `None`) |
| Docstring | `help(f)` / `f.__doc__` |
| Argument types | default, positional, keyword, `*args` (`**kwargs`) |
| Scope | local vs global; can read global, can't modify without `global` |
| Nested | inner function is local to outer |
| Objects | functions can be assigned, stored, passed, returned |
| Benefits | modularity, reuse, readability |

---

## Extra Knowledge (not in the video)

### Type hints (modern, widely used)
```python
def is_even(number: int) -> str:
    ...
```
They document inputs/outputs and help editors/tools like `mypy`. Python does **not** enforce them at runtime.

### `**kwargs` and the full parameter order
```python
def show(a, b=2, *args, **kwargs):
    print(a, b, args, kwargs)

show(1, 10, 20, 30, x=5, y=6)    # 1 10 (20, 30) {'x': 5, 'y': 6}
```
Order: `positional → defaults → *args → keyword-only → **kwargs`.
- **Keyword-only** parameters come after `*`: `def f(a, *, b)` (must call `f(1, b=2)`).
- **Positional-only** parameters before `/` (3.8+): `def f(a, /, b)`.

### Unpacking when calling
```python
nums = [1, 2, 3]
multiply(*nums)               # same as multiply(1, 2, 3)
opts = {"a": 3, "b": 2}
power(**opts)                 # same as power(a=3, b=2)
```

### Returning multiple values = returning a tuple
```python
def min_max(L):
    return min(L), max(L)

lo, hi = min_max([3, 1, 4])   # tuple unpacking
```

### Mutable default arguments are shared (classic trap)
```python
def add(item, bucket=[]):     # ❌ the same list is reused on every call
    ...
def add(item, bucket=None):   # ✅
    if bucket is None:
        bucket = []
```

### `nonlocal` vs `global`
`global x` → refers to the module-level variable; `nonlocal x` → refers to the enclosing function's variable.

### Closures
A nested function that **remembers** variables from the enclosing function even after it returns:
```python
def multiplier(n):
    def times(x):
        return x * n
    return times

double = multiplier(2)
print(double(10))             # 20
```

### Docstring conventions
Follow **PEP 257**: first line a short summary, then a blank line, then details. Tools like Sphinx and IDEs read them.

### Other facts
- Functions can have attributes, e.g. `f.counter = 0` (rarely used).
- Parameters are **local variables** of the function.
- Function names follow `snake_case` (PEP 8).
- A function can call another function that is defined later, as long as both exist when the call happens.
- `lambda`, `map`, `filter` (Video 31) rely on functions being objects.

---

## Tricky Parts / Gotchas

1. **Defining ≠ calling.** Nothing runs until you call the function.
2. **`print` vs `return`:** printing inside a function returns `None` to the caller.
3. **Code after `return` never runs.**
4. **Assigning to a global name inside a function creates a *local*** → `UnboundLocalError` if you read it first.
5. **Default arguments are evaluated once** (mutable defaults persist between calls).
6. **Positional args must come before keyword args** in a call: `f(a=1, 2)` is a `SyntaxError`.
7. **Wrong number of arguments** → `TypeError`.
8. **A nested function can't be called from outside** its parent.
9. **Deleting a function name** doesn't delete the function if another name still refers to it.
10. **`*args` is a tuple**, not a list.

---

## Interview Questions

1. **What is a function and why use it?** Reusable block of code: modularity, reuse, readability, abstraction.
2. **Parameter vs argument?** Variable in the definition vs value in the call.
3. **Types of arguments?** Positional, keyword, default, variable-length (`*args`, `**kwargs`).
4. **What does a function return if there's no `return`?** `None`.
5. **What are `*args` and `**kwargs`?** Collect extra positional args into a tuple / keyword args into a dict.
6. **Local vs global variables; can a function modify a global?** Not without `global`.
7. **What is a docstring and how do you read it?** Documentation string; `help(f)` / `f.__doc__`.
8. **Why are functions "first-class objects"?** They can be assigned, stored, passed and returned like any value.
9. **What is a nested function / closure?** A function defined inside another; a closure remembers the enclosing variables.
10. **What is `RecursionError`?** The call stack limit (about 1000) was exceeded.
11. **Why avoid mutable default arguments?** They're created once and shared across calls.
12. **Is Python pass-by-value or by-reference?** Pass-by-object-reference.

---

### Practice ideas
1. Write `is_even(n)` with a docstring and print its documentation using `help()`.
2. Write a function `power(a, b=2)` and call it positionally, by keyword, and with the default.
3. Write `average(*numbers)` that works for any count of numbers.
4. Demonstrate `UnboundLocalError`, then fix it with `global`, then fix it better by returning a value.
5. Write a function that returns another function (e.g. `make_multiplier(n)`).
6. Put a function in a list and call it through the list.
7. Trace a tricky function-call example step by step in pythontutor.com.

**Next video:** Recursion.
