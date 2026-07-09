# Python Engineer (Agentic AI / Cyber Red Team) — Interview Kit

> **Role:** Python Engineer on an internal Cyber Red Team pioneering Agentic AI to simulate real-world adversaries.
> **Hypothesis we're testing:** Can this candidate build production-grade Python, integrate LLM APIs into multi-step workflows, and ramp into offensive-security domain knowledge quickly enough to translate attacker logic into reliable automation?

---

## 1. Role Snapshot

| | |
|---|---|
| **Seniority** | Mid-level (1–2 yrs Python experience, can stretch) |
| **Core craft** | Python engineering + LLM/agent integration |
| **Domain** | Offensive security (training provided — security background NOT required) |
| **Stack** | Python 3.11+, Docker, REST APIs, OpenAI/Anthropic, LangChain/LangGraph/MCP |
| **Must-haves** | OOP, decorators, context managers, async/threading, JSON, LLM APIs, Docker |
| **Nice-to-haves** | MCP servers, red-team/offsec familiarity, observability tooling |

**What "good" looks like in 12 months:**
- Owns an end-to-end agentic workflow (ingest → tool → AI → decision → next step) for a real attack technique
- Wrapper APIs around offensive tools are reliable enough that the rest of the team treats them as primitives
- Has shipped at least one MCP server or LangGraph orchestration to production
- Onboarded into enough offensive-security concepts to participate in technique-brainstorming sessions

---

## 2. Competencies (5)

| # | Competency | Why it matters | Bar signal |
|---|---|---|---|
| 1 | **Python Engineering & Software Craft** | The craft foundation — everything else is built on it | Writes production-quality Python: OOP, async, decorators, context managers, clean module boundaries |
| 2 | **LLM & Agentic Systems** | The differentiating skill — most candidates won't have this | Designs prompt + tool + state loops; handles structured output; reasons about reliability of stochastic systems |
| 3 | **API, Backend & Systems Design** | Real systems have real backends — REST, JSON, Docker, wrappers | Designs clean API surfaces; reasons about state, retries, packaging |
| 4 | **Reliability & Production Engineering** | Agentic workflows fail in novel ways; ops maturity is the ceiling | Thinks in terms of retries, idempotency, observability, edge cases, resumability |
| 5 | **Collaboration & Domain Curiosity** | They're joining security experts as a partner, not a coder-for-hire | Ramps into unfamiliar domains; translates domain logic into specs; communicates with non-Python-fluent experts |

---

## 3. Panel Structure (4 rounds, ~3.5 hours total)

| Round | Duration | Format | Owner | Primary competency | Secondary |
|---|---|---|---|---|---|
| **R1 — Python Deep Dive** | 60 min | Live coding + walkthrough | Senior Python engineer on team | 1. Python Engineering | 4. Reliability |
| **R2 — LLM & Agentic Systems** | 60 min | System design + targeted Q&A | AI/agent lead | 2. LLM & Agentic | 4. Reliability |
| **R3 — APIs, Backend & Systems** | 45 min | Design exercise + Q&A | Platform/backend engineer | 3. API/Backend | 4. Reliability |
| **R4 — Collaboration & Domain Fit** | 45 min | Behavioral + situational | Red Team lead (hiring manager) | 5. Collaboration | 1, 2 (light) |

**Why this split:** three technical interviews lets each owner go deep without grilling the candidate four separate times on Python. The hiring manager owns the "would I want to work with this person on a real engagement?" signal in R4.

**Bias-reduction rules:**
- Each interviewer scores their primary competency independently **before** the debrief.
- Scorecards are submitted without discussion until the debrief.
- No interviewer sees the live-coding result of another interviewer.
- R4 (hiring manager) does **not** see R1/R2/R3 scores until they submit their own.

---

## 4. Process Flow

1. **Recruiter screen (30 min)** — basic fit, salary, timing, role description
2. **R1 — Python Deep Dive** — live coding
3. **R2 — LLM & Agentic Systems** — system design
4. **R3 — APIs & Backend** — design exercise
5. **R4 — Collaboration** — behavioral (hiring manager)
6. **Debrief (60 min)** — all four interviewers + recruiter
7. **Decision** — Strong Hire / Hire / No Hire / Strong No Hire

**Debias pre-mortem (before debrief):** Each interviewer writes down "what would make me wrong about this candidate?" before the meeting starts. This is read aloud first.

---

## 5. Files in this Kit

- `01-question-bank.md` — Full question bank by competency (behavioral + situational + probes)
- `02-live-exercises.md` — Live coding and design exercise prompts
- `03-scorecard.md` — Scoring rubric with behavioral anchors
- `04-debrief-template.md` — Debrief structure and decision framework

See those files for the actual interview content.
