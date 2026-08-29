---
name: capture-feedback
description: "Collaboratively design and implement explicit and implicit feedback capture for an AI agent. Use when user feedback is missing, poorly linked to traces and outcomes, trapped in support channels, or may require optional application UX changes and an opinionated implementation PR."
---

# Capture Feedback

Design a feedback system, not merely a thumbs-up button. Propose the full useful system, then let the user reduce it.

## Interaction contract

Use `Discover -> Propose comprehensively -> Explain tradeoffs -> Select/restrict -> Approve -> Connect/apply -> Verify -> Preserve deferred work`.

Do not authorize providers, modify application UX, create ingestion credentials, or open a PR before the user approves those exact actions.

## 1. Discover existing feedback

Call `virgo_get_workspace_context` with the current GitHub repository name.
Inspect existing in-product controls, retries/abandonment, support tickets,
Slack, PostHog surveys/events, CRM notes, human review, incident handling,
user/account identity, and trace correlation. If no binding exists, offer the
hosted setup flow and obtain source-processing approval before calling
`virgo_begin_setup`. Reuse connected sources and existing UI patterns.

## 2. Propose the comprehensive system

Cover the relevant layers:

- explicit in-product rating or issue reporting at the right workflow moment;
- optional structured reason codes plus free text when justified;
- implicit signals such as retry, edit, regeneration, abandonment, escalation, and task completion;
- team/support feedback from approved sources;
- trace, response, release, user/account, and workflow correlation;
- sampling, deduplication, abuse controls, privacy, retention, and redaction;
- acknowledgement, routing, owner, and close-the-loop behavior;
- verification events, dashboards, and tests.

Separate three implementation options clearly: no application UI change, minimal UI change, and comprehensive UX. For each, show user burden, coverage, bias, engineering cost, privacy risk, and the decisions it enables. Recommend an option but preserve all three.

## 3. Select and approve

Track every component as accepted, reduced, deferred, or rejected. Ask whether application changes are desired; never assume a new UI control is acceptable. Before acting, restate the exact event schema, identity link, providers, files/components, and verification plan.

After approval, include the selected providers in
`virgo_approve_setup_plan`; its result returns their authorization handoffs.
Use `virgo_start_setup_authorization` only to retry one already-approved
handoff. Use the host coding environment for repository or application changes.
Use existing component-library primitives and accessibility patterns for
approved UI work.

## 4. Verify the loop

After applying the approved changes, call `virgo_record_setup_application` with
their hash-pinned receipt. Submit a controlled representative feedback event
and prove ingestion, trace correlation, identity handling, deduplication,
privacy filtering, and downstream visibility, then call
`virgo_record_setup_verification` with that receipt. These records do not need a
second approval. Distinguish connected from collecting and verified. Create a
PR only after explicit approval of the remote action, and carry the decision
ledger plus deferred improvements into the handoff.
