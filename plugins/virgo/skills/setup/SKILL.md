---
name: setup
description: "Initialize or resume a Virgo workspace and connect its available data sources and handoff destinations. Use when a user asks to set up Virgo, initialize a repository workspace, list or connect integrations such as Slack, Linear, Google Drive, HubSpot, or PostHog, or explain connector readiness."
---

# Setup

Run Virgo's hosted setup as one guided conversation. The outcome is an
initialized repository workspace plus an honest connector receipt: what is
available, what is connected, the exact resources selected, what still needs
user action, and what is unavailable in this deployment.

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
`virgo_get_setup` as the canonical connector proposal. It is deterministically
bound to the pinned discovery receipt and the live deployment catalog. Present
the pinned repository, commit, tracked-file count, and scan limitations, then
keep these plan groups distinct:

- `suggested_connectors` were detected in the repository and are deployable;
- `detected_but_unavailable` were detected but cannot be connected here;
- `not_detected_connectors` were in the scan's supported connector set but were
  not found in the pinned repository;
- `other_available_connectors` are deployable but cannot be inferred from source;
- `existing_connectors` already have an active connection and must not be
  re-suggested;
- `detected_services_without_connector` were found but have no catalog connector.

If no connector was detected, still present the complete plan and its
`not_detected_connectors`; an empty suggestion set is a reviewable result, not a
missing capability. Never describe working-tree-only or untracked files as part
of a scan of a pinned GitHub commit.

For an existing workspace when the user only asked to inspect or change
connectors, call `virgo_get_connector_setup` and keep these catalog groups
distinct:

- source connectors provide traces, feedback, company context, or identity;
- handoff destinations receive an approved ticket, draft pull request, or
  message;
- current connections report authorization and verification state.

Interpret "all connectors" as every catalog entry whose
`product_availability` is `available` and whose `deployment_state` is
`configured`, plus only configured handoff destinations. Exclude preview,
coming-soon, configuration-required, unhealthy, blocked, and already-connected
duplicates. State those exclusions plainly rather than silently shrinking the
request.

Present one compact plan grouped by source and destination. For each item show
the provider, why it was or was not suggested, purpose, authorization kind,
whether resource selection is required, and current state. The user's request
for "all" approves the provider set, but it never approves an account, channel,
team, project, file, folder, or secret on the user's behalf.

## Connect and scope each provider

For a scan-derived plan, ask the user to approve the exact plan hash and optionally
restrict the suggested provider set. Call `virgo_approve_setup_plan` with that
exact subset. Approval does not start OAuth. Follow only the returned exact
connector actions. For an approved OAuth item, call
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

## Finish repository setup

Repository discovery and connector authorization are separate receipts. The
durable scan produces evidence; `virgo_get_setup` combines that evidence with
the deployment catalog into the hash-bound connector plan described above. Call
`virgo_approve_setup_plan` only after the user approves that exact plan hash,
current projection ETag, and suggested-provider subset. The host coding
environment owns any separately approved repository edits. Record application
and focused verification through
`virgo_record_setup_application` and `virgo_record_setup_verification` using
their exact receipt hashes.

Finish by refreshing both `virgo_get_setup` and
`virgo_get_connector_setup`. Report each item as one of: connected and verified,
authorized but awaiting resource selection, authorization pending, secure
credential setup required, unavailable in this deployment, or failed with its
safe blocker. Never collapse configured, authorized, collecting, connected,
and verified into "set up."
