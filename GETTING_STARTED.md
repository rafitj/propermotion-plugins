# Getting started with Virgo

Follow exactly one branch for the product running this session: Claude Code or
Codex. The Virgo plugin bundles one hosted OAuth-backed MCP server and six
skills. Do not configure a second server at the same URL.

## Claude Code

Complete the installation for the user. Inspect the configured marketplaces
first:

```bash
claude plugin marketplace list --json
```

If `propermotion-plugins` is missing, add it:

```bash
claude plugin marketplace add rafitj/propermotion-plugins
```

If it is already present, refresh it instead:

```bash
claude plugin marketplace update propermotion-plugins
```

Use `claude plugin install virgo@propermotion-plugins` when Virgo is missing.
Use `claude plugin update virgo@propermotion-plugins` when it is already
installed. Do not run both commands unconditionally.

```bash
claude plugin install virgo@propermotion-plugins
claude plugin update virgo@propermotion-plugins
```

If the client reports that a reload is required, run `/reload-plugins`. Open
`/mcp`, select `virgo`, and complete OAuth when authentication is required.
Start a new conversation so Claude Code loads the current skills and MCP tools.

Verify the installation by calling `virgo_get_workspace_context` with the
current GitHub `owner/repository` name. The connection is ready when the
read-only operation returns the workspace's capabilities, sources, freshness,
coverage, and safe blockers. If the repository is not configured, report that
result without starting setup automatically.

Installed skills appear under the plugin namespace, including
`/virgo:propermotion` and `/virgo:quick-diagnose`.

## Codex

Complete the installation for the user. Inspect the current state first:

```bash
codex plugin marketplace list
codex plugin list --available --json
```

If `propermotion-plugins` is missing, add it:

```bash
codex plugin marketplace add rafitj/propermotion-plugins
```

If it is already present, refresh it instead:

```bash
codex plugin marketplace upgrade propermotion-plugins
```

Install or refresh Virgo:

```bash
codex plugin add virgo@propermotion-plugins --json
```

Inspect the bundled MCP registration:

```bash
codex mcp list --json
codex mcp get virgo --json
```

Require exactly one enabled registration for
`https://api.propermotion.ai/mcp`. Do not add a duplicate Virgo server. If the
canonical server still needs authorization, run:

```bash
codex mcp login virgo
```

Complete OAuth in the browser, then start a new task so Codex loads the current
skills and MCP tools.

Verify the installation by calling `virgo_get_workspace_context` with the
current GitHub `owner/repository` name. The connection is ready when the
read-only operation returns the workspace's capabilities, sources, freshness,
coverage, and safe blockers. If the repository is not configured, report that
result without starting setup automatically.

Use `/skills` to browse the installed workflows or mention one directly, such
as `$propermotion` or `$quick-diagnose`.
