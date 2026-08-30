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
connectors, explain the current repository-processing boundary and get explicit
approval before calling `virgo_begin_setup`: Virgo pins the selected GitHub ref,
creates a read-only checkout, excludes credential-named paths and private keys,
lets Codex inspect the disclosed repository, and stores a source-anchored
evidence receipt. Git LFS, submodule, binary, and permission limitations remain
explicit. This grants no repository write authority.

Open only an exact signed GitHub authorization URL returned by Virgo. After the
callback, return to MCP and retry with the same idempotency key. Poll
`virgo_get_setup`; never hold one tool call open, run the Virgo CLI, start a local
MCP server, or operate the Virgo web UI as a fallback.

## Build the connector plan

Call `virgo_get_connector_setup` and keep these groups distinct:

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
the provider, purpose, authorization kind, whether resource selection is
required, and current state. The user's request for "all" approves the provider
set, but it never approves an account, channel, team, project, file, folder, or
secret on the user's behalf.

## Connect and scope each provider

For an approved OAuth item, call `virgo_start_connector_authorization` once
with its exact source or handoff-destination role. Open only the returned signed
URL without modification. When the host can choose the browser and an external
Google Chrome integration is available, prefer it; automatic browser launch
remains client-owned. Return to MCP after every callback and refresh
`virgo_get_connector_setup` before continuing.

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

Repository discovery and connector authorization are separate receipts. When
the durable scan produces a plan, show the complete file, runtime, identity,
instrumentation, connector, and verification scope. Call
`virgo_approve_setup_plan` only after the user approves that exact plan hash and
current projection ETag. The host coding environment owns approved repository
edits. Record application and focused verification through
`virgo_record_setup_application` and `virgo_record_setup_verification` using
their exact receipt hashes.

Finish by refreshing both `virgo_get_setup` and
`virgo_get_connector_setup`. Report each item as one of: connected and verified,
authorized but awaiting resource selection, authorization pending, secure
credential setup required, unavailable in this deployment, or failed with its
safe blocker. Never collapse configured, authorized, collecting, connected,
and verified into "set up."
