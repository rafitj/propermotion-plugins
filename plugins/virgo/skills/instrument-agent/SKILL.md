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

## Selected Virgo Observe coverage

When the user chooses `virgo-observe`, use
`virgo_create_observe_setup_prompt` for the workspace's shared, versioned
recipe. Agent is always included; System,
Product (feedback and correlation), and Metrics (business observations) are
independent and off unless selected. Metrics does not imply Product. For this
path, propose only the selected coverage rather than the comprehensive target
below. This path captures full content and uses one Observe API key across
selected channels. Do not switch it to metadata-only or request separate
feedback/metrics keys or an environment choice. Keep the SDK optional for
other instrumentation requests.

When Virgo returns a single-use Observe bootstrap prompt for coverage the user
explicitly selected, pasting that prompt is approval for its bounded host edits,
dependency changes, and one real verification. Apply that scope without a
second proposal or approval checkpoint. For all other instrumentation requests,
review and obtain approval for concrete host edits before applying them. Never
pass `recipe_hash` to `virgo_approve_setup_plan` or invent a setup run to record
application receipts. Use the canonical plan/application tools below only when
an actual scan-backed setup plan exists and those edits are within its scope.

The generated Observe prompt is the fast path and overrides the general phases
below. Run its bootstrap command immediately, before discovery or edits. It
exchanges the short-lived one-use grant and writes the runtime key to the
repository's ignored, owner-readable `.virgo/observe.env`; never read, print,
or restate that file or its value. An existing local file is preserved. If the
server is configured but the file is missing, the approved bootstrap replaces
the active runtime key and warns that other deployments must update it.

Do not call `virgo_get_workspace_context`, `virgo_begin_setup`,
`virgo_get_setup`, `virgo_approve_setup_plan`,
`virgo_record_setup_application`, or `virgo_record_setup_verification` for this
workspace-bound Observe recipe. Inspect only local application startup, the
primary agent run, model calls, and existing tool/retrieval dispatch seams. Do
not inspect the installed SDK source: `register(...)` returns the client, and
`agent_run`, `span`, `current_trace_ref`, `feedback`, and `metric` are methods
on that client.

Optimize for one verified Agent trace before optional depth. Do not add or run
tests, a full suite, all-extras resolution, or a compatibility matrix during
customer setup. Use only a fast syntax/import check and one bounded real agent
execution. Instrument selected optional coverage only at existing boundaries;
do not create a new schema, outbox, workflow, or feedback UI during initial
setup. Keep the root `agent_run` open through the application's real outcome
evaluation, attach any available stable success, reward, and termination fields
before it closes, and never invent an outcome. Report a selected category as
deferred when it has no existing safe boundary. Then call
`virgo_get_observe_status` for Agent delivery in the selected workspace and
report each optional category separately. An accepted export or old trace is
not proof of the new changes.

For a generated Observe prompt, stop here; the general workflow below does not
apply.

## 1. Discover

For instrumentation requests other than a generated Observe prompt, call
`virgo_get_workspace_context` with the current GitHub repository name. If
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
- full-content capture for Virgo Observe, with explicit privacy constraints,
  sampling, retention, and credential exclusion;
- local/in-memory exporters in tests and fail-open production behavior;
- a real trace verification path and focused regression tests.

For every proposed change show: files or subsystem, reason, data captured, privacy/operational risk, expected diagnostic or KPI value, and verification. Identify optional application behavior changes separately from backend-only instrumentation.

## 3. Let the user shape scope

Maintain a decision ledger with `accept`, `reduce`, `defer`, and `reject`. Offer a recommended first slice, but keep the comprehensive proposal visible. Ask only about consequential ambiguities such as content capture, identity grain, sampling/retention, runtime ownership, and app UX. Never treat silence as approval.

## 4. Apply and verify

For a non-Observe scan-backed setup plan, after approval call
`virgo_approve_setup_plan` with the exact plan hash and
current projection ETag. Use the host coding environment for repository
mutation and keep edits within the selected slice. Run formatting, lint, type
checks, focused tests, and a real or controlled trace check. Record the applied
plan and verification hashes with `virgo_record_setup_application` and
`virgo_record_setup_verification`; do not ask for another approval because both
receipts are within the already-approved plan. Report configured, collecting,
and verified as distinct states.

Create a PR only when the user asked for one and approved the final remote action. Include the decision ledger, verification receipts, limitations, and deferred items in the handoff so the next iteration can resume without rediscovery.
