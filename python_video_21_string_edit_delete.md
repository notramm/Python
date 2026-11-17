# 100 Days of Python (CampusX) — Video 21: Editing and Deleting Strings

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Key idea of this video: **strings are immutable**.

---

## 1. Editing a string

Suppose we have:
```python
s = "hello world"
```
and we want to change the first character `h` to `x`.

```python
s[0] = "x"
```
**Result:**
```
TypeError: 'str' object does not support item assignment
```

> Python is telling you: **you cannot edit a string.**

---

## 2. Immutability: the most important concept here

- **Mutable** = can be changed after creation. **Immutable** = **cannot** be changed once created.
- (The word comes from *mutation*, meaning change.)
- Python **strings are immutable**: after a string is created you cannot change it, and you cannot add new characters into it.

The instructor says a whole video on mutability will come later in the course. For now, **just remember: strings are immutable.**

### What you CAN do: reassign the variable
```python
s = "hello world"
s = "world"          # ✅ allowed: s now points to a different string
print(s)             # world
```
You haven't changed the old string; you've made the **variable** point to a brand-new string.

### What you CAN'T do
```python
s[0] = "x"           # ❌ can't edit a character
s[0:5] = "HELLO"     # ❌ can't replace part of it
s[5] = "!"           # ❌ can't "add" a character in place
```
Both **editing** and **adding** characters to an existing string are not allowed.

---

## 3. Deleting a string

### Delete the whole string: `del`
```python
s = "hello world"
del s
print(s)       # ❌ NameError: name 's' is not defined
```
- `del` followed by the variable name deletes that variable. Afterwards it no longer exists.

### Delete part of a string: NOT allowed
```python
s = "hello world"
del s[0]
```
**Result:**
```
TypeError: 'str' object doesn't support item deletion
```
- Removing one character would **change** the existing string (a mutation), which isn't allowed for immutable types.

### Summary of what's allowed

| Action | Allowed? |
|--------|----------|
| Reassign the variable to a new string | ✅ |
| Delete the entire string/variable (`del s`) | ✅ |
| Edit a character (`s[0] = "x"`) | ❌ |
| Add a character into the string | ❌ |
| Delete part of the string (`del s[0]`) | ❌ |

---

## 4. Quick summary

- Strings are **immutable**: no editing, no adding characters, no partial deletion.
- You can **reassign** a variable or **delete the whole variable** with `del`.
- Immutability is a core concept that applies to other types too (explained in a later video).

---

## Extra Knowledge (not in the video)

### "Changing" a string really means creating a new one
```python
s = "hello world"

# replace the first character using slicing + concatenation
s2 = "X" + s[1:]              # 'Xello world'

# replace by value
s3 = s.replace("hello", "HELLO")   # 'HELLO world'

# convert to a list, edit, and join back (mutable trick)
chars = list(s)
chars[0] = "H"
s4 = "".join(chars)           # 'Hello world'

print(s)                      # 'hello world'   ← original untouched
```

### Deleting characters (by building a new string)
```python
s = "hello world"
s_without_first = s[1:]               # 'ello world'
s_without_l = s.replace("l", "")      # 'heo word'
s_removed_index3 = s[:3] + s[4:]      # remove index 3
```

### Proof that "editing" creates a new object
```python
s = "abc"
print(id(s))
s = s + "d"
print(id(s))        # a different id → a new string object was created
```

### Why are strings immutable?
- **Hashable:** immutable values can be used as **dictionary keys** and set members.
- **Safe sharing:** many variables can refer to the same string without one changing it for the others (Python reuses/"interns" some strings to save memory).
- **Security and thread-safety:** nobody can alter a string behind your back.
- **Performance:** hash values can be cached.

### Immutable vs mutable types (quick list)
| Immutable | Mutable |
|-----------|---------|
| `int`, `float`, `complex`, `bool`, `str`, `tuple`, `frozenset`, `bytes`, `None` | `list`, `dict`, `set`, `bytearray` |

### Performance tip: building strings in loops
Repeated `+=` creates a new string each time:
```python
result = ""
for word in words:
    result += word          # can be slow for very large loops
```
Prefer:
```python
result = "".join(words)
```
Or collect pieces in a list and join once. For a mutable text buffer use `io.StringIO` or `bytearray` (for bytes).

### `del` removes the name, not (necessarily) the data
`del s` removes the **variable name**. The string object itself is cleaned up by Python's memory manager only when nothing else refers to it.
```python
a = "hello"
b = a
del a
print(b)        # hello   (still alive via b)
```

---

## Tricky Parts / Gotchas

1. **`s[0] = "x"` fails, but `s = "x" + s[1:]` works.** The second line makes a **new** string.
2. **`s.upper()` does not change `s`** (see Video 23). You must reassign: `s = s.upper()`.
3. **`del s` deletes the variable**, so using `s` later gives `NameError`; it does not just "clear the text". To empty it, use `s = ""`.
4. **Lists behave differently:** `lst[0] = "x"` and `del lst[0]` work because lists are mutable. Don't mix up the rules.
5. Methods like `replace`, `strip`, `lower` all **return new strings**; forgetting to store the result is a very common bug.
6. Two variables with the same string are safe: changing one (by reassigning) never affects the other.

---

## Interview Questions

1. **Are Python strings mutable?** No, immutable.
2. **Why does `s[0] = "x"` raise an error?** Strings don't support item assignment because they're immutable.
3. **How do you "modify" a string, then?** Create a new string via slicing/concatenation, `replace()`, or convert to a list, edit and `"".join()`.
4. **What does `del s` do?** Deletes the name `s` (and the object if nothing else refers to it).
5. **Why are strings immutable in Python?** Hashability, safe sharing, security, performance (caching/interning).
6. **Can you delete a single character from a string in place?** No.
7. **Difference between mutable and immutable types? Give examples.** See table above.
8. **Why is `"".join(list)` better than `+=` in a loop?** `+=` builds many intermediate strings; `join` builds the result once.

---

### Practice ideas
1. Try `s[0] = "x"` and `del s[0]` yourself and read the error messages.
2. Change the first letter of `"python"` to uppercase **without** `.upper()` or `.capitalize()` (hint: slicing + concatenation).
3. Remove all spaces from a sentence.
4. Use `id()` to show that `s += "x"` creates a new object.
5. Convert a string to a list, reverse the list in place, and join it back.

**Next video:** String operations.
