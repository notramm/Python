# 100 Days of Python (CampusX) — Video 3: Comments in Python

> Notes from the transcript of the third video (original in Hinglish, notes in English).
> Topic: what comments are, why we write them, and how to write them in Python.

---

## 1. What is a comment?

**Definition:** A comment is a piece of text in your code that is **not executable**. The compiler/interpreter ignores it completely.

- If you write something in your file and the interpreter doesn't run it, it's a comment.
- Comments are written **for humans**, not for the machine.

---

## 2. Why write comments?

There are two main reasons:

### Reason 1: Teamwork and handover

- In a job, you build software **in a team**.
- Tomorrow you might go on leave or leave the company. Another programmer then has to understand and maintain your code.
- If they can't follow your logic, the company has a problem.
- **Good comments make it easy for someone else to read your code and understand the logic you implemented.**

### Reason 2: Documentation for yourself

- Six months from now you may open your own code and not remember why you wrote it that way.
- Comments act as **personal documentation** for future you.

> **In short:** the purpose of comments is to improve the **readability** of your code. This helps your teammates, and it also helps you in the long run.

---

## 3. How to write a comment in Python

Use the **hash symbol `#`**. Everything after `#` on that line is ignored by the interpreter.

```python
# This is a comment
```

Running this produces **no output and no error**. The interpreter skips it.

```python
# This is a useless comment
print("Hello")   # you can also put a comment after code on the same line
```

---

## 4. Multi-line comments

- Python has **no dedicated multi-line comment syntax**. Many other languages do, and some IDEs add this as a feature, but Python itself doesn't.
- To write a comment over several lines, put a **`#` at the start of every line**:

```python
# This is line 1 of my comment
# This is line 2 of my comment
# This is line 3 of my comment
```

- The instructor notes this is **not a real problem in practice**. Comments are usually short (one to three lines), and it hasn't been an issue in years of coding.

---

## 5. Quick summary

| Point | Detail |
|-------|--------|
| What | Non-executable text, ignored by the interpreter |
| Why | Readability for teammates and for your future self |
| Syntax | `#` before the comment |
| Multi-line | No special syntax; use `#` on each line |

---

## 6. What's next

- The next video continues with the next topic in the course. The transcript for this video ends here.

---

### Practice ideas

1. Write a small program that prints your name, with a comment above explaining what it does.
2. Write a 3-line comment explaining a piece of logic, using `#` on each line.
3. Put a comment at the end of a line of code and confirm the code still runs normally.
4. Comment out a `print()` line by adding `#` at the start and confirm it no longer runs.
