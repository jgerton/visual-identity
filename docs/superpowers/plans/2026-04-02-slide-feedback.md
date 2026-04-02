# Slide Feedback Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Update 4 of 6 slides in the visual-direction skill based on Elio's review, plus update the output format and anti-slop validation to match.

**Architecture:** All changes are in one file (`skills/visual-direction/SKILL.md`). Each task replaces a specific step's content while preserving the step structure and surrounding steps. The Step 1 announcement list, Step 9 validation, Step 10 output format, and Step 10 decision log all need updates to reflect the renamed slides.

**Tech Stack:** Markdown skill instructions (SKILL.md)

---

## File Map

| Action | File | Responsibility |
|--------|------|----------------|
| Modify | `skills/visual-direction/SKILL.md` | All changes: Steps 2, 4, 5, 7, 1, 9, 10 |

---

### Task 1: Update Step 2 -- Project Context

**Files:**
- Modify: `skills/visual-direction/SKILL.md:70-85` (Step 2)

- [ ] **Step 1: Replace Step 2 content**

Replace the entire Step 2 section (from `## Step 2: Slide 0 -- Project Context (~500 chars)` through `**Save checkpoint:** Do not write to brand-brief.md yet. Hold the slide text in working memory until Step 10.`) with:

```markdown
## Step 2: Slide 0 -- Project Context (~500 chars)

**Source:** `client_input.project_synthesis.summary`, `client_input.project_synthesis.core_challenge`, `client_input.project_synthesis.success_criteria`

Write 3 short blocks (1-2 lines each):
1. What the organization is and what it does (category, scope, key facts)
2. The project challenge: why the visual identity needs to change (the gap between substance and perception)
3. The objective: what success looks like for this project (drawn from success_criteria)

**Objective:** Contextualize the project clearly and objectively. The client should understand what is being done, why, and what the target outcome is. Establish shared understanding before expressing any opinion.

**Excludes:** External events or trends unless directly referenced in the client's own materials. Personality attributes. Any solution element. Brand Foundation content (that is Slide 1).

**Character limit:** 500 characters total. Count characters and report the count.

**Save checkpoint:** Do not write to brand-brief.md yet. Hold the slide text in working memory until Step 10.
```

- [ ] **Step 2: Verify edit**

Read the modified section and confirm:
- Step 2 header is unchanged
- The 3-block structure now describes organization/challenge/objective (not sector-moment/perception-problem/stakes)
- Source list includes `success_criteria` instead of `additional_context`
- Character limit is still 500
- Step 3 follows immediately after

- [ ] **Step 3: Commit**

```bash
git add skills/visual-direction/SKILL.md
git commit -m "feat(visual-direction): update project context slide to focus on organization + challenge + objective"
```

---

### Task 2: Replace Step 4 -- Brand Personality becomes Brand DNA

**Files:**
- Modify: `skills/visual-direction/SKILL.md:104-118` (Step 4)

- [ ] **Step 1: Replace Step 4 content**

Replace the entire Step 4 section (from `## Step 4: Slide 2 -- Brand Personality (~660 chars)` through `**Save checkpoint:** Hold in working memory until Step 10.`) with:

```markdown
## Step 4: Slide 2 -- Brand DNA (~660 chars)

**Source:** `positioning` (all components), `personality.attributes`, `personality.archetypes`, `visual_exploration.recommended`, `audience_problem`

Synthesize 5-10 brand DNA keywords from the full diagnosis. Prefer fewer keywords with more depth. Each keyword is a visual concept-word that carries a layer -- it may relate to form, materiality, personality, or origin. Read together, they form a portrait of the brand without needing explanation.

**Output format:**
- Keywords presented as a slash-separated list (e.g., "Subterrâneo / Rizomático / Soberano / Territorial / Orgânico")
- Below the keywords, a brief contextual line (1-2 sentences) explaining what concept the keywords collectively point toward

**What brand DNA is NOT:**
- Not institutional values ("integrity," "excellence")
- Not voice attributes ("friendly," "authoritative")
- Not Aaker personality dimensions used directly (though they inform the synthesis)
- Not generic adjectives ("innovative," "professional," "modern")

**Keyword quality:** Each keyword must pass the Swap Test. Replace the brand name with a competitor's. If the keyword still fits, it is too generic. Replace with something specific to this brand's positioning and visual territory.

**Objective:** The client reads the keywords and recognizes their brand. The designer reads them and already knows where to go. Identity validation through concept-words, not personality exercises.

**Excludes:** Voice contrast pairs ("X but NOT Y"). Full personality descriptions. More than 10 keywords. Any visual translation at this stage.

**Character limit:** 660 characters total (keywords + contextual line). Count characters and report the count.

**Save checkpoint:** Hold in working memory until Step 10.
```

- [ ] **Step 2: Verify edit**

Read the modified section and confirm:
- Header says "Brand DNA" not "Brand Personality"
- Source list draws from full diagnosis (positioning, personality.attributes, archetypes, visual_exploration, audience_problem)
- Output format specifies slash-separated keywords + contextual line
- "What brand DNA is NOT" section is present
- Keyword quality criterion with Swap Test is present
- Excludes explicitly mentions "Voice contrast pairs"
- Character limit is still 660
- Step 5 follows immediately after

- [ ] **Step 3: Commit**

```bash
git add skills/visual-direction/SKILL.md
git commit -m "feat(visual-direction): replace brand personality with brand DNA keywords"
```

---

### Task 3: Replace Step 5 -- SMP becomes Central Concept

**Files:**
- Modify: `skills/visual-direction/SKILL.md:120-140` (Step 5)

- [ ] **Step 1: Replace Step 5 content**

Replace the entire Step 5 section (from `## Step 5: Slide 3 -- SMP (~290 chars)` through `**Save checkpoint:** Hold in working memory until Step 10.`) with:

```markdown
## Step 5: Slide 3 -- Central Concept (~250 chars)

**This is the one generative step.** The central concept does not exist in the diagnosis output. You must create it.

**Source:** `audience_problem.philosophical`, `positioning.onlyness_statement`, `visual_exploration.recommended` rationale, `chromatic_territory.available_territory`, and the brand DNA keywords from Step 4

**Generate one descriptive block** (~250 chars) that captures the central concept driving the visual identity project. This is not a tagline, not a poetic phrase, and not a slogan. It describes the core idea that will guide all visual decisions: what visual territory the brand occupies, why, and what connects it to the brand DNA.

The concept should read as a strategic description that a designer can use as a compass. When the client sees the mood board, they should be able to trace every visual choice back to this concept.

**Quality criterion (mandatory):** The concept must contain at least one element that would not exist without the diagnosis. A designer reading only the original client brief (without the diagnosis) should NOT be able to write this concept. It must incorporate a specific positioning insight, market gap, unoccupied territory, or differentiator surfaced by research.

Test this by asking: could someone who only read the client intake produce this text? If yes, revise until diagnosis-derived specificity is present.

**Excludes:** Poetic taglines or slogans. More than one concept block. Explanation of how it was built. The terms "SMP" or "conceito central" on the slide itself.

**Character limit:** 250 characters total. Count characters and report the count.

**Save checkpoint:** Hold in working memory until Step 10.
```

- [ ] **Step 2: Verify edit**

Read the modified section and confirm:
- Header says "Central Concept" not "SMP"
- Character limit is 250 (not 110+180)
- Single descriptive block (not main statement + supporting line)
- Sources include brand DNA keywords from Step 4
- Quality criterion is preserved
- Excludes mentions "Poetic taglines or slogans"
- Step 6 follows immediately after

- [ ] **Step 3: Commit**

```bash
git add skills/visual-direction/SKILL.md
git commit -m "feat(visual-direction): replace SMP with central concept descriptive block"
```

---

### Task 4: Update Step 7 -- Guidelines

**Files:**
- Modify: `skills/visual-direction/SKILL.md:159-174` (Step 7)

- [ ] **Step 1: Replace Step 7 content**

Replace the entire Step 7 section (from `## Step 7: Slide 5 -- Guidelines (~840 chars total, 3 guidelines x 280 chars each)` through `**Save checkpoint:** Hold in working memory until Step 10.`) with:

```markdown
## Step 7: Slide 5 -- Guidelines (~840 chars total, 3 guidelines x 280 chars each)

**Source:** `client_input.project_synthesis.success_criteria`, `client_input.project_synthesis.core_challenge`, `client_input.identified_challenges`, `audience_problem.villain`

Generate 2-3 project-specific guidelines. Start from `success_criteria` as the foundation, then enrich with `identified_challenges` to add specificity and stakes. The success criteria define what we want to achieve; the identified challenges define what we need to overcome. Each guideline merges both into a single project objective.

Each guideline has:
- A strong name (short, memorable, used as a slide highlight)
- A contextual description in accessible language (2-3 lines)
- Each names a specific project objective rooted in both what needs to be achieved and what needs to be overcome

**Objective:** Create a collaborative validation filter before the mood board. The client confirms the right objectives and takes responsibility for the criteria they will use to judge the identity. This reduces revision cycles driven by undeclared reasons.

**Excludes:** Visual guidelines (those come later). Generic goals ("be memorable," "be modern"). More than 3 guidelines.

**Character limit:** Each guideline (name + description) under 280 characters. Count and report each.

**Save checkpoint:** Hold in working memory until Step 10.
```

- [ ] **Step 2: Verify edit**

Read the modified section and confirm:
- Source list is unchanged
- New instruction to start from success_criteria and enrich with identified_challenges is present
- Guideline structure (name + description) is unchanged
- Character limit is still 280 per guideline
- Objective and excludes are preserved
- Step 8 follows immediately after

- [ ] **Step 3: Commit**

```bash
git add skills/visual-direction/SKILL.md
git commit -m "feat(visual-direction): update guidelines to merge success criteria with identified challenges"
```

---

### Task 5: Update Step 1, Step 9, and Step 10 for renamed slides

**Files:**
- Modify: `skills/visual-direction/SKILL.md` — Steps 1, 9, and 10

This task updates all references to the renamed slides across the remaining steps.

- [ ] **Step 1: Update Step 1 (Prerequisite Check) announcement list**

Find the announcement list in Step 1 (lines 58-66) and replace:

```markdown
If all present: announce the extraction plan:
1. Slide 0: Project Context (~500 chars)
2. Slide 1: Brand Foundation (~540 chars)
3. Slide 2: Brand Personality (~660 chars)
4. Slide 3: SMP (~290 chars: 110 main + 180 supporting)
5. Slide 4: Chromatic Territory (~350 chars)
6. Slide 5: Guidelines (~840 chars: 3x280 each)
7. Mood Board Direction (designer reference, no char limit)
8. Anti-slop validation (always runs)
```

with:

```markdown
If all present: announce the extraction plan:
1. Slide 0: Project Context (~500 chars)
2. Slide 1: Brand Foundation (~540 chars)
3. Slide 2: Brand DNA (~660 chars)
4. Slide 3: Central Concept (~250 chars)
5. Slide 4: Chromatic Territory (~350 chars)
6. Slide 5: Guidelines (~840 chars: 3x280 each)
7. Mood Board Direction (designer reference, no char limit)
8. Anti-slop validation (always runs)
```

- [ ] **Step 2: Update Step 9 (Anti-Slop Validation)**

Find test 4 (line 216) and replace:

```markdown
**4. SMP Quality Criterion:** Does the SMP contain at least one element that would not exist without the diagnosis? If a designer could have written it from just the client brief, revise.
```

with:

```markdown
**4. Central Concept Quality Criterion:** Does the central concept contain at least one element that would not exist without the diagnosis? If a designer could have written it from just the client brief, revise.

**4a. Brand DNA Swap Test:** Take each brand DNA keyword individually. Replace the brand name with a competitor's. If the keyword still fits the competitor, it is too generic. Replace with a more specific concept-word.
```

Find the character limit test (lines 218-226) and replace:

```markdown
**5. Character Limit Test:** Verify each slide is within its character limit:
- Slide 0 (Project Context): 500 chars
- Slide 1 (Brand Foundation): 540 chars
- Slide 2 (Brand Personality): 660 chars
- Slide 3 SMP main: 110 chars
- Slide 3 SMP supporting: 180 chars
- Slide 4 (Chromatic Territory): 350 chars
- Slide 5 Guidelines: 280 chars each (3 max)
If any slide exceeds its limit, compress without losing meaning. Report revised char count.
```

with:

```markdown
**5. Character Limit Test:** Verify each slide is within its character limit:
- Slide 0 (Project Context): 500 chars
- Slide 1 (Brand Foundation): 540 chars
- Slide 2 (Brand DNA): 660 chars
- Slide 3 (Central Concept): 250 chars
- Slide 4 (Chromatic Territory): 350 chars
- Slide 5 Guidelines: 280 chars each (3 max)
If any slide exceeds its limit, compress without losing meaning. Report revised char count.
```

Find the validation logging test names (line 239) and replace:

```markdown
Use these test names: `swap_test`, `hand_test`, `specificity_test`, `smp_quality`, `character_limit`, `business_type_test`. If a test failed and was fixed, use `fail_then_fix`. Write the updated `research-log.yaml`.
```

with:

```markdown
Use these test names: `swap_test`, `hand_test`, `specificity_test`, `concept_quality`, `brand_dna_swap`, `character_limit`, `business_type_test`. If a test failed and was fixed, use `fail_then_fix`. Write the updated `research-log.yaml`.
```

- [ ] **Step 3: Update Step 9.5 steps_executed list**

Find the `steps_executed` list in Step 9.5 (line 246) and replace:

```markdown
2. Write the `steps_executed` list to the current run block. For visual-direction, use: `prerequisite-check`, `project-context`, `brand-foundation`, `brand-personality`, `smp`, `chromatic-territory`, `guidelines`, `mood-board`, `anti-slop`. Include only steps that actually executed.
```

with:

```markdown
2. Write the `steps_executed` list to the current run block. For visual-direction, use: `prerequisite-check`, `project-context`, `brand-foundation`, `brand-dna`, `central-concept`, `chromatic-territory`, `guidelines`, `mood-board`, `anti-slop`. Include only steps that actually executed.
```

- [ ] **Step 4: Update Step 10 YAML output format**

Find the `presentation` YAML block in Step 10 (lines 307-345) and replace:

```yaml
presentation:
  status: complete
  slides:
    project_context:
      text: "..."
      char_count: N
    brand_foundation:
      text: "..."
      char_count: N
    brand_personality:
      text: "..."
      char_count: N
    smp:
      main: "..."
      supporting: "..."
      char_count_main: N
      char_count_supporting: N
    chromatic_territory:
      text: "..."
      char_count: N
    guidelines:
      - name: "..."
        description: "..."
        char_count: N
      - name: "..."
        description: "..."
        char_count: N
      - name: "..."
        description: "..."
        char_count: N
  mood_board_direction:
    territory_name: "..."
    color_palette: "..."
    imagery_references: "..."
    typography_direction: "..."
    composition_notes: "..."
    intended_feeling: "..."
```

with:

```yaml
presentation:
  status: complete
  slides:
    project_context:
      text: "..."
      char_count: N
    brand_foundation:
      text: "..."
      char_count: N
    brand_dna:
      keywords: "..."
      context: "..."
      char_count: N
    central_concept:
      text: "..."
      char_count: N
    chromatic_territory:
      text: "..."
      char_count: N
    guidelines:
      - name: "..."
        description: "..."
        char_count: N
      - name: "..."
        description: "..."
        char_count: N
      - name: "..."
        description: "..."
        char_count: N
  mood_board_direction:
    territory_name: "..."
    color_palette: "..."
    imagery_references: "..."
    typography_direction: "..."
    composition_notes: "..."
    intended_feeling: "..."
```

- [ ] **Step 5: Update Step 10 Decision Log template**

Find the Decision Log template in Step 10 (lines 351-364) and replace:

```
### [Date] -- Visual Direction Complete

**Presentation slides generated:** 6 slides within character limits
**SMP:** [the main statement]
**Guidelines:** [list the guideline names]
**Mood board territory:** [recommended territory name]
**Anti-slop validation:** [pass/fail summary]

**Recommended next action:**
- Designer builds mood board using mood_board_direction
- Designer reviews and adjusts presentation text tone
- Run design-briefs skill after client presentation approval
```

with:

```
### [Date] -- Visual Direction Complete

**Presentation slides generated:** 6 slides within character limits
**Brand DNA:** [the keyword list]
**Central concept:** [the concept text]
**Guidelines:** [list the guideline names]
**Mood board territory:** [recommended territory name]
**Anti-slop validation:** [pass/fail summary]

**Recommended next action:**
- Designer builds mood board using mood_board_direction
- Designer reviews and adjusts presentation text tone
- Run design-briefs skill after client presentation approval
```

- [ ] **Step 6: Update Step 10 review questions**

Find the review questions in Step 10 (lines 366-370) and replace:

```markdown
5. Present the 6 slides to the designer for review. Format each slide clearly with its character count. Ask:
- Does the tone feel right for your client?
- Does the SMP capture the core idea?
- Are the guidelines the right validation criteria for the mood board?
- Anything to adjust before building the mood board?
```

with:

```markdown
5. Present the 6 slides to the designer for review. Format each slide clearly with its character count. Ask:
- Does the tone feel right for your client?
- Do the brand DNA keywords capture the brand's essence?
- Does the central concept describe the right visual direction?
- Are the guidelines the right validation criteria for the mood board?
- Anything to adjust before building the mood board?
```

- [ ] **Step 7: Update the skill description in frontmatter**

Find the description in the YAML frontmatter (line 3) and replace:

```markdown
description: Produces client-facing presentation text from a diagnosed brand-brief. Extracts and compresses diagnosis into 6 slides within character limits, generates SMP, and provides mood board direction. Triggers when a designer has a diagnosed brand-brief.md and needs to prepare a client presentation.
```

with:

```markdown
description: Produces client-facing presentation text from a diagnosed brand-brief. Extracts and compresses diagnosis into 6 slides within character limits, synthesizes brand DNA keywords and central concept, and provides mood board direction. Triggers when a designer has a diagnosed brand-brief.md and needs to prepare a client presentation.
```

- [ ] **Step 8: Verify all edits**

Read the full file and verify:
- Step 1 announcement list says "Brand DNA" and "Central Concept (~250 chars)"
- Step 9 test 4 says "Central Concept Quality Criterion" with brand DNA Swap Test as 4a
- Step 9 character limits reference "Brand DNA" and "Central Concept: 250 chars"
- Step 9 validation log test names include `concept_quality` and `brand_dna_swap`
- Step 9.5 steps_executed uses `brand-dna` and `central-concept`
- Step 10 YAML has `brand_dna` (with keywords + context) and `central_concept` (with text)
- Step 10 Decision Log references "Brand DNA" and "Central concept"
- Step 10 review questions reference brand DNA keywords and central concept
- Frontmatter description updated
- No remaining references to "Brand Personality", "SMP", or "smp_quality"

- [ ] **Step 9: Commit**

```bash
git add skills/visual-direction/SKILL.md
git commit -m "feat(visual-direction): update Steps 1, 9, 9.5, 10 for renamed slides and new output format"
```

---

### Task 6: Manual test -- visual-direction on RHISA project

**Files:**
- Test: Run visual-direction skill against RHISA test materials

- [ ] **Step 1: Run visual-direction on test project**

Run the visual-direction skill on the RHISA test project (which should have a diagnosed brand-brief.md). Observe that:
- Slide 0 (Project Context) describes organization + challenge + objective (not external moment)
- Slide 2 produces 5-10 slash-separated brand DNA keywords + contextual line (not voice contrast pairs)
- Slide 3 produces a single descriptive concept block under 250 chars (not a poetic tagline + supporting line)
- Slide 5 guidelines are rooted in success criteria + identified challenges
- All character limits are respected
- Anti-slop catches generic keywords in brand DNA
- Research stats still work (credibility summary generated)
- YAML output in brand-brief.md uses `brand_dna` and `central_concept` structure

- [ ] **Step 2: Verify no remaining old references**

Search the full SKILL.md for any remaining references to old names:
- "Brand Personality" (should be "Brand DNA" everywhere)
- "SMP" (should be "Central Concept" everywhere, except in the anti-slop reference file which is untouched)
- "smp_quality" (should be "concept_quality")
- "brand_personality" as YAML key (should be "brand_dna")
