# Virgo plugin

Virgo adds one hosted OAuth MCP server, a guided workspace-and-connector setup
workflow, Virgo's question-focused production analysis, and four collaborative
agent-improvement workflows to Codex or Claude Code. The
installable package is published from the public
[`rafitj/propermotion-plugins`](https://github.com/rafitj/propermotion-plugins)
marketplace; its one-prompt setup guide installs or updates the plugin,
authenticates the bundled MCP, and verifies the connection.

The copied setup prompt also continues Agent instrumentation in the current
coding task, reuses existing valid tracing, and verifies fresh full-content trace
evidence from one real run. Optional System, Product, and Metrics coverage stays
off unless selected. Repeated setup preserves a current discovery receipt and
healthy connector scopes instead of restarting onboarding.

Setup and product tools use the hosted Streamable HTTP endpoint at
`https://api.propermotion.ai/mcp`. AuthKit supplies standards-based OAuth; no
Virgo CLI process or macOS Keychain access is involved. Hosted setup resolves
the repository workspace, queues the canonical durable repository scan,
presents a short relevance-based connector plan for approval, and returns
signed provider authorization handoffs. Internal plan tokens stay behind the
scenes. After host-owned changes, it records hash-pinned application and
verification receipts. It never implements a second setup or analysis state
machine or receives repository write authority.
When the host can choose where to open a returned authorization URL, Virgo asks
it to prefer an available external Google Chrome integration over the in-app
browser without changing the signed URL. The MCP client's automatic OAuth
launcher remains client-owned.
Virgo owns pattern, root-cause, impact, proposed-fix, and validation truth. The
host coding agent owns conversation and authorized repository execution.

The normal setup journey is `Connect -> Scan -> Review relevant sources ->
Authorize exact resources -> Verify -> Ready`. Do not launch `virgo mcp`, use
`virgo_local`, or ask the user to manage a local authentication lock as a
fallback for a hosted failure.

## Workflows

- Setup — initialize or resume a repository workspace, recommend only providers
  supported by repository evidence or known user context, authorize approved
  providers, and verify exact resource scopes.
- Analyze — answer a requested production question about trace patterns, root
  cause, affected customers, customer/ARR impact, churn, or validation evidence.
- Instrument Agent — collaboratively select and verify comprehensive observability changes.
- Identify KPIs — identify a primary KPI, leading measures, and guardrails.
- Capture Feedback — design explicit and implicit feedback with optional application UX changes.
- Fix and Prove — ship one approved repair and verify its behavioral and production impact.

Setup uses the shorter user-visible contract `Discover -> Recommend relevant
sources -> Approve named providers -> Authorize exact resources -> Verify`.
Internal plan hashes and projection tokens do not become user decisions.

The other mutating workflows follow this contract:

`Discover -> Propose comprehensively -> Explain tradeoffs -> Select/restrict -> Approve -> Apply -> Verify -> Preserve deferred work`

Analyze system-accepts qualifying repeated new-failure proposals, creates their
Insights, and continues eligible fixes to configured Linear, GitHub, and Slack
destinations automatically. Pending ambiguous attachments still require an
explicit human Match decision. MCP never returns credentials, raw provider
cursors, or unfiltered provider payloads and never edits repository files.

Claude Code exposes the installed skills as `/virgo:<skill-name>` commands.
Codex users can browse them with `/skills` or mention them as
`$virgo:<skill-name>` (for example, `$virgo:setup`).
