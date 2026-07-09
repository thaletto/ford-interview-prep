# Scorecard

> One scorecard per interviewer. Submit before the debrief — no peeking at other scores.

---

## The Scale

Each competency is scored **1–4** with these anchors. **3 is a hire.** **4 is a strong hire.** **2 is a no-hire unless overridden with a strong reason.** **1 is a strong no-hire.**

| Score | Label | One-line description |
|---|---|---|
| **4** | Exceeds Bar | Deep, specific expertise. Teaches you something. Multiple concrete examples. Novel applications. |
| **3** | Meets Bar | Clear, specific evidence of competence. Can apply concepts in unfamiliar-but-related situations. |
| **2** | Below Bar | Can describe concepts but cannot apply them. Examples lack depth or specificity. Notable gaps. |
| **1** | Well Below Bar | Cannot articulate fundamentals. Examples show repeated failure. Live exercise reveals lack of competence. |

**Read the column header, then the row label, then ask: "Which box does this candidate sit in?"**

---

## Competency 1 — Python Engineering & Software Craft

| | 1 — Well Below | 2 — Below | 3 — Meets | 4 — Exceeds |
|---|---|---|---|---|
| **OOP & module design** | Can't articulate when to use a class vs. a function | Uses classes but design is procedural-in-disguise | Designs clean class/module boundaries; uses protocols/ABCs where appropriate | Idiomatically uses descriptors, metaclasses, or protocols to solve a real problem |
| **Async / concurrency** | Confuses threading, multiprocessing, and asyncio | Uses `asyncio` but ships deadlock-prone code | Chooses the right primitive for the job; can reason about GIL, backpressure, cancellation | Designed a production concurrent system with bounded concurrency, cancellation, and graceful shutdown |
| **Decorators / context managers** | Doesn't know what these are | Can write one with prompting | Uses them idiomatically; can implement both class-based and `@contextmanager` | Uses advanced patterns (stacked decorators, parameterized context managers, `__enter__` returning non-self) |
| **Error handling & reliability** | Swallows exceptions or lets them crash the process | Catches everything generically | Distinguishes retryable vs. permanent; uses typed exceptions; backoff with jitter | Designs idempotent, retriable, observable failure surfaces |
| **Debugging & tooling** | Prints, then guesses | Uses some debug tooling but not systematically | Uses `pdb`/`py-spy`/`tracemalloc`/profilers; forms hypotheses before acting | Diagnosed a non-obvious bug under time pressure with measurable verification |

**P1 evidence notes (specifics only):**

```
[Your notes here]
```

**P1 score: __ / 4**

---

## Competency 2 — LLM & Agentic Systems

| | 1 — Well Below | 2 — Below | 3 — Meets | 4 — Exceeds |
|---|---|---|---|---|
| **Prompt engineering** | Has never sent a prompt to an API | Writes a prompt and hopes | Iterates with measurable feedback; uses system/user/role separation | Designs prompts as versioned, A/B-testable artifacts with eval harness |
| **Structured output** | Doesn't know how to constrain model output | Uses JSON mode but doesn't validate | Validates with Pydantic; retries with error context; chooses function calling vs. grammar-constrained | Implemented a custom output parser or schema-enforced pipeline in prod |
| **Agent loops** | Confuses "agent" with "single-shot prompt" | Can describe ReAct at a high level | Designs stateful loops with explicit termination, replanning, and error recovery | Implemented a multi-step agent with checkpointing, budgeting, and observability |
| **Reliability of stochastic systems** | Believes model output should be trusted | Knows it can't, but no strategy | Designs for failure: retries, fallbacks, validation, observability | Quantified reliability (success rate, cost-per-task) and drove systematic improvements |
| **Framework literacy** | Has only used one approach (e.g., raw API only) | Has heard of LangChain but not used it | Knows tradeoffs between raw API, LangChain, LangGraph, custom loops | Has shipped production code with at least one framework and migrated off another |

**P2 evidence notes:**

```
[Your notes here]
```

**P2 score: __ / 4**

---

## Competency 3 — API, Backend & Systems Design

| | 1 — Well Below | 2 — Below | 3 — Meets | 4 — Exceeds |
|---|---|---|---|---|
| **REST design** | Doesn't distinguish 4xx from 5xx | Designs RPC-in-disguise | Designs resource-oriented APIs with correct status codes, error model, versioning | Designed an API that survived multiple breaking changes without downtime |
| **Wrappers & abstractions** | Wraps for the sake of wrapping | Wraps correctly but creates leaky abstractions | Wraps with clear boundaries; consistent interface across heterogeneous systems | Wraps in a way that becomes the team's default primitive |
| **State & persistence** | Keeps state in memory or globals | Uses a DB but no schema thinking | Chooses appropriate state store; designs checkpointable state | Designed resumable, replayable state for a long-running workflow |
| **Docker & packaging** | Has only used `docker run` | Writes Dockerfiles that work but bloat | Multi-stage builds, slim bases, non-root, healthchecks | Optimized image size and layer cache for fast CI |
| **WebSocket / streaming** | Has not used | Uses it but doesn't handle backpressure/reconnection | Designs for reconnection, heartbeat, backpressure, ordering | Designed a streaming API that survives network partitions |

**P3 evidence notes:**

```
[Your notes here]
```

**P3 score: __ / 4**

---

## Competency 4 — Reliability & Production Engineering

| | 1 — Well Below | 2 — Below | 3 — Meets | 4 — Exceeds |
|---|---|---|---|---|
| **Retries & idempotency** | Doesn't know what idempotency means | Retries without backoff or jitter | Exponential backoff with jitter; idempotency keys; distinguishes retryable from permanent | Designed a retry policy with circuit breakers, dead-letter handling, and observability |
| **Edge cases & validation** | Doesn't think about inputs at boundaries | Some validation, gaps in coverage | Validates at boundaries; explicit handling of null/empty/oversized inputs | Defends against adversarial inputs because the system is exposed to untrusted agents |
| **Observability** | Logs "an error happened" | Logs at error level only | Structured logs, traces, metrics; correlation IDs across services | Built a debugging story that let you resolve an incident in <30 min |
| **Resumability** | Has not thought about it | Recovers from crash by restarting from scratch | Checkpoints state; replays safely; can resume from a known point | Designed a system where crashes are a non-event |
| **Cost & resource awareness** | Ignores cost | Some awareness, no monitoring | Tracks cost per request/run; sets budgets; alerts on anomalies | Made a specific design change that cut cost by N% with no functional regression |

**P4 evidence notes:**

```
[Your notes here]
```

**P4 score: __ / 4**

---

## Competency 5 — Collaboration & Domain Curiosity

| | 1 — Well Below | 2 — Below | 3 — Meets | 4 — Exceeds |
|---|---|---|---|---|
| **Ramp into new domain** | Pretends to know; doesn't ask | Asks but doesn't act on answers | Targeted reading + paired sessions + builds draft to test understanding | Becomes a translator who can teach the team the new domain |
| **Cross-functional translation** | Asks the engineer to "just explain the requirements again" | Translates literally, missing nuance | Builds spec with examples; iterates with domain expert; catches mis-translations early | Builds a shared vocabulary that outlives the project |
| **Feedback reception** | Defensive when corrected | Accepts but doesn't change behavior | Welcomes feedback; demonstrates iteration across the interview | Actively solicited feedback that changed their design |
| **Communication clarity** | Hard to follow; jargon-heavy | Clear but slow to get to the point | Clear, structured, calibrated ("I'm 70% sure, here's why") | Concise and precise; can explain a complex system in 2 minutes |
| **Curiosity & motivation** | No signal of interest in the role | Generic interest ("AI is cool") | Specific reasons this role/team; has done prep | Articulates a 12-month vision for themselves in the role |

**P5 evidence notes:**

```
[Your notes here]
```

**P5 score: __ / 4**

---

## Overall Recommendation (per interviewer)

| | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| **Strong No Hire** | ☐ | | | |
| **No Hire** | | ☐ | | |
| **Hire** | | | ☐ | |
| **Strong Hire** | | | | ☐ |

**One-paragraph summary (what they were like, what was strong, what was weak):**

```

```

**What would have to be true for you to be wrong about this candidate?**

```

```

**Submit before the debrief. Do not discuss with other interviewers beforehand.**
