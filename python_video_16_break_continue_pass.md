# 100 Days of Python (CampusX) — Video 16: `break`, `continue` and `pass`

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> These statements are mostly used together with loops (`pass` is also used elsewhere).

---

## 1. `break`: exit the loop immediately

> If a `break` statement is executed inside a loop, the loop **terminates (ends) right there**.

```python
for i in range(1, 11):
    if i == 5:
        break
    print(i)
```
Output:
```
1
2
3
4
```
- `i = 1…4` → condition false → prints `i`.
- `i = 5` → condition true → `break` → loop is over. Nothing after that runs, not even `i = 6…10`.

### Real-world use: linear search
- You have a database with **1 lakh users** and want to find the user named "Rahul", who is at position **100**.
- You search from the first user: "Is this Rahul? No → next. Is this Rahul? No → next…".
- When you reach position 100 and find him, **why continue to 1 lakh?** You `break`.

> Use `break` whenever the loop has done its job and there's no need to continue. It makes your program **faster** (less runtime).

```python
for user in users:
    if user == "Rahul":
        print("Found!")
        break
```

---

## 2. `continue`: skip the current iteration only

> When `continue` is executed, the **rest of the loop body for that iteration is skipped**, and the loop **jumps to the next iteration**. The loop does **not** stop.

```python
for i in range(1, 11):
    if i == 5:
        continue
    print(i)
```
Output:
```
1 2 3 4 6 7 8 9 10        (5 is missing, everything else prints)
```

With extra code after it:
```python
for i in range(1, 11):
    if i == 5:
        continue
    print(i)
    print("hello")
```
For `i = 5`, **neither** `5` nor `"hello"` is printed, because everything below `continue` is skipped for that pass.

### `break` vs `continue`

| | `break` | `continue` |
|---|---------|-----------|
| Effect | **Ends the whole loop** | **Skips only this iteration** |
| After it | Loop is over | Loop carries on with the next item |

### Real-world use: e-commerce product listing
- While building a Flipkart-like site, the home page must display only products that are **in stock**.
- Loop over all products; for each one, check stock.
  - **In stock** → display it.
  - **Out of stock** → `continue` (skip the display code for this product, go to the next product).

```python
for product in products:
    if not product.in_stock:
        continue
    display(product)
```

---

## 3. `pass`: do nothing (a placeholder)

> `pass` is a statement that **does nothing**. It's a placeholder for "code will go here later".

Python **doesn't allow an empty block**. This is an error:
```python
for i in range(1, 11):
# (nothing here)
print("done")        # IndentationError: expected an indented block
```

If you haven't decided what to write yet, use `pass`:
```python
for i in range(1, 11):
    pass
```
The interpreter understands "the programmer will fill this in later", and no error or output is produced.

- Explained as a childhood habit: when you didn't know the answer to a question, you said "pass" and moved on to the next.
- `pass` is used in loops, `if` blocks, and also inside **functions** and **classes** (shown in later videos).

---

## 4. Quick summary

| Statement | What it does |
|-----------|--------------|
| `break` | Terminates the loop completely |
| `continue` | Skips the rest of the current iteration, goes to the next |
| `pass` | Does nothing; fills an empty block |

---

## Extra Knowledge (not in the video)

### Loop `else` and `break` (important interview topic)
`else` on a loop runs **only if the loop was NOT ended by `break`**:
```python
for n in [3, 5, 7]:
    if n % 2 == 0:
        print("Found even number")
        break
else:
    print("No even number found")     # runs, because no break happened
```
Very handy for search problems ("not found" case) without a flag variable.

### `break` and `continue` affect only the INNERMOST loop
```python
for i in range(3):
    for j in range(3):
        if j == 1:
            break              # leaves the inner loop only
        print(i, j)
```
Python has **no labelled break**. To exit several loops, use one of:
- a **flag** variable,
- put the loops in a **function** and `return`,
- raise and catch an **exception**.

### `continue` in a `while` loop: don't skip the update!
```python
i = 0
while i < 10:
    if i == 5:
        continue         # ❌ i never increases → infinite loop
    i += 1
```
Fix: update `i` **before** `continue`, or restructure.

### `pass` vs `continue` vs `...` (Ellipsis) vs comment
| | Effect |
|---|--------|
| `pass` | Does nothing; execution **continues** with the next statement in the block |
| `continue` | Jumps to the next loop iteration, **skipping** the rest |
| `...` | Also a valid "do nothing" placeholder (common in type stubs/protocols) |
| `# comment` | Ignored, so it does **not** fill an empty block |

```python
def todo():
    pass            # empty function

class Empty:
    pass            # empty class

try:
    risky()
except ValueError:
    pass            # intentionally ignore the error (use sparingly!)
```

### More facts
- `break` / `continue` **outside a loop** → `SyntaxError`.
- `continue` is often used as a **guard clause** to avoid deep nesting: skip bad cases early instead of wrapping everything in `if`.
- Often a cleaner alternative to `break`-based search: `any(...)`, `next(...)`, or `x in collection`.
- Silent `except: pass` hides bugs. Catch specific exceptions and log them.

---

## Tricky Parts / Gotchas

1. **`continue` before the counter update in `while`** → infinite loop.
2. **`break` only exits one level.** Beginners expect it to leave nested loops.
3. **`pass` is not `continue`:** after `pass` the rest of the loop body still runs.
4. A loop `else` runs when there was **no `break`**. It is not "else of the last `if`".
5. A comment alone doesn't make an empty block valid; you need `pass`.
6. After `break`, the loop variable keeps its last value (e.g. `i == 5` after the example above).

---

## Interview Questions

1. **Difference between `break` and `continue`?** `break` ends the loop; `continue` skips to the next iteration.
2. **What is `pass` used for?** A no-op placeholder for empty blocks (loops, `if`, functions, classes).
3. **Difference between `pass` and `continue`?** `pass` does nothing and execution proceeds; `continue` skips the remaining body of the current iteration.
4. **What does `else` do with a `for`/`while` loop?** Runs if the loop finished without `break`.
5. **How do you break out of nested loops?** Flag, function with `return`, or exception (no labelled break).
6. **Can I write an empty function body without `pass`?** No (`pass`, `...` or a docstring works).
7. **Where would you use `continue` in a real project?** Skipping invalid/out-of-stock/blank records while processing a list.

---

### Practice ideas
1. Print numbers 1–20 but skip multiples of 3 (use `continue`).
2. Search a list for a value, print its position and stop (use `break`); add a "not found" message using loop `else`.
3. Print 1–10 but stop at the first number divisible by 7.
4. Write an empty function and an empty class using `pass`.
5. Write a nested loop and exit **both** loops when a condition is met.

**Next:** Built-in functions.
