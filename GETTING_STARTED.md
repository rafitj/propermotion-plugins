# Getting started with Virgo

Follow exactly one branch for the AI client running this session. Complete the
work for the user: do not send commands back for them to run, do not ask them to
invoke a skill, and do not make them repeat the original request. Reuse an
existing Virgo login. Authenticate only when the client reports that the hosted
Virgo MCP server is not authorized.

The copied setup prompt explicitly authorizes installing or updating the plugin,
using a fresh task when the client requires one, initializing or resuming the
current repository workspace, and running Virgo's read-only repository
discovery when no current receipt exists. Resolve the repository from the
current branch's configured upstream or tracking remote before falling back to
a remote named `origin`; never substitute a different fork. Reuse a completed
current discovery receipt, and rescan only when Virgo says it is missing or
stale. The prompt does not authorize connector accounts, resource scopes,
secrets, historical imports, notifications, or repository edits.

## Claude Code

Inspect the configured marketplaces first:

```bash
claude plugin marketplace list --json
```

If `propermotion-plugins` is missing, add it; otherwise refresh it:

```bash
claude plugin marketplace add rafitj/propermotion-plugins
claude plugin marketplace update propermotion-plugins
```

Run only the applicable install or update command:

```bash
claude plugin install virgo@propermotion-plugins
claude plugin update virgo@propermotion-plugins
```

If Claude reports that a reload is required, run `/reload-plugins`. Inspect
`/mcp` and authenticate `virgo` only if its current status requires OAuth. If a
fresh conversation is required to load the plugin, create it and continue the
same setup request there automatically.

Load `/virgo:setup` yourself. Resolve the current GitHub `owner/repository` from
the current branch's upstream first and initialize or resume its Virgo workspace.
Reuse a completed current discovery receipt; only when one is missing or stale,
wait for discovery to reach its connector-planning state. Reuse already-verified
connector accounts and their exact resource scopes without asking the user to
reconfirm them. If no newly relevant connector remains, verify and finish
without asking about optional external product or company context. Otherwise,
stop and ask one concise question
containing only connectors supported by repository evidence, tools the user
explicitly uses, and—when needed—where relevant product feedback or company
context lives. Do not enumerate the catalog. Continue new authorization or
resource selection only after the user answers.

## Codex

Inspect the current installation first:

```bash
codex plugin marketplace list
codex plugin list --available --json
```

If `propermotion-plugins` is missing, add it; otherwise refresh it:

```bash
codex plugin marketplace add rafitj/propermotion-plugins
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
`https://api.propermotion.ai/mcp`. Do not add a duplicate. Run
`codex mcp login virgo` only if the current registration reports that
authorization is required. If Codex must use a fresh task to load the plugin,
create that task from the current checkout and continue this same setup request
there automatically.

Load `$virgo:setup` yourself. Resolve the current GitHub `owner/repository` from
the current branch's upstream first and initialize or resume its Virgo workspace.
Reuse a completed current discovery receipt; only when one is missing or stale,
wait for discovery to reach its connector-planning state. Reuse already-verified
connector accounts and their exact resource scopes without asking the user to
reconfirm them. If no newly relevant connector remains, verify and finish
without asking about optional external product or company context. Otherwise,
stop and ask one concise question
containing only connectors supported by repository evidence, tools the user
explicitly uses, and—when needed—where relevant product feedback or company
context lives. Do not enumerate the catalog. Continue new authorization or
resource selection only after the user answers.

Keep commands, task restarts, OAuth checks, plan hashes, ETags, commit IDs,
workspace references, and unavailable or preview connectors out of normal
user-facing setup messages. A repeat setup with healthy existing connectors
should finish without a visible pause or an optional-context question. Otherwise,
the expected first pause is the relevant connector question, unless OAuth or a
repository permission screen truly needs the user in the browser.

After setup, do not ask the user to approve qualifying repeated new-failure
Match proposals, Linear tickets, or GitHub draft PRs. Virgo creates those
Insights and downstream artifacts automatically. Ask for a Match decision only
when Virgo leaves an ambiguous attachment to an existing Insight pending.
