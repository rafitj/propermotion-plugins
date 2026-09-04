# Proper Motion plugins

Install Virgo once to add Proper Motion's hosted MCP connection, guided
workspace setup, and agent-improvement workflows to Codex or Claude Code.

Copy this prompt into either client:

> Set up Virgo for this repository end to end. Open
> https://github.com/rafitj/propermotion-plugins/blob/main/GETTING_STARTED.md and
> follow exactly one branch for the AI client running this session. Complete the
> install or update yourself; do not ask me to run commands or invoke a skill.
> Reuse an existing Virgo login and authenticate only when the client reports
> that Virgo is not authorized. If this client must load the plugin in a fresh
> task, create that task and continue setup there automatically. Resolve the
> GitHub repository from the current branch's configured upstream or tracking
> remote before falling back to a remote named origin; never substitute a
> different fork. Initialize or resume this repository's Virgo workspace. Reuse
> a completed current repository-discovery receipt and rescan only when Virgo says
> it is missing or stale. Then pause and ask me one concise question about only
> the connectors supported by repository evidence, tools I explicitly use, or
> where I keep relevant product feedback and company context. Do not enumerate
> the catalog. After I answer, complete the approved connector authorization and
> exact resource selection, verify the resulting workspace and connections, and
> report their real states without exposing internal IDs, hashes, tokens, or
> credentials.

See [GETTING_STARTED.md](./GETTING_STARTED.md) for the complete product-specific
installation and verification contract.
