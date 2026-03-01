# Tag Taxonomy

Two-level hierarchical tags. A note can have many.

## Core Namespaces

### type/
`permanent` · `source` · `entity` · `comparison` · `moc` · `fleeting` ·
`clipping` · `log` · `output` · `question` · `hypothesis`

### status/
`seed` (🌱) · `growing` (🔄) · `mature` (✅) · `stale` (⚠️)

### confidence/
`verified` (cross-referenced primary) · `likely` (2+ secondary sources) ·
`uncertain` (single source or AI-generated) · `speculative` (hypothesis) ·
`contradicted` (evidence against)

AI-generated notes always start at `uncertain`.

### source/
`primary` · `secondary` · `tertiary` · `ai-generated` · `mixed`

### phase/
`broad-scan` · `triage` · `deep-dive` · `verification` · `synthesis`

## Domain-Specific Namespaces

Add namespaces relevant to the research domain here. Common additions:

```
domain/       → research area (supply-chain, manufacturing, compliance...)
industry/     → sector (plastics, electronics, textiles...)
geo/          → geography, hierarchical (china, china/guangdong...)
```

## Extending

1. Check this file — the concept may already exist
2. Kebab-case, 2-3 words max
3. Only for tags that apply to 3+ notes (otherwise use a wikilink)
4. Add new tags here with a brief description
5. To create a new namespace, add a section following the format above
