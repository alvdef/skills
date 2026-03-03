A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill for doing structured deep research in a Obsidian vault.

Feed it a topic or a deep research report. It creates atomic notes, entity profiles, comparison tables, and Maps of Content with wikilinks, structured frontmatter, and source citations. Sessions compound: the agent reads the vault before writing, links to what exists, and fills gaps.

The skill follows a [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten)-inspired methodology and enforces a specific structure for folder layout, file naming, frontmatter schemas, tagging, and linking. Every note must connect to an index page and at least one related note. Note titles state a claim, not a topic. AI-generated notes are tagged as uncertain until verified.


## How it works

```
Deep research report (Gemini, ChatGPT, Claude)
        │
        ▼
  Drop into vault as clipping
        │
        ▼
  Agent decomposes → follows source URLs → goes deeper
        │
        ▼
  Atomic notes ← wikilinks → Entity profiles
        │                          │
        ▼                          ▼
  MOC updated              Comparison tables
        │
        ▼
  Next session picks up where this one stopped
```


> Export Gemini Deep Research reports with [this Chrome extension](https://chromewebstore.google.com/detail/gemini-deep-research-expo/cihgcaoagikkalihppohhhhlkafeaomc) (not affiliated). Default Gemini exports strip URLs. The agent needs them to follow sources.

> For Claude Research, ask it for the most relevant sources used in the report.

## Install

```bash
claude skill add alvdef/skills --skill obsidian-deep-research
```

Requires:
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (Max or API)
- [Obsidian](https://obsidian.md/)
- [`obsidian` CLI](https://github.com/kepano/obsidian-skills) (auto-installed on first run)
- [`defuddle` CLI](https://www.npmjs.com/package/defuddle-cli) (optional, recommendended on `obsidian-skills`)


## Note types

| Type | What |
|------|------|
| Permanent | Atomic claim or insight |
| Entity | Org, tool, method |
| Comparison | Structured comparison |
| Source | What a source said |
| Clipping | Verbatim material |
| MOC | Hub for a topic cluster |
| Research log | Session audit trail |

Notes carry structured frontmatter (type, status, confidence, source, domain), annotated wikilinks, and a parent MOC link.

## Vault structure

```
VAULT ROOT/
├── 00-Dashboard/        # Home note, master index
├── 10-Inbox/
│   ├── Fleeting/        # Raw captures
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
├── 70-Archive/          # Completed/deprecated
└── 80-Attachments/      # Binary files only: PDFs, images, CSVs
```


## Status

Sharing early because it worked. Feedback welcome on note quality, model routing, missing features, and anything that breaks.

## Built with

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- [obsidian-skills](https://github.com/kepano/obsidian-skills) by kepano
- [Obsidian](https://obsidian.md/)

MIT license.
