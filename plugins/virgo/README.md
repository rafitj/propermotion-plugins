# Virgo plugin

Virgo is Proper Motion's production-analysis system for AI agents. This plugin
bundles the hosted, OAuth-backed Virgo MCP server with six repeatable workflows
for Codex and Claude Code.

The MCP server owns live product capabilities, authorization, evidence, and
canonical analysis. The bundled skills teach the coding host how to use those
capabilities without creating a parallel diagnosis or receiving repository
write authority.

## Workflows

- Proper Motion: analyze production failures, root cause, customer impact, and
  evidence-backed fixes.
- Quick Diagnose: check diagnostic readiness and identify evidence gaps.
- Instrument Agent: design and verify comprehensive agent observability.
- Define Success: define a primary outcome, leading measures, and guardrails.
- Capture Feedback: design explicit and implicit feedback capture.
- Fix and Prove: implement one approved repair and verify its impact.

In Claude Code, plugin skills use the `virgo:` namespace, such as
`/virgo:quick-diagnose`. In Codex, select a skill through `/skills` or mention it
as `$quick-diagnose`.

See the repository-level [getting started guide](../../GETTING_STARTED.md) for
installation and verification.
