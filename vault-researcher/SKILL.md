---
name: vault-researcher
description: >
  Use this skill whenever the user mentions an Obsidian vault, knowledge base,
  MOC, atomic notes, or research backlog — it contains the vault's note
  templates, folder structure, tagging system, and research methodology that
  cannot be replicated without consulting it. Activate for: researching topics
  and creating vault notes, processing documents or reports into atomic notes,
  querying existing vault knowledge ("what do we know about X"), verifying
  findings, managing Maps of Content, or continuing a previous research session.
  Also use when the user says "research", "investigate", "dig into", "look
  into", "explore", "go deeper", "break this down", "add to vault", "check the
  backlog", or provides a document/PDF/AI report for vault integration. Do not
  use for vault housekeeping (file renaming, reorganization), Obsidian plugin
  setup, theme customization, or Dataview queries.
license: MIT
compatibility: >
  Requires Claude Code (sub-agents, Bash tool, model routing). Depends on
  obsidian CLI (kepano/obsidian-skills) and defuddle CLI (defuddle-cli npm
  package). Not compatible with Claude.ai (no sub-agent or Bash support).
metadata:
  author: alvdef
  version: 1.0.0
  category: research
  tags: [obsidian, knowledge-management, research, zettelkasten, notes]
---

# Vault Researcher

Build and grow an Obsidian knowledge base through iterative research. The
vault uses atomic notes, wikilinks, tags, and Maps of Content (MOCs) to
create a queryable, interconnected research graph.

Dependencies: `obsidian` and `defuddle` CLI commands from the
[kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) skills
package. `obsidian` provides vault search and metadata queries; `defuddle`
extracts clean content from web pages for source notes. Both are run via the
Bash tool. See [Bootstrap](#bootstrap-first-run) for installation.

> **Every session must end with the [Closing a Session](#closing-a-session)
> checklist.** The vault's integrity depends on it — orphan notes, stale
> MOCs, and lost backlog items accumulate when sessions end abruptly. If the
> user wants to stop, run the Closing steps before exiting. If context is
> running out, warn the user and run at least the closing report so they
> can continue manually or in the next session.

## Before You Start

Read these references when creating a note type you haven't created recently:

- `references/vault-conventions.md` — folder structure, naming, linking rules
- `references/note-templates.md` — YAML frontmatter per note type
- `references/tag-taxonomy.md` — tag namespaces and how to extend them
- `references/moc-format.md` — MOC template and depth tracker
- `references/claude-md-spec.md` — what belongs in CLAUDE.md vs the vault

Also read `CLAUDE.md` (at vault root) for stable project context: client,
research domain, and agent rules.

## Bootstrap (First Run)

Run this on first use or whenever the vault environment is uncertain.

1. **Check `obsidian` CLI**: run `obsidian help` via Bash. If not found,
   install: `npx skills add kepano/obsidian-skills -g -y`
2. **Check `defuddle` CLI**: run `defuddle --help` via Bash. If not found,
   install: `npm install -g defuddle-cli`
3. **Verify both**: run `obsidian help` and `defuddle --help` again. If
   either still fails, stop and tell the user — there are no fallbacks.
4. **Check vault folder structure**: verify the folders listed in
   `references/vault-conventions.md` exist. Create any missing folders
   via `mkdir -p` (Bash tool).
5. **Create `Backlog.md`** if `00-Dashboard/Backlog.md` does not exist.
   Use the template from `references/vault-conventions.md`.
6. **Check for `CLAUDE.md`** at vault root. If missing, offer to create
   one from the template in `references/claude-md-spec.md`.

Delegate steps 1–5 to a Haiku sub-agent — these are mechanical checks.
Step 6 requires judgment (what to put in CLAUDE.md), so handle it in the
main context.

## Intake — Determine What the User Needs

Every interaction starts here. Figure out what the user provided and route:

**User gives a topic or question (no document)**
→ Go to [Research Loop](#research-loop)

**User provides a document** (paste, upload, or points to a clipping)
→ Go to [Decomposition](#decomposition)

**User references existing vault content** ("what do we know about X")
→ Search the vault, read relevant notes and MOC, summarize current state.
  Then ask: "Here's what we have. Want me to go deeper, or is this enough?"

**Ambiguous** → Ask: "Do you want me to research this from the web, or are
you giving me a document to process into the vault?"

## Research Loop

An iterative cycle: investigate a focused question, create notes, surface
new questions, ask the user for direction, repeat.

### 1. Orient

Read `CLAUDE.md` for stable project context only — client, domain, agent
rules. Do NOT use it as a source of research findings; those live in the vault.

Build session context from the vault — delegate all reads to a sub-agent
(model: haiku), since this is mechanical retrieval with no synthesis needed:

> Prompt the Haiku sub-agent:
> "Read vault context using the `obsidian` CLI (via Bash tool):
> (1) run `obsidian search tag:type/moc limit=20`, then for each MOC read
> its header, Key questions section, and Depth tracker — skip full wikilink
> lists. (2) Read `00-Dashboard/Backlog.md`. (3) Read the most recent file
> in `50-Research-Log/` by filename. Return all content verbatim."

Synthesize the returned content in this context: what the vault covers,
what's pending, open threads from the last session. If the user didn't
specify a focus, propose 2–3 items from the backlog ranked by priority.

Delegate topic search to the same Haiku sub-agent:

> "Search for existing coverage on '{topic}': run
> `obsidian search query="{topic}" limit=20` (via Bash tool).
> Return results verbatim."

Ask the user (if not already clear):
- What specific question or angle to focus on?
- Any constraints on scope or sources?

**Creating a new MOC** — only if no suitable MOC exists. Before creating:

1. Check existing MOCs for overlap: does this topic fit as a section of an
   existing MOC rather than a new top-level map?
2. Check scope: expect 5+ permanent notes spanning multiple sessions? If the
   scope is narrower, hold findings in fleeting notes until it's clearer.
3. Present options to the user before creating anything:

   > "This topic isn't covered by existing MOCs. Options:
   > A. Create `MOC-{topic}` (new top-level map)
   > B. Add `## {Subtopic}` section to `[[MOC-{existing}]]`
   > C. Hold in fleeting notes — revisit once scope is clearer"

Never create a new MOC without user approval. See `references/moc-format.md`
for the template.

**Sub-MOC promotion**: when a section within an existing MOC reaches 7+
notes, offer to promote it to a child MOC. The child links back to the parent
via `parent_moc` in frontmatter; the parent MOC replaces the section with a
link under "Related maps".

### 2. Investigate

Conduct targeted web research on the focused question.

For each finding, create the appropriate note type:
- Insight/claim → Permanent note in `30-Notes/Permanent/`
- Reference material → Source note in `20-Sources/`
- Company/person/platform → Entity profile in `30-Notes/Entity/`
- Options compared → Comparison in `30-Notes/Comparison/`
- Fragment needing more work → Fleeting note in `10-Inbox/Fleeting/`
- Open question → Question note in `10-Inbox/Fleeting/` (Q- prefix)
- Testable claim → Hypothesis note in `30-Notes/Permanent/` (HYP- prefix)

**Fleeting note lifecycle**: fleeting notes are temporary holding areas for
findings that lack enough substance to become a proper note. "Processing"
a fleeting note means: read it, synthesize its content into the note type
specified by `promote_to` (using the Phase 1/Phase 2 process below), then
delete the original fleeting note. At session close, process ALL fleeting
notes in the vault — not just those created in the current session.

Use a two-phase process, batched per investigation round — synthesize ALL
notes first, then write them all:

**Phase 1 — Synthesis (Opus):** For each note in the round, determine the
complete content: title (claim-based), key insight, supporting evidence,
implications, wikilinks to related notes and parent MOC. All datetime fields
use `YYYY-MM-DD HH:mm` (never date-only — see `references/note-templates.md`).
Do not delegate synthesis — the note body requires understanding of the
research.

**Phase 2 — Write (Haiku sub-agent):** Once ALL notes in the round are
synthesized, delegate all file writes in a single sub-agent call:

> "Create these notes:
> 1. `{path1}` with this exact content: {frontmatter + body}.
> 2. `{path2}` with this exact content: {frontmatter + body}.
> ...
> Use the template structure from `references/note-templates.md`.
> Tag: `source/ai-generated` + `confidence/uncertain`.
> Confirm each file written."

Never delegate Phase 1. Never do Phase 2 inline — file writes are
mechanical and don't need the main model's reasoning capacity.

### 3. Surface and Ask

After each investigation round, present findings to the user:

- What was found (brief summary)
- New questions that emerged
- Contradictions discovered (flag with `> [!danger]` in affected notes)
- Suggested next focus area

Then ask: **continue deeper, pivot to a new thread, or stop?**

Never continue without user direction.

### 4. Repeat or Close

If continuing → return to step 2 with the new focus.
If stopping → go to [Closing a Session](#closing-a-session).

## Decomposition

For processing an existing document into atomic vault notes. This does NOT
conduct new web research — it structures existing content.

### 1. Receive

**Preferred workflow for long documents** (e.g. Gemini Deep Research reports):
the user saves the file directly to `10-Inbox/Clippings/` in their vault, then
tells you to process it. Read it from disk — don't ask them to paste it in
chat, since that wastes context on verbatim content you can read from the file.

If the user does paste content in chat, save it to `10-Inbox/Clippings/`
yourself before processing.

Ask the user (if not already provided):
- What tool produced this? (for `source_tool` frontmatter field)
- What was the original query? (for `source_query`)
- Which MOC does this relate to?

Save the original with a `CLIP-` prefix and `processed: false`. Do not edit
the original body.

### 2. Scan

Search the vault for overlap:
```
obsidian search query="{main topic}" limit=20
```

Read the parent MOC to understand current state.

### 3. Propose

Identify atomic concepts and present them to the user BEFORE creating notes:

"I can extract these from the document:
1. {Concept} → Permanent note
2. {Entity} → Entity profile
3. {Already exists as [[PN-existing]]} → I'll link, not duplicate
4. {Comparison} → Comparison note

Proceed with all, skip some, or add others?"

Each concept must pass the atomicity test: is it linkable to notes in OTHER
research areas? If not, keep it as a section within a broader note. Every
note must be at least one substantial paragraph.

### 4. Create

After user approval, synthesize each note body in this context: write in
your own words, extract the core claim, add evidence and implications.
Do not copy-paste from the source document.

Then delegate all file writes to a Haiku sub-agent:

> "Create these notes:
> 1. `{path}` with this exact content: {frontmatter + body}.
> ...
> Include: `source/ai-generated` + `confidence/uncertain` tags,
> `Source: [[CLIP-{original}]]` link, links to parent MOC and related notes.
> Confirm each file written."

For concepts that overlap with existing notes, synthesize the update in this
context, then delegate the edit to the Haiku sub-agent. Ask the user before
updating any existing note.

### 5. Integrate

Update the MOC, mark clipping as `processed: true`, then go to
[Closing a Session](#closing-a-session).

If the decomposition surfaced gaps the user wants investigated, offer to
switch to the [Research Loop](#research-loop) targeting those gaps.

## Verification

When the user asks to verify findings, or when contradictions surface:

1. Find notes tagged `confidence/uncertain` in the relevant area
2. Search for corroborating or contradicting evidence
3. Update confidence:
   - Corroborated by 2+ independent sources → `confidence/likely`
   - Confirmed by primary data → `confidence/verified`
   - Contradicted → `confidence/contradicted` + `> [!danger]` callout
4. Report findings and contradictions to the user

## Closing a Session

**Context threshold**: at ~20% context remaining, warn the user and run this
closing checklist immediately. Do not attempt new research rounds below this
threshold.

Run this at the END of every session, regardless of what was done:

1. **No orphans**: Verify every note created links to a MOC. Process or
   discard ALL fleeting notes in the vault — not just those from this
   session. "Process" means: read, synthesize into `promote_to` type via
   Phase 1/Phase 2, then delete the fleeting note.

2. **Prepare closing content (Opus)**: Draft the full
   content for all three mechanical updates before delegating:
   - MOC: list of new wikilinks with annotations, depth tracker row updates,
     questions to check off, new gap markers
   - Backlog: new items to append, completed items to **remove** (not check
     off — completed items are deleted from the backlog since the research
     log provides the audit trail)
   - Research log: full log body (what was done, notes created, open
     questions, suggested next steps). **All mentions of vault entities,
     notes, or MOCs in the log prose must use `[[wikilinks]]`** — not just
     the "notes created" list. The research log is part of the knowledge
     graph; plain-text mentions of things like "OTCommerce" or "1688" break
     the link trail. Use `[[ENT-slug|Display Name]]` aliases for
     readability. If a log file with the same timestamp already exists,
     append a letter suffix (`a`, `b`, `c`)

3. **Delegate all writes to a Haiku sub-agent** (one batch call):

   > "Apply these closing updates:
   > 1. In `40-Maps/{MOC}.md`: add under the correct headings — {wikilinks
   >    with annotations}. Update depth tracker row for {area} to {depth}.
   >    Check off {questions}. Add gap markers: {gaps}.
   > 2. In `00-Dashboard/Backlog.md`: append to High priority — {new items}.
   >    Remove these completed items: {completed items}.
   > 3. Create `50-Research-Log/{YYYY-MM-DD_HHmm}.md` with this content:
   >    {full log body}.
   > Confirm each update."

4. **Report to user** — use this exact structure:

   **Session summary** (2–3 sentences: what was investigated, what was decided)

   **Key deliverable** (if the session produced one main note, give its full
   vault path so the user can open it directly):
   > `30-Notes/Permanent/PN-{slug}.md`

   **Notes created/modified** (list only the most relevant — not every
   fleeting note or mechanical MOC edit. Include vault path for each):
   ```
   + 30-Notes/Permanent/PN-{slug}.md  — {one-line description}
   + 30-Notes/Entity/ENT-{slug}.md    — {one-line description}
   ~ 40-Maps/MOC-{topic}.md           — updated depth tracker, 3 new links
   ```
   Use `+` for created, `~` for modified.

   **Open questions** (from the key deliverable or from research gaps — these
   are actionable next steps, not just "continue research"):
   1. {Question} — context on why it matters
   2. {Question} — what it unblocks

   **Suggested next focus** (1–2 concrete items from the backlog, ranked)
5. **CLAUDE.md hygiene**: First check that `CLAUDE.md` exists at vault root.
   If it does not exist, alert the user:
   > "No CLAUDE.md found. The vault needs one — see
   > `references/claude-md-spec.md` for the template. Should I create it
   > now from what I can infer about the project?"
   Then offer to create it and migrate any project context found in other
   root-level files (README.md, loose notes) into the correct sections.

   If `CLAUDE.md` exists, check if it has grown since the vault was last
   updated — new findings, resolved decisions, or reference URLs added
   outside the vault. If so, migrate them before closing:

   | Found in CLAUDE.md | Migrate to |
   |--------------------|------------|
   | Resolved decision  | `PN-*` note with `confidence/verified`, linked from MOC |
   | Pending decision   | `00-Dashboard/Backlog.md` — High priority item |
   | Reference URL      | `ENT-*` or `SRC-*` note in vault |
   | Research finding   | Appropriate note type per `references/note-templates.md` |

   After migrating, trim `CLAUDE.md` back to its stable sections. See
   `references/claude-md-spec.md` for what belongs there.

## Examples

**Example 1: Topic research**
User: "I've been reading about PFAS contamination in drinking water. Dig into the regulatory landscape for me — who's doing what and where are the gaps."
Actions:
1. Orient — read vault MOCs, Backlog, recent log (delegate to Haiku)
2. Synthesize existing coverage, confirm no overlap
3. Propose: "I'll create MOC-PFAS-Regulation and focus on US EPA, EU REACH, and state-level bans. Sound right?"
4. After approval → web research, create `PN-EPA-proposes-enforceable-PFAS-limits`, `ENT-EPA`, `SRC-EPA-2024-final-rule`, etc.
5. Surface findings, ask: "Emerging question — state-level limits are stricter than federal. Want me to go deeper on that, or pivot?"

**Example 2: Document decomposition**
User: "Here's a Gemini report on autonomous vehicle insurance models. Add it to the vault." [pastes report]
Actions:
1. Ask: "What tool generated this, and what was the query?" (for `source_tool` / `source_query`)
2. Save as `CLIP-20260301-av-insurance-models` in `10-Inbox/Clippings/`
3. Search vault for overlap
4. Propose: "I can extract 6 concepts: 3 permanent notes on liability models, 2 entity profiles (Waymo, Munich Re), 1 comparison (usage-based vs traditional). Proceed?"
5. After approval → synthesize each note body, batch-write via Haiku

**Example 3: Vault query**
User: "What do we have on lithium recycling?"
Actions:
1. Search vault with `obsidian search query="lithium recycling"`
2. Read matching notes and parent MOC
3. Summarize: "We have 4 notes covering hydrometallurgical processes and 2 entity profiles. The MOC shows this area at Growing depth. Gap: no coverage of direct recycling methods."
4. Ask: "Want me to research direct recycling, or is this enough for now?"

## Model Strategy

This skill uses three models with deterministic assignment — no per-step
judgment calls:

- **Opus** ("multi-hour research tasks, advanced agents") → deep research,
  synthesis, judgment calls
- **Sonnet** ("content creation, agentic tool use") → routine research
  rounds where the pattern is established
- **Haiku** ("sub-agent tasks, high-volume processing") → all mechanical
  execution (file writes, vault reads, dependency checks)

| Task | Model | Why |
|------|-------|-----|
| Intake — routing decision | Opus | Ambiguous input interpretation |
| Orient — synthesize vault state, propose focus | Opus | Multi-source synthesis |
| Investigate — web research, insight extraction | Opus | Deep synthesis |
| Investigate — detect contradictions | Opus | Reasoning across sources |
| Surface and Ask — new questions, next steps | Opus | Judgment |
| Decompose — atomicity decisions, propose plan | Opus | Design judgment |
| Verification — analyze evidence, update confidence | Opus | Cross-reference reasoning |
| Closing — draft MOC/Backlog/Log content | Opus | Session synthesis |
| CLAUDE.md hygiene — detect drift, decide migration | Opus | Judgment |
| Investigate — routine follow-up rounds | **Sonnet** | Pattern established by Opus |
| Orient — vault reads (MOC headers, Backlog, log) | **Haiku** | Mechanical I/O |
| Bootstrap — dependency checks, folder creation | **Haiku** | Mechanical checks |
| Investigate — write note files | **Haiku** | Content pre-determined by Opus |
| Decompose — write note files (post-approval) | **Haiku** | Content pre-determined by Opus |
| Closing — apply MOC edits, Backlog, Research Log | **Haiku** | Edits pre-determined by Opus |
| CLAUDE.md hygiene — write migrated notes | **Haiku** | Content pre-determined by Opus |

Rule: **never delegate synthesis; never write files in the main context.**
Use the `obsidian` CLI (via Bash tool) for all vault queries — far cheaper
in tokens than reading raw files.
