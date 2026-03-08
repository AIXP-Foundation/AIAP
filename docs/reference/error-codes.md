# Error Handling Reference

> Complete reference for error classification, circuit breaker patterns, and error handling rules in AIAP programs.

---

## Error Categories

AIAP defines three error categories. Every error must be classified into exactly one category.

| Category | Description | Behavior | User Impact |
|----------|-------------|----------|-------------|
| RECOVERABLE | Transient failures that may succeed on retry | Retry with exponential backoff, then proceed | User may experience brief delay |
| DEGRADABLE | Partial capability loss; core function still available | Fallback to reduced functionality via named alternative | User informed of reduced capability |
| FATAL | Unrecoverable failure in a critical path | Halt execution immediately, inform user | User cannot proceed with this operation |

---

## Error Category Decision Table

| Module `critical` | Error Type | Retry Succeeds? | Category | Action |
|-------------------|-----------|-----------------|----------|--------|
| true | Transient | yes | RECOVERABLE | Retry and continue |
| true | Transient | no | FATAL | Halt, inform user |
| true | Permanent | n/a | FATAL | Halt, inform user |
| false | Transient | yes | RECOVERABLE | Retry and continue |
| false | Transient | no | DEGRADABLE | Skip module, use fallback |
| false | Permanent | n/a | DEGRADABLE | Skip module, use fallback |

---

## Named Circuit Breaker Pattern (I4)

Rule I4 requires all error handling to follow the Named Circuit Breaker pattern. Every failure path must have a specific, named fallback.

### Pattern Flow

```
fails -> retry -> circuit-breaker -> fallback: {specific named alternative} -> inform user
```

### Pattern Stages

| Stage | Description | Constraints |
|-------|-------------|-------------|
| Detect | Identify that an operation has failed | Must distinguish transient from permanent failure |
| Retry | Attempt the operation again with backoff | Max retries defined by `runtime.retries` (default: 3) |
| Circuit Break | Stop retrying after threshold is reached | Threshold = `runtime.retries` consecutive failures |
| Fallback | Execute a specific named alternative | Must be a real, declared alternative (not generic) |
| Inform | Notify the user of what happened and what changed | Must describe the degradation, not just "an error occurred" |

### Retry Strategy

| Attempt | Delay | Description |
|---------|-------|-------------|
| 1 | 0s | Immediate first retry |
| 2 | 1s | Short backoff |
| 3 | 4s | Exponential backoff |
| n | `2^(n-1)` seconds | Capped at `runtime.timeout` |

### Example: Named Fallback

```
Node: FetchWeatherAPI
  fails -> retry (3 attempts)
    -> circuit-breaker
      -> fallback: UseCachedWeatherData
        -> inform user: "Live weather unavailable. Showing cached data from {timestamp}."
```

**Violation example (generic fallback, not acceptable):**
```
Node: FetchWeatherAPI
  fails -> retry -> fallback: HandleError -> inform user: "An error occurred."
```

---

## Error Classification by Module Criticality

The `critical` field on each module determines how errors escalate.

### Critical Modules (`critical: true`)

| Scenario | Response |
|----------|----------|
| Recoverable error, retry succeeds | Continue execution normally |
| Recoverable error, retry exhausted | Escalate to FATAL |
| Permanent error | Immediate FATAL |
| FATAL outcome | Halt entire program execution, inform user with specific reason |

### Non-Critical Modules (`critical: false`)

| Scenario | Response |
|----------|----------|
| Recoverable error, retry succeeds | Continue execution normally |
| Recoverable error, retry exhausted | Escalate to DEGRADABLE |
| Permanent error | Immediate DEGRADABLE |
| DEGRADABLE outcome | Skip module, activate named fallback, inform user |

---

## Error Handling Rules

### Rule 1: No Generic Error Messages

Every error message must be specific and actionable.

| Violation | Correct |
|-----------|---------|
| "An error occurred" | "Weather API returned HTTP 503. Using cached data from 2026-03-01." |
| "Something went wrong" | "File write to /data/log.csv failed: permission denied. Logging to console instead." |
| "Please try again" | "Database connection timed out after 30s. Retried 3 times. Please check your network and retry." |

### Rule 2: Named Fallbacks Only

The `fallback` in a circuit breaker must reference a specific, declared alternative.

| Violation | Correct |
|-----------|---------|
| `fallback: handleError` (generic) | `fallback: UseCachedData` (specific capability) |
| `fallback: retry` (circular) | `fallback: ManualInputFallback` (alternative path) |
| `fallback: null` (no fallback) | `fallback: InformUserNoAlternative` (explicit dead-end) |

### Rule 3: User Must Always Be Informed

Every DEGRADABLE and FATAL outcome must include a user-facing notification.

| Category | Notification Must Include |
|----------|--------------------------|
| DEGRADABLE | What capability is reduced, what alternative is active, expected impact |
| FATAL | What failed, why it is unrecoverable, what the user can do next |

### Rule 4: Side Effects on Error

| Scenario | Rule |
|----------|------|
| Error before side effect | No cleanup needed |
| Error after side effect (idempotent) | Safe to retry |
| Error after side effect (non-idempotent) | Must attempt rollback or document partial state |

### Rule 5: Error Propagation

| Propagation Rule | Description |
|-----------------|-------------|
| No silent swallowing | Errors must never be caught and ignored |
| Scope boundary | Errors in sub_mermaid nodes propagate to the parent node |
| Cross-module | Errors in one module must not silently affect other modules |
| Logging | All errors must be logged with timestamp, node, and category |

---

## Error Handling in .aisop.json

Errors are handled through the `on_fail` field in function node definitions.

```json
"functions": {
  "FetchWeatherAPI": {
    "step": "Fetch current weather data from the weather API",
    "tools": ["http_client"],
    "on_fail": "UseCachedWeatherData"
  },
  "UseCachedWeatherData": {
    "step": "Load the most recent cached weather data and inform user of staleness",
    "tools": ["file_system"],
    "on_fail": "InformNoWeatherData"
  },
  "InformNoWeatherData": {
    "step": "Inform user that weather data is completely unavailable",
    "tools": []
  }
}
```

### Fallback Chain Depth

| Depth | Acceptability |
|-------|--------------|
| 1 (primary -> fallback) | Standard |
| 2 (primary -> fallback -> last resort) | Acceptable for critical paths |
| 3+ | Discouraged; indicates design issue |

---
Align: Human Sovereignty and Benefit | Protocol: AIAP | Seed: AISOP | Executor: SoulBot
