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
| `.mcp.json` | The Ruly connector (`https://www.askruly.com/api/mcp`) |
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

Then authorize the Ruly connector when prompted.

## Authentication & privacy

The Ruly connector authenticates with **OAuth** — you sign in once and authorize access; no API keys or tokens are stored in this repository. When you use these skills, your questions and the rule text retrieved are sent to the Ruly MCP server at `https://www.askruly.com` to find and cite the relevant rules.

## License

MIT — see [LICENSE](./LICENSE).
