# Live Coding & Design Exercises

> Three live exercises, one per technical round. Each is timed and structured with clear "done" criteria and a scoring rubric tied to competencies.

---

## P1-LIVE-1 — Python Engineering (R1, 25 min)

### The Prompt
> You're building a Python client for a flaky LLM API. The API:
> - Accepts a list of prompts and returns a list of completions
> - Sometimes returns HTTP 429 (rate limit) or HTTP 500 (server error)
> - Sometimes returns 200 but with a malformed JSON body (missing fields)
> - Latency varies from 100ms to 10s
>
> Implement `class LLMClient` with:
> 1. A `complete(prompts: list[str], model: str) -> list[Completion]` method
> 2. Retries with exponential backoff + jitter on 429/5xx
> 3. Strict validation of the response body — raise a typed error on schema mismatch
> 4. Bounded concurrency (don't fire 1000 requests at once)
> 5. Resource lifecycle via context manager (`with LLMClient(...) as client:`)
>
> Use the mock `_call_api` below as the backing implementation. You don't have to implement the API itself.

```python
import time, random, json
from dataclasses import dataclass

@dataclass
class Completion:
    text: str
    model: str
    tokens: int

class APIError(Exception): ...
class SchemaError(APIError): ...
class RateLimitError(APIError): ...

def _call_api(prompts: list[str], model: str) -> dict:
    # Pretend this is the real API.
    if random.random() < 0.3: raise RateLimitError("429")
    if random.random() < 0.2: raise APIError("500")
    if random.random() < 0.1: return {"bad": "shape"}  # malformed
    time.sleep(random.uniform(0.1, 0.3))
    return {"results": [{"text": p[::-1], "model": model, "tokens": len(p)} for p in prompts]}
```

### What to Listen For
- **Strong signals:** typed exceptions, custom `__aenter__`/`__aexit__` if async, `Semaphore` for bounded concurrency, jitter in backoff, validation via Pydantic/dataclass, log on each retry, distinguishes "retryable" from "permanent" failure
- **Red flags:** swallows exceptions, sleeps without jitter, hardcoded retry count, no schema validation, no testability hook (can't inject a fake `_call_api`), prints instead of logging
- **Stretch prompts if they finish early:**
  - "Add a `cost_tracker` that totals tokens × model price"
  - "Add a `cache` keyed on `(prompt, model)` with TTL"
  - "Make this async — what's the smallest change?"

### Scoring
- **4 (exceeds):** Ships working solution in <20 min with all 5 requirements + adds one stretch + explains tradeoffs aloud
- **3 (meets):** Ships working solution with 4/5 requirements, explains at least one tradeoff
- **2 (below):** Has a structure but doesn't complete, or completes with major gaps (no retries, no validation, no concurrency control)
- **1 (well below):** Cannot articulate how to start; doesn't know what a context manager is

---

## P2-LIVE-1 — Agent System Design (R2, 25 min)

### The Prompt
> Design a simple agent on the board. Goal: *"Given a hostname, determine if it's a Windows box with SMB open, and if so, gather its hostname, OS version, and list of running services."*
>
> Don't write code — design the system. I want to see:
> 1. The tools the agent has access to (signatures + descriptions)
> 2. The state the agent carries between steps
> 3. The loop: how it decides what to do next
> 4. How it terminates (success, failure, max steps)
> 5. What you'd log on every step
> 6. The first prompt you'd send to the model

### What to Listen For
- **Tools:** `nmap_scan(host, ports)`, `smb_enum(host)`, `service_list(host)` — each with typed input/output, LLM-readable description
- **State:** message history, scratchpad with observations, current goal, step counter, cost tracker
- **Loop:** model picks tool → execute → append observation → check termination → re-prompt
- **Termination:** success condition met, max steps (say, 8), cost ceiling
- **Logging:** step #, prompt hash, model, tool called, result (truncated), latency, tokens, cost
- **First prompt:** system message with role, available tools in schema, output format requirement, current state
- **Stretch signals:** budgeted tool calls, self-critique step, structured output via tool calling, distinguishes "I don't know" from "I tried and failed"

### Scoring
- **4 (exceeds):** All 6 elements present + discusses failure modes (hallucinated tool, infinite loop, empty result) + suggests a guardrail for at least one
- **3 (meets):** All 6 elements present with reasonable answers
- **2 (below):** Misses 1–2 elements (often: state model or termination)
- **1 (well below):** Cannot describe an agent loop or confuses "agent" with "single-shot prompt"

---

## P3-LIVE-1 — API Design (R3, 20 min)

### The Prompt
> On the board, design the REST API for an agentic platform that wraps 5 offensive-security tools. The platform:
> - Accepts a high-level goal from a user
> - Runs an agent loop that calls tools
> - Streams progress back to a UI
> - Persists run state for resumability
>
> Show me:
> 1. The endpoints (method, path, request/response shape)
> 2. How a client would create a run, subscribe to its progress, and resume a crashed run
> 3. The error model
> 4. How you'd version this without breaking the agent

### What to Listen For
- **Endpoints (typical strong answer):**
  - `POST /v1/runs` — create run, returns `run_id`
  - `GET /v1/runs/{id}` — current state
  - `GET /v1/runs/{id}/events` (SSE or WS) — progress stream
  - `POST /v1/runs/{id}/resume` — restart from checkpoint
  - `GET /v1/tools` — list available tools (for the agent's prompt)
- **Resumability:** client gets `run_id` and `last_event_id`, can resume with that cursor; server side has a state store
- **Error model:** consistent shape `{error: {code, message, retryable, trace_id}}`, distinct from run errors which are *events* in the stream
- **Versioning:** URL path (`/v1/`, `/v2/`), explicit deprecation window, agent pinned to a tool schema version

### Scoring
- **4 (exceeds):** All 4 elements + discusses idempotency keys on run creation + separates "API errors" from "run errors"
- **3 (meets):** All 4 elements with reasonable answers
- **2 (below):** Misses resumability or versioning
- **1 (well below):** Treats it as CRUD with no consideration of streaming, state, or version evolution

---

## General Live-Exercise Rules

1. **Set the rules up front** — "I'm here to help if you get stuck. I'll ask questions. We're not testing for line-by-line correctness — we're testing for structure and reasoning."
2. **Think aloud is mandatory** — "Please narrate what you're thinking as you go. I'd rather hear a wrong idea with reasoning than silence."
3. **One stretch only** — if they finish, give one stretch prompt, then stop. Don't pile on.
4. **No rubber-duck silence** — if they're quiet for 60 seconds, prompt: "What are you considering right now?"
5. **No "gotchas"** — don't try to trick them. The exercise is hard enough.
6. **Score as you go** — fill in the scorecard in real time, not from memory afterward.
