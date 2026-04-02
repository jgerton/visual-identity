# Getting Started

## What you just installed

Two skills that form a linear pipeline:

1. **Diagnosis** -- reads client materials, analyzes positioning, personality, competitors, chromatic territory, and visual territories. Produces a strategic working document.
2. **Visual Direction** -- reads the diagnosis and extracts 6 client-facing presentation slides with character limits, plus mood board direction for you.

Both skills read from and write to a shared `brand-brief.md` file. This file is the connective tissue between pipeline stages.

## The pipeline concept

Each skill feeds the next. You do not run visual direction without diagnosis first.

```
client materials -> intake -> diagnosis -> visual-direction -> client presentation
```

The intake skill is not yet available. For now, you create the initial `brand-brief.md` manually or paste client materials directly.

## Your first run

### 1. Prepare your working directory

Put your client materials (briefings, documents, presentations) in a project folder. Create a `brand-brief.md` with at minimum:

```yaml
---
project: "Project Name"
brand_name: "Brand Name"
industry: "Industry description"
business_type: community  # or: saas, local_service, ecommerce, agency, content_creator
stage: intake
language: pt-br  # or en

client_input:
  business_description: "What the brand does"
  target_audience: "Who it serves"
  perceived_differentiator: "What makes it different"
  known_competitors: []
  project_objectives: "What needs to change"
  identified_challenges: "What makes this hard"
---
```

### 2. Run diagnosis

Tell Claude: "Diagnose this brand brief" or "Run brand diagnosis."

### 3. What to expect

The diagnosis skill will:

1. Read your `brand-brief.md` and assess what sections need work
2. Announce its plan and list the sections it will analyze
3. Ask for your confirmation before proceeding
4. Work through each section: competitor research (with web search), chromatic territory mapping, positioning analysis, brand personality, audience problem, and visual territory exploration
5. Run anti-slop validation on everything
6. Save checkpoints to `brand-brief.md` as it progresses
7. Present a summary with the key findings

The skill saves progress after each section. If the session breaks, it picks up where it left off.

### 4. After diagnosis: run visual direction

Once the diagnosis is complete, tell Claude: "Run visual direction" or "Prepare the client presentation."

The visual-direction skill will:

1. Verify the diagnosis sections are populated
2. Announce the extraction plan (6 slides + mood board)
3. Ask for confirmation
4. Generate each slide within character limits
5. Create mood board direction for you
6. Run anti-slop validation on all presentation text
7. Write everything to `brand-brief.md` and present the slides for your review

## Character limits

Every presentation slide has a character limit. These exist because of the 4-second read rule: if a client cannot absorb a slide's content in 4 seconds of reading, the slide has too much text. The designer complements with 20-30 seconds of spoken narrative.

| Slide | Limit |
|-------|-------|
| Project Context | 500 chars |
| Brand Foundation | 540 chars |
| Brand Personality | 660 chars |
| SMP (main) | 110 chars |
| SMP (supporting) | 180 chars |
| Chromatic Territory | 350 chars |
| Guidelines (each) | 280 chars |

The skill counts characters and compresses if needed. You can ask it to adjust after reviewing.

## The anti-slop system

Every output passes through 6 validation tests before delivery:

1. **Swap Test:** Replace brand name with a competitor's. If it still reads true, the output is generic.
2. **Hand Test:** Cover the brand name. If a stranger cannot tell whose piece this is, it lacks distinctiveness.
3. **Specificity Test:** Scans for banned generic words (both PT-BR and EN lists). "Innovative" becomes a concrete, verifiable claim.
4. **Differentiation Test:** Every value claim must trace to a specific unique attribute. No floating claims.
5. **Voice Contrast Test:** Personality definitions include "is NOT" statements. Voice without edges is not voice.
6. **Business-Type Test:** If the output works for a completely different type of organization, it is too generic.

This matters because generic brand strategy is worse than no brand strategy. It gives false confidence. The anti-slop system forces specificity at every step.

## What you need to test this

To try the plugin on a real project, you need:

- **Client materials:** Any combination of briefing documents, strategic plans, presentations, or institutional texts. PDFs, PPTX, and plain text all work. The more substance in the materials, the richer the diagnosis.
- **A brand-brief.md:** The YAML-based file that holds all the structured data. You create the initial version (see the template above in "Your first run"), and the skills fill it in from there.

The diagnosis skill does real web research on competitors, so it works best when the client has named at least 1-2 competitors or the industry is specific enough to search.

For a minimal test: create a brand-brief.md with just the `client_input` fields filled in from any brand you know well. The diagnosis skill will research and fill the rest.

## Next steps

- Review the diagnosis output and adjust anything that does not match your understanding of the client
- Run visual direction to get presentation slides and mood board direction
- Build your mood board using the provided direction
- Present to the client
- When design-briefs becomes available, run it after client approval to get detailed design specifications
