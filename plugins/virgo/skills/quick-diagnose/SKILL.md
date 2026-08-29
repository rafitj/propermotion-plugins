---
name: quick-diagnose
description: "Assess whether Virgo has enough connected and verified evidence to diagnose an AI agent, using setup projection and connection metadata. Use for a read-only diagnostic-readiness check or evidence-gap map. This workflow does not itself expose trace records, feedback items, or incident contents."
---

# Quick Diagnose

Produce an honest diagnostic-readiness assessment before proposing setup work.
This workflow is read-only. Its Virgo MCP tools expose setup projection, setup
events, connection catalog, and connection status. They do not expose trace
records, feedback items, incident records, or their contents.

## 1. Establish evidence

1. Call `virgo_get_workspace_context` with the current GitHub repository name.
   If no binding exists, report that Virgo is not configured and offer the
   hosted setup flow; do not start setup from this read-only workflow.
2. Use the returned capabilities, sources, freshness, coverage, and blockers to
   learn which trace, product analytics, feedback, or company-context evidence
   Virgo can currently access.
3. Treat `configured`, `authorized`, `collecting`, and `verified` as distinct.
   A connected provider is not evidence that its underlying records are
   available through this MCP.
4. Incorporate incident, KPI, trace, or feedback facts only when the user
   supplies them directly or another explicitly available read-only tool
   returns them. Separate those facts from setup metadata and inference.

## 2. Map diagnostic coverage

For the user's goal, list the questions that can and cannot be answered from the
available metadata. Cover relevant dimensions such as correctness, task
completion, tools, retrieval, latency, cost, retries, safety, user friction,
release, cohort, identity, feedback, and business outcomes only as a coverage
check.

Rank missing evidence by the decisions it blocks. For each gap report:

- the diagnostic question it prevents Virgo from answering;
- the connection or instrumentation state that is actually visible;
- the minimum privacy-safe evidence needed;
- the smallest next workflow or user-provided fact that would close the gap.

Never manufacture agent defects, affected volume, cohorts, root causes, or
business impact from setup status or a provider name.

## 3. Deliver the readiness verdict

Lead with `ready to diagnose` or `not enough evidence`, naming exactly which
available facts support that verdict. Summarize verified sources and the three
highest-value evidence gaps. End with an explicit choice: provide the missing
incident/KPI facts, use an independently available evidence tool, or improve the
foundation with `$instrument-agent`, `$define-success`, or `$capture-feedback`.
Use `$fix-and-prove` only after the user supplies or selects an evidence-backed
finding.
