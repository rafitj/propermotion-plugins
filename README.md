# Proper Motion plugins

Install Virgo once to add Proper Motion's hosted MCP connection, guided
workspace setup, and agent-improvement workflows to Codex or Claude Code.

Copy this prompt into either client to set up this repository and verify its
Agent traces:

> Set up Virgo for this repository end to end. Open
> https://github.com/rafitj/propermotion-plugins/blob/main/GETTING_STARTED.md
> and follow exactly one branch for the AI client running this session.
> Complete the install or update yourself; do not ask me to run commands
> or invoke a skill. Reuse an existing Virgo login and authenticate only
> when the client reports that Virgo is not authorized. If this client
> must load the plugin in a fresh task, create that task using this same
> checkout and branch and continue setup there automatically; do not
> create a worktree. Resolve the GitHub repository from the current
> branch's configured upstream or tracking remote before falling back to a
> remote named origin; never substitute a different fork. Initialize or
> resume this repository's Virgo workspace. Reuse a completed current
> repository-discovery receipt and rescan only when Virgo says it is
> missing or stale. If relevant connector accounts and their exact
> resources are already connected and verified, reuse them without asking
> me to reconfirm. On that replay path, do not ask about optional external
> product or company context; continue the instrumentation below unless a
> newly relevant account or resource scope is required for a capability I
> requested. Otherwise, pause and ask me one concise question about only
> the connectors supported by repository evidence, tools I explicitly use,
> or where I keep relevant product feedback and company context. Do not
> enumerate the catalog. After I answer, complete the approved connector
> authorization and exact resource selection, verify the resulting
> workspace and connections, and report their real states without exposing
> internal IDs, hashes, tokens, or credentials. Continue in this same task
> with Agent tracing: capture full prompts, responses, and tool inputs and
> outputs, available model usage, real outcomes, and parent/child
> relationships for the application's primary agent path. System, Product,
> and Metrics remain off unless I selected them. Reuse existing valid
> tracing and exporters where they already provide that coverage.
> Otherwise call virgo_create_observe_setup_prompt with Agent-only
> coverage and execute its whole returned prompt here immediately; do not
> ask me to paste it again. This request approves those bounded
> instrumentation edits, the required SDK dependency, and one controlled
> real application run. Preserve unrelated edits and existing telemetry;
> repeating setup must not duplicate wrappers or rotate a healthy saved
> credential. Use the current package manager and the recipe's supported
> version without upgrading application frameworks. Aim for the first
> verified trace in about five minutes when dependencies and model
> credentials are available. Use an existing representative scenario from
> the primary workflow with real model calls; for benchmarks choose a
> substantive domain, not a mock or toy domain when realistic scenarios
> exist. Verify full content and the model, tool, and retrieval children
> actually exercised by the same real verification run using Virgo's trace
> evidence tools with purpose="trace_verification". Every tool called must
> have its own child span with full inputs and outputs; tool-call text
> inside a model span alone is insufficient. Never fabricate traces,
> feedback, outcomes, or failures, and do not use a historical trace or
> HTTP acceptance as proof. If the real application cannot run, finish the
> instrumentation and report the exact blocker. Qualifying repeated
> new-failure signals must become Insights automatically; do not ask me to
> approve their Match proposals. Continue eligible Insights through
> Virgo's canonical investigation and code-draft flow so configured Linear
> tickets and GitHub draft PRs are created automatically, with Slack
> messages sent to verified notification destinations. Report actual
> delivery status and links from Virgo; distinguish queued analysis or
> delivery from completed artifacts, and do not invent an Insight when the
> run has no qualifying repeated failure. Ask about Match membership only
> when Virgo leaves an ambiguous attachment to an existing Insight pending
> review.

See [GETTING_STARTED.md](./GETTING_STARTED.md) for the complete product-specific
installation and verification contract.
