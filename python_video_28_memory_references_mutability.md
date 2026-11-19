# 100 Days of Python (CampusX) — Video 28: How Python Stores Data (References, Garbage Collection & Mutability)

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: what happens in memory when you write `a = 4`, why `del` doesn't delete data, and what "mutable" really means.
> This video explains the *why* behind many things from earlier videos (`id()`, `is`, immutable strings/tuples, mutable lists).

---

## 1. Where is data stored?

- When a program runs, its data lives in **RAM (memory)**.
- Think of memory as a huge number of **cells**. Each cell has an **address**. Addresses are continuous and are usually written in **hexadecimal**.
- Under the hood (digital electronics): memory/registers are built from **logic gates**, which are built from transistors/diodes. You don't need that for Python, only the idea of "cells with addresses".

---

## 2. A Python variable is a *reference*

```python
a = 4
```

What really happens:
1. Python creates the **object `4`** somewhere in memory (at some address).
2. The name **`a`** is a **reference** that **points to** that object.

```
 a  ───────►  [ 4 ]   (object living at memory address 0x7ff...)
```

- That's why using a name that was never assigned gives `NameError: name 'x' is not defined`. The reference doesn't exist.
- `id(a)` returns the **address of the object** the variable points to (as a decimal number; `hex(id(a))` shows it in hexadecimal).

> **Key idea:** In Python, a variable is **not a box that holds a value**. It's a **name (reference) attached to an object**.

---

## 3. Two names, one object

```python
a = 4
b = a
print(id(a), id(b))     # same number → both point to the same object
print(a is b)           # True
```
```
 a ──┐
     ├──► [ 4 ]
 b ──┘
```

### Reassigning moves the reference, not the object
```python
a = 5          # a now points to a different object
print(b)       # 4   (b still points to the old object)
```
```
 a ──► [ 5 ]
 b ──► [ 4 ]
```
Nothing was "changed" in the object `4`; `a` just switched targets.

---

## 4. Deleting a variable ≠ deleting the data

```python
a = 4
b = a
del a          # removes the NAME a (the reference)
print(b)       # 4   → object still alive (b points to it)
```
- `del` deletes the **reference**, not the object.
- The object stays in memory as long as **at least one reference** points to it.
- When **no reference** points to it, the memory is wasted (occupied, but unusable by you). It needs to be freed.

---

## 5. Garbage collection

- In languages like C/C++, the **programmer** must free memory manually (using pointers).
- **Python doesn't let you do that.** Instead it has an automatic system: the **garbage collector**.
- Python's garbage collector finds objects that **nobody refers to any more** and frees their memory so it can be reused.
- The same is true for Java, JavaScript, PHP, and other high-level languages.

So: you create objects freely; Python cleans up for you. 

---

## 6. Counting references: `sys.getrefcount()`

```python
import sys

x = 717
print(sys.getrefcount(x))     # 2
```
- It tells you **how many references** point to that object.
- It shows **one more** than you expect, because passing `x` into the function itself creates a temporary extra reference. So a value only your variable uses shows `2`.

### Why do some numbers show huge counts?
For common values like `4` or `1`, the count is large (e.g. 4 or much larger): many parts of Python and the system already use those numbers. If someone asks why, that's the answer: *they're widely shared*.

---

## 7. Why `a = 4` and `b = 4` share one object (integer caching)

```python
a = 4
b = 4
print(id(a) == id(b))       # True  (same object, no new one created)
```
For commonly used small integers, Python **reuses one object** instead of creating a new one every time. This is a **software optimisation** to save memory and time.

- Range cached: **-5 to 256**.
- Outside that range, separate statements usually create separate objects:
  ```python
  a = 1000
  b = 1000
  print(a is b)     # often False when typed on separate lines in an interactive session
  ```
- This explains the surprising results with the `is` operator from Video 9.

### Strings behave similarly (interning)
Short strings that look like identifiers (letters, digits, underscores) are commonly **reused**; strings containing spaces and other characters often are not. So never rely on `is` for comparing strings/numbers: use `==`.

---

## 8. How lists are stored in memory

A list doesn't store the values themselves. It stores **references to objects**.

```python
L = [1, 2, 3]
```
```
 L ──► [ ref ][ ref ][ ref ]      (the list object)
          │      │      │
          ▼      ▼      ▼
         [1]    [2]    [3]        (separate objects)
```
```python
print(id(L))          # address of the list object
print(id(L[0]))       # address of the object 1  (different!)
```
- The list object has its own address; each item has its own.
- For a **nested list** such as `L = [1, 2, [3, 4]]`, the third slot refers to **another list object** (which holds references to `3` and `4`).
- This is why a list can hold **mixed types**: each slot is just a reference.

---

## 9. Mutable vs immutable: what it really means

> **Mutable:** you can change the object **in place**, at the **same memory address**.
> **Immutable:** you **cannot** change the object. Any "change" creates a **new object at a new address**, and the variable is re-pointed to it.

| Immutable | Mutable |
|-----------|---------|
| `int`, `float`, `bool`, `complex`, `str`, `tuple` | `list`, `set`, `dict` |

### Immutable example
```python
s = "hello"
print(id(s))
s = s + " world"      # builds a NEW string, s is re-pointed
print(id(s))          # different address
```

### Mutable example
```python
L = [1, 2, 3]
print(id(L))
L.append(4)           # same list object, modified in place
L[0] = 100
print(id(L))          # SAME address
```

### Be careful: operations that create a NEW list
```python
L = [1, 2, 3]
L = L + [4, 5]        # creates a new list; L is re-pointed (address changes)
```
Methods like `append`, `extend`, `insert`, `remove`, item assignment, `sort` change **in place**. The `+` operator **builds a new object**.

---

## 10. The danger of mutability: aliasing

```python
a = [1, 2, 3]
b = a            # NOT a copy! both names point to the same list
b.append(4)
print(a)         # [1, 2, 3, 4]   ← a changed too!
```
```
 a ──┐
     ├──► [1, 2, 3, 4]
 b ──┘
```

**Real-life scenario:** a senior teammate creates a list. You "copy" it with `b = a` for your own work, then change `b`. The senior's list silently changes, and the team's code breaks. This kind of bug is very common.

### Fix: make a real copy
```python
b = a[:]          # slicing creates a new list (a shallow copy)
b = a.copy()      # same idea
b = list(a)
b.append(99)
print(a)          # unchanged
print(id(a) == id(b))   # False → separate objects
```

---

## 11. Four tricky questions (typical exam/interview style)

The trick: **draw the memory diagram**, then reason with *references* and *mutability*.

**Q1. Tuple containing a list: edit the list's item?**
```python
t = (1, 2, 3, [4, 5])
t[-1][0] = 500
print(t)         # (1, 2, 3, [500, 5])   ✅ works!
```
The tuple's references didn't change; the **list** (mutable) was modified in place.

**Q2. Replace the tuple's last item itself?**
```python
t[-1] = 500      # ❌ TypeError: 'tuple' object does not support item assignment
```
That would require changing a reference stored in the tuple.

**Q3. List containing a tuple: edit inside the tuple?**
```python
L = [1, 2, 3, (4, 5)]
L[-1][0] = 500   # ❌ TypeError (tuple is immutable)
L[-1] = (9, 9)   # ✅ allowed: the list can swap its reference
```

**Q4. Tuple of two lists that are also named variables:**
```python
a = [1, 2]
b = [3, 4]
c = (a, b)
c[0][0] = 200
print(a)         # [200, 2]   ← a changed, because c[0] IS the same list as a
```

**Q5. `+` vs `+=` on lists:**
```python
a = [1, 2, 3]; b = a
a = a + [4]      # new list → a re-pointed; b still [1, 2, 3]

a = [1, 2, 3]; b = a
a += [4]         # in-place extend → b sees it too: [1, 2, 3, 4]
```
For **tuples**, `t += (4,)` always builds a **new** tuple (immutable).

---

## 12. Quick summary

- Variables are **references (names)** to objects, not containers.
- `id()` = address; `is` compares addresses; `==` compares values.
- `del name` removes the reference; the **garbage collector** frees objects with no references.
- Common small ints/short strings are **shared** (optimisation).
- Lists hold **references**.
- **Mutable** = changed in place at the same address; **immutable** = changes make a new object.
- `b = a` shares the object; copy with `.copy()`, `[:]`, or `copy.deepcopy()`.

---

## Extra Knowledge (not in the video)

### How CPython manages memory (current understanding)
- **Reference counting** is the main mechanism: when an object's count drops to 0, it's freed immediately.
- A secondary **cyclic garbage collector** (the `gc` module) handles **reference cycles** (e.g. two objects pointing at each other) that counting alone can't free.
- Memory is managed in "arenas" and "pools" by Python's own allocator; you rarely need to care.

### Immortal objects (Python 3.12+)
In newer Python, some objects (small ints, `None`, `True`, `False`, some strings) are **immortal**: their reference count is a fixed huge value and never changes. This is one reason `sys.getrefcount(1)` or `sys.getrefcount(None)` now shows an enormous number.

### Shallow copy vs deep copy
```python
import copy
a = [[1, 2], [3, 4]]
b = a.copy()              # shallow: new outer list, SAME inner lists
b[0].append(99)
print(a)                  # [[1, 2, 99], [3, 4]]  ← inner list shared!

c = copy.deepcopy(a)      # deep: copies inner objects too
```

### Python is "pass by object reference" (a.k.a. call by sharing)
When you pass a variable to a function, the function gets a **reference to the same object**. If the object is mutable and the function mutates it, the caller sees the change:
```python
def add_item(lst):
    lst.append(1)         # mutates the caller's list

nums = []
add_item(nums)
print(nums)               # [1]
```
Reassigning the parameter inside the function (`lst = [...]`) does **not** affect the caller.

### Mutable default argument trap
```python
def add(item, bucket=[]):     # ❌ the list is created ONCE and reused
    bucket.append(item)
    return bucket

add(1); add(2)                # [1, 2] on the second call!
def add(item, bucket=None):   # ✅
    if bucket is None:
        bucket = []
    bucket.append(item)
    return bucket
```

### Other facts
- `is` should be used only for singletons: `None`, `True`, `False`.
- Memory profiling: `sys.getsizeof(obj)` (shallow size), `tracemalloc` module.
- Dictionaries/sets use **hashes**, which is why keys must be immutable/hashable.
- `del L[0]` removes an *item* from the list (mutation); `del L` removes the *name*.
- An object is kept alive by **any** reference: a list element, a dictionary value, another object's attribute, a closure.

---

## Tricky Parts / Gotchas

1. **`b = a` doesn't copy** (for lists, dicts, sets). Use `.copy()`/`[:]`/`deepcopy`.
2. **`.copy()` is shallow**: nested objects are still shared.
3. **`del x` doesn't free memory immediately** if other references exist.
4. **`sys.getrefcount()` shows +1** (the call's own temporary reference).
5. **`a is b` may be `True` for small ints/short strings**, but that's an implementation detail: use `==`.
6. **A tuple isn't deeply immutable**: it can contain a mutable list that can still change.
7. **`a += b` vs `a = a + b`** behave differently for lists (in place vs new object).
8. **`[[0] * 3] * 3`** makes 3 references to the **same** inner list (Video 24).
9. **Mutable default arguments** persist between function calls.

---

## Interview Questions

1. **Are Python variables containers or references?** References (names bound to objects).
2. **What does `id()` return?** The identity of an object (its memory address in CPython).
3. **What does `del x` do?** Removes the name `x`; the object is freed only when no references remain.
4. **What is garbage collection? How does Python do it?** Automatic freeing of unreferenced objects, using reference counting plus a cyclic GC.
5. **Mutable vs immutable, with examples?** See section 9.
6. **What happens with `b = a` for a list?** Both names refer to the same list.
7. **Shallow vs deep copy?** Shallow copies the container only; deep copies nested objects too.
8. **Can you change a list inside a tuple?** Yes, the list is mutable; the tuple's references don't change.
9. **Why does `a = 256; b = 256; a is b` give `True`?** Small-integer caching.
10. **`a += [1]` vs `a = a + [1]`?** In-place extension vs creating a new list.
11. **Is Python call-by-value or call-by-reference?** Neither exactly: "call by object reference".

---

### Practice ideas
1. Print `id()` of `a = 5`, `b = 5`, `c = 1000`, `d = 1000` and explain the results.
2. Create `a = [1, 2, 3]; b = a`, change `b`, and see `a`. Then fix it with `.copy()`.
3. Show that `append` keeps the same `id` while `L = L + [x]` changes it.
4. Use `sys.getrefcount()` on a new list before and after `b = a`.
5. Create a nested list, copy it with `.copy()`, mutate an inner list, and see why you need `deepcopy`.
6. Predict the output of the five tricky questions above before running them.

**Next video:** Functions.
