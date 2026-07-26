# Week 4 — System Design for Test, Mocks, and Closing (Days 22–30)

**Objective:** Convert preparation into offers. Learning shifts from ~70% of
your time to ~30%. The rest is interviewing, refining, and negotiating.

---

## Day 22 (Mon) — Test architecture & system design for SDET

Senior SDET rounds ask design questions, not syntax questions.

**Learn the frameworks for answering**

*"Design a test strategy for a ride-hailing app"* — structure your answer:
1. **Clarify** — scope, platforms, users, release cadence, existing coverage
2. **Risk map** — what breaks the business? (payments, matching, location)
3. **Test pyramid** — unit/integration/E2E/contract split with real ratios
4. **Environments & data** — staging parity, data seeding, PII handling
5. **Automation strategy** — what to automate, what to leave manual, and why
6. **Non-functional** — performance, security, accessibility, resilience
7. **CI/CD integration** — gates, feedback time targets
8. **Observability** — production monitoring as a testing input
9. **Metrics** — escaped defects, MTTD, flake rate, coverage-of-risk

**Core topics to hold opinions on**
- Test pyramid vs testing trophy vs ice-cream cone anti-pattern
- Contract testing (Pact) for microservices — when it beats E2E
- Shift-left and shift-right (canary, feature flags, testing in production)
- Chaos engineering basics
- Test environment strategy: shared vs ephemeral, containerised environments
- Flaky test management as a program, not a task
- Risk-based testing and prioritisation

**Practice:** write out full answers to three prompts:
1. Test strategy for a payments microservice
2. How to test an IoT fleet of 100k devices *(use your real experience — this is
   your question to win)*
3. Test strategy for a mobile banking app releasing weekly

---

## Day 23 (Tue) — Coding interview intensive

**3 hr timed practice** across the categories that actually appear:

| Category | Examples |
|---|---|
| String manipulation | anagram, palindrome, reverse words, compression, first unique char |
| Arrays/lists | two sum, max subarray, merge intervals, rotate, dedupe |
| Dicts/sets | frequency counts, group anagrams, first duplicate |
| Matrix | spiral print, rotate 90°, transpose |
| Recursion | fibonacci + memoisation, permutations, flatten nested |
| OOP design | parking lot, ATM, elevator, library, **test framework** |
| File/log parsing | top N errors, aggregate by timestamp, dedupe entries |
| SQL | joins, group by, second-highest, duplicates |

**Method:** 20 min per problem — 5 min thinking out loud, 12 min coding, 3 min
walking through complexity and edge cases. **Always narrate.** Silence loses
offers more often than wrong answers do.

---

## Day 24 (Wed) — Behavioural + storytelling

Write and rehearse 8 STAR stories. These get reused across every interview:

1. Hardest bug you found (aim for a production-impact one)
2. Conflict with a developer over a defect
3. A time you improved the process (automation ROI, with numbers)
4. A time you failed / missed a bug that escaped to production
5. Working under an impossible deadline
6. Learning something new quickly (your Python transition *is* this story)
7. Disagreeing with a manager
8. Mentoring or upskilling someone

**Format each as 4 sentences: Situation, Task, Action, Result — with a number in
the Result.** Numbers are what get remembered.

**Also prepare, verbatim:**
- "Why do you want to move from QA to SDET?"
  > *"I've spent four years finding and preventing defects across web, mobile,
  > IoT, and security. What kept limiting me was the boundary between writing
  > tests and building the tooling that makes testing scale. I moved deep into
  > Python so I could own that layer — frameworks, CI pipelines, load harnesses
  > — rather than consume it."*
- "Why are you leaving?" — forward-looking, never bitter
- "Where do you see yourself in 3 years?" — SDET II / test architecture
- Your salary expectation, with a number you've already decided

---

## Day 25 (Thu) — Gap-filling day

Go through your interview question log from Weeks 2–3. Every question you
fumbled is now a study item. This is the highest-value day in the plan **because
it is driven by real data instead of a generic syllabus.**

Common gaps at this stage:
- Concurrency: threading vs multiprocessing vs asyncio, and the GIL
- `async`/`await` in Playwright and `httpx`
- Design patterns: Singleton, Factory, Builder, Strategy, Observer — with a
  test-framework example for each
- Memory management, garbage collection, `is` vs `==`
- Shallow vs deep copy
- Kubernetes basics if you're targeting platform-QE roles

---

## Day 26 (Fri) — Full mock loop

Simulate an entire onsite in one sitting, no breaks:
1. 45 min — coding (2 problems)
2. 45 min — framework/system design
3. 45 min — deep-dive on your resume and past projects
4. 30 min — behavioural
5. 15 min — your questions for them

Use a peer, a paid mock service, or record yourself. Grade against: clarity,
structure, correctness, and whether you asked clarifying questions before coding.

---

## Day 27–28 (Sat–Sun) — Portfolio polish + content

**Repos:** README quality is what recruiters actually see. Each needs:
architecture diagram, quickstart, sample report screenshots, design-decisions
section, CI badge, and a "what I'd do next" section (shows engineering
judgement).

**Write one technical article** (LinkedIn or dev.to). Suggested topics — each
plays to your real edge:
- "Load-testing an AWS IoT fleet with Locust and MQTT in Python"
- "Wiring OWASP ZAP into a pytest CI pipeline"
- "From manual QA to SDET: the Python that actually mattered"

One good article generates inbound recruiter interest and gives interviewers
something concrete to ask about — on ground you chose.

**Update:** resume with the two repos linked, LinkedIn featured section, GitHub
profile README (you already have a good one — add the projects).

---

## Day 29–30 (Mon–Tue) — Close

- Follow up on every pending application with a short, specific note
- Push for final rounds on anything stalled
- Prepare negotiation: know your target number, your walk-away number, and the
  market band for SDET at your experience level in your city
- Ask for a written offer; never accept on the call
- If competing offers exist, be transparent about timelines — it usually
  improves both

**Negotiation notes for a QE → SDET move**
- Do not anchor on your current QA salary. Anchor on the SDET market band.
- Your differentiators (IoT performance + security + mobile) justify the top of
  the band, not the middle. Say so, with the numbers from your stories.
- If they lowball on the "you're transitioning" argument, counter with the repos
  and the CI/security pipeline work — you are not transitioning on their time.

---

## After Day 30 — if you don't have an offer yet

That is a normal outcome, not a failed plan. Interview cycles routinely run 6–10
weeks. What changes:

- You now have two repos, real interview data, and rehearsed answers — the
  compounding has already happened
- Keep applying at 5/day; the pipeline, not the studying, is the bottleneck now
- Continue with: contract testing (Pact), Kubernetes, one cloud certification
  (AWS Developer Associate leverages your existing IoT work), and deeper DSA if
  you're targeting product companies
- Revisit your question log monthly and close the recurring gaps
