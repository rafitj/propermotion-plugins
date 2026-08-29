---
name: instrument-agent
description: "Collaboratively design and implement comprehensive observability for an AI agent. Use when traces are missing or shallow, identity and outcomes cannot be joined, instrumentation needs improvement, or the user wants an opinionated instrumentation PR with control over every edit."
---

# Instrument Agent

Treat instrumentation as an iterative product and engineering decision, not a one-shot patch.

## Interaction contract

Follow this sequence exactly:

`Discover -> Propose comprehensively -> Explain tradeoffs -> Select/restrict -> Approve -> Apply -> Verify -> Preserve deferred work`

Do not edit code, install packages, provision credentials, or open a PR before the user approves the exact scope. An approval of the overall goal is not approval of every proposed edit.

## 1. Discover

Call `virgo_get_workspace_context` with the current GitHub repository name. If
the repository is not configured, disclose source processing once, obtain
explicit approval, call `virgo_begin_setup`, and poll `virgo_get_setup`. Open
only exact authorization URLs returned by Virgo. Inspect the agent entry points,
framework, model/provider calls, tools, retrieval, queues, existing telemetry,
runtime/deployment configuration, identity model, privacy constraints, and test
commands. Reuse existing OpenTelemetry and supported provider instrumentation
before introducing dependencies.

## 2. Propose the comprehensive target

Propose an opinionated end state even if it is broader than the likely first iteration. Cover where relevant:

- one trace per meaningful agent workflow with stable operation names;
- model, tool, retrieval, guardrail, queue, and external-call spans;
- trace propagation across HTTP, async work, jobs, and retries;
- stable user/account/session/workflow identifiers without raw sensitive values;
- provider, model, token, cost, latency, error, retry, and outcome attributes;
- exception status at the handling boundary and correlated structured logs;
- metadata-only content capture by default, sampling, retention, and redaction;
- local/in-memory exporters in tests and fail-open production behavior;
- a real trace verification path and focused regression tests.

For every proposed change show: files or subsystem, reason, data captured, privacy/operational risk, expected diagnostic or KPI value, and verification. Identify optional application behavior changes separately from backend-only instrumentation.

## 3. Let the user shape scope

Maintain a decision ledger with `accept`, `reduce`, `defer`, and `reject`. Offer a recommended first slice, but keep the comprehensive proposal visible. Ask only about consequential ambiguities such as content capture, identity grain, sampling/retention, runtime ownership, and app UX. Never treat silence as approval.

## 4. Apply and verify

After approval, call `virgo_approve_setup_plan` with the exact plan hash and
current projection ETag. Use the host coding environment for repository
mutation and keep edits within the selected slice. Run formatting, lint, type
checks, focused tests, and a real or controlled trace check. Record the applied
plan and verification hashes with `virgo_record_setup_application` and
`virgo_record_setup_verification`; do not ask for another approval because both
receipts are within the already-approved plan. Report configured, collecting,
and verified as distinct states.

Create a PR only when the user asked for one and approved the final remote action. Include the decision ledger, verification receipts, limitations, and deferred items in the handoff so the next iteration can resume without rediscovery.
