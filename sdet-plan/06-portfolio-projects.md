# Portfolio Projects — The Repos That Get You Shortlisted

Two flagship repos plus two small ones. Quality over count: **one excellent repo
beats five half-finished ones.**

The test for every repo: *could a hiring manager clone it, run one command, and
see it work?* If not, it isn't done.

---

## Repo #1 — `api-test-framework` (Week 2) ⭐ flagship

**Pitch:** A production-shaped API automation framework in Python with layered
architecture, multi-environment config, CI, and security scanning.

### Structure
```
api-test-framework/
├── src/framework/
│   ├── clients/
│   │   ├── base_client.py        # request wrapper, logging, timing, retries
│   │   └── booking_client.py     # per-service client
│   ├── models/                   # pydantic request/response models
│   ├── config/
│   │   ├── settings.py           # env loader
│   │   └── environments.yaml
│   ├── utils/
│   │   ├── logger.py
│   │   ├── assertions.py         # custom asserts with rich failure messages
│   │   ├── retry.py              # your @retry decorator
│   │   └── data_builder.py       # builder pattern for payloads
│   └── exceptions.py
├── tests/
│   ├── conftest.py               # fixtures: session, auth, test data
│   ├── smoke/  regression/  contract/
│   └── data/                     # parameterisation JSON
├── .github/workflows/
│   ├── tests.yml                 # matrix, artifacts, nightly cron
│   └── security.yml              # ZAP + bandit + pip-audit
├── Dockerfile · docker-compose.yml · pyproject.toml · pytest.ini
└── README.md
```

### Must demonstrate
- [ ] Layered design: tests never touch `requests` directly
- [ ] Fixtures at three different scopes with a justification comment
- [ ] Parameterised data-driven tests loaded from JSON
- [ ] Custom pytest CLI option `--env`
- [ ] Response schema validation with pydantic
- [ ] Retry with exponential backoff on transient failures only
- [ ] Structured logging with request/response correlation
- [ ] Allure or HTML reports uploaded as CI artifacts
- [ ] Parallel execution via `pytest-xdist`, proven parallel-safe
- [ ] ≥25 tests: happy path, negative, boundary, auth, schema
- [ ] Security workflow (ZAP baseline + bandit + pip-audit)

**Target APIs:** `restful-booker`, `reqres.in`, `petstore.swagger.io`, or a
FastAPI app you write yourself (writing the app under test is a bonus signal —
it proves you can write production code, not just test code).

---

## Repo #2 — `ui-mobile-test-framework` (Week 3) ⭐ flagship

**Pitch:** A hybrid UI framework supporting both Selenium and Playwright behind
one interface, plus an Appium mobile layer, running in Docker and CI.

### Must demonstrate
- [ ] `BasePage` + Page Objects with no assertions inside pages
- [ ] Component objects for reusable widgets
- [ ] **Driver abstraction layer** — swap Selenium ↔ Playwright without touching
      tests (this is the standout design feature; make it prominent in the README)
- [ ] Explicit-wait strategy with custom expected conditions
- [ ] Screenshot + video + trace on failure, attached to the report
- [ ] Cross-browser matrix in CI (Chromium, Firefox, WebKit)
- [ ] Auth state reuse for speed
- [ ] Network interception to test UI error states
- [ ] Appium layer sharing the same page-object philosophy
- [ ] Docker + docker-compose (Selenium Grid or Playwright image)
- [ ] Flake report: run the suite 10× and publish stability stats — **almost no
      candidate does this, and it signals real maturity**

---

## Repo #3 — `iot-load-harness` (Week 3) ⭐ your differentiator

**Pitch:** MQTT/IoT load testing with Locust — simulating a device fleet,
measuring latency percentiles under load.

This is the repo that makes you memorable. Nobody else applying has it.

### Contents
- [ ] Custom Locust user speaking MQTT via `paho-mqtt`
- [ ] Device simulator: N virtual devices publishing telemetry at a set rate
- [ ] Configurable load profiles: ramp, spike, soak
- [ ] Latency percentile capture (p50/p90/p95/p99), not averages
- [ ] Distributed master/worker mode documented
- [ ] `PERFORMANCE.md`: workload model, results table, one identified bottleneck
      with a hypothesis and a proposed fix
- [ ] Optional: `docker-compose` with Mosquitto as a local broker so it actually
      runs for anyone who clones it

**Sanitise everything.** No employer names, no real endpoints, no configuration
from your current job. Rebuild the *pattern*, never the artifact.

---

## Repo #4 — `testdata-gen` (Week 1, small)

CLI tool generating test data as JSON/CSV/SQL. Proves packaging, `argparse`,
clean module layout, and unit tests **of your own tool** (tests for the test
tool — interviewers like this).

---

## README template

Every repo uses this. The README is the interview before the interview.

```markdown
# Project Name
> One-line pitch.

[![Tests](badge)](link) [![Python 3.12](badge)](link)

## What this is
2–3 sentences: the problem, the approach.

## Architecture
[diagram — draw.io / Mermaid / ASCII]
Explain the layers and why they're separated.

## Quickstart
```bash
git clone ... && cd ...
pip install -e ".[dev]"
pytest -m smoke --env=qa
```

## Key design decisions
| Decision | Why | Trade-off accepted |
|---|---|---|
| Layered client abstraction | Tests stay readable when the API changes | More initial code |
| Function-scoped fixtures by default | Isolation and parallel safety | Slower setup |

## Reports
[screenshot of Allure/HTML report]

## CI
What runs, when, and what gates a merge.

## What I'd do next
Honest list of improvements. Shows judgement, not a finished-and-abandoned repo.
```

---

## GitHub profile hygiene

- [ ] Pin the four repos in order: API framework, UI framework, IoT harness, testdata-gen
- [ ] Add project links to your existing profile README (it's already strong —
      add a "Projects" section above the stats)
- [ ] Meaningful commit messages; no `update`, no `fix` ×40
- [ ] Commit daily for 30 days — the contribution graph is read as evidence of
      consistency
- [ ] No secrets, ever — run `gitleaks` before pushing anything
