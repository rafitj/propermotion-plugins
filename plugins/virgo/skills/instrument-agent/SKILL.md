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
explicitly selected, the request to instrument this repository or pasting that
prompt is approval for its bounded host edits, dependency changes, and one real
verification. Execute the entire returned prompt in this same task when already
working in the target repository; do not ask the user to paste it again. Apply
that scope without a second proposal or approval checkpoint. For all other
instrumentation requests,
review and obtain approval for concrete host edits before applying them. Never
pass `recipe_hash` to `virgo_approve_setup_plan` or invent a setup run to record
application receipts. Use the canonical plan/application tools below only when
an actual scan-backed setup plan exists and those edits are within its scope.

The generated Observe prompt is the fast path and overrides the general phases
below. Run its bootstrap command immediately, before discovery or edits. It
exchanges the short-lived one-use grant and writes the runtime key to the
repository's ignored, owner-readable `.virgo/observe.env`; never read, print,
or restate that file or its value. A new prompt reuses a valid saved credential
while applying its selected coverage; repeating the exact completed prompt is a
local no-op. Let the bootstrap command validate the saved configuration. If the
server is configured but the saved key is missing, corrupt, or inactive, the
approved bootstrap may replace the runtime key; other deployments using the
prior key must then update it.

Do not call `virgo_get_workspace_context`, `virgo_begin_setup`,
`virgo_get_setup`, `virgo_approve_setup_plan`,
`virgo_record_setup_application`, or `virgo_record_setup_verification` for this
workspace-bound Observe recipe. Inspect only local application startup, the
primary agent run, model calls, and existing tool/retrieval dispatch seams. Do
not inspect the installed SDK source: `register(...)` returns the client, and
`agent_run`, `span`, `current_trace_ref`, `feedback`, and `metric` are methods
on that client.

For uv installs of the recipe's pinned SDK version, include
`--refresh-package virgo-observe` so a stale package index cannot hide a newly
published release. Refresh only this package; preserve application dependencies.

Optimize for one verified Agent trace before optional depth. Do not add or run
tests, a full suite, all-extras resolution, or a compatibility matrix during
customer setup. Use only a fast syntax/import check and one bounded real agent
execution. Use an existing representative primary workflow; when a benchmark
offers substantive domains, a mock or toy domain cannot verify that workflow.
Instrument selected optional coverage only at existing boundaries;
do not create a new schema, outbox, workflow, or feedback UI during initial
setup. Register before importing framework/provider modules, including modules
that bind a provider callable with imports such as `from litellm import
completion`; registering before the first call is too late for those aliases.
Use a first-import bootstrap module when needed. Keep a required side-effect
import lint-valid by deliberately re-exporting the module or using the project's
documented narrow suppression; do not leave an unused alias or insert executable
statements between import groups.
When Observe configuration is absent, disable instrumentation and preserve normal
application imports, CLI help, and runs. Guard registration and all client uses;
do not make importing the application require an Observe key.
Keep the root `agent_run` open through the application's real outcome
evaluation, attach any available stable success, reward, and termination fields
before it closes, and never invent an outcome. Root input/output must contain
the actual user-visible request and terminal response, never serialized benchmark
tasks, hidden evaluation criteria, expected actions, or answer keys. When the
application's terminal evaluator explicitly declares the whole agent task
unsuccessful, mark the root span ERROR with a stable `task_outcome_not_achieved`
reason. Preserve the application's return value and successful child statuses;
do not synthesize exceptions or feedback, infer arbitrary score thresholds, or
claim business success for an unknown outcome. Use `client.span(name, kind="TOOL")`
or `client.get_tracer(name)` for manual children so they share Virgo's provider.
Instrument the agent's runtime tool dispatch, not a shared helper also invoked
by benchmark evaluation. Keep the root open to record the evaluated outcome,
but use `openinference.instrumentation.suppress_tracing()` around grading and
keep manual tool spans outside that path. Evaluator replays and hidden grader
prompts must not appear as agent actions. Match tool spans to actual conversation
call IDs and check for replay duplicates.
Virgo does not replace the application's global tracer provider; an unconfigured
`opentelemetry.trace.get_tracer()` may silently produce no child spans.
When reusing `trace_export="existing"` with Product or Metrics, configure the
provider's Resource before creating it: set
`platform.observe.environment_contract="observe-runtime-environment-v1"` and
`deployment.environment.name` to the registered environment. Registration checks
this contract so children exported before the root retain the same correlation
scope. Do not mutate a live provider or add an exporter just to bypass the check.
Report a selected category as
deferred when it has no existing safe boundary. Then call
`virgo_get_observe_status` for Agent delivery in the selected workspace and
report each optional category separately. Require the model, tool, and retrieval
children actually exercised by the same verification run. Each invoked tool must
have its own child span with full inputs and outputs; a model's tool-call text
alone is insufficient. A root-only Agent trace is incomplete
and must trigger an import-order/instrumentor repair. Record the verification's
UTC start time and opaque run_ref before executing. Use `virgo_search_traces`
with that start time as `since`, then `virgo_get_trace`, both with
`purpose="trace_verification"`, to check full content and children for that same
run_ref. An accepted export or old trace is not proof of the new changes.
Verify that the normalized environment and commit match the actual execution;
registration arguments alone do not prove stored provenance.
For selected Product and Metrics, retain `receipt.status.delivery_id` after
flushing. Pass the verification `since`, fresh canonical `trace_id`, and exact
`feedback_delivery_id` or `metric_delivery_id` to `virgo_get_observe_status`.
Its channel summaries distinguish receipt, canonical processing, and correlation;
require `correlated` with `matches_trace=True` when the application supplied an
execution link. An older tool signature may compare latest receipt identity and
time, but cannot prove that link. These states do not prove KPI aggregation or
customer benefit.

When Metrics is selected for a benchmark, its documented evaluator reward or
task-completion result is an existing metric boundary. Wire that measure using
its actual name, unit, grain, and version; a revenue contract, customer-account
model, or pre-existing Virgo metric definition is not required. Preserve the
measure's semantics and never invent a score or business result.

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
