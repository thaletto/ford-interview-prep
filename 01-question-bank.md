# Question Bank

> All questions map to one of the 5 competencies (P1–P5). Each question lists type, intent, follow-up probes, and "what to listen for."
>
> **Types:** B = Behavioral (past experience) · S = Situational (hypothetical) · T = Technical probe
>
> **Scoring rule:** Take notes on **specific evidence** — projects, numbers, decisions, tradeoffs the candidate names. Vague answers are evidence of vague experience.

---

## Competency 1 — Python Engineering & Software Craft

> *Map to rounds: R1 (primary), R4 (light).*

### P1-Q1 · Type B · ~6 min
**Tell me about a Python project you're proud of — something with real users, not a tutorial. Walk me through the design decisions you made.**

*What to listen for:*
- Can they articulate *why* a design choice was made, not just *what* it is
- Mentions of module boundaries, separation of concerns, testability
- Awareness of tradeoffs (chose X over Y *because…*)
- Sense of ownership: "I decided…", "I noticed…", "I refactored…"

*Follow-up probes:*
- "What did you deliberately *not* do in that project, and why?"
- "If you were starting that project today, what would you change?"
- "How did other engineers on your team push back on your design?"

### P1-Q2 · Type B · ~6 min
**Tell me about a time you had to debug a tricky Python issue — a race condition, a memory leak, an async deadlock, or a `concurrent.futures` surprise. What was your debugging approach?**

*What to listen for:*
- Specific tooling (`tracemalloc`, `objgraph`, `py-spy`, `faulthandler`, logging, profilers)
- Hypothesis-driven debugging, not random print-statement shotgunning
- Reproducer-first thinking
- Awareness of GIL, threading vs multiprocessing vs async tradeoffs

*Follow-up probes:*
- "How did you confirm it was a race vs. a logic bug?"
- "What did you put in place so it wouldn't happen again?"
- "When you have a choice between threading, multiprocessing, and asyncio, how do you decide?"

### P1-Q3 · Type B · ~5 min
**Tell me about a time you refactored existing Python code to make it more robust or maintainable. What triggered the refactor?**

*What to listen for:*
- Trigger is a real signal: bug, change request, onboarding friction, test pain
- Refactor is scoped — not a "rewrite everything" ego trip
- Behavior preserved or test coverage justifies the change
- Mentions specific tools: dataclasses/pydantic, type hints, protocols, ABCs, properties, descriptors

*Follow-up probes:*
- "How did you convince the team the refactor was worth the churn?"
- "Did you add tests first, or after?"
- "What was the diff in lines? Was it worth it?"

### P1-Q4 · Type S · ~5 min
**You're building a Python library that wraps an unreliable third-party API. Walk me through how you'd structure it.**

*What to listen for:*
- Layered design: client → wrapper → domain
- Retries with exponential backoff + jitter, idempotency keys where applicable
- Custom exceptions that hide vendor error codes
- Context manager for resource lifecycle
- Pydantic/dataclass models for input/output
- Logging at boundaries
- Async + sync versions, or clear reasoning why one only

*Follow-up probes:*
- "How do you make this testable without hitting the real API?"
- "Where do you draw the line on what to abstract away?"

### P1-Q5 · Type T · probes throughout
- "Walk me through how a Python `@contextmanager` decorator works under the hood. When would you write a class-based one instead?"
- "Show me, on the board, what this async code does and where it might deadlock: `asyncio.gather(*[fetch(u) for u in urls if not cached(u)])`"
- "What's the difference between `@staticmethod`, `@classmethod`, and a module-level function? When do you reach for each?"
- "When do you use `dataclass` vs `pydantic.BaseModel` vs a plain `dict`?"

### Live exercise — see `02-live-exercises.md` for P1-LIVE-1

---

## Competency 2 — LLM & Agentic Systems

> *Map to rounds: R2 (primary), R1 (light probes).*

### P2-Q1 · Type B · ~6 min
**Tell me about a project where you integrated an LLM into a real workflow. What worked, what didn't, and what did you ship vs. abandon?**

*What to listen for:*
- Real project, not a tutorial — even if small
- Specific model choices and *why* (cost, latency, capability, structured-output support)
- Mentions concrete failure modes they hit (hallucination, JSON breakage, latency, cost)
- Iteration loop: they measured something, changed a prompt, and saw a result
- Awareness of the gap between "works in eval" and "works in prod"

*Follow-up probes:*
- "How did you measure success? What was the metric?"
- "What was the cost per request, and did that change your design?"
- "If you had to ship it again, what would you do differently in the first week?"

### P2-Q2 · Type B · ~6 min
**Describe a time you had to enforce structure on model output — getting reliable JSON, tool calls, or a fixed schema. What approach did you use?**

*What to listen for:*
- Knows the techniques: function calling / tool use, JSON mode, grammar-constrained decoding, schema validation, retry-with-feedback loops
- Defensive design: never trust the model — parse, validate, retry with error context
- Mentions Pydantic / instructor / outlines / guidance / langchain output parsers
- Realized some outputs can't be constrained, and used post-processing or model selection instead

*Follow-up probes:*
- "What fraction of calls needed a retry? How did you measure that?"
- "When JSON mode isn't enough, what's the next thing you try?"
- "How do you version prompts so you can A/B test changes?"

### P2-Q3 · Type B · ~5 min
**Tell me about a multi-step workflow you built — input → tool → AI → decision → next step. Walk me through the state model.**

*What to listen for:*
- Explicit state representation (typed state, message history, scratchpad)
- Loop termination criteria (max steps, model-driven stop, success signal)
- Idempotency / replayability of steps
- Awareness of context window limits and how they handle long histories
- Reasoning about determinism vs. stochasticity

*Follow-up probes:*
- "How do you resume a workflow that crashed at step 47 of 50?"
- "Where does state live? In memory, in a DB, in the prompt itself?"
- "When do you reach for LangGraph vs. just writing the loop yourself?"

### P2-Q4 · Type S · ~6 min
**You're building an agent that takes a high-level goal, plans steps, calls tools, observes results, and decides what to do next. How do you design the loop? How do you handle state, errors, and a model that sometimes hallucinates function names?**

*What to listen for:*
- State model: typed, serializable, checkpointable
- Termination: explicit success criteria + max steps + cost ceiling
- Error handling: distinguish transient (retry) from permanent (replan or fail)
- Schema enforcement: tool registry with validation, not freeform JSON
- Observability: every step logged with model, prompt hash, tool, result, cost, latency
- Cost ceiling: track and stop on budget, not just steps

*Follow-up probes:*
- "How do you evaluate this agent offline before letting it touch real systems?"
- "How do you prevent the agent from looping forever on a tool that returns empty results?"
- "What does the prompt actually look like?"

### P2-Q5 · Type S · ~5 min
**Your agent is making 1000 tool calls per run and 3% are failing. The model sometimes hallucinates function names. Cost is creeping up. Walk me through how you'd diagnose and improve this.**

*What to listen for:*
- Telemetry first: failure mode breakdown (timeout, validation, hallucination, model-refusal)
- Categorization: which tools fail, which inputs trigger hallucination
- Mitigation ladder: prompt fix → schema fix → tool description rewrite → few-shot examples → fine-tune → fallback model
- Cost analysis: which calls are expensive and why
- A/B evaluation harness before changing prod

*Follow-up probes:*
- "What would you log on every call to answer this question later?"
- "If you could only fix one thing, which and why?"

### P2-Q6 · Type T · probes
- "When do you reach for LangGraph vs. raw LLM calls vs. rolling your own agent loop?"
- "What does `tool_choice="required"` actually do, and when is it dangerous?"
- "How would you implement a simple ReAct loop from scratch in 20 lines?"
- "What's the difference between MCP and a regular API? Why does it matter for agents?"

---

## Competency 3 — API, Backend & Systems Design

> *Map to rounds: R3 (primary), R1 (light).*

### P3-Q1 · Type B · ~6 min
**Tell me about a REST API you designed or significantly shaped. What did you get right, and what would you change?**

*What to listen for:*
- Resource modeling, not RPC-in-disguise
- Status codes used meaningfully, not all 200s
- Versioning strategy
- Error response shape (consistent, machine-readable)
- Idempotency on POSTs where it matters
- Auth model considered (token, scope, rotation)
- Awareness of what they got wrong

*Follow-up probes:*
- "How did you handle pagination — offset, cursor, or something else?"
- "How did you document it? OpenAPI, hand-written, just code?"
- "How would you change the error model today?"

### P3-Q2 · Type B · ~5 min
**Tell me about a time you built a wrapper or abstraction around a complex system — an internal tool, a vendor API, a CLI binary. Why did you wrap it?**

*What to listen for:*
- Clear motivation: not abstraction-for-its-own-sake
- Boundaries: what's in, what's out
- Tradeoffs: added complexity vs. gained leverage
- Real adoption: someone else used it

*Follow-up probes:*
- "Did you get pushback? From whom?"
- "What's the smallest version of this wrapper that would have been useful?"

### P3-Q3 · Type B · ~4 min
**Tell me about a Docker-related problem you had to solve. Maybe a layer-cache issue, a multi-stage build, a networking problem between containers.**

*What to listen for:*
- Hands-on, not theoretical
- Multi-stage builds, image size awareness
- docker-compose for local dev
- Network modes, volume mounts, healthchecks
- Awareness of the security model (rootless, capability drops)

*Follow-up probes:*
- "How do you keep your final image small?"
- "How do you handle secrets in local dev vs. prod?"

### P3-Q4 · Type S · ~7 min
**You need to expose 5 internal security tools — each is a CLI binary with inconsistent flags, different output formats, and different auth requirements — as a unified API for an agent to consume. Walk me through the design.**

*What to listen for:*
- Common interface: each tool gets a typed wrapper, normalized JSON output, consistent error model
- Auth abstraction: per-tool credentials, rotated centrally
- Async vs sync consideration: tools may take minutes
- State per call: correlation IDs, log shipping
- Versioning and deprecation: agent consumers can't change overnight
- "Agent contract" thinking: tool descriptions that are LLM-readable, not just human-readable

*Follow-up probes:*
- "How do you make the tool descriptions good enough that the LLM picks the right tool?"
- "What does the JSON schema for one of these tool responses look like?"

### P3-Q5 · Type S · ~5 min
**Walk me through how you'd containerize a Python service that makes LLM calls, retries on failure with backoff, persists state across restarts, and exposes its progress over a websocket.**

*What to listen for:*
- Multi-stage Dockerfile, slim base, non-root user
- Config via env vars / 12-factor
- State store choice (Redis, SQLite, Postgres, file) and why
- WebSocket lifecycle: reconnection, backpressure, heartbeat
- Health/readiness endpoints separate from liveness
- Graceful shutdown handling in-flight requests
- Observability: structured logs, traces, metrics

*Follow-up probes:*
- "Where would you put the state, and what schema would it have?"
- "What does your Dockerfile look like?"
- "How do you test this locally?"

### P3-Q6 · Type T · probes
- "What's the difference between a 401 and a 403? When do you use each?"
- "When would you reach for a queue (Celery, RQ, SQS) inside a Python service?"
- "Show me how you'd structure an OpenAPI spec for an agent-callable tool."

---

## Competency 4 — Reliability & Production Engineering

> *Map to rounds: all (this is the cross-cutting competency).*

### P4-Q1 · Type B · ~5 min
**Tell me about a system you made more reliable. What was the failure mode, what did you change, and how did you measure the improvement?**

*What to listen for:*
- Specific failure mode, not "it was flaky"
- Change was a system change, not a band-aid
- Measurement before/after (error rate, p99 latency, MTBF)
- Awareness of the cost of false positives vs. missed failures

*Follow-up probes:*
- "What monitoring did you add so you'd know if it regressed?"
- "What did you *not* do, and why?"

### P4-Q2 · Type B · ~5 min
**Tell me about a time a production issue was caused by an edge case you didn't handle. How did you find it, fix it, and prevent it from recurring?**

*What to listen for:*
- Honest — they own it, not "someone else"
- Detection: alerting, customer report, log review
- Fix: targeted, not a rewrite
- Prevention: test, linter rule, type narrowing, runbook

*Follow-up probes:*
- "What test would have caught this?"
- "What part of the system architecture allowed this to reach prod?"

### P4-Q3 · Type S · ~5 min
**An agent workflow runs for 30 minutes, calls 50 tools, and crashes at step 49 with a transient error. The user is frustrated. How do you design for resumability and how do you handle the live incident?**

*What to listen for:*
- Live: communicate, stop the bleeding, don't paper over
- Architecture: checkpointed state, idempotent steps, replayable
- Tool design: each call should be safely re-runnable
- Cost-aware: don't replay expensive steps
- Observability: clear trail from step 1 to 49

*Follow-up probes:*
- "What does the state object look like at step 49?"
- "How would you design the tool API to be safely retriable?"

### P4-Q4 · Type S · ~4 min
**You discover a memory leak that only happens after 6 hours of uptime in production but never in tests. Walk me through how you'd debug it.**

*What to listen for:*
- Can't repro locally → think about prod-specific state (long-lived connections, real load, GC pressure)
- Use prod-safe tools: heap snapshots, tracemalloc sampling, py-spy in prod
- Hypothesis list: caches without bounds, event listener leaks, circular refs with __del__, large object retention in closures
- Fix: bounded LRU cache, weak references, explicit cleanup, periodic restart as last resort

*Follow-up probes:*
- "How do you debug in prod without making it worse?"
- "When is a periodic restart a legitimate engineering choice, not a hack?"

### P4-Q5 · Type T · probes
- "Show me how you'd implement exponential backoff with jitter and a circuit breaker in Python."
- "What's the difference between a retry, a fallback, and a circuit breaker?"
- "How do you make a tool call idempotent in a system where the underlying API isn't?"

---

## Competency 5 — Collaboration & Domain Curiosity

> *Map to rounds: R4 (primary).*

### P5-Q1 · Type B · ~5 min
**Tell me about a time you worked closely with experts in a domain you didn't know well — could be ML, security, finance, healthcare, anything. How did you ramp up?**

*What to listen for:*
- Active learning: asked questions, read targeted material, paired with experts
- Humility: didn't pretend to know
- Translation: converted domain concepts into engineering specs
- Built feedback loops with the experts

*Follow-up probes:*
- "What was the most humbling moment?"
- "What did you build that the domain expert was actually excited about?"

### P5-Q2 · Type B · ~5 min
**Tell me about a time you had to translate a non-technical or cross-functional concept into something engineers could build. What was hard about the translation?**

*What to listen for:*
- Identified the gap in vocabulary, not just the gap in knowledge
- Used concrete examples, not jargon
- Iterated: built a draft, got feedback, refined
- Awareness of what got lost in translation

*Follow-up probes:*
- "What did the engineers build that wasn't quite what the expert wanted? How did you find out?"

### P5-Q3 · Type S · ~5 min
**A senior red teamer is explaining a "pass-the-hash" attack to you. You don't know the term. The team needs you to automate this technique next week. Walk me through what you do in the next 48 hours.**

*What to listen for:*
- Active listening in the meeting: ask clarifying questions, take notes
- Targeted ramp: read the relevant MITRE ATT&CK entry, find a public PoC, watch a talk
- Pair with the expert: schedule office hours, build a prototype together
- Spec-first: write up what you think you heard, get sign-off
- Safety: questions about scope, blast radius, ethics — this is offensive security

*Follow-up probes:*
- "How do you make sure you actually understood the technique vs. just nodding along?"
- "What would the spec look like?"

### P5-Q4 · Type S · ~5 min
**Your agent succeeds at the goal but does something the security expert didn't anticipate — say, it tried a technique that's noisier or more aggressive than the human would have chosen. How do you handle it?**

*What to listen for:*
- Doesn't defend the model: "it worked, so…"
- Recognizes the cost: stealth, attribution, operational risk are part of the requirement
- Debrief with the expert to understand what was missed
- Updates prompt, tool descriptions, or guardrails
- Considers: do we need a human-in-the-loop here?

*Follow-up probes:*
- "How do you encode 'stealth' as a constraint the agent can reason about?"
- "When do you make the agent ask before acting?"

### P5-Q5 · Type T · probes
- "What excites you about applying AI to offensive security?"
- "What's a recent AI/agent paper or tool that you read and what did you take from it?"
- "How do you stay current on a fast-moving field?"

---

## Question Allocation by Round

| Round | Time | Question IDs (spend ~ minutes each) |
|---|---|---|
| **R1 — Python Deep Dive** | 60 min | P1-LIVE-1 (~25 min) + P1-Q1 (6) + P1-Q2 (6) + P1-Q5 probes (10) + wrap (5) |
| **R2 — LLM & Agentic** | 60 min | P2-Q1 (6) + P2-Q2 (6) + P2-Q4 (6) + P2-LIVE-1 design (~25 min) + P2-Q6 probes (10) + wrap (5) |
| **R3 — APIs & Backend** | 45 min | P3-Q1 (6) + P3-Q4 (7) + P3-LIVE-1 (~20 min) + P3-Q6 probes (8) + wrap (4) |
| **R4 — Collaboration** | 45 min | P5-Q1 (5) + P5-Q2 (5) + P5-Q3 (5) + P5-Q4 (5) + P1-Q1 or P2-Q1 light reframe (5) + candidate Qs (15) + wrap (5) |

Note: timings are aspirational. Cut the last probe before cutting a behavioral — the behavioral signal is harder to recover.
