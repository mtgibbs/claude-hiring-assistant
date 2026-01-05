---
name: assess-interviews
description: Run the interview assessment workflow on all evaluation folders containing interview notes
---

# Assess Interviews Workflow

Run the standardized interview assessment workflow on all candidates with interview notes.

## How It Works

After conducting interviews, drop notes into the candidate's evaluation folder:

```
evaluations/developers/2024-01-15_✅_john-doe_28-35/
├── john-doe-evaluation.md      # Resume evaluation (already exists)
├── HR_SUMMARY_RESUME.md        # Resume HR summary (already exists)
├── john-doe-resume.pdf         # Resume (already exists)
└── interview_notes.md          # <-- DROP INTERVIEW NOTES HERE
```

**Supported file names for interview notes:**
- `interview_notes.md`
- `interview_notes.txt`
- `interview.md`
- `notes.md`
- `*.vtt` (Teams transcription)
- `*.srt` (subtitle transcription)

## Workflow Steps

### Step 1: Scan for Unprocessed Interviews

Scan `evaluations/developers/` and `evaluations/managers/` for:
- Folders containing interview notes file
- That do NOT already have `INTERVIEW_ASSESSMENT.md`

### Step 2: Assess Each Interview

For each candidate with unprocessed interview notes:

1. **Read the existing evaluation** to understand:
   - Role level and type (IC vs Manager)
   - Areas flagged for interview probing
   - Resume score and concerns

2. **Launch `interview-assessor` agent** with:
   - Path to interview notes
   - Path to existing evaluation
   - Role type (IC or Manager)

3. **The assessor agent will:**
   - Load interview rubric from `.org/*/`
   - Score against interview criteria
   - Apply higher standards for manager candidates
   - Detect ticket jockey patterns
   - Save `INTERVIEW_ASSESSMENT.md` to candidate folder

4. **Launch `candidate-summary-distiller`** with:
   - Evaluation folder path
   - phase=interview

5. **Agent saves** `HR_SUMMARY_INTERVIEW.md` to candidate folder

### Step 3: Report Results

Summarize:
- Total interviews assessed
- Breakdown by recommendation
- Any issues encountered

## Important Rules

- Process interviews **sequentially** to avoid conflicts
- Skip candidates who already have `INTERVIEW_ASSESSMENT.md`
- Interview notes stay in the evaluation folder (not moved)
- Both `INTERVIEW_ASSESSMENT.md` and `HR_SUMMARY_INTERVIEW.md` are created

## After Running

Each assessed candidate folder will contain:

```
evaluations/developers/2024-01-15_✅_john-doe_28-35/
├── john-doe-evaluation.md      # Resume evaluation
├── HR_SUMMARY_RESUME.md        # Resume HR summary
├── john-doe-resume.pdf         # Resume
├── interview_notes.md          # Interview notes (input)
├── INTERVIEW_ASSESSMENT.md     # Interview scoring (NEW)
└── HR_SUMMARY_INTERVIEW.md     # Interview HR summary (NEW)
```

## Example Output

```
Interview Assessment Complete!

Assessed: 3 candidates

Results:
- Strong Hire: 1 (John Doe)
- Hire: 1 (Jane Smith)
- No Hire: 1 (Bob Wilson)

Files created:
- evaluations/developers/2024-01-15_✅_john-doe_28-35/INTERVIEW_ASSESSMENT.md
- evaluations/developers/2024-01-15_✅_john-doe_28-35/HR_SUMMARY_INTERVIEW.md
- evaluations/developers/2024-01-16_⚠️_jane-smith_22-35/INTERVIEW_ASSESSMENT.md
- evaluations/developers/2024-01-16_⚠️_jane-smith_22-35/HR_SUMMARY_INTERVIEW.md
- evaluations/managers/2024-01-15_❌_bob-wilson_20-50/INTERVIEW_ASSESSMENT.md
- evaluations/managers/2024-01-15_❌_bob-wilson_20-50/HR_SUMMARY_INTERVIEW.md
```
