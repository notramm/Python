# 100 Days of Python (CampusX) — Video 25: Tuples

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> A tuple is very similar to a list, with **one big difference: it is immutable (read-only)**.

---

## 1. What is a tuple?

> A **tuple** is an **ordered, immutable** collection of items. Think of it as a **read-only list**.

- Everything you can do to **read** a list works on a tuple.
- Everything that **changes** a list (edit, add, delete items) does **not** work on a tuple.

---

## 2. Creating tuples

Tuples use **round brackets `( )`** (lists use square brackets).

```python
# Empty tuple
t = ()

# Homogeneous
t = (1, 2, 3, 4)

# Heterogeneous
t = (1, "hello", 4.5, True, 5 + 6j)

# Nested (2D / 3D)
t = (1, 2, 3, (4, 5))
t = ((1, 2), (3, 4))

# From another sequence (type conversion)
t = tuple("hello")        # ('h', 'e', 'l', 'l', 'o')
```

### ⚠️ Creating a single-item tuple
```python
t = (5)
print(type(t))        # <class 'int'>      ← NOT a tuple!

t = ("hello")
print(type(t))        # <class 'str'>      ← NOT a tuple!

t = (5,)
print(type(t))        # <class 'tuple'>    ✅ the trailing comma makes it a tuple
```
- Round brackets alone are treated as ordinary grouping brackets (like in `(2 + 3) * 4`).
- **The comma is what makes a tuple.** A single-item tuple needs a trailing comma: `(5,)`.
- (A lists has no such problem: `[5]` is a list.)

---

## 3. Accessing items

Exactly the same as lists and strings: **indexing** and **slicing**.

```python
t = (1, 2, 3, 4, 5)
t[0]        # 1
t[-1]       # 5
t[1:4]      # (2, 3, 4)
t[::-1]     # (5, 4, 3, 2, 1)

t = (1, 2, (3, 4))
t[2][0]     # 3     nested access
```

---

## 4. Editing items: NOT allowed

```python
t = (1, 2, 3)
t[0] = 100
```
```
TypeError: 'tuple' object does not support item assignment
```
Just like strings, **tuples are immutable**: once created, you can't change an item.

---

## 5. Adding items: NOT allowed

There is no `append`, `extend` or `insert` for tuples. You cannot add new items to an existing tuple.

---

## 6. Deleting items

```python
t = (1, 2, 3)

del t[0]     # ❌ TypeError: 'tuple' object doesn't support item deletion
del t        # ✅ deletes the whole tuple (the variable)
```
- You can delete the **entire** tuple, but **not a single item or part** of it (that would change the tuple).

---

## 7. Operations on tuples

Same as lists:

```python
t1 = (1, 2, 3)
t2 = (4, 5, 6)

t1 + t2          # (1, 2, 3, 4, 5, 6)     concatenation (creates a new tuple)
t1 * 3           # (1, 2, 3, 1, 2, 3, 1, 2, 3)

for i in t1:     # looping works
    print(i)

2 in t1          # True      membership works
```

---

## 8. Functions on tuples

```python
t = (3, 1, 4, 2)

len(t)                     # 4
min(t)                     # 1
max(t)                     # 4
sorted(t)                  # [1, 2, 3, 4]    ← returns a LIST, not a tuple
sorted(t, reverse=True)    # [4, 3, 2, 1]
```
- `sorted()` always returns a **list**. To get a tuple back: `tuple(sorted(t))`.
- Because a tuple can't be changed, there is **no `t.sort()`**.
- Tuple methods: only **`count()`** and **`index()`**.

---

## 9. Tuple vs List: the key difference

| | **List** | **Tuple** |
|---|----------|-----------|
| Brackets | `[ ]` | `( )` |
| **Mutable?** | ✅ Yes | ❌ **No** (immutable) |
| Edit / add / delete items | ✅ | ❌ |
| Read operations (index, slice, loop, `in`, `+`, `*`, `len`, `min`, `max`) | ✅ | ✅ |
| Single-item creation | `[5]` | `(5,)` (comma needed) |

> **Most important difference:** tuples are **immutable**, lists are **mutable**. If an interviewer asks "tuple vs list", start with this. (The single-item creation quirk is a secondary point.)

### Read-only: reading is allowed, writing is not
Everything that only **reads/accesses** the data works on a tuple. Everything that **writes/changes** it doesn't.

### Why would you want a read-only collection? (Data integrity)
The instructor's story: you're the web developer for your college, responsible for displaying results from a database. Your younger brother failed to get his preferred branch (he needed 8.5 CGPA but has 6.5) and begs you: *"Just show 8.5 on the site instead!"*
- If the results data were in a **list**, you could change it (and compromise the data).
- If it were in a **tuple**, nobody (including you) could modify it in code. The data stays exactly as it was.

So tuples are used wherever **data integrity** matters: data that must stay exactly as it is.

---

## 10. Quick summary

- Tuple = **immutable** list-like collection, written with `( )`.
- Read-only: access, loop, `in`, `+`, `*`, `len`, `min`, `max`, `sorted` (returns list) all work.
- Edit / add / partial delete: ❌. Whole-tuple `del`: ✅.
- Single-item tuple needs a trailing comma: `(5,)`.
- Use when data must not change.

---

## Extra Knowledge (not in the video)

### Parentheses are optional: the comma makes the tuple
```python
t = 1, 2, 3          # (1, 2, 3)  tuple "packing"
t = 5,               # (5,)
a, b, c = t          # tuple "unpacking" (a=1, b=2, c=3 for t=(1,2,3))
a, b = b, a          # swap: right side builds a tuple, left side unpacks it
first, *rest = (1, 2, 3, 4)    # first=1, rest=[2, 3, 4]
```
Functions that "return multiple values" are really returning a tuple.

### Immutability is shallow
```python
t = ([1, 2], 3)
t[0].append(99)      # ✅ allowed! the list inside is mutable
print(t)             # ([1, 2, 99], 3)
t[0] = [5]           # ❌ can't replace the item itself
```
A tuple's **items** (references) can't be changed, but if an item is itself mutable, *that object* can change.

### `+=` on a tuple creates a new tuple
```python
t = (1, 2)
t += (3,)            # t is now a NEW tuple (1, 2, 3)  (the old one is unchanged)
```

### Why use tuples? (practical reasons)
- **Safety / integrity:** can't be modified accidentally.
- **Hashable** (if all items are hashable): tuples can be **dictionary keys** and **set items**; lists can't.
  ```python
  locations = {(19.07, 72.87): "Mumbai"}
  ```
- **Slightly faster and smaller in memory** than lists.
- **Fixed records:** coordinates `(x, y)`, RGB colours `(255, 0, 0)`, database rows.
- Multiple return values and `for k, v in d.items()` patterns.

### Named tuples: readable records
```python
from collections import namedtuple
Point = namedtuple("Point", ["x", "y"])
p = Point(3, 4)
print(p.x, p.y)       # 3 4
```
(`typing.NamedTuple` and `dataclasses` are the modern class-based alternatives.)

### Tuple methods
```python
t = (1, 2, 2, 3)
t.count(2)     # 2
t.index(3)     # 3
```

### Memory comparison
```python
import sys
sys.getsizeof([1, 2, 3])    # larger
sys.getsizeof((1, 2, 3))    # smaller
```

---

## Tricky Parts / Gotchas

1. **`(5)` is an int; `(5,)` is a tuple.** Same for `("hi")` vs `("hi",)`.
2. **`()` is the empty tuple**; `tuple()` also works.
3. **A tuple containing a list isn't fully "frozen"**: the list can still change. Also, such a tuple is **not hashable**.
4. **`sorted(t)` returns a list.** There's no `t.sort()`.
5. **`t[0] = x` and `del t[0]` raise `TypeError`;** `del t` works.
6. `t += (x,)` looks like adding but actually creates a **new** tuple and rebinds `t`.
7. `tuple("abc")` splits into characters: `('a', 'b', 'c')`.

---

## Interview Questions

1. **Tuple vs list?** Tuples are immutable, lists are mutable (plus syntax and single-item differences).
2. **Why use a tuple?** Data integrity, hashability (dict keys), slightly better performance, fixed records, multiple return values.
3. **How do you create a single-element tuple?** `(5,)` (with the trailing comma).
4. **Can a tuple contain a mutable object?** Yes. The object can still be mutated even though the tuple can't be re-assigned.
5. **Can a tuple be a dictionary key?** Yes, if all its items are hashable.
6. **What methods does a tuple have?** `count()` and `index()`.
7. **What is tuple unpacking?** Assigning items to separate variables in one statement: `a, b = (1, 2)`.
8. **How do you "modify" a tuple?** You can't. Convert to a list, change it, convert back, or build a new tuple.
9. **Is `t = 1, 2, 3` valid?** Yes, it creates a tuple (the comma matters, not the brackets).

---

### Practice ideas
1. Create a tuple of 5 items; print the first, last and middle items.
2. Try `t[0] = 5`, `del t[0]` and `t.append(5)` and read each error.
3. Convert a tuple to a list, change an item, and convert back.
4. Swap two variables using tuple unpacking.
5. Use a tuple as a dictionary key to store the name of a city by its `(latitude, longitude)`.
6. Show that a tuple containing a list can have its inner list changed.

**Next video:** Sets.
