---
name: evidence-based-engineering
description: Use when making non-trivial engineering decisions (refactor, redesign, choose between patterns, declare code "too complex", diagnose perf/regression issues) — requires grounding the call in concrete metrics and logs instead of gut feel or aesthetics
---

# Evidence-Based Engineering

## Overview

Senior engineering is not about preferences — it's about **evidence**. Before recommending a refactor, a pattern, or declaring something "bad code", measure it. Before diagnosing a bug, read the logs. Before optimizing, profile.

Core principle: **No engineering decision without a number or a log line backing it.**

## When to Use

Use whenever you are about to:

- Recommend a refactor, rewrite, or architectural change
- Pick between design patterns (Strategy vs. State, Service Object vs. Interactor, etc.)
- Call code "complex", "slow", "untested", "smelly", or "legacy"
- Diagnose a bug, regression, incident, or performance issue
- Estimate risk of a change ("is this safe to ship?")
- Justify technical debt cleanup to a stakeholder

Do **NOT** use for:
- Pure syntax/typo fixes
- Trivial renames
- Direct, single-line user requests ("change this string to X")

## The Decision Loop

```
OBSERVE (metrics + logs) → DIAGNOSE (pattern match) → DECIDE (trade-off) → VERIFY (re-measure)
```

Skipping `OBSERVE` is the cardinal sin. If you cannot cite a metric or a log entry, you are guessing.

## Metrics Toolbox (by question)

| Question | Tool / Metric | Decision threshold (rule of thumb) |
|---|---|---|
| Is this method too complex? | Cyclomatic / Cognitive Complexity (RuboCop, SonarQube, radon, lizard) | CC > 10 → review; > 15 → refactor |
| Is this class doing too much? | LOC, methods/class, afferent/efferent coupling, LCOM | Class > 300 LOC or LCOM > 0.8 → split |
| Is this code hot / risky? | Code churn × complexity (`git log --numstat`, code-maat, CodeScene) | High churn + high complexity = top priority |
| Is it duplicated? | jscpd, flay, SonarQube duplication % | > 5% project-wide → extract |
| Is it tested? | Coverage by line/branch (SimpleCov, c8, coverage.py) | < 70% on critical paths → add tests before refactor |
| Is it slow? | Profiler (rack-mini-profiler, stackprof, py-spy, perf, flamegraphs) | Identify the top frame, don't guess |
| Is it leaking? | Memory profiler (memory_profiler, heaptrack), RSS over time | Monotonic growth → leak |
| Is the query the problem? | EXPLAIN ANALYZE, pg_stat_statements, bullet | N+1, seq scans on large tables, missing index |
| Is it failing in prod? | Error rate, p95/p99 latency, APM (Sentry, Datadog, New Relic) | Compare against baseline, not absolutes |
| Is the dependency healthy? | bundler-audit, npm audit, dependabot, license check | Any high CVE → block |
| Is the design degrading? | Trend of metrics over time, not snapshot | Worsening trend → act before threshold |

## Logs & Traces Toolbox

Before debugging, answer with evidence:

1. **What does the log say?** Grep the exact error / request_id / correlation_id. Don't paraphrase — quote it.
2. **What is the timeline?** Reconstruct event order from timestamps across services.
3. **What changed?** `git log --since=<incident-start>` + recent deploys.
4. **Is it reproducible?** Find the minimal log signature that reproduces.
5. **What's the blast radius?** Count of affected requests/users from logs, not estimates.

Tools: `grep`/`rg`, `jq` for JSON logs, `lnav`, APM trace view, `journalctl`, structured log queries (Loki/CloudWatch/Datadog).

## Patterns Knowledge (apply, don't name-drop)

Cite the pattern only when it solves a measured problem:

- **SOLID** — invoke SRP only after pointing to high LCOM / multiple change reasons in `git log`.
- **GoF** — Strategy/State/Decorator/Adapter — match the *forces*, not the name. Show the conditional/duplication metric that justifies it.
- **DDD** — Bounded Contexts, Aggregates — invoke only when there's evidence of model contention (one model with conflicting invariants across teams).
- **Refactoring catalog (Fowler)** — name the smell with a metric: *Long Method* (LOC + CC), *Feature Envy* (cross-class calls), *Shotgun Surgery* (churn correlation).
- **Concurrency** — never propose threads/async without a contention or latency profile.
- **Caching** — never propose a cache without a hit/miss baseline and a measured invalidation cost.

Anti-pattern: invoking a pattern because it sounds sophisticated. If you can't tie it to a metric or a failure mode, drop it.

## Decision Output Format

When proposing a change, structure the recommendation as:

```
EVIDENCE   : <metric / log line / profile output, with source>
DIAGNOSIS  : <which smell / pattern / failure mode this matches>
OPTIONS    : <2–3 alternatives with trade-offs (cost, risk, reversibility)>
RECOMMENDED: <chosen option + the threshold that would change the choice>
VERIFY     : <metric to re-measure after the change to confirm impact>
```

This is non-negotiable for decisions with blast radius beyond the immediate file.

## Red Flags — STOP and gather evidence

| Thought | Reality |
|---|---|
| "This looks complex" | Measure CC. Subjective complexity ≠ metric complexity. |
| "I think this is slow" | Profile. Hot path is rarely where you think. |
| "We should add a cache" | Measure hit ratio assumption + invalidation cost first. |
| "This is over-engineered" | Show the unused abstraction *with usage counts*. |
| "Let's switch to pattern X" | Show the metric pattern X improves and the predicted delta. |
| "It's flaky" | Reproduce, log, attach a trace. Flaky = not yet understood. |
| "Just rewrite it" | Compute churn × complexity. Rewrite is the last resort, not the first. |
| "It worked on my machine" | Get the prod log. Different environments, different evidence. |

## Common Mistakes

- **Cargo-cult metrics:** chasing 100% coverage / 0 RuboCop offenses while ignoring churn-weighted hotspots.
- **Snapshot obsession:** judging on a single number instead of a trend.
- **Tool worship:** trusting the tool's threshold blindly — calibrate to the codebase.
- **Logs as decoration:** adding `Rails.logger.info` everywhere instead of structured, queryable events with correlation IDs.
- **Premature optimization:** profiling the wrong layer (app code) when the bottleneck is the DB / network / GC.
- **Ignoring reversibility:** picking the "best" option without weighing how cheap it is to undo.

## Quick Self-Check Before Responding

1. Did I cite a metric, a log line, or a profile? If no → go measure.
2. Did I name the trade-off and what would flip my recommendation?
3. Did I propose how to verify the change worked?

If any answer is no, the response isn't ready.
