# Reflection

**1. Which issues were the easiest to fix, and which were the hardest? Why?**
The easiest were the style issues reported by Flake8 and Pylint: renaming functions to snake_case, adding docstrings, splitting long lines, renaming l and removing the unnecessary else. They are mechanical and don't change behaviour. The hardest were the security issues from Bandit. Fixing the SQL injection meant understanding why string formatting is dangerous and switching to parameterised queries.

**2. Did the static analysis tools report any false positives? If so, describe one example.**
Bandit's B311 warning on random.choice is a false positive here which is picking a song to shuffle is not a security use, so a predictable PRNG does nothing.Then it is switched to secrets.choice because clears the report, but `# nosec B311` with a comment would also be justified.

**3. How would you integrate static analysis tools into your actual software development workflow?**
Run Flake8 and Pylint automatically in the editor and as a pre-commit hook so issues are caught before every commit. Add all three tools to a CI pipeline that runs on each push and pull request.

**4. What tangible improvements did you observe in the code quality, readability, or potential robustness after applying the fixes?**
The Pylint score went from 4.57/10 to 9.49/10 and Bandit and Flake8 now report no issues. The code is safer now. It is more correct as separate calls to add_song can no longer share one playlist list and total_plays only catches database errors. It is easier to read and reuse.
