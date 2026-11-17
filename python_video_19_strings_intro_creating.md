# 100 Days of Python (CampusX) — Video 19: Strings (Introduction & Creating Strings)

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> This is part 1 of the 5-part Strings series (intro/creating → indexing & slicing → edit & delete → operations → functions).

---

## 1. Why strings matter

- Almost every **application programming language** (Python, Java, JavaScript...) has a string data type, because applications **talk to users**, and talking to users means text.
- In **system programming** (e.g. C, which builds software that talks to hardware) there is no real string type; C only has characters/arrays of characters.
- In real-world projects you will need strings **constantly**, so this topic gets several videos.

### Series plan
1. Intro + creating strings (this video)
2. Accessing parts of a string: **indexing and slicing**
3. **Editing and deleting** strings
4. **Operations** on strings
5. **Functions** for strings

---

## 2. What is a string?

- Plain English: a **sequence of characters**. `"Hello"` is a sequence of H, e, l, l, o.
- Python-specific definition: a **sequence of Unicode characters**.

### Why "Unicode"?
- Machines don't understand characters, only **numbers**. So every character is mapped to a number.
- **ASCII** was the early scheme: it only covered English letters, digits and a few symbols (128 characters).
- As programming spread worldwide, other languages (Hindi, Chinese, Arabic, emojis...) needed support. **Unicode** is a universal standard that gives a number (code point) to characters from virtually all writing systems.
- That's why you can store text in any language, and emojis, in a Python string.

> The video describes Unicode as "16-bit". Modern Unicode is larger than that (over a million possible code points). A Python 3 `str` is simply a sequence of Unicode code points, and it is stored/written to files using an **encoding**, most commonly **UTF-8**.

---

## 3. Creating strings

### 3.1 Single quotes
```python
s = 'hello'
```

### 3.2 Double quotes
```python
s = "hello"
```
Both create exactly the same thing. Python allows both because of one special situation:

### Why both exist: quotes inside the text
```python
s = 'It's raining outside'     # ❌ SyntaxError: Python thinks the string ends at the 2nd '
s = "It's raining outside"     # ✅ outer double quotes, inner single quote is fine
```
- The `'` inside the sentence confuses Python about where the string starts and ends.
- Fix: wrap the string in the **other kind** of quote.
- The reverse works too: `'He said "hi"'`.
- Problem case: if the text contains **both** kinds of quote (or you'd put `"` inside `"`), choose triple quotes or escape the quote (see below).

### 3.3 Triple quotes
```python
s = '''hello'''
s = """hello"""
```
Used mainly for **multi-line strings** (e.g. a blog post typed by a user in a blogging site):
```python
blog = """This is line 1
This is line 2
This is line 3"""
print(blog)
```
Single triple quotes and double triple quotes behave the same.

### 3.4 Using `str()` (type conversion)
```python
s = str("hello")
n = str(123)       # '123'
```
Converts another value to a string (from Video 7).

---

## 4. Quick summary

| Way | Example | Typical use |
|-----|---------|-------------|
| Single quotes | `'hello'` | General |
| Double quotes | `"It's fine"` | Text containing `'` |
| Triple quotes | `"""line1\nline2"""` | Multi-line text |
| `str()` | `str(45)` | Converting other types |

---

## Extra Knowledge (not in the video)

### Escape sequences (another way to include quotes and special characters)
```python
'It\'s raining'          # backslash escapes the quote
print("Line1\nLine2")    # \n new line
print("A\tB")            # \t tab
print("C:\\Users")       # \\ a literal backslash
```

### Raw strings (backslashes kept as-is)
```python
print(r"C:\new\table")   # C:\new\table
```
Good for Windows paths and regular expressions.

### f-strings: the modern way to build strings (Python 3.6+)
```python
name = "Ram"
age = 25
print(f"{name} is {age} years old")        # Ram is 25 years old
print(f"{3.14159:.2f}")                    # 3.14
print(f"{age=}")                           # age=25   (3.8+, handy for debugging)
```
- Since **Python 3.12**, f-strings can reuse the same quote type inside (`f"{d["key"]}"`) and span multiple lines more freely.
- **Python 3.14** also added *template strings* (`t"..."`, "t-strings") for safe custom string processing (advanced; used by libraries).

### Other useful facts
- An **empty string** `""` is valid (and falsy).
- Python has **no `char` type**: a single character is a string of length 1.
- `len("😀")` is `1`: Python counts Unicode code points, not bytes.
- **Encoding / decoding:** `"hello".encode("utf-8")` → `b'hello'` (bytes); `b'hello'.decode("utf-8")` → `'hello'`. `str` = text; `bytes` = raw data.
- Adjacent string literals are **auto-joined**: `("Hello " "World")` → `"Hello World"` (neat for long lines).
- Unicode escapes: `"\u0905"` → `अ`, `"\N{GRINNING FACE}"` → 😀.
- `repr("hi\n")` shows the literal form (`'hi\n'`), while `print` shows the rendered text.
- Strings are **immutable** (Video 21) and **hashable**, so they work as dictionary keys.
- Source files are UTF-8 by default in Python 3, so you can write Hindi text directly in code.

---

## Tricky Parts / Gotchas

1. `'It's ...'` → SyntaxError. Use `"It's ..."` or `'It\'s ...'`.
2. A normal (single-line) string can't span lines; use triple quotes (or `\n`).
3. `"5" + 5` → `TypeError`; convert with `str(5)` or use an f-string.
4. `"C:\new"` contains a newline and `"C:\temp"` a tab. Use a raw string or `\\`.
5. Triple-quoted strings **keep** the line breaks and indentation exactly as typed.
6. `str(None)` is `'None'` and `str([1, 2])` is `'[1, 2]'`: conversion never fails, but the result may not be what you intended.

---

## Interview Questions

1. **What is a string in Python?** An immutable sequence of Unicode characters.
2. **Is there a `char` type?** No. A character is a string of length 1.
3. **Single vs double quotes?** No difference; pick whichever avoids escaping.
4. **How do you write a multi-line string?** Triple quotes.
5. **What is Unicode and how does it differ from ASCII?** ASCII covers ~128 English-centric characters; Unicode covers virtually all scripts and symbols.
6. **`str` vs `bytes`?** `str` is text (Unicode); `bytes` is raw 8-bit data. Convert using `.encode()` / `.decode()`.
7. **What's a raw string?** A string prefixed with `r` where backslashes aren't escape characters.
8. **Preferred way to build strings with variables today?** f-strings.

---

### Practice ideas
1. Create the same string using single, double and triple quotes and compare them with `==`.
2. Make a string containing both `'` and `"` characters in at least two different ways.
3. Print your name in Hindi (Unicode) and an emoji using a Unicode escape.
4. Create a 3-line address using triple quotes and print it.

**Next video:** Indexing and slicing.
