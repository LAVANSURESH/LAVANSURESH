# SDET Interview Question Bank

Questions you will actually be asked, with the shape of a strong answer. Don't
memorise the words — memorise the *structure*, then say it in your own voice.

---

## Section 1 — Python (asked in every round)

**Q: List vs tuple vs set vs dict — when do you use each?**
> Lists for ordered mutable sequences. Tuples when immutability matters — I use
> them for locators, so a page object can't accidentally mutate one, and because
> they're hashable and can be dict keys. Sets for membership checks: `in` is O(1)
> against O(n) for a list, which matters when de-duplicating large result sets.
> Dicts for keyed lookup — most of my test data and API payloads are dicts.

**Q: `is` vs `==`?**
> `==` compares value, `is` compares identity — whether they're the same object
> in memory. Use `is` only for `None`, `True`, `False`. Small ints and short
> strings are interned, so `is` sometimes *appears* to work on values, which is
> exactly why it's a bug waiting to happen.

**Q: What's wrong with `def add_result(item, results=[])`?**
> The default is evaluated once at function definition, so every call shares the
> same list and results leak between calls. Fix: default to `None` and create the
> list inside the function. It's a classic source of cross-test contamination.

**Q: Explain decorators. Where have you used one?**
> A decorator is a function that takes a function and returns a wrapped version,
> letting you add behaviour without changing the original. In my framework I have
> `@retry(attempts=3, exceptions=(TimeoutError,))` for flaky network calls and
> `@screenshot_on_failure` for UI tests. I always use `functools.wraps` so the
> wrapper keeps the original's name and signature — without it pytest's
> introspection and fixture injection break.

**Q: Generators vs lists — why does it matter for testing?**
> A list holds everything in memory; a generator yields one item at a time. When
> I parse multi-gigabyte log files for error patterns, a generator keeps memory
> flat because I only ever hold one line. The trade-off is you can only iterate
> once and can't index it.

**Q: `@staticmethod` vs `@classmethod` vs instance method?**
> Instance methods get `self` and touch instance state — a page object's
> `login()`. Class methods get `cls` and are mostly alternative constructors —
> `Config.from_yaml(path)` is a factory. Static methods get neither and are
> namespaced utilities — a data formatter that happens to live in the class.

**Q: Shallow vs deep copy?**
> Shallow copies the outer container but shares the nested objects, so mutating a
> nested dict affects both. Deep copy recurses. It bites in test data: if a base
> payload is shallow-copied per test and one test mutates a nested field, the
> next test sees it.

**Q: How does exception handling work — what's `else` for?**
> `try` runs the risky code, `except` handles specific exceptions, `else` runs
> only if no exception was raised, `finally` always runs — which is where cleanup
> goes. I define a custom exception hierarchy rooted at `FrameworkError` so
> callers can catch at whatever granularity they need.

**Q: Composition vs inheritance in a test framework?**
> Inheritance for genuine "is-a" — `LoginPage` is a `BasePage`. Composition for
> "has-a" — a page *has* a wait helper, a logger, a driver. Deep page-object
> inheritance chains become unmaintainable fast; I keep hierarchies shallow and
> inject collaborators.

**Q: Threading vs multiprocessing vs asyncio?**
> The GIL means threads don't give CPU parallelism in CPython, but they're fine
> for I/O-bound work — parallel API calls, waiting on responses. Multiprocessing
> gives true parallelism for CPU-bound work at the cost of memory and IPC.
> Asyncio is single-threaded cooperative concurrency, best for very high-volume
> I/O. Test suites are almost always I/O-bound, so threads or `pytest-xdist`
> processes are the usual answer.

---

## Section 2 — pytest

**Q: pytest vs unittest — why pytest?**
> Plain `assert` with rich introspection instead of `assertEqual`; the fixture
> model, which is far more composable than `setUp`/`tearDown`; built-in
> parameterisation; and a plugin ecosystem — xdist, allure, rerunfailures. Less
> boilerplate, more readable tests. unittest's advantage is being in the standard
> library, which occasionally matters in locked-down environments.

**Q: Explain fixture scopes.**
> `function` (default), `class`, `module`, `package`, `session`. It's a trade
> between speed and isolation. Expensive stateless setup — an auth token, a
> driver in some designs — goes at session scope. Anything with mutable state
> stays function-scoped, otherwise you get order-dependent tests that pass alone
> and fail in a suite.

**Q: How do fixtures get injected?**
> pytest introspects the test function's signature and resolves each parameter
> name against the fixture registry, walking from the test module outward through
> `conftest.py` files. Fixtures can request other fixtures, so it builds a
> dependency graph and tears down in reverse order.

**Q: `conftest.py` — what is it and why is the hierarchy useful?**
> A per-directory fixture and hook file that pytest loads automatically — no
> import needed. Root-level `conftest.py` holds global fixtures; a `conftest.py`
> inside `tests/api/` holds API-specific ones. Fixtures resolve nearest-first, so
> a subdirectory can override a global fixture.

**Q: How do you run tests in parallel and what breaks?**
> `pytest -n 4` with xdist. What breaks: shared test data with fixed IDs, tests
> that depend on execution order, session-scoped mutable fixtures, and shared
> files or DB rows. The fixes are unique data per test (UUID suffixes), no
> inter-test dependencies, and idempotent cleanup.

**Q: How do you do data-driven testing?**
> `@pytest.mark.parametrize` for inline cases, and for larger sets I load JSON or
> CSV and parameterise from it, with `ids=` so failures name the case instead of
> showing `test_login[3]`. Parameterisation over looping inside a test, because
> each case reports independently.

**Q: Where would you use `monkeypatch`?**
> Overriding config or environment variables for a single test, and stubbing a
> function at the boundary of what I'm testing. It auto-reverts at teardown,
> which `unittest.mock.patch` only does if you use it as a context manager or
> decorator.

---

## Section 3 — Framework design (senior signal)

**Q: Design an automation framework from scratch. Walk me through it.**

Structure the answer in this order — clarify, then layer, then trade-offs:
1. **Clarify:** app type, tech stack, team size, existing CI, release cadence
2. **Layers:** tests → page objects / API clients → core utilities → drivers/transport
3. **Config:** environment-driven, no hardcoded URLs or credentials, secrets from
   the CI vault
4. **Test data:** builders and factories, created and cleaned per test, never
   shared fixtures in a mutable state
5. **Reporting:** Allure with history, artifacts on failure
6. **CI:** smoke on PR, regression nightly, full on release; failure notifications
7. **Maintainability:** naming conventions, code review for test code, linting
8. **Trade-offs I'd name out loud:** POM's boilerplate cost vs maintainability;
   session-scoped drivers' speed vs isolation risk

**Q: How do you decide what to automate?**
> Risk and repetition. High business impact plus high execution frequency plus
> stable requirements = automate first. I deliberately *don't* automate: one-off
> exploratory checks, tests against features still churning weekly, and anything
> where the oracle is subjective — visual aesthetics, usability. Automation has a
> maintenance cost, so the ROI has to clear it.

**Q: 100 UI tests, 20 fail randomly each night. What do you do?**
> First, quantify — I'd tag and track flake rate per test over a week so I'm
> working from data, not anecdote. Then categorise the causes: waits, test data
> collisions, environment instability, genuine product race conditions. Fix by
> category, not test-by-test. Quarantine the worst offenders into a separate
> non-blocking job with a tracked ticket each so the main suite becomes
> trustworthy again — a red suite everyone ignores is worse than no suite.
> Retries only as a measurement tool, never as the fix.

**Q: How do you handle test data?**
> Preference order: create what the test needs via API in setup and delete it in
> teardown; failing that, a builder that generates unique data per run; last
> resort, seeded static data that's re-seeded before each cycle. Never shared
> mutable records, because that's where parallel execution and order dependence
> break down.

**Q: How would you test an API?**
> Contract first — status codes, schema, required fields, data types. Then
> functional: happy path, negative inputs, boundary values, and idempotency.
> Then auth and authz — including the horizontal case, where user A requests
> user B's resource. Then non-functional: response time thresholds, rate
> limiting, payload size limits. And integration: does the state change actually
> persist and propagate downstream.

---

## Section 4 — Your differentiators (lead with these)

**Q: Tell me about your performance testing experience.**

Use STAR with numbers. Template from your AWS IoT work:
> *Situation:* an IoT platform ingesting telemetry from a device fleet over MQTT.
> *Task:* validate it held up at projected scale and find the breaking point.
> *Action:* built a load harness simulating N concurrent devices publishing at a
> realistic rate, ramped through load/stress/soak profiles, and captured p95/p99
> latency and broker-side metrics rather than averages.
> *Result:* identified [the bottleneck], which led to [the change], improving
> [metric] by [number].

**Fill in your real numbers before Day 1.** Vague performance stories are
forgettable; "we found the connection pool saturated at 8,000 concurrent devices
and p99 went from 4s to 300ms after tuning" is not.

**Q: Tell me about your security testing experience.**
> Cover: OWASP Top 10 familiarity, what you tested manually (auth bypass, IDOR,
> injection, session handling), tools (Burp, ZAP), and — the part most QEs can't
> claim — automating DAST into CI so a baseline scan runs on every build with
> bandit and dependency scanning alongside it. Point at the workflow file in your
> repo.

**Q: You've done manual, automation, performance, security, web and mobile —
isn't that too broad?**
> Reframe it as the point, not a weakness: *"It means I test at the right level
> instead of defaulting to the tool I know. On the IoT work, the interesting
> failures weren't functional — they were load-related and auth-related. Breadth
> is what let me see that."* Then name your depth: performance and security.

**Q: Why SDET and not senior QA?**
> The transition script in [Week 4, Day 24](04-week4-interviews.md). Keep it
> forward-looking and about ownership of tooling, never about escaping manual
> testing.

---

## Section 5 — Coding problems (practise these exact ones)

**Tier 1 — must be able to do in under 10 minutes**
1. Reverse a string / sentence word order
2. Character frequency count
3. First non-repeating character
4. Anagram check
5. Palindrome (ignoring punctuation/case)
6. Remove duplicates preserving order
7. Two sum
8. FizzBuzz variants
9. Count vowels/words in a paragraph
10. Find missing number in 1..n

**Tier 2 — 15–20 minutes**
11. Group anagrams
12. Merge overlapping intervals
13. Maximum subarray sum
14. Rotate a matrix 90°
15. Spiral matrix traversal
16. Longest substring without repeating characters
17. Flatten arbitrarily nested list
18. Fibonacci with memoisation
19. Balanced parentheses
20. Second-largest element without sorting

**Tier 3 — SDET-flavoured (most likely of all)**
21. Parse a log file, return the top 5 error types by count
22. Given a list of test-result dicts, compute pass rate per module
23. Compare two JSON objects and return the differences
24. Write `@retry` with exponential backoff
25. Write a rate limiter (token bucket)
26. Deduplicate API responses by a nested key
27. Design a `TestRunner` class that discovers and runs test functions
28. Write a context manager that times a block and logs it
29. Validate a response against an expected schema without `jsonschema`
30. Read a CSV of test data and generate parameterised cases

**Method:** talk while you code. Clarify inputs and edge cases *before* writing.
State complexity when you finish. Ask "would you like me to handle X?" rather
than silently assuming.

---

## Section 6 — SQL (~70% of interviews)

1. Second-highest salary (with and without `LIMIT`)
2. Find duplicate rows
3. Employees earning more than their manager
4. Count per group with `HAVING`
5. `INNER` vs `LEFT` vs `RIGHT` vs `FULL` join — draw them
6. `WHERE` vs `HAVING`
7. `DELETE` vs `TRUNCATE` vs `DROP`
8. What is an index, and when does it hurt?
9. Write a query joining three tables with an aggregate
10. `UNION` vs `UNION ALL`

---

## Section 7 — Questions to ask them

Never say "no questions." Ask two or three of:
- "What does the current test architecture look like, and what's the biggest
  pain point in it?"
- "What's the split between building tooling and writing tests in this role?"
- "How does the team handle flaky tests today?"
- "What's the deployment frequency, and what gates a release?"
- "How do QE and dev interact — embedded in squads or a separate function?"
- "What would a successful first 90 days look like?"

---

## Your live question log

Keep this updated after **every** interview. It becomes your real syllabus.

| Date | Company | Round | Question asked | How I did | Fix |
|---|---|---|---|---|---|
| | | | | | |
