# 100 Days of Python (CampusX) — Video 11: Indentation

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: how Python marks blocks of code. (The speaker plans if-else next; in your playlist order if-else was already covered in Video 10, so these two can be read in either order.)

---

## 1. How other languages mark blocks

In languages like C, C++, Java, JavaScript:

```c
if (name == "xyz") {
    // block of code 1
} else {
    // block of code 2
}
```

- **Curly braces `{ }`** mark where a block starts and ends.
- **Semicolons `;`** mark the end of each statement.

Why blocks need marking: a program runs line by line. Some blocks should run **only when a condition is true**, others **only when it's false**, so the language must clearly show which lines belong to which block.

---

## 2. How Python does it: indentation

**Python has no `{ }` and no `;`.** It uses **indentation**: the **empty space at the start of a line** (made with the **Tab** key).

```python
if name == "xyz":
    print("line 1")       # ← indented: belongs to the if
    print("line 2")
    print("line 3")
else:
    print("other")        # ← indented: belongs to the else
```

- After the `:`, press Enter and the editor usually **auto-indents** for you.
- If it doesn't, press **Tab**.
- To leave the block, press **Backspace/Shift+Tab** (reduce one level) and write the next line at the outer level.
- You can have **any number of lines** inside one block, as long as they all have the **same indentation**.
- Python is "closer to English" and cleaner as a result.

---

## 3. Indentation is NOT optional in Python

In other languages indentation is just for looks (the braces do the real work). In Python:

> **Indentation is what defines the block.** The interpreter uses it to know which code belongs to which block.

Wrong indentation → error or wrong behaviour:

```python
name = "abc"
if name == "xyz":
print("line 1")        # IndentationError: expected an indented block
```

Or a **silent logic change**:

```python
name = "abc"
if name == "xyz":
    print("inside if")
print("outside if")      # always runs, not part of the if
```

An **extra** indent where it doesn't belong also fails:

```python
print("hello")
    print("world")       # IndentationError: unexpected indent
```

---

## 4. Nested blocks

Blocks inside blocks need **one more level** of indentation each time:

```python
x = 5
y = 5
if x == 5:
    print("line 1")
    if y == 5:
        print("line 2")        # two levels deep: belongs to the inner if
        print("line 3")
    print("line 4")            # back to the first level
print("line 5")                # outside everything
```

- The inner `print` is **two Tab-stops** from the left edge.
- Each time you go deeper, press Tab once more.

---

## 5. Instructor's practical advice

- Beginners often get indentation wrong and then the code doesn't run.
- **Always use the Tab key** to indent. **Never press the Space bar manually** to line things up. (It looks right but causes errors.)
- Use common sense: look at which block a line belongs to and put it at the same level as its siblings.
- Press **Tab** to go in, **Shift+Tab / Backspace** to go out.

---

## 6. Why did Python choose this?

- Python's creators believe in **code readability**.
- Indentation makes it obvious at a glance which code is inside which block.
- Makes team work easier and **debugging easier**.
- Rather than leaving indentation optional (as other languages do), Python **makes it mandatory** so all Python code looks consistently clean.

---

## 7. Quick summary

- No `{ }` or `;` in Python.
- Indentation = block structure.
- Same block → same indentation; deeper block → one more level.
- Always use **Tab**, not manual spaces.
- Wrong indentation → `IndentationError` or wrong logic.

---

## Extra Knowledge (not in the video)

### The official convention (PEP 8)
- Use **4 spaces per indentation level**. Most editors (VS Code, PyCharm, Jupyter) turn the Tab key into 4 spaces for you, which is why the Tab advice works.
- **Never mix tabs and spaces** in the same file. Python 3 raises `TabError: inconsistent use of tabs and spaces in indentation`.
- Spaces are the recommended choice; the key thing is to be **consistent**.

### Where blocks (and so indentation) appear
`if/elif/else`, `for`, `while`, `def` (functions), `class`, `try/except/finally`, `with`, `match/case`. Every statement ending with `:` starts a new block.

### Related rules and tricks
- **Empty block** isn't allowed; use `pass`:
  ```python
  if x > 5:
      pass
  ```
- **One-liner blocks** are allowed: `if x > 5: print("big")` (fine for short code).
- **Blank lines and comments** don't affect indentation.
- **Semicolons are legal** but un-Pythonic: `a = 1; b = 2` works (used to put 2 statements on one line).
- **Line continuation:** break long lines with parentheses `( )` (preferred) or a backslash `\`. Inside brackets, indentation of the continued line doesn't matter.
- Python's tokenizer turns indentation changes into hidden `INDENT` / `DEDENT` tokens; that's how it parses blocks.
- **Easter eggs:**
  ```python
  import this                  # The Zen of Python: "Readability counts."
  from __future__ import braces   # SyntaxError: not a chance
  ```

### Common error messages
| Error | Cause |
|-------|-------|
| `IndentationError: expected an indented block` | Forgot to indent after `:` |
| `IndentationError: unexpected indent` | Indented a line that shouldn't be |
| `IndentationError: unindent does not match any outer indentation level` | Dedented to a level that doesn't exist |
| `TabError` | Mixed tabs and spaces |

---

## Tricky Parts / Gotchas

1. **Copy-pasting code** from websites/PDFs can mix tabs and spaces or lose indentation. Fix it in the editor ("Convert indentation to spaces").
2. A wrongly indented line often **doesn't crash**: it just changes the logic. These are the worst bugs.
3. In Jupyter/online editors, auto-indent might add or skip levels. Always check visually.
4. Don't indent the **first line** of a file/cell, or you get `unexpected indent`.
5. `else` must be at the **same level as its `if`**; if it's indented more or less, you'll get an error or attach it to the wrong `if`.
6. A comment line with odd indentation is harmless, but code lines are not.

---

## Interview Questions

1. **How does Python define code blocks?** By indentation, not braces.
2. **What is `IndentationError`?** Raised when indentation is missing, unexpected or inconsistent.
3. **How many spaces are recommended per level?** 4 (PEP 8).
4. **Can you mix tabs and spaces?** No: `TabError` in Python 3.
5. **Why did Python choose indentation over braces?** Readability and enforced consistent style.
6. **How do you write an empty `if`/function body?** Use `pass`.
7. **Is a semicolon required in Python?** No, but allowed to separate statements on one line.

---

### Practice ideas
1. Write an `if/else` with 3 lines in each block, then intentionally break the indentation and read the error.
2. Write a nested `if` (3 levels) and print a different message at each level.
3. Run `import this` and read the Zen of Python.
4. Try `from __future__ import braces`.

**Next:** Loops (while loop is covered in the next video in this set).
