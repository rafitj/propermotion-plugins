---
name: fix-and-prove
description: "Turn a selected Virgo diagnosis into a scoped, verified code change and evidence-backed result. Use when the user chooses a finding to fix, wants a safe repair plan or PR, or needs proof that an agent change improved quality without regressing cost, latency, safety, or business KPIs."
---

# Fix And Prove

Fix one agreed problem and establish whether the fix worked. Avoid bundling unrelated cleanup.

## 1. Pin the claim and baseline

Call `virgo_get_workspace_context` with the current GitHub repository name and
inspect the selected finding, affected cohort, evidence, current release/code,
baseline window, target measure, and guardrails. State what is known, inferred,
and still unmeasurable. If the user has not selected a finding, use `$virgo:analyze`
to identify and scope one first.

Define the proof contract before editing:

- expected behavioral change;
- primary evaluation or production measure;
- latency, cost, error, and safety guardrails;
- minimum evidence/window and rollback trigger;
- what result would falsify the hypothesis.

## 2. Propose and approve the exact fix

Propose the smallest credible fix, its files/operations, tests, migration or rollout effects, and alternatives. Explain failure modes and scope boundaries. Let the user reduce or restrict it. Do not edit until the exact plan is approved.

Use the host coding environment to apply only the approved fix context and
record its hash-pinned candidate provenance through Virgo. Do not expand scope
during repair; obtain a new plan for new operations.

## 3. Verify at three levels

1. Static/local: formatting, lint, type checks, focused tests, and build as appropriate.
2. Behavioral: replay or evaluate representative failing and control cases without overfitting to one trace.
3. Production/business: compare the defined metric and guardrails for the affected cohort after rollout when enough evidence exists.

Treat local tests, offline evals, and production impact as different receipts. Never claim causal business improvement from a small correlation or an incomplete observation window.

## 4. Handoff safely

Show the diff and verification evidence. If the user asked for a PR, ask for or rely on explicit approval of the final remote action before pushing/opening it. The PR should state hypothesis, selected scope, baseline, tests, rollout/rollback, verification status, and deferred work.

Finish with one of: proved, disproved, inconclusive, or collecting. Include the next evidence checkpoint rather than declaring victory early.
