# 100 Days of Python (CampusX) — Video 31: Lambda, Higher-Order Functions, `map` / `filter` / `reduce` & Comprehensions

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.

---

## 1. Lambda functions (anonymous functions)

> A **lambda function** is a small function with **no name**, written in **one line**.

### Syntax
```python
lambda inputs: expression
```
- `lambda` keyword → the inputs (parameters) → colon → **one expression** (what to compute and give back).

```python
# normal function
def square(x):
    return x * x

# lambda version
square = lambda x: x * x
print(square(5))            # 25

# two inputs
add = lambda x, y: x + y
print(add(2, 3))            # 5
```
Assigning a lambda to a name (like `square`) is only for demonstration. Real use is shown below.

### Differences: lambda vs normal function

| | Normal function | Lambda |
|---|-----------------|--------|
| Name | Has a name | **Anonymous** (no name) |
| `return` | Needs an explicit `return` | **No `return` statement.** The expression's value is returned automatically |
| Size | Many lines | **Single line / single expression only** |
| Purpose | Reusable code | Quick, **one-time** use |
| Usually used | Called by name many times | Passed **into** other functions (higher-order functions) |

### Conditional inside a lambda
Use the one-line `if/else` expression:
```python
is_even = lambda x: x % 2 == 0
print(is_even(4))           # True

check = lambda x: "even" if x % 2 == 0 else "odd"
print(check(7))             # odd

starts_with_a = lambda s: s[0] == "a"
```

---

## 2. Higher-order functions (HOF)

> A **higher-order function** is a function that either **takes another function as input** or **returns a function** as output. (Possible because functions are objects, Video 29.)

### Why they're powerful
A normal function does the *same* job every time you give it data. A higher-order function lets you also tell it **how** to behave by passing a function.

### Example: summing selected numbers
Goal: from a list, find (a) the sum of even numbers, (b) the sum of odd numbers, (c) the sum of multiples of 3.

Without HOF: three similar functions. With HOF: **one** function + a different lambda each time:

```python
L = [11, 14, 15, 18, 20, 21, 26, 30]

def transform(L, condition):
    total = 0
    for i in L:
        if condition(i):          # the passed-in function decides
            total += i
    return total

print(transform(L, lambda x: x % 2 == 0))    # sum of evens
print(transform(L, lambda x: x % 2 != 0))    # sum of odds
print(transform(L, lambda x: x % 3 == 0))    # sum of multiples of 3
```
You passed not only **data** but also the **behaviour**. Lambdas are perfect for this: short, throw-away behaviours.

---

## 3. Built-in higher-order functions

### 3.1 `map(function, iterable)`: transform every item
```python
L = [1, 2, 3, 4, 5]

result = map(lambda x: x * 2, L)
print(list(result))                       # [2, 4, 6, 8, 10]
```
- `map` returns a **map object** (an iterator), which you can't see directly. Convert it with `list(...)` (or `tuple`, `set`, or loop over it).
- It applies the function to **each** item and collects the results.

More examples:
```python
list(map(lambda x: x % 2 == 0, L))        # [False, True, False, True, False]

students = [
    {"name": "Ram",   "marks": 80},
    {"name": "Shyam", "marks": 65},
]
list(map(lambda s: s["name"], students))  # ['Ram', 'Shyam']  ← extract just the names
```
Typical use: data comes as a list of records (e.g. from a database) and you want one field from each.

### 3.2 `filter(function, iterable)`: keep only some items
The function must return **True/False**. Only items where it's `True` are kept.
```python
L = [3, 8, 2, 10, 5]
list(filter(lambda x: x > 5, L))          # [8, 10]

fruits = ["apple", "orange", "banana", "mango", "kiwi"]
list(filter(lambda f: "a" in f, fruits))  # fruits containing the letter a
```

### `map` vs `filter`
- **`map`** → works on **every** item and **transforms** it (output length = input length).
- **`filter`** → **selects** items by a **condition** (output may be shorter).

### 3.3 `reduce(function, iterable)`: combine all items into ONE value
`reduce` isn't built in; import it:
```python
from functools import reduce

L = [1, 2, 3, 4, 5]
print(reduce(lambda x, y: x + y, L))      # 15
```
**How it works (the function takes TWO inputs):**
```
step 1: take first two items      1 + 2  = 3
step 2: take that result + next   3 + 3  = 6
step 3:                           6 + 4  = 10
step 4:                           10 + 5 = 15   ← final answer
```
It keeps shrinking the list down to a single value.

More examples:
```python
reduce(lambda x, y: x * y, L)                    # 120  (product)
reduce(lambda x, y: x if x > y else y, L)        # 5    (largest item)
```
- With the "larger of two" logic, it finds the maximum; with "smaller", the minimum.

---

## 4. List and dictionary comprehensions

A **comprehension** is a compact way to build a new list/dictionary from an existing iterable (often replacing `map` and `filter`).

### List comprehension
```python
[expression  for item in iterable  if condition]
```
(`if condition` is optional.)

```python
L = [1, 2, 3, 4, 5]

doubled = [i * 2 for i in L]                        # [2, 4, 6, 8, 10]
squares = [i * i for i in range(1, 11)]
even_squares = [i * i for i in range(1, 11) if i % 2 == 0]   # [4, 16, 36, 64, 100]

fruits = ["apple", "orange", "banana", "mango"]
starts_with_a = [f for f in fruits if f[0] == "a"]           # ['apple']
```

The long way vs comprehension:
```python
# loop version
result = []
for i in L:
    result.append(i * 2)

# comprehension
result = [i * 2 for i in L]
```

### Dictionary comprehension
```python
{key_expr: value_expr  for key, value in iterable  if condition}
```
```python
squares = {i: i * i for i in range(1, 6)}       # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

scores = {"Ram": 80, "Shyam": 45, "Gita": 92}
passed = {name: s for name, s in scores.items() if s > 50}
# {'Ram': 80, 'Gita': 92}

# filter by key length
{k: v for k, v in scores.items() if len(k) > 3}
```
(Remember `.items()` gives key-value pairs, Video 27.)

---

## 5. Quick summary

| Tool | What it does | Example |
|------|--------------|---------|
| `lambda` | One-line anonymous function | `lambda x: x * 2` |
| Higher-order function | Takes/returns a function | `transform(L, lambda ...)` |
| `map(f, L)` | Apply `f` to every item | doubling, extracting fields |
| `filter(f, L)` | Keep items where `f` is True | evens, names containing "a" |
| `reduce(f, L)` | Collapse to one value | sum, product, max |
| List comprehension | Build a list in one line | `[i*2 for i in L if i > 2]` |
| Dict comprehension | Build a dict in one line | `{k: v for k, v in d.items()}` |

---

## Extra Knowledge (not in the video)

### Where lambdas shine: `key=` arguments
```python
students = [("Ram", 80), ("Shyam", 65), ("Gita", 92)]

sorted(students, key=lambda s: s[1])               # sort by marks
sorted(students, key=lambda s: s[1], reverse=True) # highest first
max(students, key=lambda s: s[1])                  # topper
words.sort(key=lambda w: len(w))                   # by length
```

### Modern Python prefers comprehensions
These are equivalent, and most Python developers prefer the comprehension for readability:
```python
list(map(lambda x: x * 2, L))        ==   [x * 2 for x in L]
list(filter(lambda x: x > 5, L))     ==   [x for x in L if x > 5]
```
Use `map`/`filter` when you already have a named function: `list(map(str.upper, names))`, `list(map(int, ["1", "2"]))`.

### `map`/`filter` return lazy iterators (single use)
```python
m = map(lambda x: x * 2, [1, 2, 3])
print(list(m))      # [2, 4, 6]
print(list(m))      # []   ← already consumed!
```

### Multiple iterables in `map`
```python
list(map(lambda a, b: a + b, [1, 2, 3], [10, 20, 30]))   # [11, 22, 33]
```

### `reduce` details
```python
from functools import reduce
reduce(lambda x, y: x + y, [], 0)       # initial value avoids an error on an empty list
reduce(lambda x, y: x + y, L, 100)      # starts at 100
```
- For common cases prefer built-ins: `sum(L)`, `max(L)`, `min(L)`, `math.prod(L)`, `all()`, `any()`.
- `operator.add` and `operator.mul` can replace trivial lambdas: `reduce(operator.mul, L)`.

### Handy `filter` trick
`filter(None, L)` removes all **falsy** items (`0`, `""`, `None`, `[]`...).

### More comprehension types
```python
{x % 3 for x in range(10)}                  # set comprehension: {0, 1, 2}
(x * x for x in range(10))                  # generator expression (lazy, no list)
sum(x * x for x in range(10))               # use generators inside sum/max/min/any/all
[(a, b) for a in range(3) for b in range(3) if a != b]    # nested loops
[x if x > 0 else 0 for x in nums]           # if/else goes BEFORE the for
```
- Loop variables in comprehensions **don't leak** into the surrounding scope in Python 3.
- Comprehensions are generally **faster than equivalent loops with `.append`**.
- Don't cram complex logic into one: if it needs more than about one line of thought, use a normal loop.

### Lambda trap: late binding in loops
```python
funcs = [lambda: i for i in range(3)]
print([f() for f in funcs])            # [2, 2, 2]  ← not [0, 1, 2]!
funcs = [lambda i=i: i for i in range(3)]   # ✅ capture value via default argument
print([f() for f in funcs])            # [0, 1, 2]
```

### Style note (PEP 8)
Assigning a lambda to a name (`f = lambda x: ...`) is discouraged. Use `def` when the function needs a name. Lambdas are for passing inline.

### Decorators preview
A **decorator** is a higher-order function that takes a function and returns a new function (it "wraps" behaviour). You'll meet them later; they rely on exactly what you learned in Videos 29 and 31.

---

## Tricky Parts / Gotchas

1. **A lambda is limited to a single expression.** No statements (`print` works as an expression but `return`, assignments, loops don't).
2. **No `return` in lambdas.** The expression value *is* the return value.
3. **`map()` / `filter()` return iterators, not lists**: wrap in `list()` to see results (and they can be used only once).
4. **`filter`'s function must return a truthy/falsy value**: returning the item itself can silently drop `0` or `""`.
5. **`reduce` needs `from functools import reduce`**; it's not a built-in in Python 3.
6. **`reduce`'s function takes exactly two inputs.**
7. **Late binding** in lambdas created inside loops (see above).
8. **Comprehension `if` vs `if/else`:** a filter goes at the **end** (`... for x in L if cond`), a conditional value goes at the **start** (`x if cond else y for x in L`).
9. **Dict comprehensions** need `.items()` to get both key and value.
10. Overusing one-liners hurts readability.

---

## Interview Questions

1. **What is a lambda function? Differences from a `def` function?** Anonymous, one-expression, implicit return, meant for short throw-away use.
2. **What is a higher-order function? Give examples.** Takes/returns functions: `map`, `filter`, `reduce`, `sorted(key=...)`, decorators.
3. **`map` vs `filter` vs `reduce`?** Transform each / select by condition / combine to a single value.
4. **What do `map` and `filter` return?** Lazy iterators (map/filter objects).
5. **Write a list comprehension for squares of even numbers 1–10.** `[i*i for i in range(1, 11) if i % 2 == 0]`
6. **List comprehension vs `map`/`filter`: which is preferred?** Comprehensions are usually more readable ("Pythonic").
7. **How do you sort a list of dictionaries by a key?** `sorted(data, key=lambda d: d["marks"])`.
8. **What is a dictionary comprehension?** `{k: v for ...}` to build dicts concisely.
9. **Why can a lambda be passed as an argument?** Functions are first-class objects.
10. **What is a generator expression?** A lazy comprehension using `()`; produces items on demand.
11. **What's the lambda late-binding gotcha?** Lambdas capture the *variable*, not its value at creation time.

---

### Practice ideas
1. Use `map` + `lambda` to convert a list of temperatures from °C to °F.
2. Use `filter` + `lambda` to keep only words longer than 4 letters.
3. Use `reduce` to find the product of a list, then the largest item.
4. Sort a list of `(name, age)` tuples by age using `sorted` and a lambda.
5. Rewrite each of your `map`/`filter` solutions as a list comprehension.
6. From a dictionary of `{name: marks}` build a new dictionary with only the students who scored above 60, using a dict comprehension.
7. Write your own higher-order function `apply_twice(f, x)` that returns `f(f(x))`.

**Next:** Continue with the next topic in the playlist (likely modules/file handling/OOP).
