# 100 Days of Python (CampusX) — Video 12: Loops (The `while` Loop)

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: what loops are, where they're used in real life, loop types in Python, and the `while` loop. (`for` loop is the next video.)

---

## 1. What is a loop?

> A **loop** (iteration / repetition control block) lets you run the **same piece of code repeatedly**.

Whenever you need to do the same thing again and again, put it inside a loop.

---

## 2. Where are loops used in the real world?

Many students know *how* to write loops but not *where* they're used. (The instructor once answered "to print patterns" in a placement interview and saw it didn't go well! Knowing real uses matters.)

**Example: Flipkart search results**
- Search "Redmi mobiles": the page shows, say, 24 results.
- Each result is a **container** with: an image (left), description (centre), price (right).
- Containers differ in **content** (different image/description/price) but are identical in **structure/layout**.

**How would you build this page?**
- ❌ Not by creating 24 separate containers. If 50 or 100 more phones arrive, you'd need 50 or 100 more containers. With lakhs of products, it's impossible.
- ✅ Create **one container template** and put it in a **loop**:
  1. Fetch all products from the **database**.
  2. Loop over them: each iteration fills the container with that product's data and displays it.
  3. The loop runs as many times as there are products.

The same idea powers Zomato (restaurant listings), OLX, Amazon, news feeds, search results, social media feeds, and so on. Mentioning this kind of example in an interview shows you've thought about real projects.

---

## 3. Loops in Python

Many languages have `while`, `do-while`, `for`, `for-each`/`for-in`. **Python has only two:**

1. **`while` loop** ← this video
2. **`for` loop** ← next video

---

## 4. `while` loop syntax

**C-style (for comparison):**
```c
while (condition) {
    // code
}
```

**Python:**
```python
while condition:
    # code (indented)
```

- No brackets around the condition, **colon** at the end, body **indented**.
- The loop body repeats **as long as the condition is `True`**.
- When the condition becomes `False`, the loop ends and execution continues after the loop.

---

## 5. Example: multiplication table

Ask the user for a number and print its table (1 to 10).

```python
number = int(input("Enter the number: "))

i = 1
while i < 11:
    print(number, "*", i, "=", number * i)
    i += 1
```

Sample (`number = 11`):
```
11 * 1 = 11
11 * 2 = 22
11 * 3 = 33
...
11 * 10 = 110
```

### Step-by-step (dry run)
1. `number = 11` (converted with `int()` since `input()` returns a string, as learned earlier).
2. Initialise `i = 1`.
3. Check `i < 11` → `1 < 11` is `True` → enter the loop.
4. Print `11 * 1 = 11`.
5. `i += 1` → `i = 2`.
6. Back to the condition: `2 < 11` → `True` → print `11 * 2 = 22` → `i = 3`.
7. Repeat ... until `i = 10`: prints `11 * 10 = 110`, then `i = 11`.
8. Check `11 < 11` → `False` → the loop **terminates**; the program ends.

### The three ingredients of a `while` loop
| Part | In the example |
|------|----------------|
| **Initialisation** | `i = 1` |
| **Condition** | `i < 11` |
| **Update** | `i += 1` |

> If you **forget the update**, `i` never changes, the condition is always `True`, and you get an **infinite loop**.

---

## 6. Quick summary

- Loop = repeat code.
- Real-world: product lists, restaurant lists, feeds: one template, many data rows.
- Python has only `while` and `for`.
- `while condition:` + indented body; remember initialise / condition / update.
- Use `i += 1` (no `i++`).

---

## Extra Knowledge (not in the video)

### `break`, `continue`, and `else`
```python
i = 1
while i <= 10:
    if i == 5:
        i += 1
        continue        # skip the rest of this iteration
    if i == 8:
        break           # exit the loop completely
    print(i)
    i += 1
```

**`while ... else`** (a favourite interview question): the `else` block runs when the loop ends **normally** (condition became false), but **not** if you left via `break`.
```python
n = 1
while n < 5:
    n += 1
else:
    print("Loop finished normally")     # runs
```

### Infinite loops (on purpose)
```python
while True:
    answer = input("Type 'q' to quit: ")
    if answer == "q":
        break
```
Press **Ctrl + C** to stop an accidental infinite loop (raises `KeyboardInterrupt`).

### Python has no `do-while`: emulate it
```python
while True:
    value = input("Enter a positive number: ")
    if value.isdigit() and int(value) > 0:
        break           # runs the body at least once
```

### Common patterns
- **Input validation:** keep asking until the input is valid. (Great for improving the login program from Video 10: allow 3 attempts.)
- **Sentinel loops:** keep going until a special value is entered.
- **Countdown:** `while n > 0: print(n); n -= 1`
- **Sum of digits:**
  ```python
  n = 1234
  total = 0
  while n > 0:
      total += n % 10
      n //= 10
  ```
- **Walrus in a loop (3.8+):** `while (line := input()) != "quit":`
- Using the loop condition directly on a collection: `while stack:` (runs while the list is non-empty).

### `while` vs `for`
| `while` | `for` |
|---------|-------|
| Use when you **don't know** in advance how many times | Use when you **know** the count or are iterating over a collection |
| You manage the counter yourself | Python manages iteration (`for i in range(...)`) |
| Easier to create infinite loops by mistake | Safer for fixed iteration |

The same table in a `for` loop is shorter: `for i in range(1, 11): print(number, "x", i, "=", number * i)`

### Nicer output with f-strings
```python
print(f"{number} x {i} = {number * i}")
```

---

## Tricky Parts / Gotchas

1. **Forgetting `i += 1`** → infinite loop (program hangs).
2. **Off-by-one:** `while i < 11` prints 1–10, but `while i <= 10` is the same, while `while i < 10` stops at 9. Check boundaries.
3. **Updating in the wrong place:** if `i += 1` is outside the loop's indentation, it never runs inside the loop.
4. **Float counters:** `while x != 1.0: x += 0.1` may never be exactly `1.0` (floating-point) → infinite loop. Use `<` or integer counters.
5. **`continue` before the update** can create an infinite loop (the update is skipped). Update first, or restructure.
6. **Forgetting `int()` around `input()`:** `"5" < 11` → `TypeError`.
7. **Modifying a list while looping over it** with `while`/`for` causes skipped items. Be careful.
8. Variables defined in a loop remain available after it (no block scope in Python).

---

## Interview Questions

1. **What loops does Python have?** `while` and `for`. (No `do-while`.)
2. **Where have you used loops in a real project?** Rendering lists of items from a DB (products, posts, orders), processing records, retrying operations.
3. **What is an infinite loop? How do you stop one?** Loop whose condition never becomes false; Ctrl+C or `break`.
4. **Difference between `break` and `continue`?** `break` exits the loop; `continue` skips to the next iteration.
5. **What does `else` do on a `while` loop?** Runs if the loop ends without `break`.
6. **How do you emulate `do-while` in Python?** `while True:` + `break` on a condition.
7. **`while` vs `for`: when to use which?** `while` for condition-driven, unknown count; `for` for known sequences/ranges.
8. **Why is there no `i++`?** Use `i += 1`.

---

### Practice ideas
1. Print numbers 1 to 20, then 20 down to 1.
2. Print the sum of numbers from 1 to `n` (user input).
3. Reverse a number (e.g. `1234` → `4321`) using `%` and `//`.
4. Count digits in a number.
5. Number guessing game: keep asking until the user guesses correctly (use `while True` and `break`).
6. Upgrade the login program: allow **3 password attempts**.

**Next video:** The `for` loop.
