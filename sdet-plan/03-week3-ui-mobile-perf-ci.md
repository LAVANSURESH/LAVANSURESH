# Week 3 — UI, Mobile, Performance, Security, CI (Days 15–21)

**Objective:** Ship **Repo #2** (hybrid UI framework) and convert your AWS IoT /
security background into demonstrable Python artifacts.

**Parallel track:** Technical rounds are happening now. Aim for 4+ interviews
this week.

> This is the week where your existing 4 years stop being a paragraph on a
> resume and start being code someone can click on.

---

## Day 15 (Mon) — Selenium with Python

You know Selenium already. This day is about doing it *in Python, properly*.

**Learn**
- WebDriver setup with `webdriver-manager` / Selenium Manager
- Locator strategies; **why XPath-by-text is fragile and CSS is preferred**
- Explicit waits (`WebDriverWait` + `expected_conditions`) vs implicit vs
  `time.sleep` — never mix implicit and explicit, and know why
- Custom expected conditions
- `ActionChains`, JS executor, iframes, windows, alerts
- Headless mode, browser options, remote WebDriver / Selenium Grid

**Practice**
Port a suite you know well to Python. Focus on wait strategy, not coverage.

**Interview answer to lock in — "how do you handle flaky tests?"**
> Root-cause first, never blanket retries. Order of attack: (1) replace implicit
> waits with explicit conditions tied to app state, (2) remove test
> interdependence and shared data, (3) stabilise locators away from generated
> IDs, (4) check for animation/network races, (5) only then quarantine with a
> tracked ticket. Retries hide bugs; I use them as a signal, not a fix.

---

## Day 16 (Tue) — Playwright (the differentiator)

Playwright + Python is what modern teams are hiring for. A day is enough to be
conversant, and being conversant beats most candidates.

**Learn**
- `sync_playwright` vs async API
- Auto-waiting and web-first assertions — **why this eliminates most flakiness**
- Locators: `get_by_role`, `get_by_text`, `get_by_test_id`, chaining, filtering
- `pytest-playwright`: `page` fixture, `--headed`, `--browser`, `--slowmo`
- Browser context isolation, `storage_state` for auth reuse
- Network interception: `page.route()` — mock APIs from the UI test
- Tracing, video, screenshots on failure
- Codegen (`playwright codegen`) as a scaffolding tool, not a crutch

**Practice**
1. Rewrite 5 Selenium tests in Playwright; compare LOC and runtime
2. Save auth state once, reuse across the suite (huge speed win — good story)
3. Intercept an API call and force an error response to test the UI error state

**Talking point:** *"Selenium is W3C-standard and ubiquitous; Playwright's
auto-waiting and context isolation cut our flake rate and runtime substantially.
I'd pick Playwright for greenfield, Selenium where there's an existing Grid
investment or unusual browser requirements."*

---

## Day 17 (Wed) — Page Object Model + framework design

**Learn**
- POM done right: no assertions inside page objects, methods return page objects
- Page Factory / fluent interfaces
- Screenplay pattern (know it exists; it's a good senior-level talking point)
- Component objects for reusable widgets (nav bar, date picker, modal)
- Locator externalisation (YAML/JSON) — pros and cons; be able to argue both
- Test data strategy: builders, factories, fixtures, cleanup

**Build Repo #2 skeleton**

```
ui-test-framework/
├── src/framework/
│   ├── pages/          # BasePage + page objects
│   ├── components/     # reusable widgets
│   ├── drivers/        # factory: local/remote/headless, Selenium+Playwright
│   ├── config/
│   └── utils/          # waits, screenshots, logger, data builders
├── tests/
│   ├── conftest.py
│   ├── e2e/
│   └── visual/
├── .github/workflows/
└── README.md
```

**Design decisions to be ready to defend:**
- Why POM and not raw scripts
- Why the driver is injected, not global
- How you'd swap Selenium → Playwright without rewriting tests (driver
  abstraction / adapter layer)
- Where assertions live, and why

---

## Day 18 (Thu) — Mobile automation with Appium + Python

You already have mobile experience. Convert it to Python and make it visible.

**Learn**
- Appium architecture: client → server → driver → device
- Desired capabilities / W3C options; UiAutomator2 (Android), XCUITest (iOS)
- Appium Python client, locators: accessibility id, UiAutomator selector,
  XPath (last resort)
- Gestures: swipe, scroll, long press, `mobile:` execute scripts
- Contexts: native vs webview (hybrid apps)
- Real device vs emulator; cloud grids (BrowserStack/Sauce/LambdaTest)
- Common problems: app state between tests, permissions dialogs, waits

**Practice**
Automate 5 flows on a sample APK (open-source demo apps work fine). Reuse the
same `BasePage` philosophy so your mobile and web layers look like one framework.

**Interview gold:** *"Mobile-specific challenges I've handled — device
fragmentation, network conditioning, app state isolation between tests, and
permission dialogs that differ per OS version."* Have one concrete war story
from your 4 years ready.

---

## Day 19 (Fri) — Performance testing in Python (your edge)

You have AWS IoT performance experience. In Python this becomes a portfolio
piece almost no other candidate has.

**Learn**
- **Locust**: `HttpUser`, `@task`, weights, `wait_time`, custom clients, the web
  UI, headless mode, distributed master/worker
- Custom Locust user for **MQTT** (via `paho-mqtt`) — this is *directly* your
  AWS IoT experience, expressed in Python
- Metrics that matter: p50/p90/p95/p99 vs average, throughput, error rate,
  saturation — **never quote averages in an interview**
- Load profiles: smoke, load, stress, spike, soak/endurance, breakpoint
- Bottleneck analysis: app vs DB vs network vs client-side
- `cProfile`, `timeit`, `memory_profiler` for profiling Python itself

**Build**
A `locustfile.py` that load-tests your API framework's target, plus a short
`PERFORMANCE.md` in the repo: test design, workload model, results table with
percentiles, and one identified bottleneck with a hypothesis.

**Story to prepare (STAR format):** your AWS IoT performance work — how many
devices simulated, what protocol, what you found, what changed as a result, with
numbers. This is likely the strongest single story you own. Rehearse it until
it's 90 seconds and quantified.

---

## Day 20 (Sat) — Security testing in the pipeline + CI polish

**Morning (3 hr): security-as-code**
- OWASP Top 10 — be able to name all ten and give a test for three of them
- **ZAP automation**: baseline scan in CI via `zap-baseline.py` or the ZAP
  GitHub Action; ZAP Python API for scripted scans
- SAST in CI: `bandit` (Python), `semgrep`
- Dependency scanning: `pip-audit`, `safety`, Dependabot
- Secret scanning: `gitleaks`
- Auth/authz test cases: IDOR, privilege escalation, JWT tampering, rate limiting

**Build:** add a `security.yml` GitHub Actions workflow to Repo #1 that runs
ZAP baseline + bandit + pip-audit and uploads the report. **A QE candidate with
DAST wired into CI is a genuinely uncommon profile — make this visible in the
README.**

**Afternoon (3 hr): CI/CD maturity**
- Jenkins: declarative `Jenkinsfile`, stages, parallel, post actions, agents
- Test selection strategy: smoke on PR, regression nightly, full on release
- Quality gates: coverage thresholds, failure budgets
- Reporting: Allure history, trend analysis, flaky-test dashboards
- Notifications: Slack/Teams on failure

Write a `Jenkinsfile` too, even if you use Actions — many Indian enterprises
still run Jenkins and will ask.

---

## Day 21 (Sun) — Integration + mock interview

**Morning (3 hr):** finish Repo #2. README must include: architecture diagram,
design rationale, how to run locally and in Docker, sample report screenshots,
CI badge.

**Afternoon (2 hr): coding drill.** 10 problems + one "design a class" question.

**Evening (2 hr): full mock interview**, recorded:
1. Framework design on a whiteboard (25 min)
2. Live coding: parse a log file and report the top 5 error types (20 min)
3. Scenario: "100 UI tests, 20 fail randomly each night. What do you do?" (10 min)
4. Your AWS IoT performance story (5 min)

**Week 3 gate:**
- [ ] Repo #2 public with CI green
- [ ] Locust + MQTT script committed with a results write-up
- [ ] ZAP/bandit/pip-audit workflow running in CI
- [ ] 4+ interviews attended, question log updated
