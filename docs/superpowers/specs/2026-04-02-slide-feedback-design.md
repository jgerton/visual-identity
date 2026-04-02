# Slide Feedback: Visual Direction Improvements from Elio Review

**Date:** 2026-04-02
**Status:** Draft
**Scope:** Update 4 of 6 slides in the visual-direction skill based on Elio Almeida's review of the RHISA test output and his actual presentation structure.

## Problem

Elio reviewed the visual-direction output for the RHISA project and tested it against his real presentation (17-page PDF, "Metodo Escala" format). Four slides need changes:

1. **Project Context** uses stale external hooks (COP30) instead of the project synthesis from diagnosis
2. **Brand Personality** produces voice contrast pairs, but Elio's presentation uses "DNA da Marca" -- visual concept-words, not personality
3. **SMP** produces a poetic tagline + supporting line, but Elio's "Conceito Central" needs a descriptive concept statement
4. **Guidelines** generates weaker text than the success criteria already in the diagnosis

Brand Foundation (Slide 1) and Chromatic Territory (Slide 4) are fine as-is.

## Source

- Elio's markdown review: `brand-toolkit-collab/2026-04-02/Visual Direction -- Rede RHISA.md`
- Elio's presentation PDF: `brand-toolkit-collab/2026-04-02/Rede Rhisa - Direção Visual.pdf`
- Feedback summary: `brand-toolkit-collab/2026-04-02/elio-feedback-summary.md`

## Changes

### Change 1: Slide 0 -- Project Context (Step 2)

**Current behavior:** Writes 3 blocks about (1) sector/moment relevance, (2) the perception problem, (3) what's at stake. Sources from `project_synthesis.summary`, `core_challenge`, `additional_context`.

**New behavior:** Write 3 blocks about (1) what the organization is and does, (2) the project challenge (why the identity needs to change), (3) the objective (what success looks like). Source primarily from `client_input.project_synthesis` -- use `summary`, `core_challenge`, and `success_criteria`.

**Rationale:** Elio says the slide should contextualize the project clearly and objectively, not focus on the external moment. Present the organization, explain the challenge, define the objective.

**What changes in SKILL.md:**
- Replace the 3-block structure description
- Update the source list
- Update the objective text
- Update the excludes list
- Character limit stays at 500

### Change 2: Slide 2 -- Brand DNA (Step 4)

**Current behavior:** Selects 3-4 voice contrast tension pairs from `personality.voice_contrast`. Formats as "X but NOT Y" for client presentation. ~660 chars.

**New behavior:** Synthesize 5-10 brand DNA keywords (prefer fewer with more depth) from the full diagnosis: `positioning`, `personality.attributes`, `personality.archetypes`, `visual_exploration.recommended`, and `audience_problem`. Each keyword is a visual concept-word that carries a layer -- it may relate to form, materiality, personality, or origin. Read together, they form a portrait of the brand without needing explanation.

**Output format:** Keywords presented as a slash-separated list (matching Elio's presentation layout on page 9). Below the keywords, a brief contextual line (1-2 sentences) explaining what concept the keywords collectively point toward.

**What is NOT brand DNA:**
- Not institutional values
- Not voice attributes
- Not Aaker personality dimensions directly (though they inform the synthesis)
- Not generic adjectives ("innovative," "professional")

**Anti-slop on keywords:** Each keyword must pass the Swap Test. If replacing the brand name with a competitor's doesn't make the keyword feel wrong, it's too generic. Replace with something more specific to this brand's positioning and territory.

**What changes in SKILL.md:**
- Rename slide from "Brand Personality" to "Brand DNA"
- Replace source list
- Replace content generation instructions
- Replace objective
- Replace excludes
- Character limit stays at 660

### Change 3: Slide 3 -- Central Concept (Step 5)

**Current behavior:** Generates two elements: a poetic main statement (~110 chars) and a supporting descriptive line (~180 chars). Called "SMP."

**New behavior:** Generate one descriptive concept block (~250 chars) that captures the central concept driving the visual identity project. This is not a tagline or a poetic phrase. It describes the core idea that will guide all visual decisions: what visual territory the brand occupies, why, and what connects it to the brand DNA.

**Quality criterion (kept from current):** The concept must contain at least one element that would not exist without the diagnosis. A designer reading only the client brief should NOT be able to write this concept. It must incorporate specific positioning insights, market gaps, or differentiators surfaced by research.

**What changes in SKILL.md:**
- Rename slide from "SMP" to "Central Concept"
- Replace the two-element structure with one descriptive block
- Update character limit from 110+180 to ~250 total
- Remove "main statement" and "supporting line" terminology
- Update the excludes
- Keep the quality criterion
- Update the YAML output structure in Step 10 (single `text` field instead of `main` + `supporting`)

### Change 4: Slide 5 -- Project Guidelines (Step 7)

**Current behavior:** Generates 2-3 guidelines from `success_criteria`, `core_challenge`, `identified_challenges`, and `audience_problem.villain`. Each has a name + description under 280 chars.

**New behavior:** Same sources, but merge `success_criteria` with `identified_challenges` to produce more complete guidelines. The success criteria define what we want to achieve; the identified challenges define what we need to overcome. Each guideline should name a specific project objective rooted in both.

**Key change:** The guidelines should read like the diagnosis success criteria (which Elio said were already better) but expanded into client-facing language with a strong name and description.

**What changes in SKILL.md:**
- Update the content generation instructions to emphasize merging success criteria + identified challenges
- Add instruction: "Start from `success_criteria` as the foundation, then enrich with `identified_challenges` to add specificity and stakes"
- Keep character limit at 280 per guideline
- Keep 2-3 guideline count

### Step 10 Output Format Update

The `presentation` YAML block in brand-brief.md needs to reflect the slide changes:

```yaml
presentation:
  slides:
    project_context:
      text: "..."
      char_count: N
    brand_foundation:
      text: "..."
      char_count: N
    brand_dna:                    # was: brand_personality
      keywords: "..."            # slash-separated keyword list
      context: "..."             # 1-2 sentence contextual line
      char_count: N
    central_concept:              # was: smp
      text: "..."                # single descriptive block
      char_count: N              # was: char_count_main + char_count_supporting
    chromatic_territory:
      text: "..."
      char_count: N
    guidelines:
      - name: "..."
        description: "..."
        char_count: N
```

### Anti-Slop Validation Update (Step 9)

- Test 4 changes from "SMP Quality Criterion" to "Central Concept Quality Criterion" (same test, renamed)
- Add keyword-level Swap Test to brand DNA validation
- Test name in research log changes from `smp_quality` to `concept_quality` (no production data exists yet, safe to rename)

## What Stays Untouched

- Slide 1: Brand Foundation (Step 3)
- Slide 4: Chromatic Territory (Step 6)
- Step 8: Mood Board Direction
- Step 9.5: Research Stats (just added)
- Step 10: Research stats output (item 6, just added)
- All diagnosis skill content

## Testing

- Run visual-direction on the RHISA test project after changes
- Verify brand DNA produces 5-10 concept-words (not voice contrast pairs)
- Verify central concept produces a single descriptive block under 250 chars
- Verify project context focuses on organization + challenge + objective
- Verify guidelines use success criteria + identified challenges as foundation
- Verify all character limits are respected
- Verify anti-slop catches generic keywords in brand DNA
- Verify research stats still work correctly after slide restructuring
