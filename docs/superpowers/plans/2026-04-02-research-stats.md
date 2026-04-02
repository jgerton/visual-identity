# Research Stats Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add per-project research logging and credibility summary generation to the diagnosis and visual-direction skills.

**Architecture:** Append-only `research-log.yaml` tracks every countable action during skill execution. A new Step 9.5 in each skill reads the log, computes stats, writes a `research_stats` block to `brand-brief.md` with a 3-line credibility summary ready for client presentations.

**Tech Stack:** Markdown skill instructions (SKILL.md), YAML data files

---

## File Map

| Action | File | Responsibility |
|--------|------|----------------|
| Modify | `skills/diagnosis/SKILL.md` | Add logging to Steps 0, 2, 3, 8. Add Step 9.5. Modify Step 10. |
| Modify | `skills/visual-direction/SKILL.md` | Add logging to Steps 0, 9. Add Step 9.5. Modify Step 10. |

No new files in the plugin repo. `research-log.yaml` is created at runtime in the project directory.

---

### Task 1: Add research log initialization and document logging to diagnosis Step 0

**Files:**
- Modify: `skills/diagnosis/SKILL.md:20-37` (Step 0: Load Context)

- [ ] **Step 1: Add logging instructions after the existing Step 0 items**

After item 7 (`Read business_type for framework adaptations throughout the diagnosis`), add:

```markdown
8. **Initialize research log.** Check if `research-log.yaml` exists in the working directory.
   - If it does not exist, create it with `runs: []`.
   - Read the existing `runs` array and determine the next `run_id` (highest existing `run_id` + 1, or 1 if empty).
   - Append a new run block to the `runs` array:
     ```yaml
     - run_id: [next_id]
       skill: diagnosis
       started: [current ISO timestamp]
       completed: null
       steps_executed: []
       actions: []
       client_documents: []
     ```
   - Write the updated file back to `research-log.yaml`.
9. **Log client documents.** For each client document read in item 1 (brand-brief.md) and any referenced client materials (PDFs, presentations, etc.):
   - Append to the current run's `actions` array:
     ```yaml
     - step: load-context
       type: document_read
       target: "[filename]"
       detail: "[page count] pages"
     ```
   - Append to the current run's `client_documents` array:
     ```yaml
     - name: "[filename]"
       pages: [page count]
     ```
   - If page count is unknown, use `detail: "unknown"` and `pages: 0`.
   - Write the updated `research-log.yaml`.
```

- [ ] **Step 2: Verify the edit preserves existing Step 0 content**

Read `skills/diagnosis/SKILL.md` and confirm:
- Items 1-7 are unchanged
- Items 8-9 are the new logging instructions
- Step 1 (Announce Plan) follows immediately after

- [ ] **Step 3: Commit**

```bash
git add skills/diagnosis/SKILL.md
git commit -m "feat(diagnosis): add research log init and document logging to Step 0"
```

---

### Task 2: Add web research logging to diagnosis Steps 2 and 3

**Files:**
- Modify: `skills/diagnosis/SKILL.md:52-128` (Steps 2 and 3)

- [ ] **Step 1: Add logging instructions to Step 2 (Competitor Research)**

After the existing item 7 (`Save checkpoint`) in Step 2, insert a new item 8:

```markdown
8. **Log research actions.** For each web search and web fetch performed during this step, append to the current run's `actions` array in `research-log.yaml`:
   - For each web search:
     ```yaml
     - step: competitor-research
       type: web_search
       query: "[the search query used]"
       results_count: [number of results returned]
     ```
   - For each web page fetched:
     ```yaml
     - step: competitor-research
       type: web_fetch
       url: "[the URL fetched]"
       layer: "[brand_guidelines | css_extraction | qualitative]"
     ```
   - Write the updated `research-log.yaml`.
```

- [ ] **Step 2: Add logging instructions to Step 3 (Chromatic Territory Mapping)**

After the existing `Save checkpoint` line in Step 3 (`Update competitors[].visual_identity...`), add:

```markdown
**Log research actions.** For each web search and web fetch performed during this step, append to the current run's `actions` array in `research-log.yaml`:
- For each web search (brand guidelines lookups, competitor color searches):
  ```yaml
  - step: chromatic-territory
    type: web_search
    query: "[the search query used]"
    results_count: [number of results returned]
  ```
- For each web page fetched (CSS extraction, brand guideline pages):
  ```yaml
  - step: chromatic-territory
    type: web_fetch
    url: "[the URL fetched]"
    layer: "[brand_guidelines | css_extraction | qualitative]"
  ```
- Write the updated `research-log.yaml`.
```

- [ ] **Step 3: Verify edits preserve existing step content**

Read the modified sections and confirm:
- All existing Step 2 and Step 3 instructions are unchanged
- Logging instructions follow each step's save checkpoint
- YAML examples use the correct step names (`competitor-research`, `chromatic-territory`)

- [ ] **Step 4: Commit**

```bash
git add skills/diagnosis/SKILL.md
git commit -m "feat(diagnosis): add web research logging to Steps 2 and 3"
```

---

### Task 3: Add validation logging to diagnosis Step 8

**Files:**
- Modify: `skills/diagnosis/SKILL.md:258-282` (Step 8: Anti-Slop Validation)

- [ ] **Step 1: Add logging instructions after the existing failure handling block**

After the existing text `After all tests pass, proceed to Step 9.`, insert:

```markdown
**Log validation results.** After running all 6 tests, append one entry per test to the current run's `actions` array in `research-log.yaml`:
```yaml
- step: anti-slop
  type: validation
  test: "[test_name]"
  result: "[pass | fail_then_fix]"
```
Use these test names: `swap_test`, `hand_test`, `specificity_test`, `differentiation_test`, `voice_contrast_test`, `business_type_test`. If a test failed and was fixed, use `fail_then_fix`. Write the updated `research-log.yaml`.
```

- [ ] **Step 2: Verify edit preserves existing Step 8 content**

Read the modified section and confirm:
- All 6 test descriptions are unchanged
- The failure handling block is unchanged
- Logging instructions appear after "proceed to Step 9" and before Step 9

- [ ] **Step 3: Commit**

```bash
git add skills/diagnosis/SKILL.md
git commit -m "feat(diagnosis): add validation logging to Step 8"
```

---

### Task 4: Add Step 9.5 (Research Stats) and update Step 10 in diagnosis

**Files:**
- Modify: `skills/diagnosis/SKILL.md:284-323` (Steps 9 and 10)

- [ ] **Step 1: Insert Step 9.5 between Steps 9 and 10**

After the Step 9 closing code block (the Decision Log template ending with the recommended next action), insert:

```markdown
## Step 9.5: Research Stats

Compute per-project research stats and write the credibility summary.

1. Write `completed` timestamp (current ISO) to the current run block in `research-log.yaml`.
2. Write the `steps_executed` list to the current run block. Include only the step names that actually executed during this run (based on which steps were not skipped in Step 0). Use these names: `competitor-research`, `chromatic-territory`, `positioning`, `personality`, `audience-problem`, `visual-territories`, `anti-slop`.
3. Read the full `research-log.yaml`.
4. Compute `current_run` stats from the latest run block:
   - `client_documents`: count of entries in this run's `client_documents` array
   - `pages_analyzed`: sum of `pages` in this run's `client_documents` array
   - `web_searches`: count of actions with `type: web_search` in this run
   - `sources_fetched`: count of actions with `type: web_fetch` in this run
   - `competitors_researched`: count of entries in the `competitors` array in `brand-brief.md`
   - `frameworks_applied`: 7 (constant: Dunford, Neumeier, Aaker, Jung, Miller, anti-slop, onlyness)
   - `validation_tests_run`: count of actions with `type: validation` in this run
   - `validation_failures_fixed`: count of actions with `type: validation` and `result: fail_then_fix` in this run
5. Compute `cumulative` stats across all run blocks:
   - `total_runs`: count of run blocks
   - `total_client_documents`: deduplicated count of unique document names across all `client_documents` arrays (same document in multiple runs counts once)
   - `total_pages_analyzed`: sum of `pages` for unique documents only
   - `total_web_searches`: count of all actions with `type: web_search` across all runs
   - `total_sources_fetched`: count of all actions with `type: web_fetch` across all runs
   - `total_competitors_researched`: count of entries in the `competitors` array in `brand-brief.md` (distinct, not per-run)
   - `total_frameworks_applied`: 7 (constant)
   - `total_validation_tests_run`: count of all `type: validation` actions across all runs
   - `total_validation_failures_fixed`: count of all `type: validation` + `result: fail_then_fix` across all runs
6. Generate `credibility_summary` -- 3 lines in the project language (read `language` from `brand-brief.md`). Each line follows the pattern: big number + short descriptor + parenthetical detail.
   - **Line 1:** client documents + pages. Example (pt-br): `"5 documentos do cliente analisados (51 páginas)"`
   - **Line 2:** sources researched + run count. If `total_runs` > 1, include run count. Example (pt-br): `"25 fontes pesquisadas em 2 rodadas de análise"`. If only 1 run: `"18 fontes pesquisadas"`.
   - **Line 3:** frameworks + validation tests. Example (pt-br): `"7 frameworks aplicados com 12 testes de validação"`
   - Use cumulative numbers for the summary. Numbers must be real counts from the log. No rounding, no vague language.
7. Write the `research_stats` block to `brand-brief.md` as a top-level YAML section:
   ```yaml
   research_stats:
     last_updated: [current ISO timestamp]
     current_run:
       run_id: [N]
       skill: diagnosis
       client_documents: [N]
       pages_analyzed: [N]
       web_searches: [N]
       sources_fetched: [N]
       competitors_researched: [N]
       frameworks_applied: 7
       validation_tests_run: [N]
       validation_failures_fixed: [N]
     cumulative:
       total_runs: [N]
       total_client_documents: [N]
       total_pages_analyzed: [N]
       total_web_searches: [N]
       total_sources_fetched: [N]
       total_competitors_researched: [N]
       total_frameworks_applied: 7
       total_validation_tests_run: [N]
       total_validation_failures_fixed: [N]
     credibility_summary:
       line_1: "[generated line 1]"
       line_2: "[generated line 2]"
       line_3: "[generated line 3]"
   ```
   If a `research_stats` block already exists in `brand-brief.md`, overwrite it entirely with the new computed values.
```

- [ ] **Step 2: Add credibility summary to Step 10 output**

In Step 10 (Present Summary), after the last bullet (`**Next step**`), add:

```markdown
- **Research stats** -- present the 3-line credibility summary from `research_stats.credibility_summary`:
  ```
  [line_1]
  [line_2]
  [line_3]
  ```
  Tell the designer: "These numbers are ready for your credibility slide. They update automatically on each run."
```

- [ ] **Step 3: Verify edits**

Read the modified file and confirm:
- Step 9 is unchanged
- Step 9.5 appears between Steps 9 and 10
- Step 10 includes the credibility summary as the last output item
- All YAML examples use correct field names matching the spec

- [ ] **Step 4: Commit**

```bash
git add skills/diagnosis/SKILL.md
git commit -m "feat(diagnosis): add Step 9.5 research stats and credibility summary output"
```

---

### Task 5: Add research log and validation logging to visual-direction Steps 0 and 9

**Files:**
- Modify: `skills/visual-direction/SKILL.md:18-23` (Step 0) and `skills/visual-direction/SKILL.md:185-209` (Step 9)

- [ ] **Step 1: Add logging initialization to Step 0**

After the existing item 4 (`Verify stage is diagnosis...`), add:

```markdown
5. **Initialize research log entry.** Open `research-log.yaml` in the working directory (it must exist from a prior diagnosis run; if it does not, warn the designer and create it with `runs: []`).
   - Read the existing `runs` array and determine the next `run_id` (highest existing `run_id` + 1).
   - Append a new run block:
     ```yaml
     - run_id: [next_id]
       skill: visual-direction
       started: [current ISO timestamp]
       completed: null
       steps_executed: []
       actions: []
       client_documents: []
     ```
   - Write the updated `research-log.yaml`.
6. **Log document reads.** If any additional client documents are read during this step (beyond brand-brief.md), log each as:
   ```yaml
   - step: load-context
     type: document_read
     target: "[filename]"
     detail: "[page count] pages"
   ```
   Append to the current run's `actions` and `client_documents` arrays. Write the updated `research-log.yaml`.
```

- [ ] **Step 2: Add validation logging to Step 9**

After the existing text `For each failure: name the test, show the failing text, show the revised text, update the slide.`, add:

```markdown
**Log validation results.** After running all 6 tests, append one entry per test to the current run's `actions` array in `research-log.yaml`:
```yaml
- step: anti-slop
  type: validation
  test: "[test_name]"
  result: "[pass | fail_then_fix]"
```
Use these test names: `swap_test`, `hand_test`, `specificity_test`, `smp_quality`, `character_limit`, `business_type_test`. If a test failed and was fixed, use `fail_then_fix`. Write the updated `research-log.yaml`.
```

- [ ] **Step 3: Verify edits preserve existing content**

Read both modified sections and confirm:
- Step 0 items 1-4 are unchanged, items 5-6 are new
- Step 9 test descriptions and failure handling are unchanged
- Logging instructions appear after the failure handling block
- Test names match visual-direction's 6 tests (note: `smp_quality` and `character_limit` replace `differentiation_test` and `voice_contrast_test` from diagnosis)

- [ ] **Step 4: Commit**

```bash
git add skills/visual-direction/SKILL.md
git commit -m "feat(visual-direction): add research log init and validation logging"
```

---

### Task 6: Add Step 9.5 (Research Stats) and update Step 10 in visual-direction

**Files:**
- Modify: `skills/visual-direction/SKILL.md:209-279` (between Steps 9 and 10)

- [ ] **Step 1: Insert Step 9.5 between Steps 9 and 10**

After the Step 9 validation logging block (added in Task 5), insert:

```markdown
## Step 9.5: Research Stats

Compute per-project research stats and write the credibility summary. This is the same computation as the diagnosis skill's Step 9.5, updated with visual-direction's contributions.

1. Write `completed` timestamp (current ISO) to the current run block in `research-log.yaml`.
2. Write the `steps_executed` list to the current run block. For visual-direction, use: `prerequisite-check`, `project-context`, `brand-foundation`, `brand-personality`, `smp`, `chromatic-territory`, `guidelines`, `mood-board`, `anti-slop`. Include only steps that actually executed.
3. Read the full `research-log.yaml`.
4. Compute `current_run` stats from the latest run block:
   - `client_documents`: count of entries in this run's `client_documents` array
   - `pages_analyzed`: sum of `pages` in this run's `client_documents` array
   - `web_searches`: count of actions with `type: web_search` in this run (typically 0 for visual-direction)
   - `sources_fetched`: count of actions with `type: web_fetch` in this run (typically 0)
   - `competitors_researched`: 0 (visual-direction does not research new competitors)
   - `frameworks_applied`: 0 (visual-direction reuses diagnosis frameworks; its validation is counted in `validation_tests_run`)
   - `validation_tests_run`: count of actions with `type: validation` in this run
   - `validation_failures_fixed`: count of actions with `type: validation` and `result: fail_then_fix` in this run
5. Compute `cumulative` stats across all run blocks (same rules as diagnosis Step 9.5):
   - `total_runs`: count of run blocks
   - `total_client_documents`: deduplicated count of unique document names across all `client_documents` arrays
   - `total_pages_analyzed`: sum of `pages` for unique documents only
   - `total_web_searches`: count of all `type: web_search` actions across all runs
   - `total_sources_fetched`: count of all `type: web_fetch` actions across all runs
   - `total_competitors_researched`: count of entries in the `competitors` array in `brand-brief.md`
   - `total_frameworks_applied`: 7 (constant from diagnosis)
   - `total_validation_tests_run`: count of all `type: validation` actions across all runs
   - `total_validation_failures_fixed`: count of all `type: validation` + `result: fail_then_fix` across all runs
6. Generate `credibility_summary` -- 3 lines in the project language (read `language` from `brand-brief.md`). Same pattern as diagnosis:
   - **Line 1:** client documents + pages
   - **Line 2:** sources researched + run count (if `total_runs` > 1, include run count)
   - **Line 3:** frameworks + validation tests
   - Use cumulative numbers. Real counts only.
7. Write the `research_stats` block to `brand-brief.md`, overwriting the existing block if present. Same YAML structure as diagnosis Step 9.5:
   ```yaml
   research_stats:
     last_updated: [current ISO timestamp]
     current_run:
       run_id: [N]
       skill: visual-direction
       client_documents: [N]
       pages_analyzed: [N]
       web_searches: [N]
       sources_fetched: [N]
       competitors_researched: 0
       frameworks_applied: 0
       validation_tests_run: [N]
       validation_failures_fixed: [N]
     cumulative:
       total_runs: [N]
       total_client_documents: [N]
       total_pages_analyzed: [N]
       total_web_searches: [N]
       total_sources_fetched: [N]
       total_competitors_researched: [N]
       total_frameworks_applied: 7
       total_validation_tests_run: [N]
       total_validation_failures_fixed: [N]
     credibility_summary:
       line_1: "[generated line 1]"
       line_2: "[generated line 2]"
       line_3: "[generated line 3]"
   ```
```

- [ ] **Step 2: Add credibility summary to Step 10 output**

In Step 10 (Write Output and Present), after item 5 (the list of review questions ending with `Anything to adjust before building the mood board?`), add:

```markdown
6. Present the updated credibility summary from `research_stats.credibility_summary`:
   ```
   [line_1]
   [line_2]
   [line_3]
   ```
   Tell the designer: "Updated research stats reflect the full pipeline. These numbers are ready for your credibility slide."
```

- [ ] **Step 3: Verify edits**

Read the modified file and confirm:
- Step 9 (with validation logging from Task 5) is unchanged
- Step 9.5 appears between Steps 9 and 10
- Step 10 includes the credibility summary as item 6
- The YAML structure matches the diagnosis version exactly
- `frameworks_applied` is 0 for current_run (visual-direction) and 7 for cumulative (from diagnosis)

- [ ] **Step 4: Commit**

```bash
git add skills/visual-direction/SKILL.md
git commit -m "feat(visual-direction): add Step 9.5 research stats and credibility summary output"
```

---

### Task 7: Manual test -- diagnosis on test project

**Files:**
- Test: Run diagnosis skill against test materials

- [ ] **Step 1: Set up test project directory**

Create a minimal test project with a `brand-brief.md` in a temporary directory. Use the existing test materials structure from `E:\Projects\ai-asst\brand-toolkit-collab\test-materials\` as reference, or create a minimal brand-brief.md with enough populated fields for a partial diagnosis run.

Verify the directory has no existing `research-log.yaml`.

- [ ] **Step 2: Run diagnosis skill**

Run the diagnosis skill on the test project. Observe that:
- `research-log.yaml` is created with `runs: [...]` containing one run block
- The run block has `run_id: 1`, `skill: diagnosis`, timestamps, actions, and client_documents
- Actions include `document_read`, `web_search`, `web_fetch`, and `validation` entries
- `brand-brief.md` now contains a `research_stats` block with `current_run`, `cumulative`, and `credibility_summary`
- The 3 credibility summary lines use real numbers and are in the project language
- The terminal output includes the credibility summary

- [ ] **Step 3: Verify re-run behavior**

Clear one section in `brand-brief.md` (e.g., null out `positioning.status`) and re-run diagnosis. Verify:
- `research-log.yaml` now has 2 run blocks (run_id 1 and 2)
- Run 2's `steps_executed` only includes the steps that actually ran
- `current_run` stats reflect only run 2
- `cumulative` stats sum both runs
- `total_client_documents` is deduplicated (not doubled)
- `credibility_summary` line 2 mentions "2 rodadas de analise" (or equivalent)

---

### Task 8: Manual test -- visual-direction after diagnosis

**Files:**
- Test: Run visual-direction skill after diagnosis completes

- [ ] **Step 1: Run visual-direction on the diagnosed test project**

Using the test project from Task 7 (which now has a completed diagnosis with `research-log.yaml`), run the visual-direction skill. Verify:
- A new run block is appended to `research-log.yaml` with `skill: visual-direction`
- `run_id` is incremented from the last diagnosis run
- Validation actions are logged with visual-direction's test names
- `research_stats` in `brand-brief.md` is updated:
  - `current_run.skill` is `visual-direction`
  - `cumulative` totals include both diagnosis and visual-direction runs
  - `total_validation_tests_run` reflects tests from all runs
- `credibility_summary` reflects the full pipeline effort
- Terminal output includes the updated credibility summary
