# 100 Days of Python (CampusX) — Video 23: String Functions

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: the most useful functions/methods for working with strings. The instructor covers roughly 70% of them; the rest you can discover yourself.

---

## 0. Two categories

1. **Common functions** that work on many data types (strings, lists, tuples, sets, dicts): `len`, `max`, `min`, `sorted`.
2. **String-specific functions (methods)** that work **only on strings**, called like `s.upper()`.

> **Important rule for all of them:** strings are **immutable** (Video 21), so these functions **never change the original string**. They return a **new** value. To keep the result, store it.

---

## 1. Common functions

```python
s = "kolkata"

len(s)         # 7          number of characters
max(s)         # 't'        largest character by Unicode/ASCII value
min(s)         # 'a'        smallest character by Unicode/ASCII value
sorted(s)      # ['a', 'a', 'k', 'k', 'l', 'o', 't']
sorted(s, reverse=True)   # descending order
```
- `max` / `min` compare characters by their **code value** (see Video 22).
- `sorted()` returns a **list** (not a string), in ascending order by default. Use `reverse=True` for descending.

---

## 2. Case-changing methods

```python
s = "hello world"

s.capitalize()   # 'Hello world'    first letter of the whole string capitalised
s.title()        # 'Hello World'    first letter of every word capitalised
s.upper()        # 'HELLO WORLD'
s.lower()        # 'hello world'
s.swapcase()     # 'HELLO WORLD'    (upper ↔ lower for each character)
```
- `title()` is what blog headings use (title case).
- `print(s)` afterwards still shows `hello world`; the original is unchanged.

---

## 3. `count()`: how many times?

```python
"hello world".count("l")      # 3
"hello world".count("lo")     # 1     works with substrings too
"hello world".count("z")      # 0     not present → 0
```
Great for finding the **frequency** of a character or substring.

---

## 4. `find()` and `index()`: where is it?

```python
s = "hello world"
s.find("o")        # 4      position of the FIRST occurrence
s.find("world")    # 6      start position of the substring
s.find("z")        # -1     not found
s.index("o")       # 4
s.index("z")       # ❌ ValueError: substring not found
```
- Both return the position of the **first** match, even if the character repeats.
- **Difference:** if not found, **`find()` returns `-1`**, **`index()` raises an error.** The instructor's advice: **prefer `find()`.**

---

## 5. `startswith()` and `endswith()`

```python
"hello world".endswith("world")      # True
"hello world".endswith("hello")      # False
"hello world".startswith("hello")    # True
```
Return `True`/`False`: check whether the string begins/ends with a given substring.

---

## 6. `format()`: fill in the blanks

```python
"Hello my name is {} and I am {}".format("Ram", 25)
# 'Hello my name is Ram and I am 25'
```
- `{}` are blanks; values are filled in order.
- Use it when you don't know the values in advance (e.g. a profile page that says "Hello, <user>" after login).

### Positions
```python
"Hello my name is {1} and I am {0}".format(25, "Ram")
# 'Hello my name is Ram and I am 25'
```
`{0}` is the first value passed, `{1}` the second, so you can rearrange.

### Names (keywords)
```python
"Hello my name is {name} and I am {age}".format(name="Ram", age=25)
```
Clearer and order-independent.

---

## 7. The `is...()` family: checking what a string contains

Return `True`/`False`. Very useful for **validating user input**.

```python
"Delhi".isalpha()          # True    only letters
"Delhi123".isalpha()       # False
"Delhi123".isalnum()       # True    letters + digits only
"Delhi 123".isalnum()      # False   space is a special character
"1234".isdigit()           # True
"12a4".isdigit()           # False
"hello_world".isidentifier()   # True    valid Python identifier (Video 5)
"hello world".isidentifier()   # False   space not allowed
"hello".islower()  / "HELLO".isupper()  / "   ".isspace()  / "Hello World".istitle()
```
Others exist too: `isnumeric()`, `isdecimal()`, `isprintable()`, etc.

---

## 8. `split()`: string → list of words

```python
"who is the PM of India".split()
# ['who', 'is', 'the', 'PM', 'of', 'India']
```
- By default splits on whitespace.
- You can split on **any separator**:
  ```python
  "a,b,c".split(",")        # ['a', 'b', 'c']
  "who is PM".split("PM")   # ['who is ', '']
  ```
- Real-world use: sites like Quora split a question into words, drop common words ("who", "is", "the") and turn the rest (PM, India) into **tags**.

---

## 9. `join()`: list → string

The opposite of `split()`.
```python
words = ["hello", "world"]
" ".join(words)        # 'hello world'
"-".join(words)        # 'hello-world'
"".join(words)         # 'helloworld'
```
- The string you call `join` on is the **separator** placed between the items.
- Used to build things like **URLs** from parts.

---

## 10. `replace()`

```python
"hi my name is nitish".replace("nitish", "campusx")
# 'hi my name is campusx'
```
`s.replace(old, new)`: swaps every `old` with `new`.

---

## 11. `strip()`: remove extra spaces

```python
"   hey   ".strip()     # 'hey'
```
- Removes **leading and trailing** spaces (not the ones in the middle).
- Why it matters: a user registering on your site may type their name with accidental spaces. If you store it as-is, `"   Ram "` goes into the database. Calling `.strip()` on input before saving avoids this.

---

## 12. Finding the rest

For a complete list: create any string, type `s.` and press **Tab**, or use `dir(str)` and `help(str.method_name)`. Try them yourself, search online, or use the `help()` function.

---

## 13. Quick summary

| Need | Use |
|------|-----|
| Length / max / min / sort | `len`, `max`, `min`, `sorted` |
| Change case | `capitalize`, `title`, `upper`, `lower`, `swapcase` |
| Count occurrences | `count` |
| Position | `find` (safe), `index` (error if missing) |
| Starts/ends | `startswith`, `endswith` |
| Fill blanks | `format` (or f-strings) |
| Check content | `isalpha`, `isdigit`, `isalnum`, `isidentifier`... |
| Split / join | `split`, `join` |
| Replace | `replace` |
| Trim spaces | `strip` |

---

## Extra Knowledge (not in the video)

### f-strings: the modern replacement for `format()`
```python
name, age = "Ram", 25
print(f"Hello my name is {name} and I am {age}")
print(f"{3.14159:.2f}")        # 3.14      two decimals
print(f"{1234567:,}")          # 1,234,567 thousands separator
print(f"{name:>10}|")          # right-align in width 10
print(f"{age=}")               # age=25    (debugging, 3.8+)
```
Use f-strings in new code; `.format()` is still good to know (and for templates stored in variables).

### Handy methods not in the video
```python
"  hi  ".lstrip(), "  hi  ".rstrip()          # strip only one side
"hello".center(11, "*")      # '***hello***'
"7".zfill(3)                 # '007'
"hello".ljust(8, ".") ; "hello".rjust(8, ".")
"hello world".partition(" ")           # ('hello', ' ', 'world')
"line1\nline2".splitlines()            # ['line1', 'line2']
"Hello.txt".removesuffix(".txt")       # 'Hello'  (3.9+)
"Mr. Ram".removeprefix("Mr. ")         # 'Ram'    (3.9+)
"Straße".casefold()                    # caseless comparison (better than lower())
"hello".isascii()                      # True (3.7+)
"hello world".rfind("o")               # 7  → search from the right
"a-b".translate(str.maketrans("-", " "))   # 'a b'
```

### `capitalize()` also lowercases the rest
```python
"hELLO wORLD".capitalize()    # 'Hello world'
```

### `strip()` removes a *set of characters*, not a substring
```python
"  hi  ".strip()              # 'hi'
"abcba".strip("ab")           # 'c'   (removes any a/b from both ends)
```
Use `removeprefix`/`removesuffix` for exact substrings.

### `split()` with and without arguments
```python
"a  b   c".split()         # ['a', 'b', 'c']        collapses multiple spaces
"a  b   c".split(" ")      # ['a', '', 'b', '', '', 'c']
"a,b,c,d".split(",", 2)    # ['a', 'b', 'c,d']      maxsplit
```

### `join()` needs strings
```python
",".join([1, 2, 3])                  # ❌ TypeError
",".join(str(n) for n in [1, 2, 3])  # '1,2,3'
```

### `isdigit()` vs `isdecimal()` vs `isnumeric()`
```python
"12".isdigit()        # True
"²".isdigit()         # True   (superscript two)  ← but int("²") fails!
"²".isdecimal()       # False
"½".isnumeric()       # True
"12.5".isdigit()      # False  (the dot is not a digit)
"-5".isdigit()        # False
```
For validating numbers, try `int()`/`float()` inside `try/except` instead.
Also: `"".isalpha()` and `"".isdigit()` are `False` for an empty string.

### `count` is non-overlapping
`"aaaa".count("aa")` → `2`.

### `title()` gotcha with apostrophes
```python
"they're here".title()          # "They'Re Here"
import string
string.capwords("they're here") # "They're Here"
```

### Performance
- `"".join(list_of_strings)` is much faster than repeated `+=` in loops.
- `in` is fine for membership; use `find`/`index` only when you need the position.
- `s.lower()` creates a new string each time. Compute once if you reuse it.

### Real-world mini patterns
```python
# clean user input
username = input("Username: ").strip().lower()

# word frequency
text = "the cat and the hat"
for word in set(text.split()):
    print(word, text.split().count(word))

# palindrome (combines slicing + methods)
word = "Madam"
print(word.lower() == word.lower()[::-1])
```
Chaining works because each method returns a new string.

---

## Tricky Parts / Gotchas

1. **Forgetting to store the result:** `s.upper()` alone does nothing visible to `s`. Use `s = s.upper()`.
2. **`find()` returns `-1` when not found.** `if s.find("x"):` is wrong (because `-1` is truthy and `0` is falsy). Use `if s.find("x") != -1:` or simply `"x" in s`.
3. **`index()` raises `ValueError`;** `find()` doesn't.
4. **`sorted(s)` returns a list.** Use `"".join(sorted(s))` to get a string back.
5. **`strip("ab")` removes characters**, not the substring `"ab"`.
6. **`isdigit()` is false for negatives and decimals.**
7. **`split()` with no argument ≠ `split(" ")`** when there are multiple spaces.
8. **`title()` mishandles apostrophes;** `capitalize()` lowers the rest of the string.
9. **`max("hello")` is `'o'`**, i.e. by code value, not length or frequency.
10. **`join` works on a list of strings only;** convert numbers first.

---

## Interview Questions

1. **How do you reverse words in a sentence?** `" ".join(sentence.split()[::-1])`.
2. **How do you check if two strings are anagrams?** `sorted(a) == sorted(b)` (after lower-casing).
3. **Difference between `find()` and `index()`?** `find` gives `-1` if missing; `index` raises `ValueError`.
4. **`split()` vs `join()`?** String → list vs list → string.
5. **How do you count vowels in a string?** `sum(ch in "aeiou" for ch in s.lower())`.
6. **`capitalize()` vs `title()`?** First letter of the string vs of every word.
7. **How do you remove leading/trailing whitespace?** `strip()` (`lstrip`/`rstrip` for one side).
8. **How do you format strings in modern Python?** f-strings; `.format()` for stored templates.
9. **Is `s.upper()` changing `s`?** No: strings are immutable; it returns a new string.
10. **How do you check whether a string is a valid number?** Try `float(s)` in `try/except`; `isdigit()` only handles non-negative integers.
11. **Difference between `isalpha`, `isalnum` and `isdigit`?** Letters only / letters+digits only / digits only.
12. **What's the quickest way to find a character's frequency?** `s.count(ch)`, or `collections.Counter(s)` for all characters.

---

### Practice ideas
1. Take a sentence from the user and print: number of words, the longest word, the title-cased version.
2. Count vowels and consonants in a word.
3. Check whether the user's password has at least one digit and one letter (using `any()` with `isdigit` / `isalpha`).
4. Reverse the order of words in a sentence.
5. Clean a messy input like `"   rAm   KuMaR "` → `"Ram Kumar"`.
6. Build a CSV line from a list of values with `join`, then split it back.
7. Read `help(str.find)` and `help(str.split)` in your console.

**Next:** Lists (the next major data type).
