# Lexponent — Court Rules & Statutes Assistant (Claude plugin)

Lexponent helps attorneys, paralegals, and legal assistants work with jurisdiction-specific court rules and statutes inside Claude. This plugin bundles two skills and the Lexponent connector:

- **`/answering-rule-questions`** — answers a specific procedural question by retrieving and citing relevant rule text.
- **`/generating-filing-checklists`** — produces a comprehensive formatting-and-content checklist for a court filing, with verified citations.

Both skills retrieve from the Lexponent rule corpus through the bundled **Lexponent connector** (a remote MCP server). Every citation is grounded in a real rule document.

## Usage

Ask Claude in plain language — the right skill activates automatically.

**Answer a rules question.** Retrieves and cites the controlling rule text for a specific procedural question.

> How do I demand a jury trial in ND Cal?

**Generate a filing checklist (and review a draft).** Produces a formatting-and-content checklist for a filing, then checks your draft against it.

> Generate a filing checklist and review my motion to strike or tax costs in LA Superior.

## Install

> **Requirement:** Lexponent needs a Lexponent account. The connector signs you in via OAuth the first time you use it — if you don't have an account yet, create one at [lexponent.ai](https://www.lexponent.ai) before installing.

### Claude desktop / web

1. Open **Customize**.
2. Click **Plugins → Add → Add marketplace → Add from a repository**, paste `ppetrick/lexponent-plugin`, and click **Sync**.
3. Next to "Lexponent — Court Rules & Statutes Assistant", click **+**.
4. Click **Gear icon → Connectors → Install** (next to Lexponent), then **Add**.
5. **Connect** and sign in with your Lexponent account.
6. Return to the Lexponent Connector and choose **Always Allow** for all tools.

### Claude Code

```
/plugin marketplace add ppetrick/lexponent-plugin
/plugin install lexponent-plugin@lexponent
```

Then authorize the Lexponent connector when prompted.

## License

MIT — see [LICENSE](./LICENSE).
