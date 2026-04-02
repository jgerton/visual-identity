# Research Stats: Per-Project Credibility Tracking

**Date:** 2026-04-02
**Status:** Draft
**Scope:** Add research logging and credibility summary generation to the visual-identity plugin

## Problem

Brand designers present identity work to clients. Clients trust deliverables more when they can see the research depth behind them (e.g., "X sources reviewed, Y documents analyzed, Z frameworks applied"). The visual-identity plugin does significant research during diagnosis but does not track or surface these numbers. Elio (domain expert and co-contributor) wants per-project stats embedded in the diagnosis output, ready for a presentation credibility slide.

## Solution

Two additions to the plugin:

1. **Research log** (`research-log.yaml`): append-only file in the project directory that records every countable action as skills execute.
2. **Stats block** (`research_stats` in `brand-brief.md`): computed summary with current-run stats, cumulative stats, and a 3-line credibility summary ready for the client presentation.

No new skill. The existing diagnosis and visual-direction skills gain logging behavior and a stats computation step.

## Research Log Format

`research-log.yaml` lives alongside `brand-brief.md` in the project working directory. Skills append to it; they never overwrite.

```yaml
runs:
  - run_id: 1
    skill: diagnosis
    started: 2026-04-02T10:00:00
    completed: 2026-04-02T10:45:00
    steps_executed:
      - competitor-research
      - chromatic-territory
      - positioning
      - personality
      - audience-problem
      - visual-territories
      - anti-slop
    actions:
      - step: competitor-research
        type: document_read
        target: "briefing_danilo_rede_rhisa.pdf"
        detail: "6 pages"
      - step: competitor-research
        type: web_search
        query: "Rede RHISA competitors sustainability Amazon"
        results_count: 8
      - step: chromatic-territory
        type: web_fetch
        url: "https://bionorte.org.br"
        layer: css_extraction
      - step: anti-slop
        type: validation
        test: swap_test
        result: pass
      - step: anti-slop
        type: validation
        test: specificity_test
        result: fail_then_fix
    client_documents:
      - name: "briefing_danilo_rede_rhisa.pdf"
        pages: 6
      - name: "reformulacao_rhisa_2025-2026.pdf"
        pages: 20
```

### Action Types

| Type | Fields | Logged At |
|------|--------|-----------|
| `document_read` | target, detail (page count if known) | Diagnosis Step 0, Visual-direction Step 0 |
| `web_search` | query, results_count | Diagnosis Steps 2, 3 |
| `web_fetch` | url, layer (brand_guidelines / css_extraction / qualitative) | Diagnosis Steps 2, 3 |
| `validation` | test (test name), result (pass / fail_then_fix) | Diagnosis Step 8, Visual-direction Step 9 |

### Run Metadata

- `run_id`: integer, incremented per run across all skills in the project
- `skill`: which skill executed (diagnosis / visual-direction)
- `started` / `completed`: ISO timestamps
- `steps_executed`: list of step names that actually ran (partial re-runs list only the steps executed)
- `client_documents`: list of client documents read, with page counts. Only populated on runs that read client documents (avoids duplicating the list on re-runs that skip Step 0 document reads).

## Stats Block in brand-brief.md

New top-level YAML section in `brand-brief.md`, alongside `positioning`, `personality`, etc.

```yaml
research_stats:
  last_updated: 2026-04-02T10:45:00
  current_run:
    run_id: 2
    skill: diagnosis
    client_documents: 5
    pages_analyzed: 51
    web_searches: 12
    sources_fetched: 18
    competitors_researched: 4
    frameworks_applied: 7
    validation_tests_run: 6
    validation_failures_fixed: 2
  cumulative:
    total_runs: 2
    total_client_documents: 5
    total_pages_analyzed: 51
    total_web_searches: 20
    total_sources_fetched: 25
    total_competitors_researched: 4
    total_frameworks_applied: 7
    total_validation_tests_run: 12
    total_validation_failures_fixed: 3
  credibility_summary:
    line_1: "5 documentos do cliente analisados (51 páginas)"
    line_2: "25 fontes pesquisadas em 2 rodadas de análise"
    line_3: "7 frameworks aplicados com 12 testes de validação"
```

### Current Run vs. Cumulative

- `current_run`: reflects only the most recent run. Overwritten each time a skill completes.
- `cumulative`: sums across all runs in `research-log.yaml`.

### Counting Rules

Some stats are cumulative (grow with re-runs), some are distinct (same items counted once):

| Stat | Counting Rule |
|------|---------------|
| `client_documents` | Distinct count. Same 5 docs read across 3 runs = 5 documents. |
| `pages_analyzed` | Distinct count. Tied to unique documents. |
| `web_searches` | Cumulative. Each search query counts, even if repeated. |
| `sources_fetched` | Cumulative. Each URL fetch counts per run. |
| `competitors_researched` | Distinct count. 4 competitors across 2 runs = 4. |
| `frameworks_applied` | Fixed. Diagnosis applies 7. Visual-direction does not add new frameworks. |
| `validation_tests_run` | Cumulative. 6 tests per diagnosis run, 7 per visual-direction run. |
| `validation_failures_fixed` | Cumulative. Each fail_then_fix entry counts. |

### Deriving Stats from the Log

The stats computation reads `research-log.yaml` and computes:

- `client_documents`: deduplicated count of unique document names across all `client_documents` arrays
- `pages_analyzed`: sum of pages for unique documents
- `web_searches`: count of all actions with `type: web_search`
- `sources_fetched`: count of all actions with `type: web_fetch`
- `competitors_researched`: count of unique competitor entries in `brand-brief.md` competitors array
- `frameworks_applied`: constant 7 for diagnosis (Dunford, Neumeier, Aaker, Jung, Miller, anti-slop system, onlyness test). Visual-direction does not add new frameworks; it reuses the diagnosis frameworks via anti-slop validation, which is already counted in `validation_tests_run`.
- `validation_tests_run`: count of all actions with `type: validation`
- `validation_failures_fixed`: count of all actions with `type: validation` and `result: fail_then_fix`

## Credibility Summary Generation

Three lines, ready for a presentation slide. Follows the KTA/ZE reference pattern: big number + short descriptor + parenthetical detail.

### Language

Follows the `language` field in `brand-brief.md`. Written natively in that language (same rule as all other plugin output).

### Line Selection

| Priority | Line | Source |
|----------|------|--------|
| 1 | Client documents + pages | Grounds the work in the client's own materials |
| 2 | Sources researched + run count | Shows research depth |
| 3 | Frameworks + validation tests | Shows analytical rigor |

### Run Count in Line 2

- If cumulative runs > 1: include run count (e.g., "25 fontes pesquisadas em 2 rodadas de analise")
- If only 1 run: omit run count (e.g., "18 fontes pesquisadas")

### Anti-Slop

The 3 lines must use real counts from the log. No rounding up, no vague language. "5 documentos do cliente" is specific. "Multiplas fontes analisadas" is banned.

## Changes to Existing Skills

### Diagnosis Skill

**Log writes added to:**

- **Step 0 (Load Context):** After reading client documents, create or open `research-log.yaml`. Start a new run block with `run_id`, `skill: diagnosis`, `started` timestamp. Log each document read as a `document_read` action with page count. Populate `client_documents` array.
- **Step 2 (Competitor Research):** Log each `web_search` with query and results_count. Log each `web_fetch` with URL and layer.
- **Step 3 (Chromatic Territory):** Log each `web_search` and `web_fetch` for CSS extraction and brand guideline lookups.
- **Step 8 (Anti-Slop Validation):** Log each of the 6 validation tests as a `validation` action with test name and result (pass or fail_then_fix).

**New Step 9.5 (Research Stats):** Between "Update Stage and Decision Log" (Step 9) and "Present Summary" (Step 10):

1. Write `completed` timestamp to the current run block in `research-log.yaml`
2. Write `steps_executed` list to the current run block
3. Read the full `research-log.yaml`
4. Compute `current_run` stats from the latest run block
5. Compute `cumulative` stats across all run blocks (applying counting rules)
6. Generate 3-line `credibility_summary` in the project language
7. Write the `research_stats` block to `brand-brief.md`

**Step 10 (Present Summary):** Add the 3-line credibility summary to the terminal output, after the existing summary sections.

### Visual-Direction Skill

**Log writes added to:**

- **Step 0 (Load Context):** Open `research-log.yaml`, start a new run block with `skill: visual-direction`. Log any document reads.
- **Step 9 (Anti-Slop Validation):** Log each validation test result.

**New Step 9.5 (Research Stats):** Same computation as diagnosis. Updates the existing `research_stats` block in `brand-brief.md` with visual-direction's contributions added to cumulative totals.

**Step 10 (Write Output and Present):** Include credibility summary in terminal output.

### What Stays Untouched

- All existing step logic, checkpoints, and output formats
- Anti-slop reference file
- Diagnosis frameworks reference file
- Decision Log format
- Plugin manifest
- brand-brief.md structure (research_stats is additive, no existing fields change)

## Research Log Lifecycle

### First Run (No Log Exists)

Diagnosis Step 0 creates `research-log.yaml` with `runs: []`, then starts appending. Stats block gets written with `run_id: 1`.

### Re-run of a Single Skill

New run block appended with incremented `run_id`. Stats recomputes cumulative totals. `current_run` reflects only the new run. `credibility_summary` regenerated with updated cumulative numbers.

### Partial Re-run

The diagnosis skill already skips completed sections based on populated fields in `brand-brief.md`. The new run block's `steps_executed` list reflects only the steps that actually ran. Cumulative totals grow by what was actually done.

### Visual-direction After Diagnosis

Appends its own run block. Stats block updates to include visual-direction's contributions. `credibility_summary` reflects full pipeline effort.

### Multiple Projects

Each project directory has its own `research-log.yaml` and `brand-brief.md`. No cross-project state. Stats are always per-project.

### Git Tracking

`research-log.yaml` contains client-specific research trails. It should be gitignored in client project repos if they are public, but tracked in private repos. The plugin repo itself does not need changes since the log is created in project directories, not the plugin directory.

## Testing

- Run diagnosis on a test project, verify `research-log.yaml` is created with correct action entries
- Verify `research_stats` block appears in `brand-brief.md` with accurate counts
- Re-run diagnosis with a section change, verify cumulative stats grow correctly and current_run reflects only the new run
- Run visual-direction after diagnosis, verify stats update to include both skills
- Verify `credibility_summary` lines use real numbers and follow the project language
- Verify partial re-runs (skipped steps) produce accurate `steps_executed` lists
