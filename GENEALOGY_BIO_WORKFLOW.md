# Genealogy Biography Workflow v1

Goal: produce biographies that are rich, source-grounded, and resistant to relationship/date hallucinations.

## Core rule
Final biography must be written from both:
1. structured factual artifacts
2. prose source documents

Facts control chronology/relationships.
Prose supplies texture, anecdotes, quotes, and voice.

## Case folder structure
For each target, create:

```text
research/<slug>/
  01_inventory.md
  02_narrative_sources.md
  03_relationship.yml
  04_facts.yml
  05_prose_excerpts.md
  06_timeline.md
  07_evidence_notes.md
  08_evidence_matrix.md
  sources/
  ocr/
  drafts/
  review/
```

## Step 1 — Inventory
Record:
- target person/couple
- GEDCOM IDs
- FamilySearch IDs
- spouse / parents / children
- all media titles + URLs
- source types: obituary, life sketch, death cert, local history, family record, diary, photo caption

Output: `01_inventory.md`

## Step 2 — Narrative source identification
Classify sources as:
- factual artifact
- narrative source
- hybrid

Narrative-bearing sources include life sketches, memoirs, diaries, obituaries with prose, funeral writeups, local histories, descendant compilations, anecdotal photo captions, and "history/story" documents.

Output: `02_narrative_sources.md`

## Step 3 — Relationship trace
Use deterministic GEDCOM traversal when possible.
Record exact path to Erik.

Output: `03_relationship.yml`

Fields:
```yaml
relationship_to_erik:
  label: 2x great-grandparents
  path:
    - Erik Byron Taylor
    - Byron William Taylor
    - ...
  verified_from: GEDCOM family chain
  confidence: high
```

## Step 4 — OCR / extraction
Download media and extract text.
Every file gets a status:
- complete
- partial
- unreadable

Save raw extraction only; no interpretation here.

Output: `ocr/*`

## Step 5 — Facts extraction
From OCR + GEDCOM, create structured facts.
Each fact should include source and confidence.

Output: `04_facts.yml`

Example:
```yaml
birth:
  - value: 1864-01-30
    source: death_certificate
    confidence: high
  - value: 1864-01-04
    source: obituary
    confidence: medium
marriage:
  date: 1882-08-03
  source:
    - obituary
    - life_sketch
  confidence: high
```

## Step 6 — Prose excerpts
Pull out rich passages from prose documents.
Do not flatten them into database facts.
For each excerpt, include:
- source
- page or image
- author
- source type (firsthand / family memoir / derivative)
- suggested use

Output: `05_prose_excerpts.md`

## Step 7 — Timeline
Build chronology from facts.
Conflicts stay visible.

Output: `06_timeline.md`

## Step 8 — Evidence notes
Record what is:
- confirmed
- probable
- disputed
- family memory only

Output: `07_evidence_notes.md`

## Step 9 — Evidence matrix
Table of major claims and support/conflicts.

Output: `08_evidence_matrix.md`

Suggested columns:
- Claim
- Source A
- Source B
- Status
- Notes

## Step 10 — Authoring readiness check
Before drafting, verify the case is actually ready to write.

Output: `09_authoring_readiness.md`

Minimum checks:
- relationship path verified
- highest-value narrative sources identified
- core narrative sources extracted enough to know what they contain
- key newspaper/certificate items extracted
- major date/lineage conflicts surfaced in evidence notes
- unresolved items listed explicitly
- enough prose excerpts exist to support a non-thin biography

Rule:
Do **not** draft the biography until this file says the case is ready.

## Step 11 — Drafting
Writer model gets only:
- relationship file
- timeline
- facts
- prose excerpts
- evidence notes
- evidence matrix
- authoring readiness file

Prompt rule:
- use prose documents for richness
- use facts for structure
- do not invent bridges
- preserve uncertainty when conflict exists

Output: `drafts/draft_a.md`

## Step 12 — Review
Independent review looks for:
- wrong generation / lineage
- date/place mismatches
- unsupported children counts
- claims present in draft but absent from artifacts
- conflicts silently resolved without evidence

Output: `review/discrepancy_report.md`

## Step 13 — Final biography
Final writer uses artifacts + discrepancy report.

## Source reliability classes
- A: firsthand / near-firsthand (diary, dictated history, contemporary obituary, certificate)
- B: family memoir / descendant history
- C: derivative compilation / local history summary

## House rules
- Never narrate beyond the evidence.
- Never let the writer infer lineage from memory.
- Never collapse conflicting dates into one without noting the conflict.
- Never claim a full source was processed unless extraction actually exists on disk.
- Quotes should come from saved excerpts, not memory.

## Minimum viable artifacts for each biography
1. Inventory
2. Narrative source identification
3. Relationship trace
4. Facts file
5. Prose excerpts
6. Timeline
7. Evidence notes
8. Evidence matrix
9. Authoring readiness check
10. Final biography
