# 100 Days of Python (CampusX) — Video 24: Lists

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Covers: list vs array, creating, accessing, editing, adding, deleting, operations, functions, and 3 practice problems.

---

## 1. What is a list?

> A **list** is a data type that lets you store **multiple items together** in one variable.

### List vs Array (a very common interview question)

If you've used C or Java you know **arrays**. Lists are similar, but different in important ways:

| | **Array (C/Java)** | **Python List** |
|---|--------------------|-----------------|
| **Types of items** | **Homogeneous**: all items must be the same type | **Heterogeneous**: can mix types (`int`, `str`, `float`, even other lists) |
| **Memory** | Items are stored in **contiguous** (side-by-side) memory | Not necessarily contiguous (the list stores *references*; explained in a later memory video) |
| **Speed** | Faster (items are packed together) | Slower |
| **Programmer friendliness** | Less friendly | **More friendly**: flexible and easy to use |
| **Size** | Fixed at creation (in C) | Grows/shrinks dynamically |

Trade-off: lists are a bit slower, but they make the programmer's life much easier and let you write more powerful logic.

---

## 2. Creating lists

```python
# Empty list
L = []

# Homogeneous (same type)
L = [1, 2, 3, 4]

# Heterogeneous (mixed types)
L = [1, True, 5.6, 5 + 6j, "Hello"]

# 2D / nested list (a list inside a list)
L = [1, 2, 3, [4, 5]]

# 3D (list inside a list inside a list)
L = [[[1, 2], [3, 4]], [[5, 6], [7, 8]]]

# Using type conversion
L = list("hello")        # ['h', 'e', 'l', 'l', 'o']
```
- Lists use **square brackets `[ ]`**.
- Nested lists can be created to any depth.
- `list(...)` converts another sequence (string, tuple, set, range...) into a list.

---

## 3. Accessing items

Everything you learned about **indexing and slicing for strings** (Video 20) works the same for lists.

```python
L = [1, 2, 3, 4, 5]

L[0]        # 1       positive indexing
L[-1]       # 5       negative indexing (last item)
L[1:4]      # [2, 3, 4]
L[::-1]     # [5, 4, 3, 2, 1]   reversed
```

### Nested lists: use multiple brackets
```python
L = [[1, 2, 3], [4, 5, 6]]
L[1]           # [4, 5, 6]     (whole inner list)
L[1][0]        # 4
```

**3D example:** how to get `7` from `[[[1, 2], [3, 4]], [[5, 6], [7, 8]]]`?
Work **from the outside in**:
1. In the outer list, `7` is in the **second** item → index `1`
2. In that inner list, `7` is in the **second** item again → index `1`
3. In that innermost list, `7` is the **first** item → index `0`

```python
L = [[[1, 2], [3, 4]], [[5, 6], [7, 8]]]
print(L[1][1][0])     # 7
```
- Each level of nesting = one more `[ ]`.
- Positive and negative indexes can be mixed.
- 3D is about the most you'll ever need. Beyond that, it's rare.

---

## 4. Editing items

> **Lists are MUTABLE**: you can change them after creation (unlike strings).

```python
L = [1, 2, 3, 4, 5]

L[-1] = 500           # [1, 2, 3, 4, 500]
L[0] = 100            # [100, 2, 3, 4, 500]

L[1:4] = [20, 30, 40] # replace a slice  → [100, 20, 30, 40, 500]
```
Works with positive/negative indexes and with slicing.

---

## 5. Adding items

Three methods: **`append`**, **`extend`**, **`insert`**.

### `append()`: add ONE item at the end
```python
L = [1, 2, 3]
L.append(4)            # [1, 2, 3, 4]
L.append("hello")      # [1, 2, 3, 4, 'hello']
L.append([5, 6])       # [1, 2, 3, 4, 'hello', [5, 6]]   ← the whole list becomes ONE item
```

### `extend()`: add MULTIPLE items at the end
```python
L = [1, 2, 3]
L.extend([4, 5, 6])       # [1, 2, 3, 4, 5, 6]
L.extend("goa")           # [1, 2, 3, 4, 5, 6, 'g', 'o', 'a']   ← each character becomes an item
```
> **Remember:** `append` always adds **one** item; `extend` adds **many** (it unpacks the iterable).

### `insert(index, item)`: add at a chosen position
```python
L = [1, 2, 3, 4]
L.insert(1, 100)          # [1, 100, 2, 3, 4]
```
You give two things: the **index** where it should go, and the **item**.

---

## 6. Deleting items

```python
L = [1, 2, 3, 4, 5, 6]
```

| Method | What it does | Example |
|--------|--------------|---------|
| `del L` | Deletes the **whole list** (variable gone) | `del L` |
| `del L[0]` | Deletes the item at an index | `[2, 3, 4, 5, 6]` |
| `del L[-3:]` / `del L[1:3]` | Deletes a **slice** | |
| `L.remove(value)` | Removes the **first occurrence of a value** (when you don't know the index) | `L.remove(3)` |
| `L.pop()` | Removes (and returns) the **last** item | `6` |
| `L.pop(index)` | Removes (and returns) the item at that index | |
| `L.clear()` | **Empties** the list but keeps the variable | `[]` |

```python
L = [1, 2, 3, 4, 5]
L.remove(3)       # [1, 2, 4, 5]    find the value 3 and remove it
L.pop()           # returns 5, list is [1, 2, 4]
L.clear()         # []
print(L)          # []   (still exists, but empty)
```
- `clear()` ≠ `del`: `clear()` leaves an **empty list**; `del L` removes the **variable**.

---

## 7. Operations on lists

Four main operations work on lists:

```python
L1 = [1, 2, 3]
L2 = [4, 5, 6]

L1 + L2          # [1, 2, 3, 4, 5, 6]      concatenation (makes a NEW list)
L1 * 3           # [1, 2, 3, 1, 2, 3, 1, 2, 3]   repetition

for i in L1:     # loop over items
    print(i)

2 in L1          # True      membership
7 not in L1      # True
```

### Membership with nested lists
```python
L3 = [1, 2, 3, [4, 5]]
5 in L3          # False  ← 5 is inside the inner list, not a direct item
[4, 5] in L3     # True   ← the inner list itself is an item
```
`in` only checks the **direct** items.

---

## 8. Functions on lists

```python
L = [3, 1, 4, 2]

len(L)                    # 4
min(L)                    # 1
max(L)                    # 4
sorted(L)                 # [1, 2, 3, 4]        returns a NEW sorted list
sorted(L, reverse=True)   # [4, 3, 2, 1]
L.index(4)                # 2   position of the item
```
- `min`/`max` need items that can be compared with each other (all numbers, or all strings). Mixed types like `[1, "hello"]` → `TypeError`.
- **`sorted()` is not permanent.** It returns a new list; the original `L` stays unchanged.
- **`L.sort()`** is different: it **sorts the list in place** (changes `L` itself) and returns `None`. Use `L.sort(reverse=True)` for descending.
- Other list methods: `count()`, `reverse()`, `copy()`... Type `L.` and press Tab or use `dir(list)` to see them all.

---

## 9. Practice problems

### Problem 1: Capitalise the first letter of every word (without `.title()`)
Input: `hi my name is ram` → Output: `Hi My Name Is Ram`

**Approach:** split into words → capitalise each → collect into a new list → join back.
```python
sentence = input("Enter a sentence: ")
words = sentence.split()

result = []
for word in words:
    result.append(word.capitalize())

print(" ".join(result))
```

### Problem 2: Get the username from an email ID
Input: `abc@gmail.com` → Output: `abc`

**Approach:** find where `@` is, slice everything before it.
```python
email = input("Enter email: ")
pos = email.find("@")
print(email[:pos])
# one-liner alternative: email.split("@")[0]
```

### Problem 3: Remove duplicates from a list (keep the original order)
Input: `[1, 2, 3, 1, 2, 4]` → Output: `[1, 2, 3, 4]`

**Approach:** make a new empty list; loop over the original; add an item only if it's not already in the new list.
```python
L = [1, 2, 3, 1, 2, 4]
unique = []
for item in L:
    if item not in unique:
        unique.append(item)
print(unique)     # [1, 2, 3, 4]
```
> Tip from the instructor: Python needs a slightly different way of thinking than other languages. Solving small problems like these builds that skill.

---

## 10. Quick summary

| Task | Tool |
|------|------|
| Create | `[ ]`, `list()` |
| Access | `L[i]`, `L[a:b:c]`, `L[i][j]` |
| Edit | `L[i] = x` (mutable) |
| Add | `append`, `extend`, `insert` |
| Delete | `del`, `remove`, `pop`, `clear` |
| Operations | `+`, `*`, `in`, loop |
| Functions | `len`, `min`, `max`, `sorted`, `index`, `count`, `sort` |

---

## Extra Knowledge (not in the video)

### How a list works internally
A Python list is a **dynamic array of references** (pointers) to objects. That's why it can hold mixed types and grow automatically.

| Operation | Time complexity |
|-----------|-----------------|
| `L[i]` index | O(1) |
| `append` | O(1) amortised |
| `pop()` (last) | O(1) |
| `insert(0, x)`, `pop(0)`, `remove(x)` | O(n) |
| `x in L` | O(n) |
| `sort()` | O(n log n) |

For fast pops/inserts at **both ends** (queues), use `collections.deque`. For fast membership tests, use a **set**.

### Best ways to remove duplicates
```python
list(set(L))               # removes duplicates but does NOT keep order
list(dict.fromkeys(L))     # removes duplicates AND keeps order (modern, fast)
```

### Copying lists (a classic trap)
```python
a = [1, 2, 3]
b = a                # NOT a copy: both names point to the same list
b.append(4)
print(a)             # [1, 2, 3, 4]

c = a.copy()         # shallow copy (also a[:] or list(a))
import copy
d = copy.deepcopy(a) # deep copy (needed for nested lists)
```

### List comprehensions (the Pythonic way to build lists)
```python
squares = [x * x for x in range(1, 6)]            # [1, 4, 9, 16, 25]
evens = [x for x in range(10) if x % 2 == 0]      # [0, 2, 4, 6, 8]
```

### Other useful things
- `L.reverse()` reverses in place; `reversed(L)` gives an iterator; `L[::-1]` gives a new list.
- `sorted(L, key=len)` / `L.sort(key=str.lower)` sort by a custom rule; sorting is **stable**.
- `L += [4, 5]` modifies the list **in place** (like `extend`), while `L = L + [4, 5]` creates a new list.
- Unpacking: `a, b, *rest = [1, 2, 3, 4]` → `a=1, b=2, rest=[3, 4]`.
- Use `enumerate(L)` for index + value; `zip(L1, L2)` to pair lists.
- `sum(L)`, `any(L)`, `all(L)` are handy built-ins.
- Python's built-in `array` module and **NumPy** arrays are real homogeneous arrays (much faster for numeric work).
- `extend()` accepts **any iterable** (list, tuple, set, string, range...).

---

## Tricky Parts / Gotchas

1. **`append` vs `extend`:** `L.append([5, 6])` adds one nested item; `L.extend([5, 6])` adds two items.
2. **`L = L.append(5)` makes `L` become `None`**: `append`, `sort`, `reverse`, `extend`, `insert`, `remove` all return `None`.
3. **`b = a` doesn't copy** a list. Use `.copy()`.
4. **`[[0] * 3] * 3` creates three references to the SAME inner list:**
   ```python
   grid = [[0] * 3] * 3
   grid[0][0] = 9
   print(grid)       # [[9,0,0],[9,0,0],[9,0,0]]  ← all rows changed!
   grid = [[0] * 3 for _ in range(3)]    # ✅ correct
   ```
5. **`remove(x)`** raises `ValueError` if `x` isn't there; it removes only the **first** match.
6. **`pop()`** on an empty list → `IndexError`.
7. **`sorted(L)` doesn't change `L`; `L.sort()` does** (and returns `None`).
8. **Don't add/remove items while looping over the same list**; you'll skip items. Loop over a copy (`for x in L[:]`) or build a new list.
9. **`del L` vs `L.clear()`**: variable removed vs list emptied.
10. `in` on a nested list checks only top-level items.
11. Mixed types can't be sorted: `sorted([3, "a"])` → `TypeError`.

---

## Interview Questions

1. **List vs array?** Lists are heterogeneous, dynamic, more flexible but slower; arrays are homogeneous, contiguous, faster.
2. **`append` vs `extend` vs `insert`?** One item at end / many items at end / one item at a given index.
3. **`remove` vs `pop` vs `del`?** By value / by index (returns the item, default last) / by index or slice (no return).
4. **`sort()` vs `sorted()`?** In place (returns `None`) vs returns a new list.
5. **How to copy a list?** `L.copy()`, `L[:]`, `list(L)` (shallow); `copy.deepcopy` for nested.
6. **How do you remove duplicates while keeping order?** `list(dict.fromkeys(L))`.
7. **Are lists mutable? Hashable?** Mutable and **not** hashable (can't be dict keys or set items).
8. **Time complexity of `append`? of `insert(0, x)`?** O(1) amortised; O(n).
9. **What does `[1, 2] * 2` give?** `[1, 2, 1, 2]`.
10. **How would you flatten `[[1, 2], [3, 4]]`?** `[x for sub in L for x in sub]`.
11. **Why is `x in list` slow for big data?** It scans linearly; use a set.

---

### Practice ideas
1. Take a list of numbers and print the largest, smallest, sum and average.
2. Reverse a list in three different ways.
3. Find the second largest number in a list.
4. Merge two lists and remove duplicates.
5. Rotate a list by `k` positions using slicing.
6. Build a 3×3 grid of zeros correctly (avoid the `*` trap).

**Next video:** Tuples.
