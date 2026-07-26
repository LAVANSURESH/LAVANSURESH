# Week 1 — Python Core, Deeply (Days 1–7)

**Objective:** By Sunday you can open a blank file and write idiomatic Python
without Googling syntax, and explain OOP concepts out loud with test-automation
examples.

**Parallel track:** Resume rewrite + first 15 applications go out this week.
Do not postpone them to Week 2.

---

## Setup — do this in the first 60 minutes of Day 1

```bash
# Python 3.12+, then:
mkdir -p ~/sdet && cd ~/sdet
python -m venv .venv && source .venv/bin/activate
pip install pytest requests python-dotenv pydantic rich ipython
git init && gh repo create python-sdet-practice --public --source=. --push
```

Install: VS Code + Python extension + Pylance + Ruff. Set up `ruff` and `black`
on save. Interviewers notice clean, formatted code.

Create one file per day: `day01.py`, `day02.py`, … Commit every night.

---

## Day 1 (Mon) — Data structures, the real ones

**Learn (1.5 hr)**
- `list`, `tuple`, `set`, `dict` — mutability, ordering guarantees, when each is
  the right choice
- Time complexity: why `x in list` is O(n) and `x in set` is O(1) — **this exact
  question gets asked**
- Slicing, negative indices, `list[::-1]`, step slicing
- List/dict/set comprehensions, including nested and conditional
- `collections`: `Counter`, `defaultdict`, `namedtuple`, `deque`

**Practice (1.5 hr)** — write these from scratch, no help:
1. Reverse a string without `[::-1]` and without `reversed()`
2. Count character frequency in a string; then redo with `Counter`
3. Find the first non-repeating character
4. Check if two strings are anagrams (two ways: sorting, and counting)
5. Remove duplicates from a list preserving order
6. Flatten a nested list of arbitrary depth (recursion)
7. Given a list of test-result dicts, group them by status into a `defaultdict`

**Interview framing:** every one of these is a real SDET screening question.
Practise saying the complexity out loud as you write.

**Job track (1 hr):** Rewrite the top third of your resume using
[`08-resume-and-job-strategy.md`](08-resume-and-job-strategy.md). Update your
LinkedIn headline to `SDET | Python Automation | API & Performance | AWS IoT`.

---

## Day 2 (Tue) — Strings, functions, scope

**Learn**
- String methods that matter: `split`, `join`, `strip`, `replace`, `startswith`,
  `find` vs `index`, `format` vs f-strings
- f-string formatting: `f"{value:.2f}"`, `f"{name!r}"`, `f"{x=}"`
- Function arguments: positional, keyword, default, `*args`, `**kwargs`
- **The mutable default argument trap** (`def f(x=[])`) — classic interview trap
- Scope: LEGB rule, `global`, `nonlocal`, closures
- `lambda`, `map`, `filter`, `sorted(key=...)`, `any`, `all`, `zip`, `enumerate`

**Practice**
1. Word frequency in a paragraph, sorted by count descending, ties alphabetical
2. Sort a list of dicts by a nested key, handling missing keys gracefully
3. Write `retry(func, attempts=3, delay=1)` as a plain higher-order function
4. Palindrome check ignoring case, spaces, punctuation
5. Longest word / longest common prefix in a list of strings
6. Explain out loud, recorded: "what is a closure and where would you use one in
   a test framework?" (Answer: config capture, parameterised assertions.)

**Job track:** Send 5 applications. Message 3 recruiters on LinkedIn.

---

## Day 3 (Wed) — OOP part 1

This is the single most-tested topic in SDET interviews after pytest.

**Learn**
- Class vs instance attributes; `__init__`; `self`
- Instance / `@classmethod` / `@staticmethod` — and when each is used in a
  framework (factory = classmethod, utility = staticmethod)
- Inheritance, `super()`, MRO basics
- Encapsulation conventions: `_single` vs `__double` name mangling
- `@property`, getters/setters
- Dunder methods: `__str__`, `__repr__`, `__eq__`, `__len__`, `__enter__`/`__exit__`

**Practice — build, don't read**

```python
# Build a mini Page Object hierarchy — the exact structure they ask you to draw
class BasePage:
    def __init__(self, driver): ...
    def find(self, locator): ...
    def wait_for(self, locator, timeout=10): ...

class LoginPage(BasePage):
    USERNAME = ("id", "user")
    def login(self, user, pwd) -> "HomePage": ...

class HomePage(BasePage): ...
```

Then:
1. `TestResult` class with `__eq__`, `__repr__`, and a `duration` property
2. An abstract `Notifier` base with `SlackNotifier` and `EmailNotifier` subclasses
3. A `Config` class using `@classmethod` as a factory: `Config.from_yaml(path)`

**Job track:** 5 applications.

---

## Day 4 (Thu) — OOP part 2 + exceptions

**Learn**
- Composition vs inheritance — **know why frameworks prefer composition**
- Abstract base classes via `abc.ABC`, `@abstractmethod`
- Polymorphism and duck typing
- Multiple inheritance and mixins (how `unittest` mixins work)
- Exceptions: `try/except/else/finally`, exception hierarchy, custom exceptions,
  `raise ... from ...`, bare `except` and why it's wrong
- Context managers: `with`, `__enter__`/`__exit__`, `contextlib.contextmanager`

**Practice**
1. Custom exception tree: `FrameworkError` → `ElementNotFoundError`,
   `APIResponseError`, `ConfigError`
2. A context manager `@contextmanager def temp_user(api):` that creates a test
   user and always deletes it in teardown — **this is the answer to "how do you
   guarantee cleanup?"**
3. A retry decorator preview: catch a specific exception, back off, re-raise
   after N attempts
4. Write a `DriverManager` context manager that quits the driver even on failure

**Job track:** 5 applications. Reply to any recruiter within 2 hours — speed
matters more than polish at this stage.

---

## Day 5 (Fri) — Decorators, generators, iterators

The topics that separate "wrote some scripts" from "writes frameworks."

**Learn**
- First-class functions, functions returning functions
- Decorator anatomy; `functools.wraps` and why omitting it breaks pytest
- Decorators with arguments (three levels of nesting)
- Class-based decorators
- Generators, `yield`, lazy evaluation, memory benefits
- `iter`/`next`, custom iterator with `__iter__`/`__next__`
- Generator expressions vs list comprehensions

**Practice**
1. `@timer` — logs execution time of a test function
2. `@retry(attempts=3, delay=2, exceptions=(TimeoutError,))` — with args
3. `@screenshot_on_failure(driver)` — a real SDET utility
4. A generator that streams a 1 GB log file line by line and yields only ERROR
   lines (answer to "how would you parse huge logs?")
5. A generator that produces test data batches of size N
6. Explain out loud: "how does `@pytest.fixture` work under the hood?"
   (It registers the function in a registry and injects by name via
   introspection of the test signature.)

**Job track:** Apply to 5. Book at least one interview for next week.

---

## Day 6 (Sat) — Files, JSON, modules, standard library

**Learn (3 hr)**
- File I/O with `with open()`, modes, encoding
- `json` — `load`/`loads`/`dump`/`dumps`, custom encoders
- `csv`, `configparser`, `yaml` (PyYAML), `.env` via `python-dotenv`
- `pathlib.Path` over `os.path`
- `os`, `sys`, `subprocess`, `argparse`
- `datetime`, `time`, `random`, `re`, `logging`
- Modules, packages, `__init__.py`, absolute vs relative imports
- Virtual envs, `pip`, `requirements.txt`, `pyproject.toml`

**Regex block (45 min)** — SDETs use this constantly:
`\d \w \s`, quantifiers, groups, `re.search` vs `match` vs `findall`,
non-greedy `.*?`. Practise: extract emails, extract a JWT from a header,
parse a log timestamp.

**Build (3 hr) — Mini Project 1: `testdata-gen`**
A CLI tool that generates realistic test data:
```
python -m testdata_gen --type user --count 100 --format json --out users.json
```
- `argparse` CLI, `Faker` or your own generators
- Outputs JSON / CSV
- Config via YAML
- Proper package layout, logging, custom exceptions
- README with usage examples

**Commit it as your first real public repo.** It is small, but it proves you
package code like a developer, not like someone writing scripts.

---

## Day 7 (Sun) — Consolidation + first mock

**Morning (3 hr): rapid-fire practice.** 15 problems, 15 min each, timer on:
strings ×5, dicts/lists ×5, OOP design ×3, decorators/generators ×2.
Sources: NeetCode "Arrays & Hashing", or the list in
[`07-interview-question-bank.md`](07-interview-question-bank.md).

**Afternoon (2 hr): SQL crash block.** Asked in ~70% of SDET interviews:
`SELECT/WHERE/GROUP BY/HAVING/ORDER BY`, all four JOIN types, subqueries,
`COUNT/SUM/AVG`, and **the second-highest-salary question**. Two hours is
genuinely enough for the level asked.

**Evening (1 hr): self-mock, recorded.**
Answer these on camera, 3–4 min each:
1. "Walk me through your background." (see the script in `07-...`)
2. "Explain OOP with an example from your automation work."
3. "What is a decorator and where have you used one?"
4. "How do you handle test data?"

Watch it back. Note filler words, rambling, missing structure. This hurts. Do it
anyway.

**Week 1 gate — do not move on until:**
- [ ] `day01.py`–`day06.py` committed
- [ ] `testdata-gen` public with README
- [ ] Can write a decorator with arguments from memory
- [ ] Resume rewritten, LinkedIn updated
- [ ] 20 applications sent
