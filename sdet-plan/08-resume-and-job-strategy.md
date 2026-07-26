# Resume, Positioning, and Job Search Strategy

The plan fails if you study for 30 days and apply on Day 31. Applications start
Day 1. This file is the parallel track.

---

## 1. Positioning — the single most important decision

**Wrong:** "QA Engineer with 4 years of experience looking to move into SDET."
That reads as a career changer and prices you as a junior SDET.

**Right:** "SDET with 4 years across functional, performance, and security
testing for web, mobile, and IoT platforms."

You are not becoming an SDET. You have been doing SDET work — performance
harnesses for an IoT fleet, security testing, automation across two platforms.
The Python is you deepening a skill you already use, not starting over. Every
word on your resume should reflect that.

**Your three-line pitch, memorised:**
> "I'm a quality engineer with four years across web, mobile, and AWS IoT
> platforms. My depth is in the non-functional side — I've built performance test
> harnesses for device fleets at scale and done hands-on security testing. Over
> the last stretch I've gone deep on Python to own the tooling layer: pytest
> frameworks, CI pipelines, and load harnesses, which is what I'm looking to do
> full-time as an SDET."

Time it. Should be 30 seconds.

---

## 2. Resume rewrite (Day 1–2)

### Title
`SDET | Python Automation | Performance & Security Testing`

### Structure, in order
1. **Header** — name, phone, email, LinkedIn, **GitHub** (critical for SDET)
2. **Summary** — 3 lines, the pitch above, compressed
3. **Technical skills** — grouped, not a wall of words
4. **Experience** — reverse chronological, metrics on every bullet
5. **Projects** — your two flagship repos, with links
6. **Education / certifications**

### Skills section format

```
Languages       Python (primary), JavaScript, SQL, Bash
Test Automation pytest, Selenium, Playwright, Appium, requests, unittest.mock
API & Contract  REST, Postman, pydantic/JSON Schema validation
Performance     Locust, JMeter, MQTT/AWS IoT load simulation, percentile analysis
Security        OWASP Top 10, Burp Suite, OWASP ZAP, bandit, pip-audit, DAST in CI
CI/CD & Infra   GitHub Actions, Jenkins, Docker, Git
Cloud           AWS (IoT Core, EC2, S3, CloudWatch)
```

### Bullet formula

**Action verb + what you built + technology + measurable result.**

| Before | After |
|---|---|
| "Responsible for testing web applications" | "Built and maintained a Python/pytest regression suite of 200+ API and UI tests, cutting release validation from 3 days to 4 hours" |
| "Did performance testing" | "Designed an MQTT load harness simulating 10,000 concurrent IoT devices; identified a broker connection bottleneck that cut p99 latency from 4.2s to 380ms" |
| "Worked on security testing" | "Integrated OWASP ZAP baseline scans and dependency auditing into CI, surfacing 12 medium-severity issues before release" |
| "Automated test cases" | "Reduced manual regression effort ~60% by automating 150 high-risk scenarios with Selenium and pytest" |

**If you don't have exact numbers, estimate honestly and say "approximately."**
An honest estimate beats a vague bullet. Do not invent specifics you can't defend
— you will be asked to walk through them.

### Rules
- Two pages maximum. One is fine at 4 years.
- No photos, no skill-percentage bars, no "references available on request."
- Plain, ATS-parseable formatting — single column, standard headings, no tables
  in the experience section.
- Mirror the job description's keywords where they're genuinely true of you.
- Tailor the summary line per application. Three minutes each; it measurably
  improves callback rate.

---

## 3. Target companies

Apply in all four tiers simultaneously. Weight your effort toward tiers 2 and 3.

| Tier | Examples | Why | Your odds |
|---|---|---|---|
| **1 — Product / global** | Amazon, Microsoft, Google, Uber, Salesforce, Atlassian, Zoho, Freshworks | Best pay, real SDET roles | Harder — needs DSA depth |
| **2 — Indian product & scale-ups** ⭐ | Razorpay, Zerodha, Swiggy, PhonePe, CRED, Postman, Chargebee, Hasura, Zeta | Genuine SDET roles, value breadth, faster loops | **Strong fit** |
| **3 — Services with strong QE** ⭐ | Thoughtworks, Publicis Sapient, EPAM, Nagarro, Infosys/TCS digital units | Volume, quick offers, decent SDET titles | **Strong fit, fastest offers** |
| **4 — IoT / hardware / embedded** ⭐⭐ | IoT platform companies, industrial automation, connected-device startups, EV and energy | **Your AWS IoT experience is directly relevant and rare** | **Highest leverage — prioritise** |

**Do not underweight Tier 4.** A company hiring for IoT platform QE will value
your MQTT/device-fleet performance experience far above a candidate with better
LeetCode scores. That is where your background stops competing and starts
winning.

---

## 4. Application system

### Daily quota (from Day 2)
- **5 tailored applications** — 15 min each, not spray-and-pray
- **3 LinkedIn recruiter messages**
- **1 warm outreach** to someone at a target company
- Total: ~1 hour/day → **~120 applications over the month**

### Channels, in order of yield
1. **LinkedIn** — set alerts for `SDET`, `Software Development Engineer in Test`,
   `Automation Engineer Python`, `QA Engineer Python`. Turn on "Open to work"
   (recruiters-only visibility).
2. **Referrals** — 5–10× the callback rate of cold applications. Message anyone
   you know at a target company. Also message strangers: a polite, specific note
   to an SDET at the company works more often than people expect.
3. **Company careers pages** directly — bypasses aggregator noise
4. **Naukri / Instahyre / Cutshort / Wellfound** — keep the profile fresh, and
   note that Naukri surfaces you by *recency of update*, so update it weekly
5. **Recruiters/consultancies** — good for Tier 3 volume

### Referral message template
```
Hi [Name] — I saw you're an SDET at [Company]. I'm a QE with 4 years across
web, mobile, and AWS IoT, focused on performance and security testing, and I've
been building Python test frameworks (repos here: [link]).

I'm applying for [role] and would really value a referral if my background looks
like a fit. Happy to send my resume either way — and if you have 10 minutes
sometime, I'd love to hear how your team approaches test tooling.
```

Short, specific, gives them an easy no. Don't send a wall of text.

### Track everything

| Company | Role | Applied | Source | Status | Round | Next action | Notes |
|---|---|---|---|---|---|---|---|

Follow up 7 days after applying. Follow up 3 days after each interview. Most
candidates lose opportunities to silence, not rejection.

---

## 5. LinkedIn optimisation (Day 1, 30 minutes)

- **Headline:** `SDET | Python · pytest · Playwright | Performance & Security Testing | AWS IoT`
- **About:** the 3-line pitch, expanded to 5–6 lines, first person
- **Experience:** mirror the resume bullets with metrics
- **Featured:** pin your two flagship repos and your technical article
- **Skills:** Python, pytest, Selenium, Playwright, Appium, API Testing, Locust,
  Performance Testing, Security Testing, AWS IoT, CI/CD, Docker — get
  endorsements on the top three
- **Post once a week** about what you're building. Recruiters search by activity,
  and it gives interviewers a reason to ask about your strengths.

---

## 6. Interview conversion notes

**Screening call (HR):** know your notice period, current and expected CTC, and
reason for leaving. Have the 30-second pitch ready. Be warm — HR screens for
communication as much as content.

**Salary question:** if pressed early, give a range anchored on the **SDET**
market band for your experience and city, not your current QA salary. "Based on
the SDET market for my experience and my performance/security specialisation,
I'm looking at X–Y, but I'm most interested in the scope of the role."

**Take-home assignments:** worth doing if scoped under ~4 hours. Submit with a
README, tests for your own code, and a "design decisions" section. That README is
often what gets you the next round.

**After every interview**, within 30 minutes, write down every question you were
asked into the log in [`07-interview-question-bank.md`](07-interview-question-bank.md).
This is the feedback loop that makes interview #8 much stronger than interview #1.

---

## 7. Honest expectations

- **Weeks 1–2:** applications go out, few responses. Normal. Don't read anything
  into it.
- **Week 3:** first technical rounds. You will fail some. Each one is data.
- **Week 4+:** conversion improves sharply because your answers are now rehearsed
  against real questions.
- **Realistic offer timeline:** 4–8 weeks from Day 1, not 30 days. The 30-day
  plan makes you *interview-ready*; the market sets the calendar. Expect 40–60
  applications per serious offer at this level.

None of that is a reason to slow down. It's a reason to start applying on Day 1
instead of Day 31.
