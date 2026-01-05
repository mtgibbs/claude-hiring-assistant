---
name: evaluate-resumes
description: Run the complete evaluation workflow on all resumes in /new folders
---

# Evaluate Resumes Workflow

Run the complete, standardized evaluation workflow on all pending resumes.

## Workflow Steps

Execute these steps in order:

### Step 1: Scan for New Resumes

Scan these directories for PDF/DOCX files:
- `resumes/developers/new/` and all subfolders
- `resumes/managers/new/` and all subfolders

The **subfolder name determines the role level** (must match config.yaml role levels).

### Step 2: Evaluate Each Resume

For each resume found:

1. **Determine role type from path:**
   - `resumes/developers/...` → IC role → use `ic-evaluator` agent
   - `resumes/managers/...` → Manager role → use `manager-evaluator` agent

2. **Extract role level from subfolder name** (e.g., "Senior Software Engineer (L2)")

3. **Launch the appropriate evaluator agent** with:
   - Resume file path
   - Role level from subfolder name

4. **The evaluator agent will:**
   - Load org config from `.org/*/`
   - Score against the rubric
   - Invoke regional advisors if candidate is from configured region
   - Create evaluation folder with emoji status: `evaluations/[developers|managers]/YYYY-MM-DD_[emoji]_[name]_[score]-[max]/`
     - ✅ = Interview, ⚠️ = Borderline, ❌ = Reject
   - Save evaluation markdown
   - MOVE resume to evaluation folder

5. **After each evaluation, launch `candidate-summary-distiller`** with:
   - Evaluation file path
   - phase=resume

### Step 3: Quality Checks (After All Evaluations)

1. **Launch `fact-checker` agent** to verify all factual claims in evaluations

2. **Launch `batch-reporter` agent** to generate:
   - Batch summary markdown
   - CSV tracking file

### Step 4: Report Results

Summarize:
- Total resumes processed
- Breakdown by role type and level
- Any issues encountered
- Location of batch report and CSV

## Important Rules

- Process resumes **sequentially** (one at a time) to avoid conflicts
- If a resume cannot be evaluated, log the error and continue with others
- All evaluations go to `evaluations/` folder (gitignored)
- Resumes are MOVED (not copied) to evaluation folders
- CSV files are gitignored - safe to generate

## Example Output

```
Evaluation Complete!

Processed: 5 resumes
- Developers: 3 (2 Senior, 1 Mid)
- Managers: 2 (1 EM, 1 Director)

Results:
- ✅ Interview: 4
- ⚠️ Borderline: 1
- ❌ Reject: 0

Evaluations created:
- evaluations/developers/2024-01-15_✅_john-doe_28-35/
- evaluations/developers/2024-01-15_✅_jane-smith_26-35/
- evaluations/developers/2024-01-15_⚠️_bob-wilson_22-35/
- evaluations/managers/2024-01-15_✅_alice-johnson_42-50/
- evaluations/managers/2024-01-15_✅_charlie-brown_38-50/

Reports:
- Batch summary: reports/BATCH_2024-01-15.md
- CSV tracking: reports/BATCH_2024-01-15.csv
```
