# Visual Identity

A Claude Code plugin for brand identity designers. Runs strategic diagnosis on client materials and produces presentation-ready output within character limits.

## What It Does

**Diagnosis skill:** Reads client materials (briefings, documents, presentations), analyzes positioning, personality, competitors, chromatic territory, and visual territories. Produces a complete strategic working document for the designer.

**Visual-direction skill:** Reads the diagnosis and extracts 6 client-facing presentation slides within character limits, plus mood board direction. The designer reviews the text, builds the mood board, and presents to the client.

## The Pipeline

```
client materials -> intake -> diagnosis -> visual-direction -> client presentation
```

Intake and design-briefs skills are coming soon.

## How This Differs from Brand Toolkit

- [Brand Toolkit](https://github.com/jgerton/brand-toolkit) is modular strategy for founders and marketers. Each skill runs independently.
- Visual Identity is a linear pipeline for designers. Each skill feeds the next. The strategy serves the visual identity, not the other way around.
- Both read and write `brand-brief.md`. Install both if you want strategy skills (positioning, messaging, voice) alongside the designer pipeline.

## Frameworks

| Area | Framework | Expert |
|------|-----------|--------|
| Positioning | 5-component positioning | April Dunford (Obviously Awesome) |
| Differentiation | Onlyness test | Marty Neumeier |
| Personality | Brand personality dimensions + archetypes | David Aaker + Carl Jung |
| Messaging structure | Problem structure (simplified) | Donald Miller (StoryBrand) |
| Anti-slop | Custom validation (6 tests) | Internal |

## Anti-Slop System

Every output is validated against 6 tests before delivery. If the output could belong to a competitor, it gets flagged and rewritten.

- **Swap Test:** Replace the brand name with a competitor's. If it still reads true, revise.
- **Hand Test:** Cover the brand name. Can you tell whose piece this is?
- **Specificity Test:** Scan for banned generic words ("innovative," "cutting-edge," "holistic"). Replace with concrete claims.
- **Differentiation Test:** Every value claim must trace to a specific unique attribute.
- **Voice Contrast Test:** Personality must include "is NOT" statements, not just "is" statements.
- **Business-Type Test:** Would this work for a completely different organization? If yes, too generic.

Includes banned word lists for PT-BR and EN.

## Get Started

Clone it, point Claude Code at it, run the diagnosis skill on a project with a `brand-brief.md`.

- [Installation guide](docs/INSTALLATION.md)
- [Getting started](docs/GETTING-STARTED.md)

## Installation

### Try it on one project

```bash
git clone https://github.com/jgerton/visual-identity.git
claude --plugin-dir ./visual-identity
```

### Install for every session

```bash
git clone https://github.com/jgerton/visual-identity.git ~/.claude/plugins/visual-identity
```

Then add to your Claude Code settings:

```json
{
  "plugins": ["~/.claude/plugins/visual-identity"]
}
```

## Quick Start

1. Put your client materials in a working directory
2. Create a `brand-brief.md` (or run the intake skill when available)
3. Ask Claude to run the diagnosis: "Diagnose this brand brief" or "Run brand diagnosis on brand-brief.md"
4. Review the strategic working document, then ask Claude to run visual direction: "Run visual direction" or "Prepare the client presentation"
5. Review the 6 presentation slides, adjust tone as needed, and build your mood board using the provided direction

## Prerequisites

- [Claude Code](https://claude.ai/claude-code) installed and working
- Git

## Current Status

| Skill | Status |
|-------|--------|
| Diagnosis | Complete, tested |
| Visual Direction | Complete, tested |
| Intake | Planned |
| Design Briefs | Planned |

## Compatibility

Works alongside Brand Toolkit. Both plugins read and write the same `brand-brief.md` format. Diagnosis enriches positioning, personality, and competitors that Brand Toolkit skills can build on.

## Contributors

- **Jon Gerton** ([@jgerton](https://github.com/jgerton)) -- plugin development
- **Elio Almeida** -- domain expertise, presentation structure, character limits, anti-slop criteria, and real-world testing from active visual identity projects

## Community

Building more tools like this in the open. Come build with us:

- [You Craft. AI Helps.](https://www.skool.com/you-craft-ai-helps/about) -- Skool community for Claude Code practitioners
- [jpgerton.com](https://jpgerton.com) -- tutorials, tools, and build-in-public updates

## License

MIT
