# 100 Days of Python (CampusX) — Video 30: Recursion

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.
> Topic: recursion, the call stack, classic problems (multiplication, factorial, palindrome, Fibonacci/rabbits, power set) and memoization.

---

## 1. What is recursion?

> **Recursion** is a technique where a **function calls itself** to solve a problem.

- It lets you solve some problems **without loops**.
- Big idea: solve a problem by solving a **smaller version of the same problem**.

### Recursion vs loops: when to use which?
- **Placements / exams / interviews:** you're often asked for the recursive solution, so learn it well.
- **Real company projects:** loops (iteration) are usually preferred: simpler for teams, usually faster, and use less memory. Recursion is often harder to read and debug.
- But some problems (trees, graphs, divide-and-conquer, subsets) are far more natural with recursion.

---

## 2. How to think recursively (2 steps)

1. **Find the base case:** the simplest version whose answer you already know. This is where recursion **stops**.
2. **Express the problem using a smaller version of itself** (the recursive case), and call the function again on it.

> No base case → the function calls itself forever → `RecursionError`.

---

## 3. Example 1: multiplication using only addition

`a × b` = `a + a + ... (b times)`. Smaller version: `a × b = a + (a × (b - 1))`.

```python
def multiply(a, b):
    if b == 1:                       # base case
        return a
    return a + multiply(a, b - 1)    # recursive case

print(multiply(3, 4))                # 12
```

**Loop version** (for comparison):
```python
def multiply(a, b):
    result = 0
    for _ in range(b):
        result += a
    return result
```

### Tracing `multiply(3, 4)`
```
multiply(3,4) = 3 + multiply(3,3)
              = 3 + (3 + multiply(3,2))
              = 3 + (3 + (3 + multiply(3,1)))
              = 3 + (3 + (3 + 3)) = 12
```
Going **down** until the base case, then values come **back up**.

---

## 4. The call stack: what's really happening

- Every function call gets its **own frame** (workspace) in memory (Video 29).
- When `multiply(3,4)` calls `multiply(3,3)`, the first call **waits**. Frames pile up: `4 → 3 → 2 → 1`.
- At the base case, `multiply(3,1)` returns `3` to its caller, which returns to *its* caller, and so on **back up**.
- **Rule:** a return value always goes to **whoever called** the function.
- The last frame created is the first to finish → **LIFO (Last In, First Out)**. That's the **Stack** data structure. (Interview: *"Which data structure is used for recursion?"* → **Stack**.)
- Visualise it at **pythontutor.com**.

---

## 5. Example 2: factorial

`n! = n × (n-1) × ... × 1`.  So `n! = n × (n-1)!`.

```python
def factorial(n):
    if n == 1:                         # base case
        return 1
    return n * factorial(n - 1)

print(factorial(5))                    # 120
```
Same pattern as multiplication (5 × 4 × 3 × 2 × 1).
(Extra: `0! = 1`, so a safer base case is `if n <= 1: return 1`.)

---

## 6. Example 3: palindrome check using recursion

A string reads the same forwards and backwards (`madam`, `abba`).

**Approach:**
- If the first and last characters match, strip them and check what's left.
- If they don't match → not a palindrome.

**Base case:** a string of length **0 or 1** is trivially a palindrome.
(Why 0 as well? For even-length palindromes like `abba`, stripping pairs eventually leaves an **empty** string. A base case of only "length 1" would never be reached and would crash.)

```python
def is_palindrome(s):
    if len(s) <= 1:                      # base case (0 or 1 characters)
        return True
    if s[0] == s[-1]:
        return is_palindrome(s[1:-1])    # strip first & last, check the rest
    return False

print(is_palindrome("abba"))     # True
print(is_palindrome("madam"))    # True
print(is_palindrome("hello"))    # False
```
Uses slicing from Video 20.

---

## 7. Example 4: the rabbit problem (Fibonacci)

**Problem:** A pair of baby rabbits produces a new pair every month (starting once mature). How many pairs are there after *n* months?

Following the pattern month by month: `1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233 ...`
**Each term = sum of the previous two** → this is the **Fibonacci sequence**.

```python
def fib(n):
    if n == 0 or n == 1:                 # base cases
        return 1
    return fib(n - 1) + fib(n - 2)

print(fib(12))                           # 233
```
Short and elegant. But...

---

## 8. The problem: this recursion is extremely slow

Try bigger inputs:
```python
import time

start = time.time()
print(fib(35))
print("Time:", time.time() - start)
```
Run time **explodes** as `n` grows: a little more than doubles with each increment. Beyond ~40–50 it effectively never finishes. On a real website, one such call could freeze the page.

### Why?
To compute `fib(5)` you compute `fib(4)` and `fib(3)`. But `fib(4)` *also* computes `fib(3)` again, and so on. The **same subproblems are solved over and over**:

```
                    fib(5)
                  /        \
             fib(4)        fib(3)
            /     \        /    \
        fib(3)  fib(2)  fib(2) fib(1)
        ...
```
The number of calls grows roughly like **2ⁿ** (exponential time complexity).

---

## 9. The fix: memoization (dynamic programming)

> **Memoization:** store the answer of each subproblem the first time you compute it. Next time, just look it up.

Use a **dictionary** as the "memory" (key = `n`, value = `fib(n)`):

```python
def fib(n, memo):
    if n in memo:                         # already computed? just return it
        return memo[n]

    if n == 0 or n == 1:                  # base cases
        return 1

    memo[n] = fib(n - 1, memo) + fib(n - 2, memo)    # compute once, store
    return memo[n]

memo = {}
print(fib(50, memo))                      # instant
```
- Each value is computed **once** → time drops from exponential to **linear (O(n))**.
- Cost: extra **memory** for the dictionary. This is the **space–time trade-off**: spend a bit more memory to save a lot of time.
- This technique is the core idea of **Dynamic Programming** (a big topic in advanced DSA courses).

### The modern, one-line way
```python
from functools import cache        # Python 3.9+

@cache
def fib(n):
    if n < 2:
        return 1
    return fib(n - 1) + fib(n - 2)

print(fib(100))
```
`@cache` (or `@lru_cache(maxsize=None)`) adds automatic memoization.

---

## 10. Example 5: power set (all subsets)

The **power set** of `{1, 2, 3}` is the set of **all subsets**:
`[], [1], [2], [3], [1,2], [1,3], [2,3], [1,2,3]` → 2ⁿ subsets.

**Idea ("take or skip"):** pick the first item. Build all subsets **without** it, and all subsets **with** it. Do that recursively on the rest.

```
items = [1, 2, 3]
 take 1?  → no:  subsets of [2,3]
          → yes: 1 + each subset of [2,3]
 ... keep splitting until the list is empty (base case: one subset, the empty one)
```

```python
def power_set(items):
    if not items:                          # base case
        return [[]]

    first, rest = items[0], items[1:]
    without_first = power_set(rest)                    # subsets not containing first
    with_first = [[first] + s for s in without_first]  # subsets containing first
    return without_first + with_first

print(power_set([1, 2, 3]))
# [[], [3], [2], [2, 3], [1], [1, 3], [1, 2], [1, 2, 3]]
```
> *(The video's transcript ends mid-explanation here; the code above completes the idea it describes.)*

---

## 11. Quick summary

| Concept | Remember |
|---------|----------|
| Recursion | A function calling itself on a smaller problem |
| Base case | The stopping condition: mandatory |
| Call stack | Frames stack up, then return values flow back up (LIFO) |
| Naive Fibonacci | Exponential time (repeated work) |
| Memoization | Store results (dictionary) → linear time; space–time trade-off |
| Industry vs interviews | Loops preferred at work; recursion asked in interviews |

---

## Extra Knowledge (not in the video)

### Recursion limit
```python
import sys
print(sys.getrecursionlimit())     # 1000 by default
sys.setrecursionlimit(5000)        # can be raised, but with care
```
Python **does not optimise tail recursion**, so very deep recursion (like `fib(5000)` even with memoization, or `factorial(5000)`) raises `RecursionError`. For deep problems, use **iteration** (or an explicit stack).

### Iterative Fibonacci (O(n) time, O(1) space)
```python
def fib(n):
    a, b = 1, 1
    for _ in range(n - 1):
        a, b = b, a + b
    return b
```
(With this indexing `fib(0) = fib(1) = 1`.) Many books index so that `fib(0) = 0`, `fib(1) = 1`; check the definition before comparing answers.

### Recursion vs iteration summary
| | Recursion | Iteration (loop) |
|---|-----------|------------------|
| Code | Often shorter, mirrors the math | Often longer but explicit |
| Memory | Uses call stack (risk of `RecursionError`) | Constant |
| Speed | Function-call overhead | Faster |
| Best for | Trees, graphs, backtracking, divide & conquer | Simple repetition |

### Time complexity cheat-sheet
- Factorial / multiply recursion: **O(n)** calls.
- Naive Fibonacci: **O(2ⁿ)**.
- Memoized Fibonacci: **O(n)**.
- Power set: **O(2ⁿ)** (there are 2ⁿ results).

### Other classic recursion problems
Sum of digits, reverse a string, GCD (Euclid), binary search, Tower of Hanoi, permutations, N-Queens, flatten nested lists, tree traversals, merge sort / quick sort.

### Memoization pitfalls
- Don't use a **mutable default** for the memo (`def fib(n, memo={})`). It persists across calls (sometimes handy, often a bug).
- `@cache` stores results **forever** (memory grows): use `lru_cache(maxsize=...)` for bounded caches, and `fib.cache_clear()` to reset.
- Works only for **pure functions** (same input → same output) with **hashable** arguments.

### Timing code properly
Use `time.perf_counter()` instead of `time.time()` for benchmarks (Video 18 notes).

### Mutual recursion
Two functions can call each other (`is_even` ↔ `is_odd`). Also fine, but needs base cases for both.

---

## Tricky Parts / Gotchas

1. **Missing or wrong base case** → infinite recursion → `RecursionError: maximum recursion depth exceeded`.
2. **Palindrome base case must include length 0** (not only 1).
3. **Naive recursive Fibonacci is exponential**, not "just slow".
4. **Return values flow to the caller**: the value from the innermost call is passed back up step by step.
5. **Don't forget `return`** before the recursive call (`multiply(a, b-1)` without `return` gives `None`).
6. **Python has no tail-call optimisation**, so deep recursion isn't free.
7. **Memoization needs the dictionary shared across calls** (pass it in, use a global, or use `@cache`).
8. **Off-by-one in Fibonacci indexing:** know whether `fib(0)` is 0 or 1.

---

## Interview Questions

1. **What is recursion? What are its components?** A function calling itself; base case + recursive case.
2. **What happens without a base case?** Infinite recursion → `RecursionError`.
3. **Which data structure does recursion use internally?** The call stack (a stack, LIFO).
4. **Recursion vs iteration, pros and cons?** See the table above.
5. **Why is naive recursive Fibonacci slow? Time complexity?** Repeated subproblems; O(2ⁿ).
6. **What is memoization? How does it help?** Cache results of subproblems; O(n) time; costs memory.
7. **Difference between memoization and dynamic programming?** Memoization = top-down caching; DP also includes bottom-up tabulation.
8. **What's Python's default recursion limit and how do you change it?** 1000; `sys.setrecursionlimit()`.
9. **Write a recursive function to reverse a string / sum digits / compute GCD.** (Practice below.)
10. **What is tail recursion? Does Python optimise it?** Recursive call as the last action; Python doesn't optimise it.
11. **How many subsets does an n-element set have?** 2ⁿ.

---

### Practice ideas
1. Write recursive functions for: sum of 1..n, sum of digits, reverse a string, `a ** b`, GCD.
2. Trace `factorial(4)` on paper, drawing the stack.
3. Time `fib(30)`, `fib(32)` and `fib(35)` (naive) and note the growth; then use memoization.
4. Use `@cache` on `fib` and compare with the dictionary version.
5. Write recursive binary search on a sorted list.
6. Generate all permutations of `[1, 2, 3]` recursively.

**Next video:** Lambda functions, higher-order functions (`map`, `filter`, `reduce`) and comprehensions.
