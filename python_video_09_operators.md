# 100 Days of Python (CampusX) — Video 9: Operators

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.

---

## 1. What are operators?

> **Operators** are symbols used to perform operations on variables and values.

Python has **seven** groups:

1. Arithmetic
2. Comparison (relational)
3. Logical
4. Bitwise
5. Assignment
6. Identity
7. Membership

---

## 2. Arithmetic operators

Let `x = 5`, `y = 2`.

| Operator | Name | Example | Result |
|----------|------|---------|--------|
| `+` | Addition | `x + y` | `7` |
| `-` | Subtraction | `x - y` | `3` |
| `*` | Multiplication | `x * y` | `10` |
| `/` | Division | `x / y` | `2.5` |
| `//` | Integer (floor) division | `x // y` | `2` |
| `%` | Modulus (remainder) | `x % y` | `1` |
| `**` | Power | `x ** y` | `25` |

- `%` gives the **remainder** after division.
- `//` removes the decimal part: `5 / 2 = 2.5`, but `5 // 2 = 2`.

---

## 3. Comparison (relational) operators

Used to compare values; the result is always `True` or `False`. Very common in logic building.

| Operator | Meaning | Example (`x=5, y=2`) |
|----------|---------|---------|
| `>` | Greater than | `x > y` → `True` |
| `<` | Less than | `x < y` → `False` |
| `>=` | Greater than or equal | `x >= y` → `True` |
| `<=` | Less than or equal | `x <= y` → `False` |
| `==` | Equal to | `x == y` → `False` |
| `!=` | Not equal to | `x != y` → `True` |

> `=` is **assignment**; `==` is **comparison**. Don't mix them up.

---

## 4. Logical operators

Three: `and`, `or`, `not`. Let `x = True`, `y = False`.

| Operator | Rule | Example | Result |
|----------|------|---------|--------|
| `or` | `True` if **at least one** is true | `x or y` | `True` |
| `and` | `True` only if **both** are true | `x and y` | `False` |
| `not` | Reverses the value | `not x` | `False` |

Truth tables:

| A | B | A and B | A or B |
|---|---|---------|--------|
| T | T | T | T |
| T | F | F | T |
| F | T | F | T |
| F | F | F | F |

You'll use these constantly (e.g. login: email correct **and** password correct).

---

## 5. Bitwise operators

They work on the **binary** representation of numbers.

Take `x = 2` (binary `010`) and `y = 3` (binary `011`):

| Operator | Name | Example | Working | Result |
|----------|------|---------|---------|--------|
| `&` | Bitwise AND | `x & y` | `010 & 011 = 010` | `2` |
| `\|` | Bitwise OR | `x \| y` | `010 \| 011 = 011` | `3` |
| `^` | Bitwise XOR | `x ^ y` | `010 ^ 011 = 001` | `1` |
| `~` | Bitwise NOT | `~x` | `-(x+1)` | `-3` |
| `>>` | Right shift | `x >> 1` | `010 → 001` | `1` |
| `<<` | Left shift | `x << 1` | `010 → 100` | `4` |

> `&` is **not** the same as `and`. `&` works bit-by-bit on binary values; `and` is a logical operator on truth values.

### Where bitwise operators are used
- **Image processing** (e.g. applying filters/masks: images are arrays of numbers, and operations like AND/OR/XOR combine them).
- Some low-level / hardware / robotics areas.
- For typical websites or ordinary software you'll rarely need them, but you should know they exist.

---

## 6. Assignment operators

```python
a = 2           # plain assignment
a += 3          # a = a + 3  → 5
```

Compound versions: `+=`, `-=`, `*=`, `/=`, `//=`, `%=`, `**=` (and bitwise ones like `&=`, `|=`, `<<=`).

### No `++` / `--` in Python

```python
a++      # SyntaxError
a += 1   # ✅ the Python way to increment
a -= 1   # ✅ decrement
```

---

## 7. Identity operators: `is`, `is not`

> Check whether two variables point to the **same memory location** (same object).

```python
a = 3
b = 3
print(a is b)       # True   (same value AND same memory location)

a = "Hello"
b = "Hello"
print(a is b)       # True   (here, same object)

a = [1, 2, 3]
b = [1, 2, 3]
print(a is b)       # False  (equal values, but two separate list objects)
print(a == b)       # True   (== compares values)
```

**Key lesson:** two things looking the same ≠ same memory location. `is` tells you about the object; `==` tells you about the value. Strings can behave differently (see gotchas).

---

## 8. Membership operators: `in`, `not in`

> Check whether something is **inside** another thing.

```python
x = "Delhi"
print("D" in x)          # True
print("Z" in x)          # False
print("Z" not in x)      # True

print(1 in [1, 2, 3])    # True
print(5 in [1, 2, 3])    # False
```

- Works on strings, lists, tuples, sets, dictionaries.
- Other languages don't give you this directly, so you'd need a loop. Python gives it as a single operator.

---

## 9. Quick summary

| Group | Operators |
|-------|-----------|
| Arithmetic | `+ - * / // % **` |
| Comparison | `> < >= <= == !=` |
| Logical | `and or not` |
| Bitwise | `& \| ^ ~ << >>` |
| Assignment | `= += -= *= /= //= %= **=` |
| Identity | `is`, `is not` |
| Membership | `in`, `not in` |

---

## Extra Knowledge (not in the video)

### Division facts
- `/` **always** returns a float: `4 / 2` → `2.0`.
- `//` is **floor division** (rounds toward negative infinity), not "truncate":
  ```python
  7 // 2       # 3
  -7 // 2      # -4    ← floor, not -3
  7.0 // 2     # 3.0   (float if either operand is float)
  ```
- `%` takes the sign of the **divisor**: `-7 % 2` → `1`, `7 % -2` → `-1`.
- `divmod(7, 2)` → `(3, 1)` gives quotient and remainder together.
- Identity: `a == (a // b) * b + (a % b)`.
- Handy uses of `%`: even/odd (`n % 2 == 0`), cycling (`i % 7`), last digit (`n % 10`), divisibility.

### Operator precedence (high → low)
1. `()` parentheses
2. `**`
3. `+x`, `-x`, `~x`
4. `* / // %`
5. `+ -`
6. `<< >>`
7. `&`
8. `^`
9. `|`
10. Comparisons, `is`, `in` (`== != > < >= <=`)
11. `not`
12. `and`
13. `or`

Examples:
```python
-2 ** 2          # -4    (** binds tighter than unary minus)
2 ** 3 ** 2      # 512   (** is right-associative: 2 ** 9)
not True or True  # True  (not first)
```

### Chained comparisons
```python
1 < x < 10          # valid, reads like math
10 == 10 > 5        # True
```

### `and` / `or` return operands, not just True/False
```python
2 and 3        # 3     (returns last evaluated operand)
0 and 3        # 0
0 or 5         # 5
"" or "default"  # 'default'  ← common idiom for defaults
```
They also **short-circuit**: `False and f()` never calls `f()`; `True or f()` never calls `f()`.

### Bitwise tricks
- `n & 1` → `1` if `n` is odd.
- `x << 1` = `x * 2`, `x >> 1` = `x // 2`.
- `a ^ a = 0`, `a ^ 0 = a`: XOR swap/"find the single number" interview tricks.
- `~x == -(x + 1)`.

### More assignment features
- **Walrus operator** (3.8+): `if (n := len(items)) > 10:` assigns and uses in one expression.
- `x = y = 0`, `a, b = b, a` (swap) are also assignments.

### `is` and caching (CPython implementation detail)
- Small integers (about **-5 to 256**) are pre-created and reused, so `a = 256; b = 256; a is b` is typically `True`, while `1000 is 1000` may not be.
- Short, identifier-like strings are often **interned** (reused). Strings with spaces or built at runtime often aren't.
- Lesson: **use `==` to compare values; use `is` only for `None`, `True`, `False`** (singletons).
  ```python
  if x is None:
  ```

### Membership speed
- `in` on a **list** scans one-by-one: O(n).
- `in` on a **set** or **dict** (keys) is a hash lookup: roughly O(1). Prefer sets for frequent lookups.
- On a dict, `in` checks **keys**, not values.
- On strings, `in` checks **substrings**: `"ell" in "Hello"` → `True`.

### Operators on other types
- `"ab" + "cd"` → `"abcd"`; `"ab" * 3` → `"ababab"`; `[1] + [2]` → `[1, 2]`; `[0] * 3` → `[0, 0, 0]`.

---

## Tricky Parts / Gotchas

1. **`++a` is not an error** but does nothing (it's `+(+a)`); `--a` is `-(-a)` = `a`. People coming from C think it increments.
2. `5 / 2` is `2.5` but `5 // 2` is `2`, and `-5 // 2` is **-3**.
3. `0.1 + 0.2 == 0.3` → `False` (floating-point). Use `math.isclose(0.1 + 0.2, 0.3)`.
4. `is` vs `==`: `[1,2] == [1,2]` is `True`; `[1,2] is [1,2]` is `False`.
5. `&` vs `and`: `2 & 3` → `2`; `2 and 3` → `3`.
6. `-2 ** 2` is `-4`, not `4`.
7. `not x == y` means `not (x == y)`, because `not` has lower precedence than `==`.
8. `a = a + 1` and `a += 1` are the same for numbers, but for **lists** `+=` modifies in place while `a = a + [..]` creates a new list.

---

## Interview Questions

1. **Difference between `/` and `//`?** True division (float) vs floor division.
2. **What does `%` do?** Remainder; works on negatives with divisor's sign.
3. **`is` vs `==`?** Identity (same object) vs equality (same value).
4. **Why is there no `++` in Python?** Integers are immutable and the design favours explicitness: use `+= 1`.
5. **What do `and`/`or` return?** One of the operands (short-circuit), not necessarily a bool.
6. **What is short-circuit evaluation?** The second operand isn't evaluated if the first decides the result.
7. **Operator precedence of `not`, `and`, `or`?** `not` > `and` > `or`.
8. **How do you check if a number is even using bitwise?** `n & 1 == 0` (careful: use `(n & 1) == 0`).
9. **What's the output of `2 ** 3 ** 2`?** `512`.
10. **What is the walrus operator?** `:=` assigns inside an expression (3.8+).

---

### Practice ideas
1. Compute `17 // 5`, `17 % 5`, `-17 // 5`, `-17 % 5`, `2 ** 10` and verify your predictions.
2. For `x = 6, y = 3` compute all bitwise operations and check them by hand in binary.
3. Check if a character is in a word using `in`, and check list identity vs equality.
4. Try `a = 5; a++` and see the error; then use `a += 1`.

**Next video:** If-else statements.
