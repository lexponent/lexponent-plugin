# Ruly — Court Rules Assistant (Claude plugin)

Ruly helps attorneys and paralegals work with jurisdiction-specific court rules and statutes inside Claude. This plugin bundles two skills and the Ruly connector:

- **`/answering-rule-questions`** — answers a specific procedural or factual question by retrieving and citing rule text.
- **`/generating-filing-checklists`** — produces a comprehensive formatting-and-content checklist for a court filing, with verified citations.

Both skills retrieve from the Ruly rule corpus through the bundled **Ruly connector** (a remote MCP server). Every citation is grounded in a real rule document — no hand-typed URLs.

## What's inside

| Component | Purpose |
|---|---|
| `skills/answering-rule-questions/` | Rule Q&A skill |
| `skills/generating-filing-checklists/` | Filing checklist skill + class reference files |
| `.mcp.json` | The Ruly connector (`https://mcp.askruly.com/mcp`) |
| `.claude-plugin/` | Plugin + marketplace manifests |

## Install

### Claude desktop / web / Cowork

1. Open **Customize Claude → Plugins**.
2. Click **+**, add this marketplace (or **Browse plugins** if listed), and **Install** "Ruly — Court Rules Assistant".
3. When prompted, **authorize the Ruly connector** (one-time OAuth sign-in).
4. Ask a rules question, or type `/generating-filing-checklists`.

### Claude Code

```
/plugin marketplace add ppetrick/ruly-plugin
/plugin install ruly@ruly
```

> **Note:** Add the marketplace via the GitHub shorthand shown above (or a git URL), not a direct link to `marketplace.json` — relative plugin paths only resolve when the marketplace is added from git. In `/plugin install ruly@ruly`, the first `ruly` is the plugin name and the second is the marketplace name (they happen to share the identifier).

Then authorize the Ruly connector when prompted.

## Authentication & privacy

The Ruly connector authenticates with **OAuth** — you sign in once and authorize access; no API keys or tokens are stored in this repository. When you use these skills, your questions and the rule text retrieved are sent to the Ruly MCP server at `https://mcp.askruly.com` to find and cite the relevant rules.

## Updates

This plugin uses pinned semantic versions, so a new release is only delivered after its version is bumped. To pull the latest version after an update is published:

- **Claude desktop / web / Cowork:** Customize → Plugins → Ruly → Update.
- **Claude Code:** `/plugin update ruly@ruly`.

## License

MIT — see [LICENSE](./LICENSE).
