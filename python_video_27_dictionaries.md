# 100 Days of Python (CampusX) — Video 27: Dictionaries

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> After this video, all of Python's core data types are covered. Next come programs on them, then functions.

---

## 1. What is a dictionary?

> A **dictionary** stores data as **key : value pairs**. You look up a **value** by its **key**.

```python
person = {"name": "Ram", "gender": "male", "weight": 70}
```
- Real-life analogy: a phone book: look up a **name** (key) to get a **number** (value).
- Written with curly brackets `{ }`, each item as `key: value`.

---

## 2. The 4 rules of dictionaries

1. **No indexing.** You can't use `d[0]`. Items are accessed **by key**.
2. **A dictionary is mutable.** You can edit, add and delete items.
3. **Keys must be immutable** (strings, numbers, tuples...). **Values can be anything** (mutable or immutable).
4. **Keys are unique.** A key can't repeat; if it does, the **last value wins**.

### Which types are mutable vs immutable? (cheat sheet)

| **Mutable** (can be changed in place) | **Immutable** (can't be changed in place) |
|---------------------------------------|-------------------------------------------|
| `list`, `set`, `dict` | `int`, `float`, `bool`, `complex`, `str`, `tuple` |

Why it matters: if you "change" an immutable value, Python creates a new object at a new memory address. Mutable values change at the same address.

### Proof of rule 4 (duplicate keys)
```python
d = {"name": "Nitish", "name": "Rahul"}
print(d)           # {'name': 'Rahul'}
```
The same key was repeated, so only one entry remains, with the **most recent** value.

---

## 3. Creating dictionaries

```python
# Empty dictionary
d = {}
d = dict()

# Simple dictionary
d = {"name": "Ram", "gender": "male"}

# Immutable keys of other types (e.g. tuple)
d = {(1, 2, 3): 1}          # ✅ tuple key (immutable)
d = {[1, 2, 3]: 1}          # ❌ TypeError: unhashable type: 'list'

# 2D (nested) dictionary: dictionary inside a dictionary
student = {
    "name": "Rohit",
    "college": "CalU",
    "sem": 4,
    "subjects": {
        "dsa": 50,
        "maths": 67,
        "english": 34
    }
}

# Using sequences of pairs / keyword arguments
d = dict([("name", "Ram"), ("age", 25)])
d = dict(name="Ram", age=25)
```
- **Values can be anything**: numbers, strings, lists, tuples, other dictionaries.
- You can nest to any depth (2D, 3D...) for **values**, but remember **keys** must be immutable.

---

## 4. Accessing items

Use the **key** inside square brackets:

```python
person = {"name": "Ram", "gender": "male"}

person["name"]         # 'Ram'
person["weight"]       # ❌ KeyError: 'weight'   (key not present)
```

### Nested dictionaries: chain the keys
```python
student["subjects"]["maths"]      # 67
```
Go from the outside in, one key at a time (just like nested lists).

### The `get()` method
```python
person.get("name")             # 'Ram'
person.get("weight")           # None   (no error!)
person.get("weight", 0)        # 0      (custom default)
```
The instructor mostly uses `d["key"]`, but `get()` is safer when a key may be missing. It's worth knowing for exams and real code (see extra knowledge).

---

## 5. Editing items

Assign a new value to an existing key:

```python
person["name"] = "Shyam"

student["subjects"]["maths"] = 60     # nested edit
```
Same syntax as accessing, with `=` and a new value. Works for flat and nested dictionaries.

---

## 6. Adding new items

Assign to a **new key**; if the key doesn't exist, Python adds it:

```python
person["age"] = 25
print(person)       # {'name': 'Shyam', 'gender': 'male', 'age': 25}

student["subjects"]["science"] = 80    # new subject in the nested dictionary
```
So **editing and adding use the same syntax**: if the key exists, its value is updated; if not, a new pair is created.

---

## 7. Deleting items

```python
person = {"name": "Ram", "gender": "male", "age": 25, "weight": 70}

del person              # ✅ deletes the whole dictionary
del person["age"]       # delete one key-value pair
person.pop("weight")    # removes that key and RETURNS its value
person.popitem()        # removes the LAST inserted pair (returns it as a tuple)
person.clear()          # empties the dictionary → {}
```
- `del d[key]` and `pop(key)` both remove a pair by key; `pop` also gives you the value back.
- Works on nested values too: `del student["subjects"]["dsa"]`.

---

## 8. Operations on dictionaries

- `+` and `*` **don't work** on dictionaries.
- **Loops** work, but they go over the **keys**:
  ```python
  for key in person:
      print(key, person[key])       # key and its value
  ```
- **Membership** (`in`) checks **keys only**, not values:
  ```python
  "name" in person         # True   (a key)
  "Ram" in person          # False  (it's a value, not a key)
  ```
  To check a value: `"Ram" in person.values()`.

---

## 9. Functions on dictionaries

```python
d = {"b": 2, "a": 1, "c": 3}

len(d)               # 3       number of key-value pairs
min(d)               # 'a'     smallest KEY (lexicographic for strings)
max(d)               # 'c'     largest KEY
sorted(d)            # ['a', 'b', 'c']       sorted KEYS, returned as a list
sorted(d, reverse=True)
```
- `min`, `max`, `sorted` operate on the **keys**. They work when keys are comparable (all numbers or all strings).

### Dictionary-specific methods
```python
d.items()      # dict_items([('b', 2), ('a', 1), ('c', 3)])   all key-value pairs
d.keys()       # dict_keys(['b', 'a', 'c'])
d.values()     # dict_values([2, 1, 3])
```
Looping with both key and value:
```python
for key, value in d.items():
    print(key, value)
```
More methods: `update()`, `copy()`, `setdefault()`, `fromkeys()`: use `dir(dict)` or `help(dict.update)`.

---

## 10. Quick summary

| Task | Syntax |
|------|--------|
| Create | `{k: v}`, `dict()` |
| Access | `d[key]`, `d.get(key)` |
| Edit | `d[key] = new` |
| Add | `d[new_key] = value` |
| Delete | `del d[key]`, `pop(key)`, `popitem()`, `clear()` |
| Loop | `for k in d`, `for k, v in d.items()` |
| Membership | `key in d` (keys only) |
| Functions | `len`, `min`, `max`, `sorted` (on keys), `items`, `keys`, `values` |

---

## Extra Knowledge (not in the video)

### Dictionaries keep insertion order (Python 3.7+)
Items come out in the order you added them (this is guaranteed by the language). So `popitem()` removes the **most recently added** pair, and `reversed(d)` works (3.8+). Equality ignores order though: `{"a": 1, "b": 2} == {"b": 2, "a": 1}` is `True`.

### Keys must be **hashable**
"Immutable" is the beginner's version; the precise rule is **hashable**. Strings, numbers, bools and tuples **of hashable items** qualify; lists, sets, dicts don't.
```python
{(1, [2]): "x"}      # ❌ TypeError: tuple contains a list → unhashable
```
Dictionaries are hash tables, which is why key lookup is **O(1) on average**: extremely fast even with millions of items.

### `d[key]` vs `d.get(key)`
- `d[key]` → `KeyError` if missing (good when a missing key is a bug).
- `d.get(key, default)` → no error (good for optional data).
- `d.setdefault(key, default)` → returns the value, and **inserts** the default if the key was missing.

### Merging dictionaries (modern syntax)
```python
a = {"x": 1}
b = {"y": 2, "x": 99}
merged = a | b            # {'x': 99, 'y': 2}  (Python 3.9+; right side wins)
a |= b                    # in-place merge
{**a, **b}                # works in older 3.x as well
a.update(b)               # in-place
```

### Useful patterns
```python
# Loop over keys, values or both
for k in d: ...
for v in d.values(): ...
for k, v in d.items(): ...

# Key with the largest value
scores = {"a": 50, "b": 90, "c": 70}
max(scores, key=scores.get)                   # 'b'

# Sort by value
sorted(scores.items(), key=lambda kv: kv[1])  # [('a', 50), ('c', 70), ('b', 90)]

# Count things
from collections import Counter
Counter("banana")                             # Counter({'a': 3, 'n': 2, 'b': 1})

# Dictionary comprehension
{x: x * x for x in range(4)}                  # {0: 0, 1: 1, 2: 4, 3: 9}

# Group / default values
from collections import defaultdict
groups = defaultdict(list)
groups["fruits"].append("apple")
```

### Views are live
`d.keys()`, `d.values()`, `d.items()` return **view objects** that reflect later changes to `d`. Wrap in `list(...)` if you need a snapshot.

### Copying
`d2 = d` is not a copy (same object). Use `d.copy()` (shallow) or `copy.deepcopy(d)` for nested dictionaries.

### Dictionaries ↔ JSON
Python dictionaries map almost directly to JSON objects. `import json; json.dumps(d)` converts a dictionary to a JSON string and `json.loads(s)` converts back. You'll use this constantly with APIs.

### All four collections at a glance (great for interviews)

| | **List** | **Tuple** | **Set** | **Dict** |
|---|----------|-----------|---------|----------|
| Brackets | `[ ]` | `( )` | `{ }` | `{k: v}` |
| Ordered / indexable | ✅ / ✅ | ✅ / ✅ | ❌ / ❌ | ✅ (insertion order) / by key |
| Mutable | ✅ | ❌ | ✅ | ✅ |
| Duplicates | ✅ | ✅ | ❌ | keys ❌ (values ✅) |
| Items must be hashable | no | no | **yes** | **keys: yes** |
| Hashable itself | ❌ | ✅ (if items are) | ❌ | ❌ |
| Empty | `[]` | `()` | `set()` | `{}` |
| Single item | `[1]` | `(1,)` | `{1}` | `{"a": 1}` |

---

## Tricky Parts / Gotchas

1. **`d[key]` raises `KeyError` for a missing key**; use `get()` if the key may be absent.
2. **`in` checks keys, not values.** `"Ram" in person` is `False`.
3. **Duplicate keys silently overwrite:** the last value wins, with no error.
4. **Lists can't be keys**, but they can be values.
5. **Loops and `min`/`max`/`sorted` work on keys**, not values.
6. **Don't change a dictionary's size while looping over it** → `RuntimeError: dictionary changed size during iteration`. Loop over `list(d)` or `d.copy()` instead.
7. **`{}` is an empty dict, not an empty set.**
8. **Keys `1`, `1.0` and `True` collide** (they're equal and hash the same): `{1: "a", True: "b"}` → `{1: "b"}`.
9. **`dict.fromkeys(keys, [])` shares one list across all keys**: use a comprehension instead: `{k: [] for k in keys}`.
10. **`d2 = d` creates an alias, not a copy.**
11. `pop(key)` on a missing key raises `KeyError` (use `pop(key, default)`).

---

## Interview Questions

1. **How is a dictionary different from a list?** Key-based access vs index-based; keys are unique and hashable.
2. **Can any object be a key?** Only hashable (immutable-in-practice) ones.
3. **What happens with duplicate keys?** The last value replaces the earlier one.
4. **Are dictionaries ordered?** Insertion-ordered since Python 3.7 (guaranteed).
5. **How do you merge two dictionaries?** `a | b` (3.9+), `{**a, **b}`, or `a.update(b)`.
6. **`d[key]` vs `d.get(key)`?** Error vs `None`/default for missing keys.
7. **How do you loop over keys and values?** `for k, v in d.items()`.
8. **What is the time complexity of key lookup?** O(1) on average (hash table).
9. **How do you find the key with the maximum value?** `max(d, key=d.get)`.
10. **How do you delete a key safely?** `d.pop(key, None)`.
11. **Mutable vs immutable types; examples?** See the cheat sheet in section 2.
12. **Why can a tuple be a key but a list can't?** Tuples are hashable (if their items are); lists are mutable and unhashable.
13. **How do you check whether a value (not key) exists?** `value in d.values()`.

---

### Practice ideas
1. Create a dictionary of 5 students and their marks; print the topper (`max(d, key=d.get)`).
2. Count how many times each word appears in a sentence using a dictionary (then try `Counter`).
3. Build the nested student dictionary from this video and print the average of his subject marks.
4. Merge two dictionaries; show what happens with duplicate keys.
5. Invert a dictionary (`{v: k for k, v in d.items()}`).
6. Write a program that asks the user for a key and prints its value, or "not found" using `get`.

**Next:** programs on lists, tuples, sets and dictionaries, then Functions.
