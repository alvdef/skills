# Note Templates

**All `created` fields use `YYYY-MM-DD HH:mm` (with time).** This is not optional — it disambiguates notes created on the same day and makes the research log timeline precise. Never write `2026-03-01`; always write `2026-03-01 14:30`.

## Permanent Note (30-Notes/Permanent/)

```yaml
---
title: Claim-based title stating the insight
aliases: []
tags:
  - type/permanent
  - status/seed
  - confidence/uncertain
  - source/ai-generated
  - phase/{current}
created: YYYY-MM-DD HH:mm
last_updated: YYYY-MM-DD HH:mm
related:
  - "[[PN-related]]"
up:
  - "[[MOC-parent]]"
---
```

Body: Core insight (1 paragraph) → Supporting evidence (linked sources) → Nuances/caveats (`> [!warning]` for confidence notes) → Implications.

## Source Note (20-Sources/)

```yaml
---
title: "Source Title as Published"
tags:
  - type/source
  - status/seed
  - confidence/{level}
  - source/{primary|secondary|tertiary}
created: YYYY-MM-DD HH:mm
source_url: "https://..."
source_type: article|report|whitepaper|video|book
source_date: YYYY-MM-DD
source_author: "Author or Organization"
up:
  - "[[MOC-parent]]"
---
```

Body: Summary (own words) → Key claims (linked to permanent notes) → Credibility assessment.

## Entity Profile (30-Notes/Entity/)

```yaml
---
title: Official Name
aliases:
  - Common Name
tags:
  - type/entity
  - status/seed
  - confidence/uncertain
  - source/ai-generated
entity_type: (domain-specific: company, person, platform, etc.)
created: YYYY-MM-DD HH:mm
last_updated: YYYY-MM-DD HH:mm
website: "https://..."
related:
  - "[[ENT-related]]"
up:
  - "[[MOC-parent]]"
---
```

Body: Overview → Key facts (table with domain-relevant attributes) → Assessment → Sources.

If the user hasn't defined what entity fields matter for their domain, ask them on first entity creation.

## Comparison (30-Notes/Comparison/)

```yaml
---
title: "X vs Y: dimension"
tags:
  - type/comparison
  - status/seed
  - confidence/uncertain
  - source/ai-generated
created: YYYY-MM-DD HH:mm
last_updated: YYYY-MM-DD HH:mm
compared_entities:
  - "[[ENT-x]]"
  - "[[ENT-y]]"
related:
  - "[[CMP-related]]"
up:
  - "[[MOC-parent]]"
---
```

Body: Context (what decision this informs) → Matrix table → Analysis.

## Fleeting Note (10-Inbox/Fleeting/)

```yaml
---
title: Brief description of the fragment
tags:
  - type/fleeting
  - source/ai-generated
created: YYYY-MM-DD HH:mm
up:
  - "[[MOC-parent]]"
promote_to: permanent|entity|comparison  # intended note type once mature
---
```

Body: The finding (1–3 sentences) → Source URL → Why it matters (what question does it answer?).

Fleeting notes are temporary — they must be promoted to a proper note type or discarded before session end. The `promote_to` field signals what note type this should become once there's enough substance.

## Clipping (10-Inbox/Clippings/)

```yaml
---
tags:
  - type/clipping
  - source/ai-generated
created: YYYY-MM-DD HH:mm
source_tool: (ask user)
source_query: (ask user)
processed: false
up:
  - "[[MOC-parent]]"
---
```

Preserve original content verbatim below frontmatter. Set `processed: true` after decomposition.

## Research Log (50-Research-Log/)

```yaml
---
tags:
  - type/log
created: YYYY-MM-DD HH:mm
research_project: "[[MOC-project]]"
---
```

Body: Session summary → Notes created (wikilinks) → Open questions → Next steps.

**All entity, note, and MOC mentions in the log prose must use `[[wikilinks]]`**, not just the "notes created" list. Use `[[ENT-slug|Display Name]]` aliases for readability. The log is part of the knowledge graph — plain-text mentions break the link trail.

## Output (60-Outputs/)

```yaml
---
title: "Output type: subject"
tags:
  - type/output
  - source/ai-generated
created: YYYY-MM-DD HH:mm
output_type: brief|report|summary
up:
  - "[[MOC-parent]]"
sources:
  - "[[PN-source-note]]"
  - "[[ENT-entity]]"
---
```

Body: Structure depends on `output_type`. Cite vault notes as `[[wikilinks]]` throughout — the output is part of the knowledge graph. Flag thin coverage with `> [!warning] Gap` callouts.

## Question Note (10-Inbox/Fleeting/ → 30-Notes/Permanent/ when answered)

```yaml
---
title: "Question: {question text}"
tags:
  - type/question
  - status/seed
  - source/ai-generated
created: YYYY-MM-DD HH:mm
last_updated: YYYY-MM-DD HH:mm
up:
  - "[[MOC-parent]]"
---
```

Body: The question → Why it matters → What answering it would unblock → Known leads (wikilinks to relevant notes).

Question notes live in `10-Inbox/Fleeting/` with a `Q-` prefix until answered. Once answered, promote to a Permanent note in `30-Notes/Permanent/` — the answer becomes the claim-based title.

## Hypothesis Note (30-Notes/Permanent/)

```yaml
---
title: "Hypothesis: {claim}"
aliases: []
tags:
  - type/hypothesis
  - status/seed
  - confidence/speculative
  - source/ai-generated
created: YYYY-MM-DD HH:mm
last_updated: YYYY-MM-DD HH:mm
evidence_for:
  - "[[PN-supporting]]"
evidence_against:
  - "[[PN-contradicting]]"
related:
  - "[[PN-related]]"
up:
  - "[[MOC-parent]]"
---
```

Body: The hypothesis (1 sentence) → Rationale → Evidence for (linked) → Evidence against (linked) → What would confirm or refute it.

Hypotheses start at `confidence/speculative`. As evidence accumulates, update confidence per the tag taxonomy. Use `> [!warning]` callouts for key assumptions.
