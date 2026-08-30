---
name: identify-kpis
description: "Identify and define the primary KPI, leading measures, and operational guardrails for an AI agent. Use when a team needs measurable outcomes, wants to replace vanity metrics, cannot connect agent behavior to business impact, or needs an opinionated KPI contract with selectable scope."
---

# Identify KPIs

Build a measurement contract the team can actually operate. Be comprehensive
first and selective second.

## Interaction contract

Use `Discover -> Propose comprehensively -> Explain tradeoffs -> Select/restrict -> Approve -> Configure -> Verify -> Preserve deferred work`.

Do not configure a KPI, add instrumentation, or edit application code until the
user approves its exact definition and source.

## 1. Discover the business and evidence model

Call `virgo_get_workspace_context` with the current GitHub repository name.
Inspect the product workflow, buyer/user, job to be done, current metrics,
product analytics, trace identity, feedback, revenue/retention context, release
cadence, and decision the KPI should support. If no repository binding exists,
offer the hosted setup flow and obtain source-processing approval before calling
`virgo_begin_setup`. Ask for business context only when it cannot be derived.

## 2. Propose a complete KPI framework

Recommend:

- one primary product or business outcome;
- two to five leading agent-quality or task-completion measures;
- operational guardrails for latency, cost, errors, and safety;
- segmentation by workflow and meaningful customer cohort;
- a review cadence, owner, and decision threshold.

Every metric definition must state name, behavioral meaning, grain, numerator,
denominator, source, identity join, aggregation, window, baseline, direction,
target, exclusions, freshness, and known gaming risk. Distinguish an outcome
metric from a proxy and a diagnostic metric.

Show a recommended framework plus credible alternatives and their tradeoffs.
Favor the smallest set that changes decisions; reject attractive metrics that
the team cannot measure or act on.

## 3. Select and approve

Keep a ledger of accepted, reduced, deferred, and rejected metrics. If a metric
is valuable but unmeasurable, specify the missing trace, feedback, identity, or
outcome event and route that work to `$instrument-agent` or
`$capture-feedback`. Do not silently weaken the metric to fit available data.

Before configuration, restate the exact primary KPI contract and ask for
approval. Include supported KPI configuration in the exact setup plan and call
`virgo_approve_setup_plan`; do not invent an unsupported direct configuration
tool or force a lossy mapping.

## 4. Apply and verify usefulness

Use the host coding environment for approved repository changes. Record their
hash-pinned receipt with `virgo_record_setup_application`, then verify schema
and identity joins, one representative event, baseline availability,
freshness, segmentation, and dashboard/query behavior. Record that evidence
with `virgo_record_setup_verification`; neither receipt needs a second approval.
Mark the metric separately as configured, collecting, or verified. End with the
decisions this framework enables now and the deferred measurements for the next
iteration.
