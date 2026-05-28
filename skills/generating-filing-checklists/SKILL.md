---
name: generating-filing-checklists
description: Generates a comprehensive checklist of formatting and content requirements for a court filing. Use when the user asks for requirements, a checklist, rules, or "what I need" for filing a specific document — e.g., "formatting requirements for an MSJ", "checklist for a motion to compel", "what do I need in my opposition brief".
---

# Formatting and Content Checklist

Produce a checklist of every requirement a filing of this type could need to satisfy — what must appear in the document's four corners, what must be attached, and how it must be laid out. Every item must be verifiable by reading the filing itself.

## Determine scope

Before retrieving any rules, determine the documents in scope.

1. Call `Ruly:list_rulesets` to see all available rulesets, each with a `label` and `description`.
2. If the user named specific jurisdictions, courts, or rule bodies (e.g., "LA Superior", "FRCP", "California CCP"), match them to ruleset labels and descriptions. If scope isn't stated, ask the user.
3. Call `Ruly:list_documents` with `rulesetIds: [...]` for the matched rulesets. The response includes each document's `id` (use this in subsequent calls) and `citationFormat` (use this when composing citation labels).

Use the returned `id`s for all later `Ruly:search_rules`, `Ruly:grep_rules`, `Ruly:find_rule_by_title`, and `Ruly:cite` calls.

## Output format

Mandatory structure:

```
# Formatting and Content Checklist
## [Filing type]
**Documents:** [list of rule documents the checklist is based on]

## Formatting
### [Sub-category]
- [ ] [Requirement]. [(Rule citation)](url)
- [ ] [Condition: triggering choice] [Conditional requirement]. [(Rule citation)](url)

## Content
### [Sub-category]
- [ ] [Requirement]. [(Rule citation)](url)
- [ ] [Condition: triggering choice] [Conditional requirement]. [(Rule citation)](url)
```

The top-level structure is mandatory: the title, the filing-type heading, the **Documents** line, and the two sections **## Formatting** and **## Content**. Sub-categories under each are filing-type-specific — they emerge from the rules retrieved, not a fixed taxonomy.

Example:

```
# Formatting and Content Checklist
## Motion to Strike or Tax Costs
**Documents:** California Rules of Court (Titles 1, 2, 3) · LASC Local Rules (Chapters 1–3) · California Code of Civil Procedure

## Formatting

### Margins
- [ ] Left margin is at least 1 inch. [(Cal. R. Ct. 2.107)](https://ruly-rule-documents.s3.us-east-1.amazonaws.com/ca-crc-title-2.pdf#page=8)

### Memorandum Page Limits
- [ ] Opening memorandum does not exceed 15 pages. [(Cal. R. Ct. 3.1113(d))](https://ruly-rule-documents.s3.us-east-1.amazonaws.com/ca-crc-title-3.pdf#page=171)
- [ ] [Condition: memorandum exceeds 10 pages] Memorandum includes a table of contents and table of authorities. [(Cal. R. Ct. 3.1113(f))](https://ruly-rule-documents.s3.us-east-1.amazonaws.com/ca-crc-title-3.pdf#page=171)

## Content

### Service and Filing Deadlines
- [ ] Moving and supporting papers are served and filed at least 16 court days before the hearing. [(Code Civ. Proc., § 1005(b))](https://ruly-rule-documents.s3.us-east-1.amazonaws.com/ca-code-ccp.pdf#page=804)
- [ ] [Condition: service by mail within California] 16-court-day period is increased by 5 calendar days. [(Code Civ. Proc., § 1005(b))](https://ruly-rule-documents.s3.us-east-1.amazonaws.com/ca-code-ccp.pdf#page=804)
```

The **Documents** header lists the rule documents the checklist is based on (these define the jurisdiction). Sub-categories emerge from the rules, not a fixed taxonomy. If a rule offers a choice (e.g., "Times New Roman or Arial"), include both. Conditional rules apply to both Formatting and Content; flag with `[Condition: triggering choice]` at the start of the item — never omit on the assumption the choice doesn't apply. Use the `Ruly:cite` MCP tool to produce verified citation links.

## What's in scope

Any requirement whose compliance produces something visible in the filing. The dimensions you'll encounter on any filing type:

- **Layout** — physical and visual properties of every page
- **Constituent documents** — what documents make up the filing
- **Content elements** — substantive recitals, citations, references, attachments within each document
- **Procedural artifacts** — anything in the filing that proves a required prerequisite procedure was followed

Every dimension applies to every filing type; specific items in each dimension vary — let the rules tell you what they are.

If complying with a rule requires something visible in the filing — an attached declaration, a recital, a particular document — the rule is in scope, even if the rule's text describes a hearing, a service procedure, or another off-the-page activity. Check what compliance produces, not what the rule's surface text describes.

Out of scope: filer actions that produce no artifact at all (paying a fee, calling the clerk, the act of serving).

## Approach

Identify the filing class from the filing type. The keywords below are common examples, not exhaustive — use semantic understanding of the filing description, not just literal string match.

- "complaint", "cross-complaint", "answer", "amended complaint" / "amendment to complaint", "statement of damages", "petition" (trial-court level, e.g., petition for administrative writ to a trial court) → **pleading**
- "interrogatories", "request for admission", "request for production", "demand for inspection", "responses to ..." → **discovery-request**
- "request for entry of default", "default judgment", "prove-up" → **default-package**
- "motion ...", "demurrer" (or jurisdiction's equivalent failure-to-state motion), "ex parte application" / "ex parte", "motion in limine", "opposition" / "reply" / "supplemental brief" (these are constituent papers within a motion event) → **motion**
- appellate-level filings ("brief on appeal", "opening brief", "respondent's brief", "petition for writ to an appellate court") → confirm the in-scope corpus covers appellate practice; if it doesn't, flag and ask the user
- default → **motion**

Read `references/common.md` (research areas universal across all filings) and `references/<class>.md` (class-specific) before researching. Class files currently available: `motion.md`, `pleading.md`, `default-package.md`, `discovery-request.md`. For other classes, use `common.md` plus general knowledge of the filing. The references list research areas and starter output sections — they are research hints, not output templates; extend or replace as the rules demand.

Treat the dimensions as a research frame: for each category, query the rule corpus and capture any rules that impose a requirement. A category may yield several rules, one, or none. For each retrieved rule, place each requirement under the category it addresses — a single rule may contribute items to multiple categories. If a requirement doesn't fit any category, the frame is missing a category; extend it. Stop when every category has been checked.

Use the three retrieval MCPs in combination:

- `Ruly:search_rules` — semantic matches; surfaces rules that address a concept without the obvious keyword.
- `Ruly:grep_rules` — POSIX regex against rule body text; verifies wording or confirms a concept appears in a specific document.
- `Ruly:find_rule_by_title` — follow cross-references; when a retrieved rule cites another rule by number or section, look it up.

`Ruly:search_rules` returns top-N per query and won't enumerate the full corpus alone. `Ruly:grep_rules` searches one document per call — for any concept, issue parallel calls across all in-scope documents (one per documentId) in the same turn. Walk `has_more` / `next_cursor` through every page when matches are likely. Each in-scope document is a separate corpus and each enumeration category requires independent coverage; partial sweeps produce checklists that silently omit requirements from un-searched documents.

## Final action

Output the complete markdown checklist as your response, following the mandatory structure in "Output format" above. The response itself is the canonical artifact. If a writable working directory is available, also save the same markdown to `checklist.md` in it so the user can download the file; if no filesystem is available or the write fails, that is fine — the response remains canonical. Do not truncate or summarize the checklist in the response.
