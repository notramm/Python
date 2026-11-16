# 100 Days of Python (CampusX) — Video 2: Data Types in Python

> Notes from the transcript of the second video (original in Hinglish, notes in English).
> Topic: an overview of the data types Python offers. This video is an introduction, with detailed coverage in later videos.

---

## 1. The big picture

Python data types fall into **three categories**:

| Category | What it is | Types |
|----------|------------|-------|
| **Basic types** | The standard types you'd find in most programming languages | Integer, Float, Complex, Boolean, String |
| **Container types** | Hold **multiple** values inside a single container | List, Tuple, Set, Dictionary |
| **User-defined types** | Types you create yourself | Classes and objects (covered under OOP) |

```
Data Types
├── Basic
│   ├── int
│   ├── float
│   ├── complex
│   ├── bool
│   └── str
├── Container
│   ├── list
│   ├── tuple
│   ├── set
│   └── dict
└── User-defined
    └── classes / objects (OOP)
```

---

## 2. Basic types

### 2.1 Integer (`int`)

- Whole numbers: positive, negative or zero.
- **Python has no practical upper limit on integers.** You can store extremely large numbers (the instructor tried values like 10 raised to the power of several hundred, and Python handled them).
- The only limit is your computer's memory. So for practical purposes, integers are effectively unlimited.

```python
print(5)
print(10 ** 400)   # a huge number, still works fine
```

### 2.2 Float (`float`)

- Numbers with a **decimal point**, e.g. `3.14`.
- Unlike integers, floats **do have a limit**. The maximum is roughly **1.7 × 10^308**.
- Beyond that, Python shows **`inf`** (infinity).

```python
print(1.5)
print(1e309)   # inf  (too big for a float)
```

> **Takeaway:** For all practical purposes, integers and floats are big enough. You will almost never need a number larger than what they hold.

### 2.3 Boolean (`bool`)

- Only two values: **`True`** and **`False`** (capital first letter).
- Used a lot, especially in conditions and decision-making. It comes up heavily as the course goes on.

```python
print(True)
print(False)
```

### 2.4 Complex (`complex`)

- A special feature that most other languages don't have built in.
- Python supports **complex numbers** (from high-school maths), written with a `j`:

```python
print(5 + 6j)
```

- Only useful if you work in a **mathematics-heavy domain**. The instructor says they've rarely used it, so you can mostly ignore it for now.

### 2.5 String (`str`)

- Text data. **Very important**: any application that deals with users uses strings heavily.
- You can write strings in several ways:
  - Single quotes: `'Hello'`
  - Double quotes: `"Hello"`
  - Triple quotes: `'''Hello'''` or `"""Hello"""`
- All of them work the same for creating a string. When to use which is explained in the dedicated Strings videos.

```python
print('Hello')
print("Hello")
print('''Hello''')
```

---

## 3. Container types

- A container type lets you store **multiple values in one container**.
- There are four: **list, tuple, set, dictionary**.
- Each one gets deep coverage later (several videos each). This video is just to show **what they look like**.

### 3.1 List

- Same idea as arrays or lists in other languages: a collection of items.
- Created with **square brackets `[ ]`**.

```python
[1, 2, 3, 4]
```

### 3.2 Tuple

- Looks and behaves **similar to a list**, but is used differently (this comes later when tuples are studied).
- **Biggest visible difference:** tuples use **round brackets `( )`**, lists use square brackets.
- There are other differences too, explained later.

```python
(1, 2, 3, 4)
```

### 3.3 Set

- Works like a **set from high-school maths**: union, intersection and so on are available.
- Defined with **curly brackets `{ }`**.
- Visually similar to a list apart from the brackets, and its operations are useful and fast.

```python
{1, 2, 3, 4}
```

### 3.4 Dictionary

- Stores data as **key : value pairs**.
- Also uses curly brackets `{ }`, but each item has a key and a value.
- Very useful and you will use it a lot. Lookups are fast.

```python
{
    "name": "Nitish",
    "gender": "Male",
    "weight": 70
}
```

Here `name`, `gender` and `weight` are the **keys**; `"Nitish"`, `"Male"` and `70` are their **values**.

### Brackets cheat sheet

| Type | Brackets | Example |
|------|----------|---------|
| List | `[ ]` | `[1, 2, 3]` |
| Tuple | `( )` | `(1, 2, 3)` |
| Set | `{ }` | `{1, 2, 3}` |
| Dictionary | `{ key: value }` | `{"a": 1, "b": 2}` |

---

## 4. User-defined types

- Types you define yourself using **classes and objects**.
- Studied once **Object-Oriented Programming (OOP)** starts, since that's where this concept belongs.
- For now, just know that a third category exists.

---

## 5. Quick summary

- Python has **3 categories** of data types: basic, container, user-defined.
- **Basic:** `int`, `float`, `complex`, `bool`, `str`.
- **Integers** have no practical limit; **floats** max out at about 1.7 × 10^308 (then `inf`).
- **Complex numbers** are built in, but rarely needed outside maths.
- **Containers:** `list [ ]`, `tuple ( )`, `set { }`, `dict { k: v }`.
- Each container type will get several dedicated videos later.

---

## 6. What's next

- **Next video:** Comments in Python.

---

### Practice ideas

1. Print one value of each basic type: an int, a float, a complex number, a boolean and a string.
2. Create a list, a tuple, a set and a dictionary, each with 3 items, and print them.
3. Try `print(1e308)` and `print(1e309)` to see the float limit in action.
4. Try printing a very large integer such as `10 ** 1000` to confirm there's no overflow.
