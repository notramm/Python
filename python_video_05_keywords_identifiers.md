# 100 Days of Python (CampusX) — Video 5: Keywords and Identifiers

> Notes from the transcript (Hinglish → English), with extra knowledge, gotchas and interview material added.

---

## 0. First: Python is case-sensitive

- `Name` and `name` are **different** identifiers. Upper and lower case are not interchangeable.
- Keep this in mind throughout the course. It's one of the most common beginner errors.

---

## 1. Keywords

> **Keywords** are **reserved words** that have a special meaning to the Python interpreter. They **cannot** be used as variable names (or any other identifier).

### Why do keywords exist?

- You write code in "English-like" **high-level** language (`for`, `while`, `if`, `else`, `True`, `False`...).
- The machine only understands **low-level** code (binary). The compiler/interpreter converts one to the other.
- To make sense of your English-like code, the interpreter relies on certain words that have **fixed special meaning**. When it sees `if`, it knows a conditional is coming. When it sees `while`, it knows a loop is coming.
- If you used one of those words as a variable name, the interpreter would be confused (is `for` a loop or your variable?). So these words are reserved.

### Seeing the list

```python
import keyword
print(keyword.kwlist)
```

- The video says there are **33** keywords. In current Python versions the list is **35** (see extra knowledge).
- You can recognise many of them: `False`, `True`, `None`, `and`, `or`, `not`, `while`, `for`, `if`, `else`, `import`, `def`, `class`...
- **You do not need to memorise them.** You'll learn them through use (and through errors).

```python
for = 5        # SyntaxError: invalid syntax
```

---

## 2. Identifiers

> **Identifier:** the **name** you give to a variable, function, class, module or object.

### Rules for naming identifiers

1. Must **start with** a letter (`a–z`, `A–Z`) or an **underscore** `_`.
2. After the first character, you can use letters, **digits**, and underscores.
3. **No special characters** except underscore (no `@`, `$`, `-`, space, etc.).
4. Cannot be a **keyword**.
5. Case-sensitive.

| Identifier | Valid? | Why |
|------------|--------|-----|
| `name` | ✅ | letters only |
| `_name` | ✅ | starts with underscore |
| `name1` | ✅ | digit after the first character |
| `name_1_x` | ✅ | letters, digits, underscores |
| `__name` | ✅ | double underscore is allowed (special meaning in OOP, covered later) |
| `1name` | ❌ | starts with a digit |
| `name-1` | ❌ | hyphen is a special character (it's read as subtraction) |
| `my name` | ❌ | space |
| `for` | ❌ | keyword |

```python
1name = "Ram"     # SyntaxError: invalid decimal literal
name-1 = 5        # SyntaxError: cannot assign to expression
```

---

## 3. Quick summary

| | Keywords | Identifiers |
|---|----------|-------------|
| What | Reserved words with special meaning | Names you give to things |
| Who decides | Python | You |
| Can you use them as names? | **No** | Yes (that's their purpose) |
| Check list | `keyword.kwlist` | Follow naming rules |

---

## Extra Knowledge (not in the video)

### Keyword count depends on the version
- Python ≤ 3.6: 33 keywords. Python 3.7+: **35** (`async` and `await` became full keywords).
- **Soft keywords** (only reserved in certain contexts): `match`, `case`, `_` (3.10+), and `type` (3.12+). You can still use them as variable names, but it's best not to.
- Always check your own version: `keyword.kwlist`, `keyword.softkwlist` (3.9+), and `keyword.iskeyword("for")`.

### The full keyword list (current)
```
False   None    True    and     as      assert  async   await
break   class   continue def    del     elif    else    except
finally for     from    global  if      import  in      is
lambda  nonlocal not    or      pass    raise   return  try
while   with    yield
```
Note: `True`, `False`, `None` are capitalised. `true`, `false`, `none` are **not** keywords (just valid identifiers, but a terrible idea).

### Built-ins are NOT keywords, and that's a trap
`print`, `list`, `str`, `int`, `sum`, `max`, `min`, `id`, `type`, `input`, `len` are **built-in functions**, not keywords. Python lets you overwrite them, which breaks things silently:

```python
list = [1, 2, 3]
print = 5
print("hi")        # TypeError: 'int' object is not callable
list("abc")        # TypeError, you shadowed the built-in
```
Fix: `del print` restores the built-in, or just don't use these names.

### Underscore conventions
| Pattern | Meaning |
|---------|---------|
| `_x` | "Internal use" by convention (not enforced) |
| `x_` | Avoid clashing with a keyword, e.g. `class_` |
| `__x` | Name mangling inside classes (OOP) |
| `__x__` | "Dunder" / magic methods, e.g. `__init__`, `__str__` |
| `_` | Throwaway variable, e.g. `for _ in range(3)` |

### More facts
- **Unicode identifiers are allowed** in Python 3: `नाम = "Ram"` or `π = 3.14` work (not recommended for shared code).
- **No length limit** on identifiers (but keep them readable).
- `"abc".isidentifier()` tells you whether a string is a syntactically valid identifier.
- **PEP 8 naming:** `snake_case` (variables, functions), `CamelCase` (classes), `UPPER_SNAKE` (constants).
- Descriptive names (`first_number`) beat short ones (`a`) in team projects.

---

## Tricky Parts / Gotchas

1. `print = ...`, `list = ...`, `sum = ...`, `id = ...` shadow built-ins and cause confusing errors later.
2. `Name` ≠ `name`. A typo in case gives `NameError`.
3. `if` is a keyword but `If` is not (so `If = 5` is valid, yet confusing).
4. `True` is a keyword but `true` is not: `true` will raise `NameError` if you forgot to define it.
5. Module names matter too: naming your file `random.py` or `keyword.py` can break `import random` / `import keyword`.

---

## Interview Questions

1. **What are keywords in Python? How do you get the list?** Reserved words; `import keyword; keyword.kwlist`.
2. **Can a keyword be used as a variable name?** No (SyntaxError).
3. **What are the rules for identifiers?** Start with letter/underscore, then letters/digits/underscores, no special characters, not a keyword, case-sensitive.
4. **Is `print` a keyword?** No, it's a built-in function (it was a statement in Python 2).
5. **Difference between a keyword and a built-in?** Keywords are part of the language grammar and can't be reassigned; built-ins are ordinary names in the `builtins` module that *can* be shadowed.
6. **What does a single leading underscore mean?** A convention for "private/internal".

---

### Practice ideas
1. Print `keyword.kwlist` and `len(keyword.kwlist)` on your machine.
2. Try creating variables named `1abc`, `abc-1`, `_abc`, `abc_1`, `for` and note which fail.
3. Run `list = [1,2]` then `list("abc")`, observe the error, then `del list`.

**Next video:** User input.
