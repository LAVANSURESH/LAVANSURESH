# Week 2 — pytest Mastery + API Automation Framework (Days 8–14)

**Objective:** Ship **Repo #1** — a production-shaped API test framework — and be
able to defend every design decision in it.

**Parallel track:** Interviews begin. Target 3+ screening calls this week.

> Why API first, before UI? It builds faster, runs in CI without a browser, and
> is what interviewers probe hardest for design quality. It is also the honest
> centre of modern SDET work.

---

## Day 8 (Mon) — pytest fundamentals

**Learn**
- Test discovery rules, naming conventions, `assert` rewriting
- `-v`, `-s`, `-k`, `-x`, `--lf`, `--ff`, `-q`, `--collect-only`
- Markers: built-in (`skip`, `skipif`, `xfail`) and custom, registered in
  `pytest.ini`
- `pytest.raises`, `pytest.approx`
- `pytest.ini` / `pyproject.toml` config, `addopts`, `testpaths`
- Project layout: `tests/`, `conftest.py`, `src/`

**Practice**
Write 20 tests against a public API (`reqres.in`, `jsonplaceholder`,
`restful-booker`). Use markers to split `@pytest.mark.smoke` and
`@pytest.mark.regression`. Run each subset separately.

**Compare out loud:** "pytest vs unittest — why pytest?" (plain asserts, fixture
model, parameterisation, plugin ecosystem, less boilerplate). This is a
guaranteed question.

---

## Day 9 (Tue) — Fixtures, deeply

Fixtures are *the* pytest interview topic. Know them cold.

**Learn**
- Fixture scopes: `function`, `class`, `module`, `package`, `session` — and the
  cost/isolation trade-off
- `yield` fixtures for setup/teardown
- `autouse=True` — and why to use it sparingly
- Fixture composition (fixtures requesting fixtures)
- `conftest.py` hierarchy and resolution order
- Parameterised fixtures: `@pytest.fixture(params=[...])`
- Built-ins: `tmp_path`, `monkeypatch`, `capsys`, `caplog`, `request`
- Factory-as-fixture pattern (a fixture that returns a function)

**Practice**
1. `session` fixture: authenticated `requests.Session` with a token
2. `function` fixture: creates a test user, deletes it on teardown
3. Factory fixture: `make_booking(**overrides)` returning a created resource
4. `monkeypatch` a config value and assert the framework reads it
5. Draw the teardown order for nested fixtures — then verify by running with `-s`

**Interview line to memorise:** *"Scope is a trade between speed and isolation. I
default to function scope and only widen it when setup is expensive and provably
stateless — an auth token, say, not a database record."*

---

## Day 10 (Wed) — Parameterisation, plugins, reporting

**Learn**
- `@pytest.mark.parametrize` — single param, multiple params, stacked decorators
  (cartesian product), `ids=`, `pytest.param(..., marks=pytest.mark.xfail)`
- Indirect parameterisation
- Data-driven from JSON/CSV/YAML → this is "data-driven testing" in interviews
- Hooks: `pytest_addoption`, `pytest_configure`, `pytest_collection_modifyitems`,
  `pytest_runtest_makereport`
- Custom CLI options: `--env=qa|staging|prod`
- Plugins: `pytest-xdist` (parallel), `pytest-html`, `allure-pytest`,
  `pytest-rerunfailures`, `pytest-cov`, `pytest-timeout`, `pytest-mock`

**Practice**
1. Parameterise a login test from an external JSON file
2. Add `--env` via `pytest_addoption`, read it in a fixture, load the right
   base URL
3. Run with `-n 4` and fix whatever breaks — **parallel-safety issues are a
   favourite interview probe**
4. Generate an Allure report locally and screenshot it for your README

**Key answer to prepare:** *"How do you make tests parallel-safe?"* → no shared
mutable state, unique test data per worker, no fixed IDs, function-scoped
fixtures, idempotent cleanup.

---

## Day 11 (Thu) — API testing with `requests`

**Learn**
- `requests`: methods, params, headers, `json=` vs `data=`, timeouts
- `Session` objects, connection pooling, retry adapters (`HTTPAdapter`,
  `urllib3.Retry`)
- Auth: Basic, Bearer/JWT, OAuth2 flow, API keys
- Status codes, response validation, JSON schema validation (`jsonschema`)
- `pydantic` models for response contracts — a strong differentiator
- Mocking: `responses` / `requests-mock` / `pytest-mock`
- File upload/download, multipart

**Build**
A `clients/` layer — do **not** call `requests` directly from tests:

```python
class BaseClient:
    def __init__(self, base_url, session): ...
    def _request(self, method, path, **kw):  # logging, timing, error wrapping
        ...

class BookingClient(BaseClient):
    def create(self, payload) -> BookingResponse: ...
    def get(self, booking_id) -> BookingResponse: ...
    def delete(self, booking_id) -> None: ...
```

**This layered separation — tests → client → transport — is the thing that makes
an interviewer decide you can design.**

---

## Day 12 (Fri) — Framework assembly

Wire everything into Repo #1. Target structure:

```
api-test-framework/
├── src/framework/
│   ├── clients/          # BaseClient + per-service clients
│   ├── models/           # pydantic request/response models
│   ├── config/           # env configs, settings loader
│   ├── utils/            # logger, retry, data builders, assertions
│   └── exceptions.py
├── tests/
│   ├── conftest.py
│   ├── smoke/
│   ├── regression/
│   └── data/
├── .github/workflows/tests.yml
├── pyproject.toml
├── pytest.ini
├── Dockerfile
└── README.md
```

**Must-haves**
- Env switching via `--env`
- Structured logging with per-test correlation
- Custom assertion helpers with useful failure messages
- Allure or pytest-html reports published as CI artifacts
- ≥25 real tests: happy path, negative, boundary, auth failures, schema checks
- README with architecture diagram, run instructions, and a CI badge

**Job track:** By today you should have had at least one screening call.
Write down every question you were asked — that list is your real syllabus.

---

## Day 13 (Sat) — Git, CI/CD, Docker

**Learn (3 hr)**
- Git beyond basics: branching, rebase vs merge, interactive rebase, cherry-pick,
  stash, resolving conflicts, PR workflow, `.gitignore`
- GitHub Actions: workflow syntax, triggers, jobs, matrix builds, secrets,
  artifacts, scheduled runs
- Docker: `Dockerfile`, layers, `docker build/run`, volumes, `docker-compose`
- Why containerised test runs matter: reproducibility, parallel isolation,
  identical local/CI environments

**Build (3 hr)**
1. GitHub Actions workflow: run on push + PR + nightly cron; matrix over
   Python 3.10/3.11/3.12; upload the HTML report as an artifact
2. Dockerfile for the framework; `docker run` executes the suite
3. `docker-compose.yml` with the app-under-test + test runner
4. Add the CI badge to your README

**This single day of work puts you ahead of most candidates**, who have
frameworks that only run on their laptop.

---

## Day 14 (Sun) — Mocking, review, mock interview

**Morning (2 hr): mocking and test doubles**
- `unittest.mock`: `Mock`, `MagicMock`, `patch`, `patch.object`, `side_effect`,
  `return_value`, `assert_called_with`
- Where to patch (patch where it is *used*, not where it is defined) — a
  classic gotcha question
- Stub vs mock vs fake vs spy — know the definitions
- WireMock / MockServer concept for service virtualisation

**Afternoon (2 hr): coding drill.** 12 problems on dicts, strings, and a small
class-design question, timed.

**Evening (2 hr): mock interview.** Get a friend, or record yourself:
1. Design an API test framework from scratch on a whiteboard (20 min)
2. Live code: `@retry` decorator with exponential backoff (15 min)
3. pytest fixture-scope rapid fire (10 min)
4. "Tell me about a hard bug you found." (5 min)

**Week 2 gate:**
- [ ] Repo #1 public, CI green, ≥25 tests
- [ ] Can explain fixture scopes and parallel-safety unprompted
- [ ] 3+ screening calls attended
- [ ] Question log started from real interviews
