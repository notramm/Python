# 100 Days of Python (CampusX) — Video 14: The `for` Loop

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: `range()`, sequences, how Python's `for` loop works, and `for` vs `while`.

---

## 1. Python's `for` is different

In C, C++ and Java, a `for` loop looks like this:

```c
for (int i = 0; i < 10; i++) {
    // code
}
```
It has an **initialisation**, a **condition** and an **increment**.

**Python's `for` loop has none of these.** It simply **goes through the items of something**, one at a time. It's simpler and more powerful.

To understand it you need two things first: **`range()`** and **sequences**.

---

## 2. The `range()` function

A built-in function that **generates integers in a given range**.

```python
range(1, 11)
```
Displays as `range(1, 11)`. To see the numbers, convert to a list:

```python
print(list(range(1, 11)))     # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

> **The start is included, the stop is NOT.** `range(1, 11)` gives 1 to **10**, not 11.

### Three parameters: `range(start, stop, step)`

| Call | Meaning | Result |
|------|---------|--------|
| `range(5)` | Only **stop** given → start is 0 | `0, 1, 2, 3, 4` |
| `range(1, 11)` | start and stop | `1 … 10` |
| `range(1, 11, 2)` | start, stop, **step** | `1, 3, 5, 7, 9` |
| `range(10, 0, -1)` | negative step → counts **backwards** | `10, 9, 8, … 1` |

- **One argument** → it's the **stop** value (start defaults to 0).
- **Step** = the gap between consecutive numbers (default 1).
- A **negative step** produces a countdown, so `start` must be bigger than `stop`.

```python
print(list(range(5)))             # [0, 1, 2, 3, 4]
print(list(range(1, 11, 2)))      # [1, 3, 5, 7, 9]
print(list(range(10, 0, -1)))     # [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

---

## 3. Sequences (and iterables)

A **sequence** is something that holds items **in order**.

- A **string** is a sequence of characters: `"Kolkata"` → K, o, l, k, a, t, a
- A **list** is a sequence of items: `["Kolkata", "Delhi", "Mumbai"]`
- A **tuple** is a sequence too, and so is `range()`.
- You can also loop over **sets** and **dictionaries** (the video says dictionaries behave slightly differently; covered later).

The `for` loop's job is to go through any such object **item by item**.

---

## 4. How the `for` loop works

```python
for i in range(1, 11):
    print(i)
```
Output: `1 2 3 4 5 6 7 8 9 10` (each on its own line)

- First pass: `i = 1`; second pass: `i = 2`; … last pass: `i = 10`. Then the loop ends.
- **No end condition, no increment, no termination check.** Python handles all of that behind the scenes. When the items run out, the loop stops by itself.

### Looping over a string
```python
for i in "Kolkata":
    print(i)
```
Prints `K`, `o`, `l`, `k`, `a`, `t`, `a`, one per line. `i` takes each character in turn.

### Backward counting
```python
for i in range(10, 0, -1):
    print(i)       # 10, 9, ... 1
```

### Other types
You can loop over a list, a tuple, a set, or a dictionary in the same way:
```python
for city in ["Kolkata", "Delhi", "Mumbai"]:
    print(city)
```

> General form: `for variable in <iterable object>:`. Give it a `range`, a string, a list… and it iterates.

---

## 5. `for` vs `while`: which to use?

| Use **`for`** when… | Use **`while`** when… |
|---------------------|-----------------------|
| You **know in advance** how many times the loop will run | You **don't know** how many times it will run |
| You're going through a sequence/collection | The loop depends on a condition |
| Example: print a table, loop through a list | Example: the guessing game (Video 13) |

The instructor's rule: *if you know beforehand → `for`; if you don't → `while`.* Many cases can use either, but `while` is more important when the count is unknown.

---

## 6. Quick summary

- Python's `for` has **no initialise / condition / increment**; it iterates over an object.
- `range(start, stop, step)`: stop is **excluded**; one argument = stop.
- Negative step = backwards.
- `for x in <sequence or range>:` works for strings, lists, tuples, sets, dicts.
- Known iterations → `for`. Unknown → `while`.

---

## Extra Knowledge (not in the video)

### `range` is lazy (very memory-efficient)
In Python 3, `range(1, 10**9)` doesn't create a billion numbers. It stores only start, stop and step and computes values on demand. It supports `len()`, indexing (`range(10)[3]`), slicing, and fast membership tests (`5 in range(10)`).
```python
r = range(0, 10, 2)
print(len(r), r[2], 6 in r)     # 5 4 True
```

### Iterable vs iterator vs sequence (precise terms)
- **Iterable:** anything you can loop over (list, str, tuple, set, dict, range, file, generator).
- **Sequence:** an *ordered, indexable* iterable (`str`, `list`, `tuple`, `range`, `bytes`).
- **Sets** and **dicts** are iterable but **not** sequences (sets are unordered; dicts keep insertion order since Python 3.7 but are mappings).
- Under the hood: `for` calls `iter()` on the object and keeps calling `next()` until `StopIteration`.

### Looping over a dict
```python
person = {"name": "Ram", "age": 25}
for key in person:                  # keys by default
    print(key)
for key, value in person.items():   # key and value together
    print(key, value)
```

### Useful companions
```python
for index, city in enumerate(cities):        # index + item
for a, b in zip(list1, list2):                # two lists side by side
for x in reversed(items):                     # reverse order
for x in sorted(items):                       # sorted order
```
Prefer `enumerate` to `for i in range(len(items))` (the latter is an anti-pattern in Python).

### `for ... else`
```python
for n in range(2, 10):
    if n == 7:
        print("found")
        break
else:
    print("not found")     # runs only if the loop finished WITHOUT break
```

### `_` as a throwaway variable
```python
for _ in range(3):
    print("Hello")         # when you don't need the counter
```

### Other points
- `range()` accepts **only integers**. `range(0, 1, 0.1)` → `TypeError`. Use a list comprehension or `numpy.arange` for float steps.
- `range(5, 1)` is **empty** (default step is +1), so you need `-1` as the step.
- The loop variable **remains available after the loop** (it keeps the last value).
- A `for` loop over an **empty** sequence runs zero times.
- `sum(range(1, 101))` → `5050` (no loop needed).
- Python's `for` is conceptually like Java's *enhanced for / for-each*.

---

## Tricky Parts / Gotchas

1. **Stop is exclusive:** `range(1, 10)` gives 1–9. For 1–10 use `range(1, 11)`.
2. **Reassigning the loop variable inside the loop doesn't affect iteration:**
   ```python
   for i in range(3):
       i = 100          # next pass i is still 1, then 2
   ```
3. **Don't add/remove items from a list while looping over it.** You'll skip items. Loop over a copy: `for x in items[:]`.
4. `range(10, 0)` prints nothing; you need `range(10, 0, -1)`.
5. **Strings loop over characters**, not words. Use `.split()` for words.
6. `for i in 5:` → `TypeError: 'int' object is not iterable`. Use `range(5)`.
7. `print(range(5))` shows `range(0, 5)`, not the numbers. Wrap it with `list()`.
8. If you want the last value `10` included but wrote `range(1, 10)`, it's a classic off-by-one bug.

---

## Interview Questions

1. **How is Python's `for` different from C's?** It iterates over items of an iterable; no init/condition/increment.
2. **What does `range(2, 10, 3)` give?** `2, 5, 8`.
3. **Is the stop value of `range` inclusive?** No.
4. **Is `range()` a list in Python 3?** No, it's a lazy immutable sequence object.
5. **What is an iterable? An iterator?** Iterable: can be looped over. Iterator: object that produces the next item (`__next__`).
6. **When do you use `for` vs `while`?** Known count/collection → `for`; unknown/condition-driven → `while`.
7. **What does `for...else` do?** The `else` runs if the loop finishes without `break`.
8. **How do you loop with index and value?** `enumerate()`.
9. **What happens when you loop over a dictionary?** You get its keys.
10. **How do you loop backwards?** `range(n, -1, -1)` or `reversed(...)`.

---

### Practice ideas
1. Print the table of any number using a `for` loop (compare with the `while` version from Video 12).
2. Print numbers from 20 down to 1, then even numbers from 2 to 20.
3. Count how many vowels are in a word by looping over its characters.
4. Sum all numbers from 1 to 100 using a `for` loop, then with `sum(range(...))`.
5. Loop through a list of cities and print each in upper case.

**Next video:** Nested loops (transcript not yet available; see note at the end of this batch).
