# 100 Days of Python (CampusX) — Video 6: Taking User Input

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: the `input()` function, why it always returns a string, and the `type()` function.
> (Type conversion, which fixes the problem found at the end, is Video 7.)

---

## 1. Why user input matters: static vs dynamic software

| | Static software | Dynamic (interactive) software |
|---|---|---|
| Communication | **One-way**: software talks to you, you can't talk back | **Two-way**: you give input, software responds |
| Examples | Alarm clock, calendar, college/government websites, blogs | YouTube (search, comment, like), WhatsApp, food-ordering apps, OLX |
| Needs user input? | No | **Yes, essential** |

Most apps you use today are dynamic. Every language gives a way to take input: `scanf` (C), `cin` (C++), `Scanner`/`System.in` (Java). Python's is the built-in `input()`.

---

## 2. The `input()` function

```python
input()
```
- Pauses the program and shows a box/prompt; the user types something and presses **Enter**; the program gets that value.
- **Problem:** the user sees a blank box and doesn't know what to type.

### Fix: use the prompt argument

```python
input("Enter your name: ")
```
- The string you pass is displayed as the **prompt**. (You can check this with Shift+Tab in Jupyter: `input(prompt='')`.)
- Always tell the user what you expect (name, email, age...).

### Storing the input

```python
name = input("Enter your name: ")
print(name)
```

---

## 3. Mini-program: add two numbers (with a bug!)

```python
first_number = input("Enter the first number: ")
second_number = input("Enter the second number: ")

result = first_number + second_number
print(result)
```

Input `56` and `76` → output **`5676`** instead of `132`.

> Use **descriptive variable names** (`first_number`, not `a`). It improves readability, which matters when working in a team.

---

## 4. Why it went wrong: `input()` ALWAYS returns a string

> **Key rule:** Whatever the user types, `input()` gives you a **string** (`str`).

So `"56" + "76"` is **string concatenation**, not math addition.

### Why did Python's designers choose string?
- `str` is the **universal format** that can represent anything.
  - A number → can be stored as a string (`5` → `"5"`).
  - Text → naturally a string.
  - A list/boolean → can be written as a string too.
- The reverse isn't always possible: you can't turn `"Kolkata"` into an integer.
- So they chose the format that can hold every kind of input.

---

## 5. The `type()` function

Tells you the data type of anything:

```python
type(4)           # <class 'int'>
type(4.5)         # <class 'float'>
type("hello")     # <class 'str'>
type(True)        # <class 'bool'>
type(4 + 6j)      # <class 'complex'>
type([1, 2, 3])   # <class 'list'>
```

Checking our input:

```python
first_number = input("Enter the first number: ")   # user types 56
print(type(first_number))                          # <class 'str'>
```

The number looks like 56, but its type is `str` (that's why it prints as `'56'` in the console).

**Solution:** convert the strings to integers. That's **type conversion** (next video).

---

## 5.1 Quick summary

- Interactive software needs user input.
- `input(prompt)` reads from the user; always show a helpful prompt.
- **`input()` always returns `str`.**
- Use `type()` to check the type of any value.
- Math on input requires converting it first (`int()`, `float()`).

---

## Extra Knowledge (not in the video)

### The fix (preview of Video 7)
```python
first_number = int(input("Enter the first number: "))
second_number = int(input("Enter the second number: "))
print(first_number + second_number)    # 132
```
Convert **at the moment you take input** so you don't repeat conversions later.

### Reading several values in one line
```python
a, b = input("Enter two numbers: ").split()        # "3 4" → a="3", b="4" (strings)
a, b = map(int, input("Enter two numbers: ").split())  # ints
nums = list(map(int, input().split()))             # any count of ints
```

### Other behaviours worth knowing
- `input()` **strips the trailing newline** but keeps other spaces: typing `  hi ` returns `'  hi '`. Use `.strip()` to clean.
- Pressing **Enter with nothing typed** returns an empty string `""`.
- If input is closed (Ctrl+D / piped input ends) → `EOFError`.
- `Ctrl+C` while waiting → `KeyboardInterrupt`.
- **Python 2 vs 3:** In Python 2 `raw_input()` returned a string and `input()` *evaluated* what you typed. In Python 3, `input()` is the old `raw_input()`. (Classic interview trivia.)
- **Never use `eval(input())`** on untrusted input: it executes arbitrary code. Use `int()`, `float()`, or `ast.literal_eval()` instead.
- Faster input for competitive programming: `import sys; sys.stdin.readline()`.
- Passwords: use `getpass.getpass()` so the typed text isn't shown on screen.

### `type()` vs `isinstance()`
```python
type(5) == int            # True
isinstance(5, int)        # True  ← preferred (also works with subclasses)
isinstance(True, int)     # True  (bool is a subclass of int)
type(True) == int         # False
```
Everything in Python is an object, and types are classes. That's why `type()` prints `<class 'int'>`.

---

## Tricky Parts / Gotchas

1. `input("a") + input("b")` concatenates strings, it doesn't add numbers.
2. `int(input())` crashes with `ValueError` if the user types `"abc"` or `"4.5"`. Handling this properly uses `try/except` (later in the course).
3. `age = input(); if age > 18:` → `TypeError: '>' not supported between 'str' and 'int'`.
4. The prompt string doesn't add a space automatically: `input("Name:")` shows `Name:` stuck to the cursor. Write `"Name: "` with a trailing space.
5. `print(type(x))` shows `<class 'str'>`, but `type(x) == str` is the comparison you'd use in code.

---

## Interview Questions

1. **What does `input()` return?** Always a string.
2. **How do you read an integer from the user?** `int(input())`.
3. **How do you read multiple integers on one line?** `map(int, input().split())`.
4. **Difference between `input()` in Python 2 and 3?** Py2's `input()` evaluated the text; Py3's returns a string (same as Py2's `raw_input`).
5. **Why is `eval(input())` dangerous?** It executes whatever the user types.
6. **`type()` vs `isinstance()`?** `isinstance` respects inheritance and is generally preferred.

---

### Practice ideas
1. Ask for the user's name and age, then print a sentence using both.
2. Take two numbers and print their sum (using `int()`).
3. Read three space-separated numbers in one `input()` call and print their sum.
4. Print `type()` of the value returned for typed input `5`, `4.5` and `True`.

**Next video:** Type conversion.
