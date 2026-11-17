# 100 Days of Python (CampusX) — Video 20: String Indexing and Slicing

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: extracting one character (**indexing**) or a part of a string (**slicing**).

---

## 1. Indexing: get one character

A string is a **sequence of characters**, and every character has a **position (index)**.

```python
s = "hello"
```

| Character | h | e | l | l | o |
|-----------|---|---|---|---|---|
| **Positive index** | 0 | 1 | 2 | 3 | 4 |
| **Negative index** | -5 | -4 | -3 | -2 | -1 |

```python
print(s)       # hello
print(s[0])    # h
print(s[1])    # e
print(s[4])    # o
print(s[5])    # ❌ IndexError: string index out of range
```
Indexing uses **square brackets** `[ ]` around the index. Going beyond the last position is not allowed.

### Two kinds of indexing

**Positive indexing:** starts at **0** and moves **left → right**.

**Negative indexing:** starts at **-1** (the last character) and moves **right → left**.
```python
print(s[-1])   # o   (last character)
print(s[-2])   # l
print(s[-5])   # h
print(s[-6])   # ❌ IndexError
```

> Unlike many languages, Python supports **both**. Negative indexing is extremely handy when you don't know the string's length and want the **last character**: `name[-1]` instead of `name[len(name) - 1]`.

---

## 2. Slicing: get a part of a string

Indexing gives one character; **slicing** gives **several characters** using a range.

```python
s = "hello world"
```
Index map:

| h | e | l | l | o | (space) | w | o | r | l | d |
|---|---|---|---|---|---|---|---|---|---|---|
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |

### Syntax
```python
s[start : stop : step]
```
- **start**: where to begin (**included**)
- **stop**: where to end (**NOT included**)
- **step**: gap between picked characters (default 1)

### Basic slice
```python
print(s[0:5])     # hello       (indexes 0,1,2,3,4. Stop 5 is excluded)
```
To get "hello" you give start `0` and stop `5` (one more than the last index you want).

### Leaving out values

| Slice | Meaning | Result for `"hello world"` |
|-------|---------|----------------------------|
| `s[2:]` | From index 2 **to the end** | `llo world` |
| `s[:5]` | From the **beginning** up to (not incl.) 5 | `hello` |
| `s[:]` | The **whole** string | `hello world` |

- Missing **start** → starts at the beginning (0).
- Missing **stop** → goes to the end.

### Using a step
```python
print(s[0:10:2])    # hlowr    (indexes 0,2,4,6,8: take one, skip one)
print(s[::2])       # hlowrd   (whole string, every 2nd character)
```
Example idea: from `h e l l o _ w o r l d` take `h`, skip `e`, take `l`, skip `l`, take `o`, skip space, take `w`, skip `o`, take `r`...

### Negative slicing and reversing
With a **negative step**, the slice moves **right → left**.

```python
print(s[-1:-6:-1])   # dlrow   (from last char backwards; stop -6 excluded)
print(s[::-1])       # dlrow olleh   ← the whole string reversed!
```

Rules:
- With a **positive step** you move left → right, so start must be **before** stop.
- With a **negative step** you move right → left, so start must be **after** stop.
- Mismatched direction gives an **empty string**: `s[0:6:-1]` → `''`.

> **`s[::-1]` is the standard way to reverse a string** in Python. With no start/stop and a step of `-1`, you go from the very end to the very beginning.

---

## 3. Quick summary

- **Indexing:** `s[i]` gives one character. Positive indexes start at 0 (left → right), negative start at -1 (right → left).
- **Out-of-range index** → `IndexError`.
- **Slicing:** `s[start:stop:step]` gives a substring; stop excluded.
- Omit start/stop to mean "from the beginning" / "to the end".
- Negative step moves backwards; `s[::-1]` reverses.
- The same ideas apply to **lists, tuples** and other sequences.

---

## Extra Knowledge (not in the video)

### Slices never raise `IndexError`
Out-of-range slice limits are quietly clipped:
```python
s = "hello"
print(s[0:100])    # hello
print(s[10:20])    # ''   (empty, no error)
```
But single indexing (`s[10]`) **does** raise `IndexError`.

### Handy slicing patterns
```python
s[:3]          # first 3 characters
s[-3:]         # last 3 characters
s[1:-1]        # everything except first and last
s[::-1]        # reverse
s[::2]         # every other character, from the first
s[1::2]        # every other character, from the second
s[:n] + s[n:]  # always equals s (split at n and rejoin)
```

### Palindrome check (classic interview question)
```python
def is_palindrome(text):
    cleaned = text.lower()
    return cleaned == cleaned[::-1]

print(is_palindrome("Madam"))   # True
```

### Slices are copies
`s[a:b]` creates a **new string**; the original isn't changed (strings are immutable). Slicing cost is proportional to the slice length.

### The `slice` object
```python
first_five = slice(0, 5)
print("hello world"[first_five])    # hello
```
Useful for naming reusable slices.

### How defaults work with a negative step
With `step < 0`, the default start is the **last** character and the default stop is **before the first**. That's why `s[::-1]` covers the whole string reversed.

### Other facts
- `step` cannot be `0`: `s[::0]` → `ValueError`.
- Equivalent: `s[-1]` is the same as `s[len(s) - 1]`.
- Length of `s[a:b]` (positive indices within range) is `b - a`.
- Slicing works on **code points**, not on "visible characters". Some emoji or accented characters built from several code points can be split incorrectly.
- Indexing a string gives a **string** (length 1), not a char type: `type("hi"[0])` → `str`.

---

## Tricky Parts / Gotchas

1. **Stop is exclusive**: `s[0:5]` stops before index 5. To include index 5, use `s[0:6]`.
2. `s[5]` on a 5-character string → `IndexError` (valid indexes 0–4).
3. A negative step with a start smaller than stop returns `''`.
4. `s[-1:-6:-1]` vs `s[-6:-1]`: the first goes backwards (`dlrow`), the second goes forwards (`o worl`).
5. Using `s[-0]` is just `s[0]` (negative zero doesn't exist).
6. Assigning to an index/slice fails (`s[0] = "x"` → `TypeError`). See Video 21.
7. `s[1:3] = "xy"` also fails for strings (it works for lists).

---

## Interview Questions

1. **How do you reverse a string in Python?** `s[::-1]`.
2. **How do you get the last character?** `s[-1]`.
3. **What does `s[2:5]` return for `"abcdefg"`?** `'cde'`.
4. **What does `s[::2]` do?** Takes every second character.
5. **What happens if the slice range is out of bounds?** No error; it's clipped. An out-of-range *index* raises `IndexError`.
6. **Is the stop index included in a slice?** No.
7. **Does slicing modify the original string?** No; it returns a new one.
8. **What does `s[::-1]` do mathematically?** Starts from the last character and moves left by 1 until the first character.

---

### Practice ideas
1. For `s = "Python Programming"` print: the first 6 chars, the last 11 chars, every 3rd char, the reversed string.
2. Extract just `"Programming"` using a **negative** slice.
3. Check if a user-entered word is a palindrome.
4. Extract the domain from `"user@gmail.com"` using slicing (hint: find the `@` position with `.find`, next video group).

**Next video:** Editing and deleting strings.
