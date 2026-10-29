# Reflection

**1. Which issues were the easiest to fix, and which were the hardest? Why?**
The easiest were the style issues reported by Flake8 and Pylint: renaming functions to snake_case, adding docstrings, splitting long lines, renaming `l` and removing the unnecessary `else`. They are mechanical and don't change behaviour. The hardest were the security issues from Bandit. Fixing the SQL injection meant understanding why string formatting is dangerous and switching to parameterised queries. Replacing `pickle` needed a check that JSON could represent the same data. The mutable default argument also took thought, because the program's own demo never triggers it: you only see the bug when `add_song` is called twice without passing a playlist.

**2. Did the static analysis tools report any false positives? If so, describe one example.**
Bandit's B311 warning on `random.choice` is effectively a false positive here: picking a song to shuffle is not a security or cryptographic use, so a predictable PRNG does no harm. I switched to `secrets.choice` anyway because it costs nothing and clears the report, but `# nosec B311` with a comment would also be justified. Similarly, `pickle` in this program only loads data it created itself, so the real risk is low today; it becomes a real problem the moment a playlist is loaded from a file or a network request.

**3. How would you integrate static analysis tools into your actual software development workflow?**
Run Flake8 and Pylint automatically in the editor and as a pre-commit hook so style issues are caught before every commit. Add all three tools to a CI pipeline (e.g. GitHub Actions) that runs on each push and pull request, failing the build on any Bandit medium/high finding or a Pylint score below a set threshold such as 9.5. Keep a shared config (`.pylintrc`, `.flake8`) in the repo so the whole team uses the same rules.

**4. What tangible improvements did you observe in the code quality, readability, or potential robustness after applying the fixes?**
The Pylint score went from 4.57/10 to 10.00/10, and Bandit and Flake8 now report no issues. The code is safer: titles containing apostrophes no longer break the INSERT, and loading a playlist can no longer execute arbitrary code. It is more correct: separate calls to `add_song` no longer share one playlist list, and `total_plays` only catches database errors. It is easier to read and reuse thanks to consistent naming, docstrings, short lines and a `__main__` guard that lets the module be imported without running the demo.
