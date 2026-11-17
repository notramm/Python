# 100 Days of Python (CampusX) — Video 10: If-Else Statements

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Also called **decision control statements** or **conditional statements**.

---

## 1. Why do we need if-else?

- Normally a program runs **top to bottom, line by line**.
- Sometimes there is **branching**: at some point the logic can go one way or another depending on a condition.
- At that branching point you use `if` / `else`.

**Example: login**
- Case 1: user gives correct email and password → show profile page.
- Case 2: user gives something wrong → show an error.

Each case needs different code, so you branch with `if-else`. It is used so heavily that you'll soon write it without thinking.

---

## 2. Basic syntax

```python
if condition:
    # runs when condition is True
else:
    # runs when condition is False
```

Python-specific points:
- A **colon `:`** ends the `if` line (and `else:` too).
- **No brackets** needed around the condition (unlike C/Java).
- The code inside is **indented** (Tab). See Video 11.
- `else` has **no condition**; it simply catches everything else.
- `else` must line up **vertically with its `if`**.

---

## 3. Example: simple login

Correct credentials: email `campusx@gmail.com`, password `1234`.

```python
email = input("Enter email: ")
password = input("Enter password: ")

if email == "campusx@gmail.com" and password == "1234":
    print("Welcome")
else:
    print("Incorrect credentials")
```

- Uses `==` (comparison) and `and` (logical): **both** must be right.
- Wrong email/password → goes to `else` automatically.

Test runs: wrong values → "Incorrect credentials"; correct values → "Welcome".

---

## 4. `elif`: more than two branches

Manager's new requirement: if the **email is right but the password is wrong**, give the user **one more chance**.

Now there are three cases:
1. Both correct → welcome.
2. Email correct, password wrong → second chance.
3. Anything else → error.

When there are more than two branches you use **`elif`** ("else if"):

```python
email = input("Enter email: ")
password = input("Enter password: ")

if email == "campusx@gmail.com" and password == "1234":
    print("Welcome")
elif email == "campusx@gmail.com" and password != "1234":
    # second chance
    password = input("Incorrect password. Enter again: ")
    if password == "1234":
        print("Welcome")
    else:
        print("Incorrect password!")
else:
    print("Not a correct email")
```

### Rules for the chain
- **First** is always `if`, **last** is always `else` (optional), the **middle** ones are `elif` (as many as you want).
- In Python the keyword is **`elif`**, not `else if`.
- Only **one** branch in the whole chain runs.

### Nested if
Notice the `if/else` *inside* the `elif`: that's a **nested if**. You can nest as deeply as needed (just keep indentation right).

---

## 5. Adding another layer: check for `@`

If the email has no `@`, it can never be valid, so reject it immediately without asking for a password.

```python
email = input("Enter email: ")

if "@" in email:
    password = input("Enter password: ")
    if email == "campusx@gmail.com" and password == "1234":
        print("Welcome")
    elif email == "campusx@gmail.com" and password != "1234":
        password = input("Incorrect password. Enter again: ")
        if password == "1234":
            print("Welcome")
        else:
            print("Incorrect password!")
    else:
        print("Not a correct email")
else:
    print("Email format is wrong")
```

This single example covers: `if`, `elif`, `else`, nested `if`, `and`, `in`, `!=`, `==` (use of operators from Video 9).

---

## 6. Quick summary

| Part | Purpose |
|------|---------|
| `if cond:` | First check |
| `elif cond:` | Additional checks (any number) |
| `else:` | Fallback, no condition |
| Nested `if` | An `if` inside another block |
| Indentation | Defines which code belongs to which branch |

---

## Extra Knowledge (not in the video)

### Truthy / falsy conditions
You can use any value as a condition:
```python
items = []
if items:                # empty list is falsy
    print("Has items")
else:
    print("Empty")

name = ""
if not name:
    print("Name required")
```
Falsy: `0`, `0.0`, `""`, `[]`, `()`, `{}`, `set()`, `None`, `False`.

### Ternary (one-line) expression
```python
status = "adult" if age >= 18 else "minor"
```

### Chained comparison
```python
if 18 <= age < 60:
    print("Working age")
```

### Check order matters
Python checks from the top; the **first** true condition wins and the rest are skipped:
```python
marks = 85
if marks >= 40:      # catches everything ≥ 40
    print("Pass")
elif marks >= 80:    # never reached for 85!
    print("Distinction")
```
Put the **most specific** conditions first.

### `elif` chain vs separate `if`s
```python
if x > 5: print("A")
if x > 3: print("B")      # separate ifs → BOTH can run
```
With `elif`, only one block runs.

### `pass`: placeholder for an empty block
```python
if x > 5:
    pass      # TODO
```
An empty block is a syntax error; use `pass`.

### No `switch` (until 3.10)
Python 3.10 added `match` / `case` (structural pattern matching). Before that, people used `if/elif` chains or dictionaries.

### Other facts
- `if` blocks **don't create a new scope**: variables set inside remain available after.
- `else` also works with loops (`for...else`, `while...else`) and `try...else`.
- Case-insensitive comparison: `email.lower() == "campusx@gmail.com"`.
- Compare with `None` using `is None`, not `== None`.
- **Security note:** real apps never store plain-text passwords; they store hashes. For typed passwords use `getpass.getpass()`.
- `@` check is a *very* rough email validation. Real validation uses regex or a library.

---

## Tricky Parts / Gotchas

1. `if x = 5:` → `SyntaxError` (good, since Python prevents the classic `=` vs `==` bug). Use `==`.
2. Forgetting the **colon** after `if`/`elif`/`else` → `SyntaxError`.
3. `else if` is wrong; use `elif`.
4. Wrong indentation silently changes meaning:
   ```python
   if x > 5:
       print("big")
   print("done")       # always runs (not part of the if)
   ```
5. `if x == 1 or 2:` is **always True** (`2` is truthy). Write `if x == 1 or x == 2:` or `if x in (1, 2):`.
6. Comparing floats with `==` is unreliable; use `math.isclose`.
7. Mixing types: `if input() == 5:` is `False` forever (`"5" != 5`). Convert first.
8. Deep nesting gets hard to read; use **early returns / guard conditions** and `and`/`elif` to flatten.

---

## Interview Questions

1. **How does Python's `if` differ from C's?** No braces/parentheses; uses colon + indentation; `elif` instead of `else if`.
2. **Does Python have `switch`?** Not before 3.10; now `match-case`.
3. **What values are falsy?** `0`, `""`, `[]`, `{}`, `()`, `set()`, `None`, `False`.
4. **What does `if x == 1 or 2` do?** Always true: a classic bug.
5. **Write a ternary expression.** `a if cond else b`.
6. **What's the difference between `elif` and multiple `if`s?** `elif` stops at the first match; separate `if`s are all evaluated.
7. **What is a nested `if`?** An `if` inside another `if`/`elif`/`else` block.
8. **Can `else` be used without `if`?** Not on its own; but it can pair with `for`, `while`, `try`.

---

### Practice ideas
1. Check whether a number is positive, negative or zero.
2. Find the largest of three numbers.
3. Grade calculator: marks → A/B/C/D/F using `elif`.
4. Leap year check (divisible by 4, but not by 100 unless by 400).
5. Extend the login program to allow **three** password attempts. (Hint: loops, coming up!)

**Next video:** Indentation.
