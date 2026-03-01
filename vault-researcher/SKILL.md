---
name: vault-researcher
description: >
  Conducts deep research and organizes findings into an Obsidian vault as a
  structured knowledge base. Handles two entry points: a topic/question to
  investigate, or an existing document to decompose into atomic notes. Use
  when the user says "research", "investigate", "find out about", "go deeper",
  "process this report", "add this to the vault", "break this down", "what do
  we know about", or references a MOC. Use whenever the user wants to build
  or query a research knowledge base in Obsidian. Do not use for general vault
  housekeeping like renaming or reorganizing files.
---

# Vault Researcher

Build and grow an Obsidian knowledge base through iterative research. The
vault uses atomic notes, wikilinks, tags, and Maps of Content (MOCs) to
create a queryable, interconnected research graph.

Dependencies: `obsidian-cli`, `obsidian-markdown`, `defuddle` (from
kepano/obsidian-skills).

## Before You Start

On first use or when unsure about vault conventions, read these references:

- `references/vault-conventions.md` — folder structure, naming, linking rules
- `references/note-templates.md` — YAML frontmatter per note type
- `references/tag-taxonomy.md` — tag namespaces and how to extend them
- `references/moc-format.md` — MOC template and depth tracker

Once familiar, you don't need to re-read them every session — just consult
when creating a note type you haven't created recently.

Also read `CLAUDE.md` (at vault root) for stable project context: client,
research domain, and agent rules. Current priorities and research state live
in the vault — see the Orient step in Research Loop.

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
> "Read vault context using obsidian-cli if available:
> (1) run `obsidian search tag:type/moc limit=20`, then for each MOC read
> its header, Key questions section, and Depth tracker — skip full wikilink
> lists. (2) Read `00-Dashboard/Backlog.md`. (3) Read the most recent file
> in `50-Research-Log/` by filename. Return all content verbatim.
> If obsidian-cli is not available, use Glob to list `40-Maps/*.md`, read
> each MOC file, read `00-Dashboard/Backlog.md` directly, and Glob
> `50-Research-Log/*.md` sorted by name to find the most recent."

Synthesize the returned content in this context: what the vault covers,
what's pending, open threads from the last session. If the user didn't
specify a focus, propose 2–3 items from the backlog ranked by priority.

Delegate topic search to the same Haiku sub-agent:

> "Search for existing coverage on '{topic}': run
> `obsidian search query="{topic}" limit=20` if obsidian-cli is available,
> otherwise use Grep with pattern='{topic}' across the vault.
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

Fleeting notes are temporary holding areas for findings that lack enough
substance to become a proper note. They contain: the finding, source URL,
and why it matters. They must be promoted or discarded before session end.

For every note, follow a two-phase process:

**Phase 1 — Synthesis (this context, Opus/Sonnet):** Determine the complete
note content: title (claim-based), key insight, supporting evidence,
implications, wikilinks to related notes and parent MOC. Do not delegate
synthesis — the note body requires understanding of the research.

**Phase 2 — Write (Haiku sub-agent):** Once content is decided, delegate
file creation:

> "Create a note at `{path}` with this exact content: {frontmatter + body}.
> Use the template structure from `references/note-templates.md`.
> Tag: `source/ai-generated` + `confidence/uncertain`.
> Confirm when written."

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

Ask the user (if not already provided):
- What tool produced this? (for `source_tool` frontmatter field)
- What was the original query? (for `source_query`)
- Which MOC does this relate to?

Save the original in `10-Inbox/Clippings/` with a `CLIP-` prefix and
`processed: false`. Do not edit the original body.

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

> "Create `{path}` with this exact content: {frontmatter + body}.
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

Run this at the END of every session, regardless of what was done:

1. **No orphans**: Verify every note created links to a MOC. Process or
   discard all fleeting notes.

2. **Prepare closing content (this context, Opus/Sonnet)**: Draft the full
   content for all three mechanical updates before delegating:
   - MOC: list of new wikilinks with annotations, depth tracker row updates,
     questions to check off, new gap markers
   - Backlog: new items to append, items to check off (with wikilinks)
   - Research log: full log body (what was done, notes created, open
     questions, suggested next steps)

3. **Delegate all writes to a Haiku sub-agent** (one batch call):

   > "Apply these closing updates:
   > 1. In `40-Maps/{MOC}.md`: add under the correct headings — {wikilinks
   >    with annotations}. Update depth tracker row for {area} to {depth}.
   >    Check off {questions}. Add gap markers: {gaps}.
   > 2. In `00-Dashboard/Backlog.md`: append to High priority — {new items}.
   >    Check off: {completed items}.
   > 3. Create `50-Research-Log/{YYYY-MM-DD_HHmm}.md` with this content:
   >    {full log body}.
   > Confirm each update."

4. **Report to user**: Summarize the session, show MOC state, suggest next
   steps from the backlog.
6. **CLAUDE.md hygiene**: First check that `CLAUDE.md` exists at vault root.
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

## Model Strategy

Use three tiers based on task type, following Anthropic's model selection
guidance (Opus = multi-hour research; Sonnet = agentic tool use; Haiku =
sub-agent tasks):

**Main context** — run the skill with Opus for deep research sessions
(synthesis across conflicting sources, complex MOC design, multi-session
investigations). Use Sonnet for routine sessions (backlog review,
decomposing a single document, incremental updates).

**Haiku sub-agents** — all mechanical operations, regardless of the main
model. Haiku is explicitly designed for sub-agent tasks.

| Task | Model |
|------|-------|
| Intake — routing decision | Main (Opus/Sonnet) |
| Orient — synthesize vault state, propose focus | Main |
| Investigate — web research, insight extraction | Main |
| Investigate — detect contradictions, new questions | Main |
| Surface and Ask | Main |
| Decompose — atomicity decisions, propose plan | Main |
| Verification — analyze evidence, update confidence | Main |
| Closing — draft MOC/Backlog/Log content | Main |
| CLAUDE.md hygiene — detect drift, decide migration | Main |
| Orient — vault reads (MOC headers, Backlog, last log) | **Haiku** |
| Investigate — write note files | **Haiku** |
| Decompose — write note files (post-approval) | **Haiku** |
| Closing — apply MOC edits, Backlog updates, Research Log | **Haiku** |
| CLAUDE.md hygiene — write migrated notes | **Haiku** |

Rule: **never delegate synthesis; never write files in the main context.**
Use `obsidian-cli` for all vault queries — far cheaper in tokens than
reading raw files.

