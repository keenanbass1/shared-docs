---
visibility: public
title: Future Work
description: Planned improvements and recommendations for underdeveloped synthesis categories
category: meta
tags: [planning, future-work, recommendations]
updated: 2026-01-09
status: ongoing
---

# Future Work: Recommendations for Syntheses Folder

This document tracks planned improvements and recommendations for underdeveloped categories in the syntheses folder.

---

## Priority Recommendations

### 1. Create Master Index (HIGH PRIORITY)

**Status:** Not started

**Action:** Create `/syntheses/README.md` or `/syntheses/INDEX.md`

**Contents:**
- Overview of the three-tier knowledge system (as mentioned in repo README)
- Directory structure explanation with brief descriptions
- Quick links to major synthesis documents
- Status indicators (🟢 Complete, 🟡 In Progress, ⚪ Stub)
- Last updated dates
- Estimated reading time / depth level for each major document

**Suggested Structure:**
```markdown
# Syntheses: Comprehensive Reference Documents

## Major Synthesis Documents
- [Philosophy Brain](./philosophy/philosophy-brain.md) - 2,500+ years across 6 traditions 🟢
- [Literature Brain](./literature/literature-brain.md) - Genre theory + canonical works 🟢
- [Art Brain](./art/art-brain.md) - Ancient → Contemporary art movements 🟢
- [Music Brain](./music/music-brain.md) - 14 super-genres, 280+ genres 🟢
- [Neurodivergent Brain](./health/neurodivergent-brain.md) - Critical psychiatry framework 🟡

## Research Collections
- [Surveillance](./research/surveillance/) - Australian metadata, pattern detection, effectiveness
- [Neurodiversity](./research/) - BPD-autism, differing minds, intelligence measures
- [Substances](./research/) - Psychedelics research
- [Speculative](./research/) - UAPs, alternative life, US-China dynamics

## Specialized Areas
- [Development & Automation](./dev/ai-research/) - Personal automation ecosystem
- [Tattooing](./tattooing/) - Historical origins and cultural context
```

---

## Underdeveloped Categories

### 2. Health Category

**Current Status:** 1 file - `neurodivergent-brain.md` (in progress)

**Recommendation:** Develop into comprehensive health synthesis

**Suggested Structure:**
- **Option A:** Expand `neurodivergent-brain.md` to cover broader health topics
- **Option B:** Create `health-brain.md` as master synthesis covering:
  - Neurodivergent healthcare navigation
  - Evidence-based nutrition
  - Exercise science and movement
  - Mental health frameworks
  - Sleep science
  - Chronic condition management

**Priority:** Medium

**Rationale:** Health is a core life domain that intersects with neurodiversity research already present. A comprehensive health synthesis would complement the existing critical psychiatry framework.

---

### 3. Tattooing Category

**Current Status:** 1 file - `tattooing-origins.md` (complete)

**Recommendation:** Decide between expansion or maintaining as single-topic synthesis

**Option A - Expand into Tattooing Brain:**
- Historical origins (current content)
- Technique fundamentals (hand-poke, machine, rotary)
- Style traditions (Japanese, American Traditional, Realism, etc.)
- Health and safety protocols
- Cultural appropriation and ethics
- Apprenticeship and professional development
- Business management for artists

**Option B - Keep focused:**
- Maintain current historical focus
- Add separate focused documents as needed
- Don't force comprehensive synthesis if expertise/interest doesn't warrant it

**Priority:** Low

**Rationale:** Depends on personal interest level. If tattooing is a professional/serious hobby domain, expand. If it's peripheral interest, keep focused historical document.

---

### 4. Literature Category Consolidation

**Current Status:**
- ✅ `literature-brain.md` (complete, comprehensive)
- 📦 `genres.md` (archived - subsumed by literature-brain)
- 📦 `essential-canon.md` (archived - subsumed by literature-brain)

**Recommendation:** No further action needed

**Status:** Complete

The literature-brain.md successfully consolidates genre theory, canonical works, and literary analysis into a single comprehensive reference.

---

## Additional Recommendations

### 5. Create CONTRIBUTING.md

**Status:** Not started

**Purpose:** Guidelines for adding new syntheses based on existing patterns

**Contents:**
- Entry format requirements (frontmatter, structure, symbols)
- Detail level expectations (as described in CLAUDE.md for philosophy-brain)
- When to create new synthesis vs. expand existing
- Research methodology standards
- Citation and source requirements

**Priority:** Medium

**Location:** `/syntheses/CONTRIBUTING.md`

---

### 6. Metadata Tracking

**Status:** Not started

**Purpose:** Track synthesis document metrics

**Suggested metrics to add to master index:**
- Word/line counts for each major synthesis
- Estimated reading time
- Depth level (survey vs. comprehensive reference vs. deep learning)
- Completion percentage for in-progress documents

**Priority:** Low

**Implementation:** Could be automated via script or manually maintained in master index

---

### 7. Cross-References Between Syntheses

**Status:** Partial (some exist organically)

**Purpose:** Strengthen interconnections in knowledge graph

**Examples:**
- philosophy-brain ↔ literature-brain (philosophical movements influence literary movements)
- art-brain ↔ music-brain (artistic movements parallel musical developments)
- neurodivergent-brain ↔ differing-minds research (consciousness variation connects to neurodiversity)

**Priority:** Low

**Implementation:** Add "Related Syntheses" sections to major documents

---

## Completed Improvements (2026-01-09)

- ✅ Standardized file naming across all categories
- ✅ Added consistent YAML frontmatter to all markdown files (visibility, title, description, category, tags, updated, status)
- ✅ Fixed directory typo and flattened structure: `surveillance/surveilence/` → `surveillance/`
- ✅ Archived superseded literature documents to `literature/archive/`
- ✅ Updated major synthesis files with consistent frontmatter structure

---

## Next Steps

1. **Immediate:** Create master index (`README.md`)
2. **Short-term:** Decide on health category expansion approach
3. **Medium-term:** Create CONTRIBUTING.md guidelines
4. **Ongoing:** Continue developing neurodivergent-brain.md
5. **Future:** Consider tattooing expansion if interest warrants

---

## Notes

- This document should be updated as recommendations are implemented
- Mark completed items with ✅ and move to "Completed Improvements" section
- Add new recommendations as they emerge
- Review quarterly to reassess priorities
