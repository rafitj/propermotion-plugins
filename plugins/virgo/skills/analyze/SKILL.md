---
name: analyze
description: "Analyze a specific production question through Virgo, including trace or failure patterns, root cause, affected customers or accounts, ARR or business impact, churn, a selected Insight, a trace cohort, or validation evidence. Use when the user asks what happened, why it happened, who was affected, how large the impact was, or what evidence Virgo has."
---

# Analyze

Answer the user's requested production question through Virgo's server-owned
analytical chain. Run the smallest canonical analysis that answers the request;
do not force the full pattern-to-fix workflow when the user asked for one
pattern, impact slice, customer cohort, cause, or validation result.

Virgo owns pattern analysis, root-cause Investigations, identity and impact
joins, proposed-fix context, and validation receipts. Use MCP as the control and
progressive-evidence interface. Do not turn the host agent into a parallel
analyst over raw traces.

## Establish the exact question and readiness

Call `virgo_get_workspace_context` for the repository binding and
`virgo_get_connector_setup` when connector state matters. Treat capability,
connection, coverage, and freshness fields only as readiness context. They
cannot establish a defect, cause, affected customer, ARR risk, or fix.

Preserve any scope the user supplied: workflow, release, environment, time
window, trace IDs, account/customer reference, Insight, Investigation, or
comparison window. Ask only when an ambiguity would materially change the
analysis. If Virgo lacks enough verified evidence, lead with the exact gap and
the decision it blocks; do not manufacture a finding from setup metadata.

If the repository is not configured, explain Virgo's current source-processing
disclosure once and obtain explicit approval before calling
`virgo_begin_setup`. Open only exact authorization or secure-configuration URLs
returned by Virgo, return to MCP after the user completes them, and poll
`virgo_get_setup`. Never start a local MCP server, invoke the Virgo CLI, or use
browser control as a substitute for a hosted capability.

When the user supplies an exact trace cohort, forward its IDs unchanged.
Omitted IDs mean the authorized source and time scope; an empty or invalid list
must fail rather than silently widening the query.

## Route the requested analysis

- For a named behavior, feature, workflow, error, or trace cohort, run
  `trace_patterns` under that exact scope. Return only patterns supported by the
  canonical receipt.
- For "why" or root-cause questions, use an accepted canonical Insight and its
  Investigation. Treat an unreviewed Match proposal as provisional until the
  user explicitly accepts or rejects its exact membership.
- For affected customers or accounts, run `customer_impact` for the relevant
  Insight or scope. Preserve eligible, linked, affected, confirmed, estimated,
  and unmapped counts instead of flattening them into one total.
- For ARR, revenue, retention, or business impact, use the server-owned impact
  join. Preserve currency, source, as-of time, identity coverage, and the
  distinction between exposure and predicted churn.
- For a specific customer or active churn movement, run `churn_diagnosis` with
  the exact account and explicit current/baseline windows.
- For a proposed or shipped change, read the immutable fix context and Virgo
  validation receipt. Keep local tests, replay/canary evidence, and production
  proof distinct.

Reuse a fresh canonical analysis, Insight, Investigation, or validation receipt
when its scope already answers the question. If a trace source is stale,
synchronize only the needed time window and poll its durable status. Use bounded
trace search and individual trace reads only to explain representative evidence
or cited code paths; never dump a trace corpus into context.

## Preserve evidence meaning

Lead with the direct answer to the user's question, then state:

- scope, time range, environment, and data freshness;
- method version and finding state (`observed`, `correlated`, `inferred`, or
  `causally_verified`);
- confidence, coverage, evidence references, and material limitations;
- canonical Insight and Investigation revision when present;
- what the receipt proves and what remains unknown.

Do not upgrade correlation to causality, partial identity coverage to a precise
financial claim, or a connected provider to verified evidence.

## Govern membership, fixes, and delivery

Do not accept Match membership automatically. Read the exact proposal version,
restate its evidence and consequence, obtain the user's explicit decision, and
then use `virgo_review_match_proposal`. If state changed, review the new version
again.

If the user chooses a finding to repair, continue with `$fix-and-prove`. When
the user asks for a Linear, Slack, or GitHub artifact, prepare the exact preview
without delivering it. Show its destination, content, and preview hash; deliver
only after explicit approval of that unchanged preview.
