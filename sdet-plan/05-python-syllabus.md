# The Python Syllabus — SDET-Scoped

Everything you need. Nothing you don't. Tick as you go.

**Priority key:** 🔴 asked in almost every interview · 🟡 asked often ·
🟢 asked at senior level or for platform roles

---

## 1. Core language 🔴

- [ ] Variables, dynamic typing, `type()`, `isinstance()`
- [ ] Numbers, `int`/`float` precision, `//`, `%`, `**`, `divmod`
- [ ] Strings: immutability, all common methods, slicing, f-strings
- [ ] Booleans, truthiness, `and`/`or` short-circuiting, `None`
- [ ] `is` vs `==` 🔴 (identity vs equality — near-guaranteed question)
- [ ] Type hints: `list[str]`, `dict[str, int]`, `Optional`, `Union`, `Callable`

## 2. Data structures 🔴

- [ ] `list` — methods, complexity of each, when to use
- [ ] `tuple` — immutability, unpacking, as dict keys
- [ ] `set`/`frozenset` — O(1) membership, set operations
- [ ] `dict` — insertion ordering, `get`/`setdefault`/`pop`, `items`/`keys`/`values`
- [ ] Comprehensions: list, dict, set, generator — including nested
- [ ] `collections`: `Counter`, `defaultdict`, `OrderedDict`, `namedtuple`, `deque`
- [ ] Shallow vs deep copy 🔴 (`copy` vs `deepcopy` — trap question)
- [ ] Time complexity of every operation you use 🔴

## 3. Control flow & functions 🔴

- [ ] `if/elif/else`, ternary, `match` (3.10+)
- [ ] Loops, `range`, `enumerate`, `zip`, `break`/`continue`/`else` on loops
- [ ] Function definition, return values, multiple returns
- [ ] `*args`, `**kwargs`, keyword-only args
- [ ] Mutable default argument trap 🔴
- [ ] Scope: LEGB, `global`, `nonlocal`
- [ ] Closures 🟡
- [ ] `lambda`, `map`, `filter`, `sorted(key=)`, `reduce`
- [ ] Recursion + memoisation (`functools.lru_cache`)

## 4. OOP 🔴 — the highest-yield section

- [ ] Classes, `__init__`, `self`, instance vs class attributes
- [ ] `@classmethod` vs `@staticmethod` vs instance method 🔴
- [ ] Inheritance, `super()`, MRO
- [ ] Multiple inheritance, mixins 🟡
- [ ] Encapsulation: `_protected`, `__private` name mangling
- [ ] Polymorphism, duck typing
- [ ] Abstract base classes (`abc`) 🟡
- [ ] `@property`, setters
- [ ] Dunders: `__str__`, `__repr__` 🔴, `__eq__`, `__hash__`, `__len__`,
      `__enter__`/`__exit__`, `__call__`, `__getitem__`
- [ ] `dataclasses` 🟡 and `pydantic` models
- [ ] Composition over inheritance 🔴 — know *why* frameworks prefer it
- [ ] SOLID principles with a test-framework example for each 🟡

## 5. Exceptions & context managers 🔴

- [ ] `try/except/else/finally` — and what `else` is actually for
- [ ] Exception hierarchy, catching specific vs bare `except`
- [ ] Custom exception classes, exception chaining (`raise ... from ...`)
- [ ] `with` statement, `__enter__`/`__exit__`
- [ ] `contextlib.contextmanager`, `suppress`, `ExitStack` 🟡

## 6. Decorators & generators 🔴

- [ ] Functions as first-class objects
- [ ] Decorator anatomy, `functools.wraps` 🔴
- [ ] Decorators with arguments (three-level nesting) 🔴
- [ ] Class-based decorators 🟢
- [ ] Stacking decorators and execution order
- [ ] `yield`, generator functions, laziness and memory 🔴
- [ ] Generator expressions
- [ ] `__iter__`/`__next__`, custom iterators 🟡
- [ ] `itertools`: `chain`, `product`, `combinations`, `groupby`, `islice` 🟡

## 7. Modules, packaging, environments 🟡

- [ ] Imports: absolute vs relative, `__name__ == "__main__"`
- [ ] Packages, `__init__.py`, package layout
- [ ] `venv`, `pip`, `requirements.txt`, `pyproject.toml`
- [ ] `sys.path`, import errors and how to debug them
- [ ] `poetry` or `uv` basics 🟢

## 8. Standard library for testing 🔴

- [ ] `os`, `sys`, `pathlib`, `shutil`, `subprocess`
- [ ] `json` 🔴, `csv`, `yaml` (PyYAML), `configparser`, `python-dotenv`
- [ ] `re` — regex 🔴 (extraction from logs/responses is a common live task)
- [ ] `datetime`, `time`, timezones
- [ ] `logging` 🔴 — levels, handlers, formatters, per-module loggers
- [ ] `random`, `uuid`, `hashlib`, `base64`
- [ ] `argparse` for CLI tools
- [ ] `typing`, `functools`, `operator`

## 9. Testing libraries 🔴

- [ ] `pytest` — full coverage: see [Week 2](02-week2-pytest-api.md)
- [ ] `unittest` — enough to compare and contrast 🔴
- [ ] `unittest.mock` — `Mock`, `patch`, `side_effect`, where to patch 🔴
- [ ] `requests` + `Session` + retries 🔴
- [ ] `jsonschema`, `pydantic` for response validation 🟡
- [ ] `selenium`, `playwright`, `appium-python-client`
- [ ] `locust` for load 🟡
- [ ] `faker` for test data
- [ ] `allure-pytest`, `pytest-html` for reporting
- [ ] `pytest-xdist`, `pytest-rerunfailures`, `pytest-cov`, `pytest-mock`

## 10. Concurrency 🟢 (🟡 for platform roles)

- [ ] Threading, `ThreadPoolExecutor`
- [ ] Multiprocessing, `ProcessPoolExecutor`
- [ ] The GIL — what it does and doesn't block 🟡
- [ ] I/O-bound vs CPU-bound: which tool for which 🟡
- [ ] `asyncio`: `async`/`await`, event loop, `gather`
- [ ] `concurrent.futures` for parallel API calls in tests

## 11. Code quality 🟡

- [ ] PEP 8 and why it matters in a shared framework
- [ ] `black`, `ruff`/`flake8`, `mypy`
- [ ] Docstrings, type hints as documentation
- [ ] `pre-commit` hooks
- [ ] Code review habits — what you look for in someone else's test code

## 12. Design patterns for test frameworks 🟡

| Pattern | Where it appears in a framework |
|---|---|
| Singleton | Config / driver manager (and why it's often a smell) |
| Factory | Driver creation, test data builders |
| Builder | Fluent request/test-data construction |
| Strategy | Swappable wait strategies, reporters |
| Observer | Test event listeners, hooks |
| Adapter | Wrapping Selenium and Playwright behind one interface |
| Page Object | UI abstraction 🔴 |
| Facade | Simplified client over a complex API |

Be able to name the pattern **and point at the file in your repo where you used
it.** That combination is what makes the answer land.

---

## Learning resources (pick few, go deep)

**Primary**
- *Automate the Boring Stuff* — only if you need the Week 1 basics faster
- *Fluent Python* (Ramalho) — chapters on data model, functions, OOP. This is
  the book that makes you sound senior.
- Real Python articles on decorators, generators, context managers
- Official pytest docs — genuinely excellent, read them end to end
- Playwright Python docs

**Practice**
- NeetCode 150 — Arrays & Hashing, Two Pointers, Strings sections only
- HackerRank Python track for syntax fluency
- Exercism Python track for idiomatic feedback

**Skip these** (common time sinks that don't pay off for SDET interviews):
long video courses watched passively, Django/Flask, data science libraries,
advanced algorithms (DP, graphs) unless targeting FAANG.
