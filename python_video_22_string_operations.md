# 100 Days of Python (CampusX) — Video 22: Operations on Strings

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: which operators work on strings, and how.

---

## 1. Which operators work on strings?

Not all operators apply to strings. Available ones:

| Operator group | Works on strings? |
|----------------|-------------------|
| Arithmetic | **Only `+` and `*`** |
| Relational (comparison) | ✅ Yes |
| Logical | ✅ Yes (with special behaviour) |
| Loops (`for`) | ✅ Yes (iterate over characters) |
| Membership (`in`, `not in`) | ✅ Yes |

---

## 2. Arithmetic operators

### `+`: concatenation (joining strings)
```python
print("hello" + "world")           # helloworld
print("hello" + " " + "world")     # hello world
print("a" + "b" + "c" + "d")       # abcd
```
- Add as many strings as you like by chaining `+`.
- To get a space, join a `" "` in between.
- Very common in real applications (building messages, names, etc.).

### `*`: repetition
```python
print("hello" * 3)       # hellohellohello
print("*" * 20)          # ********************
print("=" * 30)          # ==============================
```
Useful for printing **separators, patterns and formatted output**.

### Others don't work
```python
"hello" - "h"     # ❌ TypeError
"hello" / 2       # ❌ TypeError
```
Subtraction, division etc. are not defined for strings.

---

## 3. Relational operators: `==`, `!=`, `>`, `<`, `>=`, `<=`

### Equality
```python
print("hello" == "hello")     # True
print("hello" == "world")     # False
print("hello" != "world")     # True
```
Comparison is **case-sensitive**: `"Hello" == "hello"` is `False`.

### Greater / less than: **lexicographical** (dictionary) order
```python
print("Mumbai" > "Pune")      # False
print("Goa" < "Kolkata")      # True
```
- You might expect "Mumbai" > "Pune" because Mumbai is longer. **It's not about length.**
- Strings are compared **like words in a dictionary**: compare character by character from the left.
  - Compare `M` vs `P`: `M` comes **before** `P` in the alphabet → "Mumbai" is the **smaller** one.
- **Rule of thumb:** the word that appears **earlier** in a dictionary is **smaller**; the one that appears **later** is **greater**.
  - `"Goa" < "Kolkata"` → `G` comes before `K` → `True`.

### Capital vs small letters
```python
print("A" < "a")       # True
print("a" < "A")       # False
```
Capital letters come **before** small letters (because of their code values: `A` = 65, `a` = 97).
So `"Zebra" < "apple"` is `True`!

---

## 4. Logical operators: `and`, `or`, `not`

Python treats strings as follows:
- **Empty string `""` → `False`** (falsy)
- **Any non-empty string → `True`** (truthy)

### `and`
```python
print("hello" and "world")    # world
print("" and "world")         # ''  (the empty string)
```
- `and` gives back the **first falsy** value; if none is falsy, it gives back the **last** value.
- For `"hello" and "world"` both are truthy, so the result is the last one: `"world"`.

### `or`
```python
print("hello" or "world")     # hello
print("" or "world")          # world
```
- `or` gives back the **first truthy** value.
- `"hello"` is truthy, so Python doesn't even look at `"world"`.

### `not`
```python
print(not "hello")     # False   (non-empty is True → not True = False)
print(not "")          # True    (empty is False → not False = True)
```
`not` always returns a real `True`/`False`.

> Don't be confused when you see strange-looking code in other people's projects. Just remember: **empty string = False, non-empty string = True.**

---

## 5. Looping over strings

A string is a sequence, so a `for` loop picks one character per iteration:

```python
for ch in "hello world":
    print(ch)          # h, e, l, l, o, (space), w, o, r, l, d  (one per line)
```

Combine with **slicing** (Video 20) to customise what you loop over:
```python
for ch in "hello world"[2:6]:     # llo 
    print(ch)

for ch in "hello world"[::-1]:    # reversed string, character by character
    print(ch)
```

---

## 6. Membership operators: `in` and `not in`

Check whether one string (or character) is **inside** another:
```python
print("D" in "Delhi")          # True
print("Z" in "Delhi")          # False
print("Del" in "Delhi")        # True   (works for substrings too)
print("Z" not in "Delhi")      # True
```
- Case-sensitive: `"d" in "Delhi"` is `False`.

---

## 7. Quick summary

| Operation | Example | Result |
|-----------|---------|--------|
| Concatenation | `"a" + "b"` | `"ab"` |
| Repetition | `"ab" * 3` | `"ababab"` |
| Equality | `"a" == "a"` | `True` |
| Ordering | `"Mumbai" > "Pune"` | `False` (dictionary order) |
| Logical | `"hi" and "yo"` | `"yo"` |
| Logical | `not ""` | `True` |
| Loop | `for c in "hi"` | each character |
| Membership | `"el" in "hello"` | `True` |

---

## Extra Knowledge (not in the video)

### How string comparison really works
- Python compares strings by the **Unicode code point** of each character, left to right, until a difference is found; if one string is a prefix of the other, the **shorter** one is smaller.
  ```python
  "app" < "apple"     # True (prefix)
  "abc" < "abd"       # True
  ord("A"), ord("a")  # (65, 97)
  ```
- This is **not** the same as dictionary order in every language (accents, local sorting rules). For locale-aware sorting use `locale.strxfrm` or a library like ICU.

### Case-insensitive comparison and sorting
```python
a.casefold() == b.casefold()               # best for caseless matching (handles ß, etc.)
sorted(words, key=str.casefold)            # alphabetical, ignoring case
```

### `+` needs strings on both sides
```python
"Age: " + 25                 # ❌ TypeError
"Age: " + str(25)            # ✅
f"Age: {25}"                 # ✅ preferred
```

### Other `*` facts
- `"ab" * 0` → `""`, `"ab" * -1` → `""`.
- `"ab" * 2.5` → `TypeError` (the multiplier must be an int).

### Concatenation tips
- Adjacent literals concatenate automatically: `"Hello " "World"`.
- Building big strings in loops: use `"".join(parts)`, not repeated `+` (see Video 21).
- Use f-strings for readability: `f"{first} {last}"`.

### One more operator that works on strings: `%`
Old-style formatting: `"Hello %s, you are %d" % ("Ram", 25)`. It's legacy; prefer f-strings. (This is why "only `+` and `*`" is true for *arithmetic*, but `%` is overloaded for formatting.)

### Membership details
- `in` on strings checks **substrings** (contiguous text), not "any of these characters".
- `"" in "abc"` is `True` (the empty string is a substring of everything).
- For multiple options: `any(x in text for x in ["a", "b"])`.
- For position information use `.find()` / `.index()` (Video 23).

### Truthiness is handy
```python
name = input("Name: ")
if not name:                 # same as: if name == ""
    print("Name is required")
```
Careful: a string of only spaces `"   "` is **truthy**. Use `if not name.strip():` to catch it.

### Never compare strings with `is`
`"hello" is "hello"` may be `True` because of interning, but it's an implementation detail. Always use `==` for values.

---

## Tricky Parts / Gotchas

1. `"Mumbai" > "Pune"` is about alphabetical order, **not length**.
2. `"a" > "B"` is `True` (code 97 vs 66): lowercase comes **after** uppercase.
3. `"10" < "9"` is `True` (compared character by character: `"1" < "9"`). Convert to numbers before numeric comparison.
4. `"hello" and "world"` returns `"world"`, **not** `True`.
5. `"   "` (spaces) is truthy, an empty string `""` is falsy.
6. `"5" * 3` is `"555"`, while `5 * 3` is `15`; check types after `input()`.
7. `"abc" - "a"` or `"abc" / 2` → `TypeError`.
8. `"hello" in ["hello world"]` is `False` (it checks list items, not substrings).

---

## Interview Questions

1. **Which arithmetic operators work on strings?** `+` (concatenate) and `*` (repeat).
2. **How does Python compare two strings?** Lexicographically by Unicode code points.
3. **Why is `"Zebra" < "apple"` True?** Uppercase letters have lower code values than lowercase.
4. **What does `"hello" and "world"` return?** `"world"`.
5. **What does `"" or "default"` return?** `"default"` (common idiom for fallback values).
6. **Is an empty string truthy?** No, it's falsy.
7. **How do you check for a substring?** `sub in text`.
8. **How do you compare strings ignoring case?** `.casefold()` (or `.lower()`).
9. **Why shouldn't you use `is` to compare strings?** It checks identity, not value; results depend on interning.

---

### Practice ideas
1. Print a 40-character line of `-` and a centred title using `*`.
2. Predict, then check: `"apple" < "banana"`, `"Apple" < "apple"`, `"10" < "9"`, `"abc" < "abcd"`.
3. Predict, then check: `"a" and "b"`, `"" or "x"`, `not "0"`.
4. Loop over `"Python"` in reverse and print each character.
5. Ask for a username and check whether it contains a space.

**Next video:** String functions.
