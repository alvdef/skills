# MOC Format

Create a MOC at the start of any research initiative. Every note must have
a parent MOC.

```yaml
---
title: MOC title
tags:
  - type/moc
  - status/growing
created: YYYY-MM-DD HH:mm
last_updated: YYYY-MM-DD HH:mm
---
```

```markdown
# {Title}

> [!abstract] Scope
> What this covers. What is out of scope.

## Key questions
- [ ] Question 1
- [x] Answered → see [[PN-answer]]

## {Subtopic 1}
- [[PN-note]] — annotation
- ⚠️ **Gap**: {what's missing}

## Depth tracker

| Area | Depth | Priority |
|------|-------|----------|
| Subtopic 1 | 🔄 Growing | High |
| Subtopic 2 | 🌱 Seed | Medium |

## Related maps
- [[MOC-sibling]]
```

Depth: 🌱 Seed (1-3 notes) · 🔄 Growing (4-10) · ✅ Mature (10+)

## Updating

1. Place new wikilinks under correct heading with annotation
2. Update depth tracker
3. Check off answered questions
4. Add `⚠️ **Gap**:` for new holes
5. Update `last_updated`
