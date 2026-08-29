---
name: propermotion
description: "Use Virgo's canonical production-analysis workflow for trace patterns, root cause, customer or ARR impact, churn diagnosis, proposed fixes, validation, and governed Linear/GitHub handoff. Applies when a user asks to analyze traces, explain a failure or churn movement, identify impact, or create and prove a fix."
---

# Proper Motion

Route the request through Virgo's server-owned analytical chain. Virgo owns
pattern analysis, root-cause Investigations, impact joins, proposed-fix context,
and validation receipts. Use MCP as the control and progressive-evidence
Interface; do not turn the host agent into a parallel analyst over raw traces.

## Establish scope and readiness

Call `virgo_get_workspace_context` when the repository already has a workspace
binding. Treat its capability, connection, coverage, and freshness fields only
as readiness context. They cannot establish a defect, root cause, affected
cohort, ARR risk, or fix.

If the repository is not configured, explain Virgo's current source-processing
disclosure once and obtain explicit approval. Then call `virgo_begin_setup`.
Open only an exact signed authorization URL returned by Virgo, return to MCP
after its callback, and retry with the same idempotency key. Poll long-running
work with `virgo_get_setup`. When the host can choose the browser and an external
Google Chrome integration is available, prefer Chrome to the in-app browser;
never modify the signed authorization URL to do so. Never wait inside a tool
call, start a local MCP server, invoke the Virgo CLI, or access a local
credential store as a fallback.

When the scan is complete, present one comprehensive setup plan. After the user
approves that exact plan, call `virgo_approve_setup_plan` with its hash and the
current projection ETag. Repository edits remain owned by the host coding
environment. After applying and checking that plan, record its hash-pinned
receipts through `virgo_record_setup_application` and
`virgo_record_setup_verification` without another approval stop. Never silently
request external-action approval; each exact delivery preview still needs its
own human decision.

Reuse a fresh canonical analysis, Insight, or Investigation when its scope
answers the request. If an authorized trace source is stale, synchronize only
the needed time window and wait for its durable status. Ask the user only about
a scope or authority choice that materially changes the answer, such as an
ambiguous environment or Insight.

When the user supplies an exact application trace cohort, forward its IDs in
`trace_ids` unchanged. Each sync admits one `connection_ids` entry. Omitted IDs
mean the authorized source/time scope; an empty or invalid list must fail, not
widen to all traces. Follow the tool's bounded ID-input schema; this does not
limit the number of traces Virgo can acquire from an unfiltered scope.

## Route to the smallest deep workflow

- For feature or failure patterns, start `trace_patterns` analysis and read its
  result.
- For affected customers or ARR, start `customer_impact` analysis under the
  relevant scope or Insight. Do not calculate financial totals from trace
  excerpts.
- For an active churn movement, start `churn_diagnosis` with the explicit
  current and baseline windows.
- For root cause, use an accepted canonical Insight, then start or read its
  canonical Investigation. A Match proposal may be discussed as provisional
  evidence, but the user must explicitly accept the exact membership before an
  Investigation is opened.
- For a requested fix, read Virgo's fix context for the immutable Investigation
  revision before changing code.

Poll the workflow's status when it is asynchronous. Use bounded trace search
and individual trace reads only to explain representative evidence or to work
against cited code paths. Never dump a trace corpus into context or invent a
diagnosis that is absent from Virgo's canonical analysis or Investigation.

## Execute and validate repository work

When the user asks for a fix, use the host coding environment to inspect, edit,
and test the repository. MCP has no repository write authority. Follow Virgo's
proposed-fix context, add a focused regression test, and keep the change scoped
to the selected canonical finding.

After local verification, record only the repository snapshot, base commit,
diff hash, test receipt hash, and Investigation revision through
`virgo_record_fix_candidate`. Start the canonical Virgo validation workflow and
read its receipt. Keep local tests, replay/canary evidence, and production proof
distinct; report production proof as collecting when it is not yet established.

## Preserve evidence meaning

Lead with the answer, then state:

- scope, time range, environment, and data freshness;
- method version and finding state (`observed`, `correlated`, `inferred`, or
  `causally_verified`);
- confidence, coverage, evidence references, and material limitations;
- canonical Insight and Investigation revision when present;
- what changed and what did not.

For business impact, preserve eligible, linked, affected, confirmed, estimated,
and unmapped counts; ARR source, currency, and as-of time; and the distinction
between exposure and predicted churn. Do not upgrade a correlation to causality
or partial identity coverage to a precise financial claim.

## Govern membership and external delivery

Do not accept Match membership automatically. Read `virgo_get_match_proposal`
for the exact `proposal_version_id`, restate its evidence and consequence, and
obtain the user's explicit approve or reject choice. Then use
`virgo_review_match_proposal` with `confirmation: user_confirmed_match_review`
and a stable idempotency key. `create_new` creates the first canonical Insight;
`attach_existing` also requires the reviewed `expected_insight_version`.
Use the returned canonical Insight for the Investigation. If state changed,
read and review it again; never substitute a new version under old approval.
Read the same exact proposal version to check accepted/rejected status after it
leaves the pending list. This requires `insights:write`, not external-action
approval authority.

When the user asks for a Linear or GitHub artifact, prepare the exact preview
without delivering it. Show the destination, artifact kind, title/body, and
preview hash. Call `virgo_approve_external_action` only after the user explicitly
approves that unchanged preview; if its hash changes, show the new preview and
ask again. Never request or return credentials, raw provider cursors, or
unfiltered provider payloads.
