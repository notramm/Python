# 100 Days of Python (CampusX) — Video 13: Number Guessing Game

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: a small project that combines `if-else` + `while` loop + user input + type conversion, plus the `random` module.

---

## 1. The game

1. When the program starts, it **randomly picks a number between 1 and 100** (the **jackpot**).
2. The user has to **guess** it.
3. After every wrong guess the program says:
   - **"Guess higher"** if the guess is too small
   - **"Guess lower"** if the guess is too big
4. When the guess is correct, the program says it's correct and tells the user **how many attempts** it took.

Example run (jackpot = 50 hidden from the user):
```
Guess the number: 25     → Guess higher
Guess again: 75          → Guess lower
Guess again: 60          → Guess lower
Guess again: 50          → Correct guess!  Attempts: 4
```

---

## 2. New concept: the `random` module

To pick a random number we need a **module** (a ready-made library; covered fully in Video 18).

```python
import random

jackpot = random.randint(1, 100)
```

- `import random` loads the module (like `#include` in C/C++, `import` in Java).
- `random.randint(a, b)` returns a random **integer between `a` and `b`, including both end points**. So 1 and 100 are both possible.
- Run the program several times and you get a different number each time.

---

## 3. Building the logic (step by step)

**Step 1: generate the jackpot**
```python
import random
jackpot = random.randint(1, 100)
```

**Step 2: take the first guess.** `input()` returns a string, so convert to `int` (Video 7), otherwise you can't compare it with a number.
```python
guess = int(input("Guess the number: "))
counter = 1
```

**Step 3: decide the loop.** We don't know how many guesses the user will need, so use a **`while` loop** (Video 12).
> Keep going **as long as the guess is not equal to the jackpot.**

```python
while guess != jackpot:
```

**Step 4: inside the loop** (meaning the guess is **wrong**), there are exactly two possibilities:
```python
    if guess < jackpot:
        print("Guess higher")
    else:
        print("Guess lower")
```

**Step 5: ask again (inside the loop!) and count the attempt**
```python
    guess = int(input("Guess again: "))
    counter += 1
```

**Step 6: after the loop** means the guess was right:
```python
print("Correct guess!")
print("Attempts:", counter)
```

### Complete program

```python
import random

jackpot = random.randint(1, 100)
counter = 1
guess = int(input("Guess the number: "))

while guess != jackpot:
    if guess < jackpot:
        print("Guess higher")
    else:
        print("Guess lower")
    guess = int(input("Guess again: "))
    counter += 1

print("Correct guess!")
print("Attempts:", counter)
```

### How the counter works
- `counter = 1` because the first guess has already been taken.
- Each time the user is asked again, add 1.
- When the loop ends, `counter` = total number of attempts.

---

## 4. The big lesson: build in small modules

The instructor's advice:
- Don't try to write the whole thing at once, since it will feel hard.
- First solve the **core logic** (random number, compare, loop). Ignore the attempt counter at first.
- When that works, **add the next small feature** (the counter), then test again.
- Keep growing a small working program into a bigger one.

This "start small, extend step by step" approach is how real projects are built.

---

## 5. Concepts used in this project

| Concept | Where |
|---------|-------|
| Module import | `import random` |
| Function call | `random.randint(1, 100)` |
| User input | `input()` |
| Type conversion | `int(input(...))` |
| `while` loop | repeat until guessed correctly |
| `if / else` | higher or lower |
| Comparison operators | `!=`, `<` |
| Counter variable | `counter += 1` |

---

## Extra Knowledge (not in the video)

### Why this needs a `while` loop and not `for`
You don't know in advance how many guesses will be needed. Unknown iteration count → `while`.

### Best strategy: binary search
Always guess the **middle** of the remaining range (50, then 25 or 75, ...). Each guess halves the possibilities, so for 1–100 you will **never need more than 7 guesses** (2⁷ = 128 ≥ 100). This is the idea behind **binary search**, a very important algorithm (a common interview topic).

### A cleaner structure (avoids repeating the `input` line)
```python
import random

jackpot = random.randint(1, 100)
attempts = 0

while True:
    guess = int(input("Guess the number: "))
    attempts += 1

    if guess == jackpot:
        print(f"Correct! You took {attempts} attempts.")
        break
    elif guess < jackpot:
        print("Guess higher")
    else:
        print("Guess lower")
```

### A more robust version: validation and an attempt limit
```python
import random

jackpot = random.randint(1, 100)
max_attempts = 7

for attempt in range(1, max_attempts + 1):
    try:
        guess = int(input(f"Attempt {attempt}/{max_attempts}, your guess: "))
    except ValueError:
        print("Please enter a whole number.")
        continue                      # (note: this still uses up an attempt)

    if guess == jackpot:
        print(f"Correct! Attempts: {attempt}")
        break
    print("Guess higher" if guess < jackpot else "Guess lower")
else:
    print(f"Out of attempts. The number was {jackpot}.")
```
(Uses `for/else`, `try/except`, `continue`, `break`, f-strings and a ternary expression, all covered in the next videos.)

### About the `random` module
- `random.randint(a, b)` → includes **both** `a` and `b`.
- `random.randrange(a, b)` → **excludes** `b` (works like `range`).
- `random.random()` → float in `[0.0, 1.0)`.
- `random.seed(42)` makes results **repeatable** (very useful when testing a game like this).
- It's **pseudo-random** and **not secure**. For passwords, tokens or anything security-related, use the **`secrets`** module (`secrets.randbelow(100)`).

---

## Tricky Parts / Gotchas

1. **Forgetting `int()`** → comparing `"50" < 30` raises `TypeError`.
2. **Putting the "ask again" `input` outside the loop** → infinite loop (the guess never changes).
3. **Off-by-one in the counter:** start at 1 if the first guess is taken before the loop, or at 0 if you count inside the loop. Mixing them gives a wrong count.
4. The loop condition is "**not** equal", so the code after the loop is only reached on the correct guess.
5. Typing letters or an empty line crashes the program with `ValueError` unless you handle it.
6. `random.randint(1, 100)` includes 100; `random.randrange(1, 100)` does not.

---

## Interview Questions

1. **What does `random.randint(1, 10)` return?** An integer from 1 to 10, both inclusive.
2. **`randint` vs `randrange`?** `randint` includes the end; `randrange` excludes it.
3. **Why use `while` instead of `for` here?** The number of iterations isn't known beforehand.
4. **What is the maximum number of guesses needed with the best strategy for 1–100?** 7 (binary search).
5. **Is `random` safe for passwords?** No; use `secrets`.
6. **How would you make a random result reproducible?** `random.seed(value)`.

---

### Practice ideas
1. Add a limit of 7 attempts and show "You lose" if it's exceeded.
2. Ask the user to choose the range (e.g. 1–1000) before the game starts.
3. Handle invalid input with `try/except`.
4. Reverse the game: you think of a number and the computer guesses using binary search.

**Next video:** the `for` loop.
