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

Also read `CLAUDE.md` (at vault root) for the user's research domain,
current priorities, and project-specific rules.

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

Read `00-Dashboard/Backlog.md` for pending research items and open threads.
If the user didn't specify a focus, propose items from the backlog.

Search the vault for existing coverage:
```
obsidian search query="{topic}" limit=20
```

If a MOC exists, read it. Report what the vault already covers and where the
gaps are. If no MOC exists, create one (see `references/moc-format.md`) and
confirm the initial scope and key questions with the user.

Ask the user (if not already clear):
- What specific question or angle to focus on?
- Any constraints on scope or sources?

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

For every note created:
- Use the correct template from `references/note-templates.md`
- Tag with `source/ai-generated` + `confidence/uncertain`
- Link to the parent MOC and at least 1 related note

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

After user approval:
- Create each note using templates from `references/note-templates.md`
- Write as synthesis in your own words — do not copy-paste
- Tag: `source/ai-generated` + `confidence/uncertain`
- Link back to the clipping as evidence: `Source: [[CLIP-{original}]]`
- Link to parent MOC and related existing notes

For concepts that overlap with existing notes, propose updating instead of
duplicating. Ask the user.

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

1. **No orphans**: Every note created must link to a MOC. Process or discard
   all fleeting notes.
2. **Update the MOC**: Add new links, refresh depth tracker, check off
   answered questions, add gap markers for new holes.
3. **Update the backlog**: Append new research threads, open questions, and
   contradictions to `00-Dashboard/Backlog.md`. Check off any items completed
   this session. Include wikilinks to the relevant MOC or note for context.
4. **Write a research log**: Create or append to
   `50-Research-Log/YYYY-MM-DD_HHmm.md` (see template in
   `references/note-templates.md`). Document: what was done, notes created,
   open questions, suggested next steps.
5. **Report to user**: Summarize the session, show MOC state, suggest next
   steps from the backlog.

## Cost Efficiency

This skill is designed to run on lower-cost models (Sonnet, Haiku). Vault
reads, searches, note creation, and MOC updates don't require heavy reasoning.
Use `obsidian-cli` for vault queries instead of reading raw files — it's
orders of magnitude cheaper in tokens. Reserve deeper reasoning for synthesis
and insight extraction during the Investigate and Decomposition steps.
