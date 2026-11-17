# 100 Days of Python (CampusX) — Video 4: Variables

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topics: what variables are, creating them in Python, **dynamic typing**, **dynamic binding**, multiple-assignment syntax.

---

## 1. Why do we need variables?

**Example 1: a login website**
- Millions of users log in, and each sees their own name on a profile page.
- You don't build one page per user. You build **one page** and put a placeholder where the name goes.
- At coding time you don't know whose name it will be, so you write a **variable** there. It gets filled in when the user logs in.

**Example 2: adding two numbers**
- You don't know in advance which two numbers the user will enter. So you use two variables and fill them later.

> **Definition:** A variable is a **container for future use**. Whenever you don't know in advance what value will come at runtime, you use a variable.

This is a fundamental concept in **every** programming language.

---

## 2. Declaring variables: C vs Python

**C (static typing):** you must declare the data type first.

```c
int a = 5;      // a can only ever hold integers
```

**Python:** no data type, no separate declaration.

```python
name = "Hello World"
print(name)      # Hello World
```

- Python is "smart enough" to work out the type automatically from the value.
- There is **no variable declaration** step. You create a variable the moment you assign to it.
- If you forget the quotes around a string, Python treats it as a variable name and throws an error (`NameError`).

---

## 3. Dynamic typing

> **Dynamic typing:** Python decides the data type of a variable automatically, at runtime, from the value assigned. You never specify the type.

- Opposite: **static typing** (C, C++, Java), where you must declare the type.
- PHP is another dynamically typed language.

---

## 4. Dynamic binding

```python
name = "Hello World"
print(name)      # Hello World

name = 4
print(name)      # 4

name = True
print(name)      # True
```

> **Dynamic binding:** One variable can hold **different data types at different times**. The variable is not locked to a single type.

- Opposite: **static binding** (C, C++, Java). Once `int a` is declared, `a` stays an integer for the whole life of the program.
- C/Java programmers may find this unsettling at first. The instructor notes that after 4–5 years of Python work it has rarely caused real trouble.

| Concept | Meaning | Python | C / Java |
|---------|---------|--------|----------|
| Typing | Do you declare the type? | **Dynamic**: no | **Static**: yes |
| Binding | Can a variable change type? | **Dynamic**: yes | **Static**: no |

---

## 5. Special ways to declare multiple variables

```python
# 1. Semicolon-separated (one line)
a = 5; b = 6; c = 7

# 2. Tuple-style / multiple assignment
a, b, c = 5, 6, 7

# 3. Same value to all
a = b = c = 5
print(a, b, c)    # 5 5 5
```

All three are just convenience syntax. Nothing special happens behind the scenes (see gotcha below for #3).

---

## 6. Quick summary

- Variable = container for values you don't know ahead of time.
- No declaration, no type needed: `x = 10`.
- **Dynamic typing** = type is figured out automatically.
- **Dynamic binding** = the same variable can hold different types over time.
- Multiple assignment: `a, b, c = 1, 2, 3` and `a = b = c = 0`.

---

## Extra Knowledge (not in the video)

### A variable is a *name tag*, not a box
In Python, a variable is a **name that points to (references) an object** in memory. The *object* has the type, not the variable.

```python
a = 5
b = a
print(id(a), id(b))     # same id → both names point to the same object
a = 10                  # a now points to a different object; b still points to 5
```

This is why dynamic binding works: rebinding a name to a different object is easy.

### Dynamically typed ≠ weakly typed
| | Static | Dynamic |
|---|---|---|
| **Strong** | Java, C# | **Python** |
| **Weak** | C (loosely) | JavaScript, PHP |

Python is **dynamically typed** *and* **strongly typed**: it won't silently mix incompatible types.

```python
"5" + 5        # TypeError: can only concatenate str (not "int") to str
```
(JavaScript would give `"55"`.)

### Type hints (optional, not enforced)
```python
age: int = 25
name: str = "Ram"
```
These are documentation/tooling hints (used by `mypy`, IDEs). Python itself does **not** enforce them at runtime.

### Other handy things
- **Swap without a temp variable:** `a, b = b, a`
- **Unpacking:** `first, *rest = [1, 2, 3, 4]` → `first = 1`, `rest = [2, 3, 4]`
- **Delete a variable:** `del a` (using `a` afterwards raises `NameError`)
- **Placeholder with no value yet:** `x = None` (see Video 8, literals)
- **Naming convention (PEP 8):** `snake_case` for variables/functions, `UPPER_CASE` for constants, `CamelCase` for classes. Python has no real constants; it's just a convention.
- Variable names are **case-sensitive**: `Name` and `name` are different.

---

## Tricky Parts / Gotchas

1. **Chained assignment with mutable objects shares one object:**
   ```python
   a = b = []
   a.append(1)
   print(b)       # [1]   ← b changed too! Both names point to the same list
   ```
2. **Using a variable before assigning it → `NameError`.** Python has no "declare without a value".
3. **Semicolons work but are un-Pythonic.** Avoid `a = 5; b = 6` in real code.
4. **Rebinding is not mutation.** `a = 10` doesn't change the object `5`; it makes `a` point elsewhere.

---

## Interview Questions

1. **What is dynamic typing? Is Python statically or dynamically typed?**
   Dynamic. The type is determined at runtime from the value; variables have no fixed type.
2. **Difference between dynamic typing and dynamic binding?**
   Typing: you don't declare the type. Binding: the same name can later refer to a different type.
3. **Is Python strongly or weakly typed?** Strongly typed (no implicit `str` + `int`), though it does implicitly convert numeric types (`int` + `float`).
4. **What does `a = b = []` do and what's the risk?** Both names refer to the same list.
5. **How do you swap two variables in Python?** `a, b = b, a`.
6. **Are Python variables "containers"?** Informally yes, but technically they are references (name bindings) to objects.

---

### Practice ideas
1. Create a variable, print it, then reassign it to a different type and print `type()` each time.
2. Use `id()` to check whether `a = 5; b = 5` share an object.
3. Try `a = b = []`, append to `a`, and observe `b`.
4. Swap two variables in one line.

**Next video:** Keywords and Identifiers.
