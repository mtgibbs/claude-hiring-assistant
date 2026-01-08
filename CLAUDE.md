# CLAUDE.md

Bias-aware hiring methodology for evaluating software developers and engineering managers.

---

## Configuration

Organization-specific data lives in `.org/[org-name]/` (gitignored). **Convention over configuration** - just drop files in folders and agents load them automatically.

### Folder Structure
```
.org/[org-name]/
├── config.yaml              # Main settings (thresholds, levels, tech stack)
├── rubrics/
│   ├── ic_*.md              # All IC rubrics/guides (auto-loaded)
│   └── manager_*.md         # All manager rubrics/guides (auto-loaded)
├── matrices/
│   ├── ic_matrix.md         # IC competency expectations
│   └── manager_matrix.md    # Manager competency expectations
└── regional/
    └── *.md                 # All regional context (auto-loaded)
```

### How It Works
- **Agents scan folders** - Drop a file, it gets loaded
- **Prefix convention** - Files prefixed with `ic_` load for IC evaluations, `manager_` for managers
- **Regional is global** - All files in `regional/` load for any regional analysis

### Setup
**First time?** Use the `setup-wizard` agent - it guides you through the entire process.

**Manual setup:**
1. Copy `.org/example/` to `.org/my-company/`
2. Edit files in `.org/my-company/` as needed
3. Agents automatically discover first non-example folder in `.org/`

### Org Discovery Rules (CRITICAL)

**When loading organization config, agents MUST follow this order:**

1. **Scan `.org/` for folders** - List all directories in `.org/`
2. **ALWAYS IGNORE `.org/example/`** when any other folder exists
3. **Use the first non-example folder** (e.g., `.org/onedatascan/`, `.org/mycompany/`)
4. **If ONLY `.org/example/` exists** → STOP and ask the user:
   > "No organization-specific configuration found. Only the example template exists at `.org/example/`. Would you like to:
   > 1. Run the `setup-wizard` to create your organization config
   > 2. Proceed using example defaults (not recommended for real evaluations)"

**Why this matters:**
- `.org/example/` contains generic templates with placeholder values
- Real evaluations require org-specific rubrics, levels, and tech stack
- Using example config will produce inaccurate scoring and recommendations

**Never:**
- Load rubrics/matrices from `.org/example/` when another org folder exists
- Silently fall back to example config
- Mix files from example and org-specific folders

### Config Contents (config.yaml)
- **internal_levels**: Your organization's internal grade system (G5, G6, G7, etc.)
- **role_levels**: External hiring roles mapped to internal grades
- **competency_matrices**: Dimensions and expectations per level
- **thresholds**: Pass/fail scoring thresholds
- **tech_stack**: Technology priorities for alignment scoring

---

## Protected Files (DO NOT MODIFY)

The following files are **templates** intended for other users of this framework. **NEVER modify these files:**

### Template File List
| Path | Purpose |
|------|---------|
| `config/config.yaml.example` | Configuration template |
| `config/schema.md` | Configuration documentation |
| `.org/example/` | Complete org folder template |
| `.org/example/config.yaml` | Full config with all options |
| `.org/example/matrices/*` | Competency matrix templates |
| `.org/example/rubrics/*` | Evaluation rubric templates |
| `.org/example/regional/*` | Regional context templates |
| `examples/*` | Sample output files |

### Rules
- **NEVER** modify files in `.org/example/` - this is the reference template
- **NEVER** rename `.org/example/` to something else
- **ALWAYS** create a new folder (e.g., `.org/mycompany/`) for org-specific data
- Any file with `.example` extension or in `examples/` folders is protected

### When Creating Org-Specific Config
1. Copy `.org/example/` to `.org/[your-org-name]/`
2. Modify only the copy in `.org/[your-org-name]/`
3. Never touch `.org/example/` - it stays as a reference

---

## Role Routing

**First, identify role type:**
- "Manager", "Director", "VP", "Head of" → Management role → Use `manager-evaluator` agent
- "Engineer", "Developer", "Architect" (without management) → IC role → Use `ic-evaluator` agent

---

## Agent Dispatch Table

| Task | Agent | When to Use |
|------|-------|-------------|
| **Setup & Config** |
| Initial setup | `setup-wizard` | First-time setup or new org configuration |
| **Resume Evaluation** |
| Evaluate IC resume | `ic-evaluator` | Any developer/engineer resume |
| Evaluate manager resume | `manager-evaluator` | Any EM/Director resume |
| Regional education context | `regional-education-advisor` | Candidates from configured regions |
| Critical resume analysis | `regional-resume-analyst` | Candidates from configured regions |
| Cultural bias check | `cultural-bias-advisor` | When presentation feels "off" |
| **Interview** |
| Prepare interview questions | `interview-prep` | Before interviewing a candidate |
| Assess interview responses | `interview-assessor` | After interview, with notes or transcription |
| **Post-Evaluation** |
| Compare candidates | `candidate-comparer` | When choosing between multiple candidates |
| Create HR summaries | `candidate-summary-distiller` | After evaluation (phase=resume) or interview (phase=interview) |
| Verify factual claims | `fact-checker` | After completing all evaluations in batch |
| Generate batch reports & CSV | `batch-reporter` | After completing batch of evaluations |

---

## Evaluation Principles

### Contextualize & Filter (Presentation vs Substance):
- Photos, age, marital status, religion, personal details
- Resume length (1 page vs. 5 pages)
- Formatting style, colors, templates
- Tone (formal vs. casual, humble vs. confident)
- Generic accolades ("Employee of the Month", "Star Performer")
- Job titles (focus on actual responsibilities)
- Educational prestige (focus on practical experience)
- Grammar quirks if meaning is clear

### Always Focus On (evaluate strictly):
- Specific technical accomplishments with measurable outcomes
- Depth of experience with required technologies
- Evidence of code quality practices (testing, code review, CI/CD)
- Problem-solving examples with clear challenge/solution
- Growth trajectory and increasing responsibility
- Code samples and portfolio quality
- Consistency and honesty of claims

### Red Flags That Matter:
- Cannot identify specific projects or contributions
- Vague descriptions throughout
- Inconsistent timelines or impossible claims
- No code samples for senior positions
- Excessive job hopping without explanation
- Technology time travel (5 years with 2-year-old framework)

### Green Flags That Matter:
- Specific metrics ("reduced load time from 3s to 800ms")
- Problem-solving stories with challenge/solution/outcome
- Active GitHub with meaningful projects
- Clear progression in responsibility
- Quality focus without prompting (mentions testing, architecture)
- Teaching/sharing (blogs, mentoring, open source)

---

## File Organization

### Directory Structure
```
/
├── CLAUDE.md
├── .org/                        # Organization-specific (gitignored)
│   └── [org-name]/
│       ├── config.yaml          # All org settings + matrices
│       ├── matrices/
│       │   ├── ic_matrix.md     # IC competency expectations
│       │   └── manager_matrix.md
│       ├── rubrics/
│       │   ├── ic_rubric.md     # IC evaluation rubric
│       │   └── manager_rubric.md
│       └── regional/
│           └── india_context.md
├── config/
│   ├── config.yaml.example      # Template (committed)
│   └── schema.md                # Configuration docs
├── resumes/                     # All candidate data (gitignored)
│   ├── developers/
│   │   └── new/
│   │       └── [Grade Subfolder]/    # e.g., "Senior Software Engineer (G6)"
│   └── managers/
│       └── new/
│           └── [Grade Subfolder]/
├── evaluations/                 # All evaluations (gitignored)
│   ├── developers/
│   │   ├── 2024-01-15_⭐_john-doe_32-35/      # ⭐ Strong/Exceptional (80%+)
│   │   ├── 2024-01-16_✅_jane-smith_25-35/    # ✅ Interview (68-79%)
│   │   ├── 2024-01-17_⚠️_bob-wilson_22-35/    # ⚠️ Borderline (57-67%)
│   │   └── 2024-01-18_❌_alex-jones_15-35/    # ❌ No Match (<57%)
│   │       ├── [name]-evaluation.md
│   │       ├── HR_SUMMARY_RESUME.md
│   │       ├── INTERVIEW_NOTES.md (for ✅/⭐ - fillable template)
│   │       ├── INTERVIEW_ASSESSMENT.md (after interview)
│   │       ├── HR_SUMMARY_INTERVIEW.md (after interview)
│   │       └── [resume].pdf
│   └── managers/ (same structure, scores out of 50)
├── reports/                     # Batch reports & CSV (gitignored)
│   ├── BATCH_YYYY-MM-DD.md
│   └── BATCH_YYYY-MM-DD.csv
├── examples/                    # Anonymized examples (committed)
│   ├── sample_evaluation.md
│   └── sample_hr_summary.md
└── docs/
    └── GETTING_STARTED.md
```

### Folder Naming Convention

Format: `YYYY-MM-DD_[emoji]_[candidate-name]_[score]-[max]/`

**Status Emoji:**
| Emoji | IC (35 pts) | Manager (50 pts) | Meaning |
|-------|-------------|------------------|---------|
| ⭐ | 80%+ (28+) | 80%+ (40+) | Strong/Exceptional - Fast Track |
| ✅ | 68-79% (24-27) | 64-79% (32-39) | Interview |
| ⚠️ | 57-67% (20-23) | 52-63% (26-31) | Borderline |
| ❌ | <57% (<20) | <52% (<26) | No Match |

**Examples:**
- `2024-01-15_⭐_john-doe_32-35/`
- `2024-01-15_✅_jane-smith_25-35/`
- `2024-01-15_⚠️_bob-wilson_22-35/`
- `2024-01-15_❌_alex-jones_15-35/`

**Rules:**
- **ALWAYS** include emoji and score for at-a-glance status
- **ALWAYS** move resume into candidate's evaluation folder
- **NEVER** create consolidated summary files - individual summaries per candidate per phase

### HR Summary Files (Phase-Separated)
| File | When Created | Contains |
|------|--------------|----------|
| `HR_SUMMARY_RESUME.md` | After resume evaluation | Strengths/Opportunities from resume only |
| `HR_SUMMARY_INTERVIEW.md` | After interview assessment | Strengths/Opportunities from interview |

This separation enables comparing initial impressions vs. interview performance.

---

## Team-Based Evaluation Format

Evaluations use a **team-based structure** where each contributing agent has its own section. This provides:

- **Transparency**: See what each agent found and recommended
- **Collaboration**: Work with individual agents to refine their analysis
- **Audit Trail**: Understand how the final recommendation was reached
- **Team Metaphor**: Treat agents like team members reviewing a candidate

### Evaluation Document Structure

```
# RESUME EVALUATION
**Candidate:** Name
**Role:** Position
**Date:** YYYY-MM-DD

---
# AGENT ASSESSMENTS

## IC Evaluator Assessment (or Manager Evaluator Assessment)
[Primary scoring and analysis from main evaluator]

## Regional Education Advisor Notes
[Educational context - if candidate from configured region]

## Regional Resume Analyst Notes
[Critical analysis - if candidate from configured region]

## Cultural Bias Advisor Notes
[Cultural context - if invoked]

---
# CONSOLIDATED EVALUATION

## Final Score
## Recommendation
## Summary (Strengths, Concerns, Interview Focus)
## Role Level Assessment
```

### Agent Section Attribution

Each agent section includes:
- **Agent:** Agent name (e.g., `ic-evaluator`, `regional-resume-analyst`)
- **Focus:** What this agent specializes in
- **Analysis:** Agent's specific findings and assessment
- **Recommendations:** Agent's suggestions for next steps

This structure enables reviewing individual agent contributions and comparing perspectives before seeing the consolidated recommendation.

---

## Quick Reference Thresholds

**Note:** These are default thresholds. Configure your organization's thresholds in `.org/[org]/config.yaml`.

### IC Candidates (default: 35 points, 7 categories)
| Percentage | Action |
|------------|--------|
| 91%+ | Exceptional - Fast track |
| 80-89% | Strong - Interview |
| 68-79% | Acceptable - Interview if pipeline allows |
| 57-67% | Borderline |
| <57% | Does Not Meet Threshold |

### Manager Candidates (default: 50 points, 10 categories)
| Percentage | Action |
|------------|--------|
| 90%+ | Exceptional - Fast track |
| 80-89% | Strong - Interview |
| 64-79% | Acceptable - Careful evaluation |
| 52-63% | Borderline |
| <52% | Does Not Meet Threshold |

---

## Standard Workflow

### For Resume Evaluations:

**Step 1: Drop resumes in folders**
```
resumes/developers/new/
├── Senior Software Engineer (G6)/
│   ├── john-doe-resume.pdf
│   └── jane-smith-resume.pdf
└── Lead Software Engineer (G7)/
    └── bob-wilson-resume.pdf
```
The **subfolder name determines the grade level** (must match config.yaml role_levels).

**Step 2: Process resumes**
1. Scan `resumes/developers/new/` and `resumes/managers/new/` subfolders
2. For each resume, launch appropriate evaluator (`ic-evaluator` or `manager-evaluator`)
3. Evaluator:
   - Reads resume and all org context from `.org/*/`
   - Applies rubric and scores
   - Creates evaluation folder: `evaluations/developers/YYYY-MM-DD_[emoji]_[name]_[score]-35/`
   - **MOVES** resume to evaluation folder (removes from resumes/)
   - Saves evaluation markdown
4. For regional candidates: invokes `regional-education-advisor` and `regional-resume-analyst`
5. Creates HR summary with `candidate-summary-distiller`

**Step 3: Generate batch report**
- Launch `batch-reporter` agent to create summary and CSV
- Reports saved to `reports/` folder (gitignored)

**Result:** After processing, `resumes/*/new/` folders are empty. All materials live in `evaluations/`.

### For Interview Assessment:
1. **Assess interview**: Launch `interview-assessor` agent with notes/transcription
2. **Create interview HR summary**: Launch `candidate-summary-distiller` agent with phase=interview
3. Agent saves `HR_SUMMARY_INTERVIEW.md` to candidate folder

### Role Level Mapping:
Role levels are configured in `.org/[org-name]/config.yaml`. Create subfolders in `resumes/` that match your configured `subfolder` values. Example mapping:

| Subfolder | Expectations |
|-----------|--------------|
| Software Engineer (L1) | Entry level, 0-2 years |
| Senior Software Engineer (L2) | Independent delivery, 2-5 years |
| Lead Software Engineer (L3) | Technical leadership, 5+ years |
| Engineering Manager | People management + technical |
| Director | Org-level impact |

### For Interview Prep:
1. Launch `interview-prep` agent with candidate evaluation path
2. Agent selects questions from `.org/[org]/rubrics/*_interview_questions.md` question banks
3. Agent adds custom questions targeting gaps identified in the evaluation
4. Saves `INTERVIEW_NOTES.md` - a fillable template for the interviewer to use during the interview
5. After interview, the filled-in `INTERVIEW_NOTES.md` is read by `interview-assessor`

### For Candidate Comparison:
1. Launch `candidate-comparer` agent with list of candidate evaluation paths
2. Agent produces comparison analysis and recommendation

---

## Context Management (CRITICAL)

Processing many candidates can overflow agent context. Follow these rules:

### Before Large Batches
- Run `/compact` before processing more than 10 candidates
- Check context usage and compact if needed between batches

### Parallel Processing (Up to 5 at a time)
- Launch **up to 5 agents in parallel** for efficiency
- Use `run_in_background: true` for parallel execution
- Check results with `TaskOutput block=true, timeout=300000` (blocking with 5-min timeout)
- **Never poll repeatedly** with `block=false` - each poll adds to context
- Wait for all agents in a batch to complete before starting next batch
- Run `/compact` between large batches (10+ candidates)

### Agent Return Messages
Agents should return concise completion messages, not full analysis. Example:
- Good: "Created evaluation for John Doe. Score: 28/35 (⭐). Recommendation: Interview."
- Bad: [Full evaluation content repeated in return message]

---

## Commands

### `/evaluate-resumes` - Run Full Evaluation Workflow

**This is the preferred way to process resumes.** Runs the complete standardized workflow:

1. Scans `resumes/developers/new/` and `resumes/managers/new/` subfolders
2. For each resume:
   - Launches appropriate evaluator (`ic-evaluator` or `manager-evaluator`)
   - Evaluator invokes regional advisors if applicable
   - Creates evaluation folder and moves resume
   - Generates HR summary
3. Runs `fact-checker` to verify claims
4. Runs `batch-reporter` to generate CSV
5. For candidates recommended for interview (✅ or ⭐):
   - Launches `interview-prep` agent to prepare questions from question bank
   - Saves `INTERVIEW_NOTES.md` (fillable template) to each candidate's folder

**Usage:** Just type `/evaluate-resumes` - no manual agent selection needed.

### `/assess-interviews` - Run Interview Assessment Workflow

**Use after conducting interviews.** Fill in the `INTERVIEW_NOTES.md` template during the interview, then run:

1. Scans evaluation folders for filled-in interview notes
2. For each candidate with notes:
   - Launches `interview-assessor` agent
   - Generates interview HR summary
3. Reports results

**Supported note files:** `INTERVIEW_NOTES.md` (preferred - structured template), `*.vtt`, `*.srt`, `interview_notes.md`, `notes.md`

**Usage:** Fill in `INTERVIEW_NOTES.md` during interview, then type `/assess-interviews`

---

## Example Interactions

**"Process new resumes" / "Run evaluations"**
→ Use `/evaluate-resumes` command

**"Evaluate this specific resume"**
→ Identify role type and level, launch appropriate evaluator agent

**"Compare these candidates"**
→ Launch `candidate-comparer` agent

**"Prepare interview questions for [candidate]"**
→ Launch `interview-prep` agent

**"Create HR summaries for all candidates"**
→ Launch `candidate-summary-distiller` for each candidate individually

**"This resume is 5 pages with a photo, should I disqualify?"**
→ "No - resume length and photos are cultural differences we contextualize. Let me evaluate the technical substance."
