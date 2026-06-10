# Ruly — Court Rules Assistant (Claude plugin)

Ruly helps attorneys, paralegals, and legal assistants work with jurisdiction-specific court rules and statutes inside Claude. This plugin bundles two skills and the Ruly connector:

- **`/answering-rule-questions`** — answers a specific procedural question by retrieving and citing relevant rule text.
- **`/generating-filing-checklists`** — produces a comprehensive formatting-and-content checklist for a court filing, with verified citations.

Both skills retrieve from the Ruly rule corpus through the bundled **Ruly connector** (a remote MCP server). Every citation is grounded in a real rule document.

## Usage

Ask Claude in plain language — the right skill activates automatically.

**Answer a rules question.** Retrieves and cites the controlling rule text for a specific procedural question.

> How do I demand a jury trial in ND Cal?

**Generate a filing checklist (and review a draft).** Produces a formatting-and-content checklist for a filing, then checks your draft against it.

> Generate a filing checklist and review my motion to strike or tax costs in LA Superior.

## Install

> **Requirement:** Ruly needs a Ruly account. The connector signs you in via OAuth the first time you use it — if you don't have an account yet, create one at [askruly.com](https://www.askruly.com) before installing.

### Claude desktop / web / Cowork

1. Open **Customize Claude → Plugins**.
2. Click **+ → Create plugin → Add marketplace → Add from a repository**, paste this repo's URL (`https://github.com/ppetrick/ruly-plugin`), and click **Sync**.
3. Click **Browse plugins**, find "Ruly — Court Rules Assistant", and click **Install**.
4. When prompted, **authorize the Ruly connector** (one-time OAuth sign-in).

### Claude Code

```
/plugin marketplace add ppetrick/ruly-plugin
/plugin install ruly-plugin@ruly
```

Then authorize the Ruly connector when prompted.

## License

MIT — see [LICENSE](./LICENSE).
