**Name:** OP Medha | **SRN:** PES2UG24CS332

# Issues Found and Fixed

File: `tower_maintenance.py`. Line numbers refer to the original file (commit 8689fd4).

| # | Issue | Type | Line(s) | Description | Fix Approach |
|---|-------|------|---------|-------------|--------------|
| 1 | SQL injection (Bandit B608, Medium) | Security | 11 | addTower builds the INSERT query with % string formatting, so a site name containing SQL (like x'); DROP TABLE towers;--) would be run as SQL. | Used a parameterised query with ? placeholders and passed the values as a tuple. |
| 2 | Unsafe pickle (Bandit B301, Medium; B403, Low) | Security | 1, 43 | snapshot() pickles and unpickles the rows. pickle.loads can execute arbitrary code on untrusted data, and the round trip does nothing useful. | Removed the pickle import and made snapshot() return the fetched rows directly. |
| 3 | Hardcoded password (Bandit B105, Low) | Security | 6 | NOC_PASSWORD is written in the source code, so it ends up in the repo and git history. | Now read from the environment with os.environ.get("NOC_PASSWORD"). |
| 4 | Broad exception (Pylint W0718) | Bug / Quality | 20-21 | except Exception hides the real error and only prints "service failed". | Catch sqlite3.Error only and log the actual error message with logging. |
| 5 | Global statement (Pylint W0603) and constant naming (C0103) | Design | 7, 16 | service_count is a module-level variable changed with global, which hides state and is badly named. | Moved the counter into a STATS dictionary, so no global is needed. |
| 6 | Function naming (Pylint C0103) | Style | 10, 15, 34 | addTower, serviceTower and riskLevel use camelCase instead of snake_case (PEP 8). | Renamed to add_tower, service_tower and risk_level. |
| 7 | Missing docstrings (Pylint C0114, C0116) | Style | 1, 10, 15, 25, 34, 42, 46 | The module and every function have no docstring. | Added a docstring to the module and each function. |
| 8 | Line too long (Flake8 E501, Pylint C0301) | Style | 5, 11, 18, 35, 43 | Several lines are longer than 79 characters. | Split long strings and calls across lines. |
| 9 | Too many blank lines (Flake8 E303) | Style | 25 | Three blank lines before overdue. | Reduced to two. |
| 10 | Missing whitespace (Flake8 E225) | Style | 50 | total=len(...) has no spaces around =. | Changed to total = len(...). |
| 11 | Unnecessary else after return (Pylint R1705) | Style | 36-39 | The else after return is not needed. | Removed the else and returned "normal" directly. |
| 12 | Old-style string formatting (Pylint C0209) | Style | 11 | % formatting used for the query. | Removed along with fix 1. |
| 13 | No input validation | Robustness | 10-12 | addTower accepts empty sites and non-integer values. | Added checks that raise ValueError. |

## Results

| Tool | Before | After |
|------|--------|-------|
| Pylint | 5.14/10 | 10.00/10 |
| Bandit | 4 issues (2 Medium, 2 Low) | 0 issues |
| Flake8 | 7 issues | 0 issues |

Program output is unchanged: `['Ridge-A'] high 2 1`. The original reports are in `reports_before/`.
