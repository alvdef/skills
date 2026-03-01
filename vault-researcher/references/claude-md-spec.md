# CLAUDE.md Specification

`CLAUDE.md` lives at the vault root and loads at every Claude Code session.
It is a **project README for the agent** — stable domain context. It is NOT
a research notepad, a backlog, or a summary of findings. Those live in the
vault.

## The Rule

If a piece of information would be correct next week without any changes,
it belongs in `CLAUDE.md`. If it changes as research progresses, it belongs
in the vault.

## Sections That Belong in CLAUDE.md

| Section | Purpose | Changes? |
|---------|---------|----------|
| **Client** | Who is the client, what do they do | Rarely |
| **Research Domain** | What we're investigating and why | Rarely |
| **Active MOCs** | Wikilinks to live maps — just navigation | When new MOC is created |
| **Agent Rules** | Behavioral constraints (language, confidence tags, approval required) | Rarely |

That's it. Four sections.

## Sections That Do NOT Belong in CLAUDE.md

| Common drift pattern | Where it actually belongs |
|---------------------|--------------------------|
| Resolved decisions ("QINCheck is the viable option") | `PN-*` note with `confidence/verified` |
| Pending decisions ("Scrape vs API?") | `00-Dashboard/Backlog.md` — High priority |
| Current priorities (numbered list) | `00-Dashboard/Backlog.md` |
| Key references (URLs) | `ENT-*` or `SRC-*` note in vault |
| Research findings | `PN-*`, `CMP-*`, or `ENT-*` note |
| Session notes | `50-Research-Log/YYYY-MM-DD_HHmm.md` |

## Why This Matters

When CLAUDE.md accumulates research, the same information ends up in two
places — the file and the vault. They drift. Findings in CLAUDE.md don't
benefit from wikilinks, tags, confidence tracking, or MOC integration.
The vault becomes incomplete; CLAUDE.md becomes a parallel (and unreliable)
knowledge base.

The vault is the second brain. CLAUDE.md is just the door.

## Template

```markdown
# CLAUDE.md

<!--
  Stable project context. Read at every session start.
  Vault conventions → vault-researcher skill references/
  Research state → vault (MOCs, Backlog.md, Research-Log/)
  This file answers: what are we researching and who for?
-->

## Client

{Client name and one-paragraph description of their business and workflow.}

## Research Domain

{What we're investigating, and the problem it solves. 2–4 sentences.}

## Active MOCs

- [[MOC-{topic}]]

## Agent Rules

- All AI-generated notes: `source/ai-generated` + `confidence/uncertain`. No exceptions.
- Always check the vault for existing coverage before researching.
- After every session, update the MOC and write a research log.
- Ask me before creating new MOCs or changing priorities.
- Language: {notes language}. Frontmatter keys and tag values always in English.
```

## Hygiene Check (run at session close)

At the end of every session the agent checks whether `CLAUDE.md` has grown
beyond these four sections. If it has:

1. Identify what drifted in (findings, URLs, decisions, priorities)
2. Migrate each item to the correct vault location (see the table above)
3. Remove the migrated content from `CLAUDE.md`
4. Report to the user: "Migrated X items from CLAUDE.md to the vault."

If the user intentionally added something to `CLAUDE.md` during a session,
ask before migrating: "I see you added {X} to CLAUDE.md — should I move
it to {suggested location} or keep it here?"
