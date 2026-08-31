---
name: setup
description: "Initialize or resume a Virgo workspace and connect its available data sources and handoff destinations. Use when a user asks to set up Virgo, initialize a repository workspace, list or connect integrations such as Slack, Linear, Google Drive, HubSpot, or PostHog, or explain connector readiness."
---

# Setup

Run Virgo's hosted setup as one guided conversation. The outcome is an
initialized repository workspace plus a short, plain-language connector
receipt: what Virgo recommends, what is connected, the exact resources
selected, and what the user needs to do next. Do not dump the deployment
catalog or internal setup metadata into the conversation.

## Resolve the workspace

Determine the current GitHub `owner/repository` from the checked-out repository;
do not guess it. Start with `virgo_get_workspace_context`. If a binding exists,
use `virgo_get_connector_setup` to answer read-only questions about Slack,
Linear, or other integrations. Do not infer connector state from the workspace
context's `sources` list: it is evidence readiness, not the connector catalog.

If the workspace does not exist, or the user asked to initialize or change its
connectors, explain the current repository-processing boundary. Treat an explicit
user request to set up or initialize Virgo for the current repository as approval
for this initial read-only repository processing; do not ask for a second
confirmation. Then call `virgo_begin_setup`: Virgo pins the selected GitHub ref,
creates a read-only checkout, excludes credential-named paths and private keys,
lets Codex inspect the disclosed repository, and stores a source-anchored evidence
receipt. Git LFS, submodule, binary, and permission limitations remain explicit.
This grants no repository write authority. If the user did not explicitly request
setup or initialization, obtain approval after the disclosure before calling the
tool.

Open only an exact signed GitHub authorization URL returned by Virgo. After the
callback, return to MCP and retry with the same idempotency key. Poll
`virgo_get_setup`; never hold one tool call open, run the Virgo CLI, start a local
MCP server, or operate the Virgo web UI as a fallback. For a new or rescanned
workspace, wait for `plan_ready` and use the returned `setup_plan`; do not invent
a plan or ask the user to choose connectors before repository discovery finishes.

## Build the connector plan

For a completed repository scan, treat the `setup_plan` returned by
`virgo_get_setup` as internal safety and recommendation state. It is bound to
the pinned discovery receipt and live deployment catalog, but it is not a
user-facing checklist. Keep these groups distinct while reasoning:

- `suggested_connectors` were detected in the repository and are deployable;
- `detected_but_unavailable` were detected but cannot be connected here;
- `not_detected_connectors` were in the scan's supported connector set but were
  not found in the pinned repository;
- `other_available_connectors` are deployable but cannot be inferred from source;
- `existing_connectors` already have an active connection and must not be
  re-suggested;
- `detected_services_without_connector` were found but have no catalog connector.

Use `suggested_connectors` as the repository-backed recommendation set. Never
recommend a connector merely because it appears in `other_available_connectors`
or in the deployment catalog. Do not enumerate or proactively mention
`detected_but_unavailable`, `not_detected_connectors`, preview, coming-soon,
configuration-required, unhealthy, blocked, or unsupported connectors. If the
user explicitly asks about a named connector that cannot be connected, say only
that it cannot be connected right now; do not expand that into a catalog of
other unavailable products.

Repository source cannot reliably reveal human workflow tools such as chat,
document storage, or meeting notes. Before recommending one of those, require
one of these signals:

- the user explicitly named it;
- the current conversation or other user-authorized host context already shows
  that the user uses it; or
- the user answers a short question about where their relevant feedback or
  company context lives.

Do not infer usage from catalog availability, popularity, or a generic product
category. If host context is inconclusive, ask a simple open question such as
"Do you keep useful product feedback or company context outside this repo? If
so, where?" Do not present a long menu. Validate any user-named or context-backed
provider against `virgo_get_connector_setup` before offering to connect it.

If no connector was detected, say the repository scan is complete and ask the
non-code context question above; do not show the empty or rejected plan groups.
Never describe working-tree-only or untracked files as part of a scan of a
pinned GitHub commit.

For an existing workspace when the user only asked to inspect or change
connectors, call `virgo_get_connector_setup` and keep these catalog groups
distinct:

- source connectors provide traces, feedback, company context, or identity;
- handoff destinations receive an approved ticket, draft pull request, or
  message;
- current connections report authorization and verification state.

Interpret "all connectors" in a general setup request as all relevant
connectors supported by repository evidence, explicit user choices, or reliable
host context. Only enumerate every connectable catalog entry when the user
specifically asks to see the full available catalog. Even then, omit unavailable
and preview entries rather than announcing exclusions.

Present one compact recommendation grouped by source and destination. For each
item show only the provider, why it is relevant, what it would add, and the next
user action. Use friendly provider names and ordinary status language. In normal
setup conversation, do not show commit SHAs, receipt hashes, `plan_hash`,
`projection_etag`, run IDs, workspace refs, schema versions, raw status enums,
or idempotency keys. These values are internal tool arguments. Surface scan
limitations only when they materially weaken a recommendation or the user asks
for technical details.

An explicit request to connect named providers approves that provider set; do
not ask the user to approve it again. A general setup request does not approve
newly recommended providers: show the short human-readable list and ask once,
for example, "I found Langfuse in this repo. Do you want Virgo to connect its
traces?" Provider approval never chooses an account, channel, team, project,
file, folder, or secret on the user's behalf.

## Connect and scope each provider

For a scan-derived plan, call `virgo_approve_setup_plan` only after the user has
approved the visible, named repository-backed provider set. Pass the current
`plan_hash` and `projection_etag` returned by `virgo_get_setup` as internal
arguments; never ask the user to read, repeat, or approve those tokens. If the
plan changed, refresh it and explain only any user-visible recommendation change.
Pass only approved `suggested_connectors` in the tool's `providers` argument.
If that suggestion set is empty, record the plan with an empty `providers` list
without asking the user to approve internal metadata; this does not authorize
any catalog connector.
Handle an approved non-code connector separately through the connector setup
tools after the scan plan is recorded; do not pretend it was detected in source.
Approval does not start OAuth. Follow only the returned exact connector actions.
For an approved OAuth item, call
`virgo_start_connector_authorization` once with its exact source or
handoff-destination role. Open only the returned signed URL without modification.
When the host can choose the browser and an external Google Chrome integration
is available, prefer it; automatic browser launch remains client-owned. Return
to MCP after every callback and refresh `virgo_get_connector_setup` before
continuing.

If an authorized connection needs a bounded resource scope, call
`virgo_get_connector_resources`, show the safe names grouped by provider, and
ask the user to select the exact resources. Never default to every Slack
channel, Linear team, Drive file or folder, Sentry project, Notion page, or
other account resource. Call `virgo_verify_connector` only with those selected
IDs and approved non-secret settings.

Never ask the user to paste API keys, tokens, passwords, or credentials into
chat or MCP tool arguments. For an `api_key` or `one_time_credential` catalog
item, open only the exact `secure_configuration_url` returned by
`virgo_get_connector_setup` and ask the user to complete that provider's
first-party form. Do not operate the form or read its secret fields. Return to
MCP, refresh connector setup, and continue only after the connection record
confirms the new state. A managed destination follows the same secure handoff
unless an exact hosted MCP configuration capability exists.

For an approved KPI or account/user identity mapping, prefer the exact hosted
`virgo_configure_setup_primary_kpi` and
`virgo_configure_setup_identity_mapping` tools when available. These calls
persist non-secret configuration only. They do not authorize a connector and do
not prove that observations, a baseline, or an identity join exists; refresh
`virgo_get_setup` and verify real evidence afterward.

## Finish repository setup

Repository discovery and connector authorization are separate receipts. The
durable scan produces evidence; `virgo_get_setup` combines that evidence with
the deployment catalog into the internally versioned connector plan described
above. The user's approval is for the plain-language provider list; Virgo's
current plan hash and projection ETag preserve that choice safely behind the
scenes. The host coding environment owns any separately approved repository
edits. Record application and focused verification through
`virgo_record_setup_application` and `virgo_record_setup_verification` using
their exact receipt hashes.

Finish by refreshing both `virgo_get_setup` and
`virgo_get_connector_setup`. Report each item as one of: connected and verified,
authorized but awaiting resource selection, authorization pending, secure
credential setup required, or failed with a safe, useful next step. Do not add a
section for unavailable or preview connectors. Never collapse configured,
authorized, collecting, connected, and verified into "set up."
