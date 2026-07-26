# SDET in 30 Days — Master Plan

**Owner:** Lavan S
**Starting point:** 4 years QE — manual, automation, AWS IoT performance testing, security testing, web + mobile
**Goal:** Land an SDET (Software Development Engineer in Test) role
**Window:** 30 days of learning **while interviewing in parallel** (not after)

---

## The one thing to internalise first

You are not a beginner making a career change. You are a QE with 4 years of
production experience who is **missing one thing: the ability to prove you can
write and defend production-grade code.**

That distinction changes the whole plan. You do not need 6 months of
fundamentals. You need roughly 4 weeks of concentrated Python + framework
design, backed by two portfolio repos and a rehearsed story. Everything in this
plan serves that single objective.

Your rarest asset is **not** Selenium. It is the combination of
**AWS IoT / MQTT performance testing + security testing + mobile**. Almost no
SDET candidate has that. Most of them have "Selenium + TestNG + Java." Lead with
the rare thing; the Python is what makes you hireable for it.

---

## The 30-day shape

| Week | Theme | Outcome by Sunday night |
|---|---|---|
| **1** | Python core, deeply | You can solve string/list/dict problems live and explain OOP without notes |
| **2** | pytest + API automation framework | Repo #1 public: an API test framework with fixtures, config, reporting, CI |
| **3** | UI + mobile + performance + CI/CD | Repo #2 public: hybrid UI framework, Page Objects, Docker, GitHub Actions |
| **4** | System design for test, mocks, closing | 3+ rehearsed rounds done, offers in motion |

**Interviews start Day 8, not Day 30.** Applying is a Week-1 task. Early
interviews are free diagnostics — every one tells you exactly what to fix next.

---

## Daily time budget

| | Weekday | Weekend |
|---|---|---|
| Python / coding practice | 1.5 hr | 3 hr |
| Framework building | 1.5 hr | 3 hr |
| Interview prep / applications | 1 hr | 1 hr |
| **Total** | **~4 hr** | **~7 hr** |

≈ **110 hours over 30 days.** That is enough. It is not enough if half of it is
watching tutorials. Rule: **for every 1 hour of video, 2 hours of typing code.**

---

## The files

| File | What it is |
|---|---|
| [`01-week1-python-core.md`](01-week1-python-core.md) | Day-by-day Python foundation |
| [`02-week2-pytest-api.md`](02-week2-pytest-api.md) | pytest mastery + API framework build |
| [`03-week3-ui-mobile-perf-ci.md`](03-week3-ui-mobile-perf-ci.md) | Playwright/Selenium, Appium, Locust, CI/CD, Docker |
| [`04-week4-interviews.md`](04-week4-interviews.md) | System design for test, mocks, negotiation |
| [`05-python-syllabus.md`](05-python-syllabus.md) | The complete Python checklist, SDET-scoped |
| [`06-portfolio-projects.md`](06-portfolio-projects.md) | The 2 repos that get you shortlisted |
| [`07-interview-question-bank.md`](07-interview-question-bank.md) | Real questions + how to answer them |
| [`08-resume-and-job-strategy.md`](08-resume-and-job-strategy.md) | Resume rewrite, targeting, application system |
| [`09-daily-tracker.md`](09-daily-tracker.md) | Tick boxes. Print it. |

---

## Non-negotiable rules

1. **Type every line.** Never copy-paste code you are learning. Muscle memory is
   what survives interview pressure.
2. **Commit daily.** A green GitHub graph for 30 days is itself a hiring signal.
3. **No tutorial past Week 1.** After Day 7 you learn by building and by reading
   real library source, not by watching.
4. **One recruiter conversation per day minimum**, starting Day 3.
5. **Record yourself** answering 2 questions per week. Watching it back is
   uncomfortable and is the single highest-ROI hour in this plan.
6. **Do not wait to feel ready.** You will not feel ready on Day 30 either. The
   interviews are part of the training, not the exam at the end.

---

## What "SDET" actually means to an interviewer

Different companies mean different things. Know which one you are talking to:

| Flavour | What they test | Where you already stand |
|---|---|---|
| **Automation-heavy SDET** (most Indian services + product cos) | Framework design, Selenium/Playwright, API, CI | Strong — needs Python + framework depth |
| **Dev-leaning SDET** (FAANG-adjacent, product startups) | DSA, code quality, build test tooling from scratch | Weakest area — Week 1 + 4 target this |
| **Platform / infra QE** | CI/CD, Docker, K8s, test environments, observability | Medium — Week 3 targets this |
| **Performance / reliability SDET** | Load modelling, profiling, distributed systems | **Your unfair advantage — AWS IoT at scale** |
| **Security-aware SDET / AppSec QE** | SAST/DAST in pipelines, threat modelling | **Second unfair advantage** |

Apply broadly, but prioritise the bottom two rows. That is where 4 years of your
history instantly outranks a stronger coder with a generic background.

---

## Success criteria for Day 30

- [ ] Two public GitHub repos with real README, CI badge, and ≥30 commits each
- [ ] Can write a Page Object class, a pytest fixture, and a decorator from a
      blank file with no reference
- [ ] Can solve a medium string/dict problem in ≤20 min while talking
- [ ] Resume reframed from "QA Engineer" to "SDET" with metrics on every bullet
- [ ] 40+ targeted applications sent, 8+ interviews attended
- [ ] At least one offer or final round in progress

Start with [`01-week1-python-core.md`](01-week1-python-core.md).
