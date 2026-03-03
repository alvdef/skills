# Vault Conventions

## Folder Structure

```
VAULT ROOT/
├── 00-Dashboard/        # Home note, master index
├── 10-Inbox/
│   ├── Fleeting/        # Raw captures (process within 48h or discard)
│   └── Clippings/       # AI reports, web clips preserved verbatim
├── 20-Sources/          # Markdown notes summarizing references (1 note per source)
│   ├── Articles/
│   └── Reports/
├── 30-Notes/
│   ├── Permanent/       # Atomic insight notes (one idea per note)
│   ├── Entity/          # Profiles: companies, people, platforms
│   └── Comparison/      # Side-by-side analyses
├── 40-Maps/             # Maps of Content (MOC) — topic hub notes
├── 50-Research-Log/     # Session logs (YYYY-MM-DD_HHmm.md)
├── 60-Outputs/          # Deliverables: briefs, reports
├── 70-Archive/          # Completed/deprecated (ask user first)
└── 80-Attachments/      # Binary files only: PDFs, images, CSVs
```

Folders organize by **note type**, never by topic. Topics use tags, links, MOCs.

**Sources vs Attachments**: Sources (`20-Sources/`) are markdown notes with frontmatter that summarize a reference. Attachments (`80-Attachments/`) are raw binary files embedded in notes via `![[file.pdf]]`.

## Naming

| Type          | Pattern                   |
|---------------|---------------------------|
| Permanent     | `PN-{concept-slug}`       |
| Source        | `SRC-{source-slug}`       |
| Entity        | `ENT-{name-slug}`         |
| Comparison    | `CMP-{subject}`           |
| MOC           | `MOC-{topic}`             |
| Log           | `YYYY-MM-DD_HHmm`        |
| Output        | `OUT-{type}-{subject}`    |
| Question      | `Q-{question-slug}`       |
| Hypothesis    | `HYP-{claim-slug}`        |
| Fleeting      | `FL-YYYYMMDD-{slug}`      |
| Clipping      | `CLIP-YYYYMMDD-{slug}`    |

Kebab-case. No spaces.

**Research Log collision rule**: if a log file with the same timestamp already exists, append a letter suffix: `YYYY-MM-DD_HHmma`, `YYYY-MM-DD_HHmmb`, `YYYY-MM-DD_HHmmc`, etc.

**Permanent note titles** state the core claim, not the topic. This makes wikilinks self-documenting when scanning a MOC — you read the finding without opening the note.

## Linking

- `[[wikilinks]]` for all internal links
- Every note links to its parent MOC + at least 1 related note
- `[[Name|Display Text]]` when the prefix makes links ugly
- Link back to source clippings or source notes as evidence trail
- No orphan notes — if it can't be linked, it belongs in Fleeting/

## Backlog

`00-Dashboard/Backlog.md` is a centralized research todo. The agent reads it at session start to propose what to work on, and appends new items at session end. The user can also edit it between sessions to reprioritize.

**Removal policy**: completed items are **removed entirely** from the backlog, not checked off. The session's research log already records what was done, so keeping completed items in the backlog creates clutter. Remove the line; the log provides the audit trail.

```markdown
# Research Backlog

## High priority
- [ ] {task} — from [[YYYY-MM-DD_HHmm]] session, see [[MOC-topic]]

## To explore
- [ ] {idea} — emerged from [[CLIP-source]] or user suggestion

## Parked
- [ ] {low priority item}
```

Items should include a wikilink to where they originated (log, MOC, or note) so context is recoverable.
