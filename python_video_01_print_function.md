# 100 Days of Python (CampusX) — Video 1: The `print()` Function

> Notes from the transcript of the first video of the playlist (original in Hinglish, notes in English).
> Topic: writing your first Python code and mastering `print()`.

---

## 1. Overview

- This is the first coding video of the course. It starts with the most basic thing in any language: **printing output**.
- Other languages need different things to print:
  - C → `printf`
  - C++ → `cout`
  - Java → `System.out.println`
- Python has a **built-in function** called `print()`. Built-in means you do **not** need to import anything.

---

## 2. Hello World

```python
print("Hello World")
```

- One line is enough. No imports, no `main` function, no boilerplate.
- Run the cell (**Shift + Enter** in Jupyter) to see the output.

---

## 3. What can you print?

`print()` accepts any data type:

```python
print("Hello")        # string
print(5)              # integer
print(3.14)           # float
print(True)           # boolean
```

Output is shown exactly as you would expect for each type.

---

## 4. Printing multiple items at once

You can pass several values, separated by **commas**:

```python
print("India", "Pakistan", "Nepal", "Sri Lanka")
# India Pakistan Nepal Sri Lanka
```

- Different data types can be mixed:

```python
print("Hello", 4, 5.5, True)
# Hello 4 5.5 True
```

- There is **no limit** on the number of arguments. 4 items, 10 items, or more all work, so `print()` is very flexible.

---

## 5. Looking up documentation

- In Jupyter / Anaconda, put the cursor on the function name (e.g. `print`) and press **Shift + Tab** to see its documentation.
- You can also read the full docs for any function this way.

Key points from the `print` documentation:

| Parameter | Meaning | Default |
|-----------|---------|---------|
| `*values` | The things to print (any number) | — |
| `sep` | Separator placed **between** values | `" "` (a single space) |
| `end` | What is added **at the end** of the output | `"\n"` (new line) |
| `file` | Where to print (stdout / a file) | `sys.stdout` |
| `flush` | Whether to force-flush the stream | `False` |

> `file` and `flush` will be covered later in **File Handling**.

---

## 6. The `sep` parameter

`sep` controls what goes **between** multiple values.

- Default is a space, which is why `print("a", "b")` outputs `a b`.
- Change it to anything you like:

```python
print("India", "Pakistan", "Nepal", sep="/")
# India/Pakistan/Nepal

print("India", "Pakistan", "Nepal", sep="-")
# India-Pakistan-Nepal
```

- It applies **between all inputs**.
- You can use it with mixed types too, e.g. `print("Hello", 4, True, sep="-")`.

---

## 7. The `end` parameter

`end` controls what is printed **after** the output.

- Default is `"\n"` (new line). This is why every `print()` starts its output on a **new line**:

```python
print("Hello")
print("World")
# Hello
# World
```

- To print on the **same line**, change `end`:

```python
print("Hello", end=" ")
print("World")
# Hello World
```

- You can use any string, e.g. `end="-"` or `end=""`.

---

## 8. Quick summary

| Concept | Remember |
|---------|----------|
| `print()` | Built-in function, no import required |
| Data types | Strings, numbers, booleans, mixed, all work |
| Multiple values | Separate with commas, no limit |
| `sep` | Separator **between** values (default: space) |
| `end` | Ending **after** the output (default: newline) |
| Docs | Shift + Tab on the function name in Jupyter |

---

## 9. What's next

- `print()` is used **constantly** (you'll use it almost unconsciously in big projects).
- `file` and `flush` parameters come later with File Handling.
- **Next video:** Data Types in Python.

---

### Practice ideas

1. Print your name and age in one `print()` call.
2. Print `2026`, `11`, `16` with `sep="-"` to make a date.
3. Print three separate words on one line using `end=" "`.
4. Combine `sep` and `end` in a single `print()` call.
