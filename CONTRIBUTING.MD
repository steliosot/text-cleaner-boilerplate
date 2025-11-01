# Contributing Guidelines

These rules ensure clean, safe, beginner-friendly data cleaning code.

If you're opening a pull request or writing new logic, follow the checklist below to meet quality and consistency standards.

## Auto-Checkable Data Cleaning Rules

These checks are ideal for automation (e.g., with Watchflow.dev) or as a human code review guide.

1. Encoding Check

- Use `encoding="utf-8"` on all `open(...)` calls.

- Prevents unreadable characters and OS-specific issues.

- Example:

  ```python
  open("file.txt", "r", encoding="utf-8")
  ```

2. Context Manager Check

- Always use `with open(...)` to read/write files.

- Prevents file handle leaks and ensures files are properly closed.

- Example:

  ```python
  with open("file.txt", "r", encoding="utf-8") as f:
      ...
  ```

3. Output Directory Rule

- Never write outputs to `data/raw/`. Always use `data/clean/`.
- This protects original/raw data for reproducibility.

4. Rules Config Usage

- Use `rules.yaml` to control cleaning behavior.

- Avoid hardcoding `.lower()`, `.strip()`, etc.

- Example:

  ```python
  if rules.get("lower", True):
      text = text.lower()
  ```

5. Lowercasing and Stripping Check

- Normalize input using `.strip()` and `.lower()` (if rules allow).

- ✅ Example:

  ```python
  text = text.strip().lower()
  ```

6. Error Handling with Try/Except

- Wrap file access in try/except to prevent crashes and give friendly errors.

- ✅ Example:

  ```python
  try:
      with open("file.txt", "r", encoding="utf-8") as f:
          ...
  except FileNotFoundError:
      print("❌ Input file not found.")
  ```

7. Input Validation Before Cleaning

- Check that a line exists before processing.

- ✅ Example:

  ```python
  if line:
      text = clean_line(line, rules)
  ```

8. File Exists Check

- Use `Path(...).exists()` before trying to open files.

- ✅ Example:

  ```python
  from pathlib import Path
  if not Path("data/raw/input.txt").exists():
      raise FileNotFoundError("Input file is missing.")
  ```

9. PEP8 Compliance

- Follow the Python PEP8 style guide for code formatting.
- ✅ Include:
  - 4 spaces per indent level
  - Spaces around operators: `x = 3`, not `x=3`
  - Max line length: 79 characters
  - No trailing whitespace

10. PR Too Large

- Pull requests should not exceed ~400 lines of changes.

- Why: Large PRs are hard to review, prone to merge conflicts, and usually mix too many changes.

- ✅ What to do:

  - Split large changes into multiple PRs by feature or file

  - Use descriptive commit history for each step

11. Missing Conventional Commit Format

- All PRs must include commits that follow the Conventional Commits format.
- Why: Helps automation, changelogs, and clearer git history.
- ✅ **Valid examples:**

```
feat(cleaner): add support for punctuation removal
fix(cli): handle missing input file with clear error
docs(readme): add example input/output
```

* ❌ **Invalid:**

```
"update cleaner.py"  
"more fixes"  
"final changes"
```