# Note Type Decisions

The reasoning behind each note type and the boundary rules for ambiguous cases. Read this when the cascade in `SKILL.md` doesn't clearly resolve which type to create, or when you're creating a type you haven't used recently.

## Fleeting Notes

**Canonical basis:** Sönke Ahrens' *How to Take Smart Notes* (2017) — quick captures meant to be processed and discarded within one to two days. Luhmann himself never used this term; his working notes were informal reading excerpts that were not filed permanently.

**What it is:** The lowest-fidelity note type — a triage buffer for information whose significance, type, or relationship to existing knowledge is not yet clear. Raw text, quotes, observations, source URLs. Minimal structure, no linking required.

**Gray areas:**
- *"I have a finding but I'm not sure if it's a permanent note or a hypothesis."* Create a fleeting with `promote_to: permanent` or `promote_to: hypothesis`. Don't agonize — the cascade will resolve it at processing time.
- *"This is a small fragment from a source."* If you already have a source note for it, add the fragment there instead.

**Anti-patterns:** Fleeting notes that survive across sessions. A vault with 20+ unprocessed fleeting notes is hoarding, not researching. If you keep creating fleeting notes on the same topic, you probably have enough substance for a permanent note — write it.

## Clippings

**Canonical basis:** Not a standard Zettelkasten type. Maps to what Ahrens and zettelkasten.de consider *pre-notes* — material preserved verbatim before reformulation. Closest analog: Tiago Forte's progressive summarization Layer 1 (raw capture).

**What it is:** Verbatim or near-verbatim content from a source: a full passage, report, data table, code snippet. Preserved in original form as input material for decomposition.

**Gray areas:**
- *"Should I clip a whole article or just the relevant passage?"* Clip the whole document if it's an AI-generated report or a document the user explicitly wants preserved. For web articles, create a source note with a summary instead — clipping entire articles is the collector's fallacy.
- *"The user pasted something in chat."* Save to `10-Inbox/Clippings/` before processing.

**Anti-patterns:** Clippings that remain unprocessed. Every clipping should eventually produce source notes, permanent notes, or both — then get marked `processed: true`. A clipping is evidence of *intake*, not *knowledge*.

## Source Notes

**Canonical basis:** Directly maps to Luhmann's bibliographic cards (Literaturzettel) and Ahrens' "literature notes." Ahrens considers these a subcategory of permanent notes — they persist indefinitely as reference anchors.

**What it is:** A note *about a source*, not about any individual idea within it. One per external source, always. Contains: bibliographic metadata, a summary of the source's argument in your own words, and links to permanent/entity/question/hypothesis notes extracted from it.

**Gray areas:**
- *"The source makes one strong claim and nothing else."* Still create a source note (metadata + summary) AND a permanent note (the claim, reformulated). The source note answers "what did this source say?"; the permanent note answers "what idea did I extract?"
- *"Two sources cover the same ground."* Never merge sources. Each gets its own note. If their claims converge, the *permanent notes* they generate may be the same — link both source notes from that permanent note.

**Anti-patterns:** Substantive arguments, original analysis, or your own claims living inside a source note. Those belong in permanent notes linking back to the source. A source note that reads like an essay is doing two jobs.

## Entity Notes

**Canonical basis:** Not in canonical Zettelkasten literature. Recognized as a practitioner pattern — Sascha Fast (zettelkasten.de) describes entity-type Zettels with consistent templates for similar entity types.

**What it is:** A profile of a specific, named thing (person, organization, technology, platform, methodology, compound, structure) that serves as a reference anchor. Answers "what is X?" — ontological, not epistemic.

**Gray areas:**
- *"Is this an entity or a permanent note?"* Entity: "what X is" (definition, attributes, classification). Permanent: "what is true *about* X" (a claim, argument, insight involving X). If the note makes an argument, it's a permanent note that *links to* the entity.
- *"The entity is well-known (e.g., 'Python')."* Only create if you need to track specific attributes or relationships not available externally, or if 3+ vault notes reference it. If it's just a mention, a wikilink to a future entity note is enough — create it when the threshold is crossed.
- *"I found new info about an existing entity."* Add attributes to the entity note. If the new info is a *claim about* the entity, create a permanent note that links to the entity.

**Anti-patterns:** Entity notes that contain arguments or analysis. An entity note for "Company X" that includes "Company X's strategy is flawed because..." — the analysis belongs in a permanent note.

## Question Notes

**Canonical basis:** Not in canonical literature. Recognized by zettelkasten.de practitioners using `#type/question` tags. Function as "gap notes" documenting what is not yet known.

**What it is:** A specific, answerable research question requiring further investigation. Precise enough that you would recognize an answer if you found one.

**Gray areas:**
- *"Is this a question or a hypothesis?"* A question asks ("Does X cause Y?"); a hypothesis asserts ("X likely causes Y, based on evidence Z"). If you have enough evidence to take a position, even tentatively, it's a hypothesis.
- *"The question is very broad."* Narrow it until it's answerable. "What about memory?" is a topic, not a question. "Does spaced repetition improve retention for procedural knowledge?" is a question.
- *"The question has no connection to existing notes."* It's probably too disconnected from current research. Hold in a fleeting note or add to the backlog instead.

**Anti-patterns:** Vague questions that can't be closed. Every question should have a clear "done" state: promote to a permanent note where the answer becomes the claim-based title.

## Hypothesis Notes

**Canonical basis:** Aligns with Sascha Fast's building block type: "Hypotheses formulate statements on how reality actually is." Also aligns with zettelkasten.de's concept of treating tentative claims as first-class objects.

**What it is:** A testable claim that could be true or false based on current evidence. States the claim, summarizes evidence for and against (linked), specifies what would confirm or refute it.

**Gray areas:**
- *"Is this a hypothesis or a permanent note?"* If the claim is well-established and you have high confidence → permanent. If speculative, contested, or based on limited evidence → hypothesis. Hypotheses carry an implicit "as of this evidence" qualifier.
- *"Evidence accumulated and now I'm confident."* Promote to permanent note. Keep the hypothesis frontmatter history (evidence_for, evidence_against) in the new note as a collapsed section or callout.
- *"The hypothesis was wrong."* Mark `confidence/contradicted`, add `> [!danger]` callout, and link to the permanent note or source that refuted it. Do not delete — the refutation is knowledge.

**Anti-patterns:** Hypotheses without specified confirmation/refutation criteria. If you can't say what evidence would change your mind, it's not a hypothesis — it's an opinion.

## Comparison Notes

**Canonical basis:** Not in canonical literature. Maps to Sascha Fast's "model" building block — relating entities to each other, providing part-to-part and part-to-whole relationships.

**What it is:** An explicit analysis of similarities, differences, tradeoffs, or relationships between 2+ specific things. The comparison *is* the point — not incidental to a claim.

**Gray areas:**
- *"Is this a comparison or a permanent note?"* If the note makes a claim that involves comparing ("X is more effective than Y because...") → permanent note. If the note's purpose is to *map the relationship space* without arriving at a single claim → comparison. Comparisons are analytical tools; permanent notes are conclusions.
- *"I'm comparing two approaches but I have a clear winner."* Write both: a comparison note mapping the dimensions, and a permanent note stating the conclusion. The comparison is the evidence; the permanent note is the finding.

**Anti-patterns:** Comparisons without explicit dimensions. A note that says "X and Y are different" without specifying *how* or *along what axes* is not a comparison — it's a vague observation.

## Permanent Notes

**Canonical basis:** The core of Luhmann's system (Hauptzettel). Ahrens: "written as if for print," self-contained, one idea per note, never discarded. Christian Tietze's Principle of Atomicity (zettelkasten.de, 2013): "one knowledge building block per note."

**What it is:** A single, self-contained knowledge claim, principle, argument, or insight expressed in your own terms, independent of any specific source. The intellectual core of the vault. Sascha Fast's taxonomy of building blocks: concept, argument, model, hypothesis, or empirical observation.

**Gray areas:**
- *"Too broad or too narrow?"* Too broad: contains information that could be true even if other parts were false (split it). Too narrow: lacks context to be understood standalone, or only makes sense as part of another note (merge it). When 2 of 3 atomicity tests (title, link, size) suggest splitting, split.
- *"Should I extend an existing note or create a new one?"* Extend only when the new information directly elaborates the *same atomic idea* without changing scope. If the note's title would need to change → create new.
- *"This is just a definition."* Definitions of named things → entity notes. Definitions of concepts ("what is neuroplasticity?") can be permanent notes if they carry a claim or non-obvious framing. Pure encyclopedia entries with no intellectual contribution → skip or entity note.

**Anti-patterns:**
- The encyclopedia entry: describes without claiming. "PFAS are persistent chemicals used in..." is a description. "PFAS persistence makes traditional water treatment insufficient" is a claim.
- The mega-note: covers three topics because they came from the same source. Split by idea, not by source.
- The orphan: no inbound or outbound links. If it can't be linked to anything, it may be too disconnected from current research.

**The stranger test** (additional atomicity check): a reader with domain knowledge but no context about your research goals should understand the note completely on its own.

## MOC (Maps of Content)

**Canonical basis:** Maps to Luhmann's Übersichtszettel (hub/overview notes) and zettelkasten.de's "structure notes." Nick Milo's LYT framework popularized the MOC term. A meta-note about other notes and their relationships.

**What it is:** An organizational note that navigates, sequences, and reveals structure within a topic cluster. Every link in a MOC has an annotation explaining why it's included and what the linked note contributes.

**Gray areas:**
- *"Should I create a new MOC or add a section to an existing one?"* Add a section first. Create a new MOC only when: (a) the existing MOC has grown past 20–25 links and a sub-area warrants its own overview, or (b) the cluster has no natural parent MOC.
- *"Sub-MOC promotion?"* When a section within a MOC reaches 7+ notes, offer to promote it. The child links back via `parent_moc`; the parent replaces the section with a link under "Related maps."
- *"How many MOCs is healthy?"* Ratio of 15–25 permanent notes per MOC. 35 MOCs for 322 notes is bloated. MOCs should emerge bottom-up from accumulated content — never created proactively before content exists.

**Anti-patterns:** Unannotated link lists. A MOC that is just `[[PN-a]], [[PN-b]], [[PN-c]]` without explanations is a failed MOC — the annotations are where the organizational thinking happens. Every link must say *why*.

---

## Atomicity Deep Dive

The single hardest judgment call in a Zettelkasten. "One idea per note" is clear in principle, ambiguous in practice. Three independent checks:

**Title test:** If the note can be given a clear, specific, declarative title (e.g., "Cortisol suppresses hippocampal neurogenesis under chronic stress"), it is atomic. If the title requires "and" or covers multiple distinct claims, split.

**Link test:** If the note would need to be linked from two fundamentally different contexts for two different reasons, it likely contains two ideas that should be separated.

**Stranger test:** A reader with domain knowledge but no context about your research goals should understand the note completely.

**Size guideline:** 100–300 words. Luhmann's DIN A6 cards held roughly 150–200 words — enough for a developed paragraph, not an essay. Shorter notes may lack standalone context; longer notes probably cover multiple ideas.

**When to split vs merge:** If 2 of 3 tests suggest splitting, split. If separating content makes both notes less useful, the split is too aggressive — merge back.

**New note vs extend existing:** Create new when the idea is a distinct building block. Extend only when new information elaborates the same atomic idea without changing scope. If the title would need to change, create new.

---

## Linking Discipline

Links generate the Zettelkasten's distinctive value — and are where AI agents most commonly fail. Link context is the most important principle: the explicit statement of *why* is created knowledge.

**Three link types with distinct purposes:**

*Structure links* flow from MOCs to notes. Hierarchical and navigational — they organize notes into readable sequences. Live inside MOCs with annotations explaining each note's role.

*Content links* connect notes laterally across topics. These generate unexpected insights. Each must include a relationship phrase: "contradicts," "provides evidence for," "is an instance of," "extends."

*Sequence links* connect notes in a reasoning chain — the digital Folgezettel. "This observation leads to [[next]]." Valuable because they preserve the *order of reasoning*, making it possible to reconstruct how a conclusion was reached.

**Over-linking:** following a link provides no insight — topically related but adds nothing. Rule: link only when you can articulate what the reader gains. Symptom: more than 8 outbound links on a single note.

**Under-linking:** orphan notes with no connections. Rule: every note (except fleeting and clippings) must have at least 1 outbound content link and be referenced from at least 1 MOC or other note.

---

## Five Vault Pitfalls

**Collector's fallacy:** saving everything without processing. Tietze (zettelkasten.de): "Having a text at hand does nothing to increase our knowledge." Every clipping and fleeting note that persists beyond a session is a symptom.

**Encyclopedia anti-pattern:** treating the vault as a database. Notes that describe without claiming produce static references that never generate insights. Prioritize claims over descriptions.

**Orphan notes:** signal a linking discipline breakdown. If a note can't be linked, it may not belong in the vault — or it needs more context before it's ready.

**Over-categorization:** too many tags, rigid hierarchies, elaborate metadata. The zettelkasten.de consensus leans toward minimalism — a few type tags, a few status tags, let links do the organizing.

**MOC bloat:** MOCs proliferating beyond usefulness. Healthy ratio: 15–25 permanent notes per MOC. Unannotated link-list MOCs add no value.

---

## Contradictions Protocol

When sources disagree:
1. Create separate permanent notes for each position, clearly stating each claim.
2. Link them with "contradicts" or "provides counter-evidence to" annotations.
3. Optionally create a comparison note mapping the disagreement dimensions.
4. If evidence strongly favors one side, create a hypothesis stating your current assessment and why.

**Never silently overwrite an existing note with contradictory information.** Create new notes and link with "supersedes" or "updates" to preserve intellectual history.
