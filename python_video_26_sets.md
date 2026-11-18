# 100 Days of Python (CampusX) — Video 26: Sets

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Sets in Python work like the **sets from school maths**: union, intersection, difference...

---

## 1. The 4 golden rules of sets

Remember these and you'll never get a set question wrong:

1. **No duplicates.** A set only keeps unique items.
2. **No indexing.** Items have no position, so you can't use `s[0]` or slicing.
3. **Only immutable items inside.** A set can't contain lists, other sets or dictionaries. (No 2D/3D sets.)
4. **The set itself is mutable.** You can add and remove items after creating it.

> Rule 3 and rule 4 can sound contradictory: a set can't *contain* mutable items, yet the set *is* mutable. That's correct. The restriction is about what goes **inside**.

---

## 2. Creating sets

```python
# Using curly braces
s = {1, 2, 3}

# Heterogeneous items (mixed types are fine, if immutable)
s = {1, "hello", 4.5, (1, 2, 3)}

# Duplicates are removed automatically
s = {1, 1, 2, 2, 3}
print(s)               # {1, 2, 3}

# From another sequence
s = set([1, 2, 2, 3])  # {1, 2, 3}
s = set("hello")       # {'h', 'e', 'l', 'o'}
```

### ⚠️ Empty set: `{}` is NOT an empty set
```python
s = {}
print(type(s))         # <class 'dict'>   ← Python makes a dictionary!

s = set()
print(type(s))         # <class 'set'>    ✅ the only way to make an empty set
```
Both sets and dictionaries use `{ }`, and Python gave the default to dictionaries.

### Mutable items are not allowed
```python
{1, 2, [3, 4]}         # ❌ TypeError: unhashable type: 'list'
{1, 2, {3, 4}}         # ❌ TypeError: unhashable type: 'set'
{1, 2, (3, 4)}         # ✅ tuples are immutable, so allowed
```
This is why **2D and 3D sets are not possible**.

### The order looks "random"
When you print a set, items may appear in a different order than you typed them. That's because sets have **no index and no order**. Python decides where each item goes using a technique called **hashing** (covered in the data-structures part of the course). You can't predict or rely on the order.

---

## 3. Accessing items

```python
s = {1, 2, 3}
s[0]        # ❌ TypeError: 'set' object is not subscriptable
s[-1]       # ❌ same error
s[0:2]      # ❌ slicing doesn't work either
```
There's **no indexing**, so you can't pick an item by position. You can only loop over a set or check membership.

---

## 4. Editing items: NOT possible

You can't edit an existing item (no indexing, so there's nothing to point at).

### "Can't I just convert to a list, edit, and convert back?"
```python
s = {1, 2, 3}
print(id(s))             # e.g. 1001
l = list(s)
l[0] = 100
s = set(l)
print(id(s))             # a different id → a NEW set was created
```
The **memory address changed**, so you didn't edit the set. You created a brand-new set and made `s` point to it. That isn't editing.

> **Proof that a set is mutable:** `s.add(...)` keeps the **same** memory address. Items can be added or removed in place, but individual items can't be edited.

---

## 5. Adding items

```python
s = {1, 2, 3}

s.add(4)                 # {1, 2, 3, 4}      add ONE item
s.update([5, 6, 7])      # {1, 2, 3, 4, 5, 6, 7}   add MULTIPLE items
```
- `add()` → one item. `update()` → many items (any iterable).
- The memory address stays the same, which proves sets are mutable.

---

## 6. Deleting items

```python
s = {1, 2, 3, 4, 5}

del s            # ✅ deletes the whole set
del s[0]         # ❌ TypeError (no indexing)

s.discard(3)     # removes 3 if present (no error if missing)
s.remove(4)      # removes 4 (raises KeyError if missing)
s.pop()          # removes and returns an ARBITRARY item
s.clear()        # empties the set → set()
```

| Method | If item is missing |
|--------|--------------------|
| `discard(x)` | Nothing happens ✅ |
| `remove(x)` | **`KeyError`** ❌ |

- `pop()` takes **no argument** and removes some item. Since sets have no order, you can't predict which one.
- `clear()` empties but keeps the variable.

---

## 7. Operations on sets

- `+` (concatenation) and `*` (repetition) **don't work** on sets.
- **Loops** work:
  ```python
  for i in {1, 2, 3}:
      print(i)
  ```
- **Membership** works:
  ```python
  1 in {1, 2, 3}      # True
  ```

---

## 8. Functions on sets

### Common functions
```python
s = {3, 1, 4, 2}
len(s)                     # 4
min(s)                     # 1
max(s)                     # 4
sorted(s)                  # [1, 2, 3, 4]       (returns a LIST)
sorted(s, reverse=True)    # [4, 3, 2, 1]
```
`sorted()` returns a list, because a set can't hold an order.

### Set-specific operations (the maths part)
```python
s1 = {1, 2, 3, 4, 5}
s2 = {4, 5, 6, 7, 8}
```

| Operation | Method | Operator | Result |
|-----------|--------|----------|--------|
| **Union**: everything from both | `s1.union(s2)` | `s1 \| s2` | `{1, 2, 3, 4, 5, 6, 7, 8}` |
| **Intersection**: common items | `s1.intersection(s2)` | `s1 & s2` | `{4, 5}` |
| **Difference**: in `s1` but **not** in `s2` | `s1.difference(s2)` | `s1 - s2` | `{1, 2, 3}` |
| **Symmetric difference**: in either, **not both** | `s1.symmetric_difference(s2)` | `s1 ^ s2` | `{1, 2, 3, 6, 7, 8}` |

> Order matters for difference: `s1 - s2` ≠ `s2 - s1` (`{6, 7, 8}`).

### Relationship checks (all return `True`/`False`)
```python
s1.isdisjoint(s2)       # True if they have NO common items ("disjoint sets")
{1, 2}.issubset(s1)     # True: every item of {1,2} is in s1
s1.issuperset({1, 2})   # True: s1 contains all items of {1,2}
```
- **Disjoint:** no common items.
- **Subset:** all items of A are also in B.
- **Superset:** A contains all items of B.

More methods exist; type `s.` + Tab or use `dir(set)`.

---

## 9. Quick summary

| Rule | Detail |
|------|--------|
| Unique items | Duplicates removed |
| No indexing/slicing | Can't access by position |
| Immutable items only | No list/set/dict inside |
| Set is mutable | `add`, `update`, `remove`, `discard`, `pop`, `clear` |
| No `+`/`*` | Loops and `in` work |
| Math ops | `union`, `intersection`, `difference`, `symmetric_difference` |

---

## Extra Knowledge (not in the video)

### Set operators vs methods
- Operators (`| & - ^`) need **both sides to be sets**.
- Methods accept **any iterable**: `s1.union([9, 10])` works, `s1 | [9, 10]` doesn't.
- In-place versions: `s1 |= s2`, `s1 &= s2`, `s1 -= s2`, `s1 ^= s2` (also `update`, `intersection_update`, `difference_update`).
- Comparison operators: `s1 <= s2` (subset), `s1 < s2` (proper subset), `s1 >= s2` (superset).

### Speed: why sets are great
Membership tests (`x in s`) are **O(1) on average** because of hashing, versus O(n) for lists.
```python
big_list = list(range(1_000_000))
big_set = set(big_list)
999_999 in big_set      # near-instant; the list version scans linearly
```

### Hashable = can be in a set
An item must be **hashable** (have an unchanging hash value). Immutable built-ins are: `int`, `float`, `str`, `bool`, `tuple` (of hashables), `frozenset`, `bytes`. Lists, sets and dicts are not.

### `frozenset`: an immutable set
```python
fs = frozenset([1, 2, 3])
fs.add(4)                 # ❌ AttributeError
{fs, frozenset([4])}      # ✅ a frozenset CAN be inside a set, or be a dict key
```

### Order and equality quirks
- Sets are **unordered**: don't rely on iteration order. (Dictionaries, by contrast, keep insertion order since Python 3.7.)
- `1`, `1.0` and `True` are equal, so they collapse into one item: `{1, 1.0, True}` → `{1}`.
- Strings may print in a different order on each run (hash randomisation); small integers often look sorted but that's not guaranteed.

### Everyday uses
```python
set(names)                              # unique values
len(set(names))                         # count of unique items
list(set(L))                            # dedupe (order lost)
list(dict.fromkeys(L))                  # dedupe, keeping order
common = set(a) & set(b)                # common friends / shared tags
only_in_a = set(a) - set(b)
```

### Set comprehension
```python
squares = {x * x for x in range(5)}     # {0, 1, 4, 9, 16}
```

### Empty-set gotchas
```python
set()             # empty set (repr shows "set()")
{*()}             # also an empty set, but obscure; just use set()
```

---

## Tricky Parts / Gotchas

1. **`{}` is a dict, not a set.** Use `set()`.
2. **`s[0]` fails**: sets aren't indexable.
3. **`remove()` raises `KeyError` if the item is missing; `discard()` doesn't.**
4. **`pop()` removes an arbitrary item** (don't assume smallest/largest/last).
5. **A set can't contain a list or another set**, but it can contain a tuple (without lists inside).
6. **Converting to a list and back creates a NEW set**; it isn't editing.
7. **`sorted(set)` returns a list.**
8. **Set order is unpredictable** and shouldn't be relied on.
9. **`s1 - s2` ≠ `s2 - s1`** (difference isn't symmetric).
10. **Operators need sets on both sides** (`{1} | [2]` fails; `{1}.union([2])` works).
11. `{1, True}` is `{1}`; `{0, False}` is `{0}`.

---

## Interview Questions

1. **What is a set? Properties?** Unordered collection of unique, hashable items; mutable; no indexing.
2. **How do you create an empty set?** `set()`; `{}` creates a dict.
3. **Why can't a list be in a set?** Lists are unhashable (mutable).
4. **`remove` vs `discard`?** `remove` raises `KeyError` if missing; `discard` doesn't.
5. **Union vs intersection vs difference vs symmetric difference?** All / common / in first only / in either but not both.
6. **Why is membership checking faster in a set than in a list?** Hash table lookup, O(1) vs O(n).
7. **How do you remove duplicates from a list?** `set(L)` (order lost) or `dict.fromkeys(L)` (order kept).
8. **What is a `frozenset`?** An immutable, hashable set.
9. **Is a set ordered?** No.
10. **Is a set mutable? Are its elements?** The set is mutable; its elements must be immutable (hashable).
11. **What's the difference between `add` and `update`?** One item vs many (iterable).

---

### Practice ideas
1. Given two lists of friends, print the common friends and the friends only one person has.
2. Count how many unique words are in a sentence.
3. Check if one set is a subset of another.
4. Find items present in exactly one of two sets (symmetric difference).
5. Use a set to check whether a list has duplicates (`len(L) != len(set(L))`).
6. Time `x in list` vs `x in set` for a million items.

**Next video:** Dictionaries.
