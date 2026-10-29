# Issues Found — music_catalog.py

| Issue | Type | Line(s) | Description | Fix Approach |
|-------|------|---------|-------------|--------------|
| SQL injection (Bandit B608, Medium) | Security | 10 | INSERT query built with `%` string formatting, so a title like `Don't Stop` breaks the query and crafted input could inject SQL | Use a parameterised query with `?` placeholders and pass values as a tuple |
| Unsafe deserialisation (Bandit B301/B403, Medium/Low) | Security | 1, 33 | `pickle.loads` can execute arbitrary code if the blob comes from an untrusted source | Replace `pickle` with `json.dumps` / `json.loads` (a playlist is just a list of strings) |
| Insecure random generator (Bandit B311, Low) | Security | 17 | `random.choice` is a non-cryptographic PRNG | Use `secrets.choice` |
| Mutable default argument (Pylint W0102) | Bug | 9 | `playlist=[]` is created once and shared across calls, so songs leak into later playlists | Default to `None` and create a new list inside the function |
| Bare except (Pylint W0702, Flake8 E722) | Bug | 39 | Catches every exception, including `KeyboardInterrupt`, hiding real errors | Catch `sqlite3.Error` only; also return 0 when `SUM` gives `None` |
| Non-snake_case function names (Pylint C0103) | Style | 9, 15, 28, 32, 36 | `addSong`, `shufflePick`, etc. violate PEP 8 naming | Rename to `add_song`, `shuffle_pick`, `save_playlist`, `load_playlist`, `total_plays` |
| Missing docstrings (Pylint C0114/C0116) | Style | 1, 9–43 | Module and functions undocumented | Add a module docstring and one per function |
| Unnecessary else after return (Pylint R1705) | Style | 24 | `else` adds needless nesting | Remove `else`, de-indent the return |
| Line too long (Flake8 E501, Pylint C0301) | Style | 6, 10, 21, 49 | Lines over 79 characters | Split across lines using implicit continuation |
| Ambiguous variable name `l` (Flake8 E741) | Style | 47 | `l` looks like `1` / `I` | Rename to `loaded` |
| Multiple statements on one line (Flake8 E701, Pylint C0321) | Style | 48 | `if ...: print(...)` on one line | Put `print` on its own indented line |
| No main guard | Style | 52 | `main()` runs on import, which also makes the module hard to test | Wrap in `if __name__ == "__main__":` |
