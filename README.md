# Hiring Evaluation Framework

A bias-free, consistent methodology for evaluating software developers and engineering managers using Claude Code agents.

---

## Why This Exists

**The Challenge:** Every candidate deserves a fair, focused, and consistent review. However, human reviewers naturally suffer from fatigue; the 50th resume rarely gets the same fresh attention as the 1st. Unconscious biases can creep in based on formatting, name familiarity, or non-standard backgrounds, causing great potential to be overlooked.

**The Goal:** This framework acts as a tireless partner in the hiring process. It ensures consistency by applying the exact same rubric to every single candidate, regardless of when their application is reviewed. It helps:

- **Level the Playing Field** - By strictly distinguishing "presentation style" (resume length, photos, cultural norms) from "substance," we focus purely on skills, experience, and potential.
- **Reduce Bias** - It forces a focus on evidence of capability rather than "gut feeling," ensuring candidates are evaluated on what they've done, not how they look on paper.
- **Stay Consistent** - Every candidate gets evaluated against the same rubric, whether they're the first resume you see or the fiftieth.
- **Respect Candidate Time** - By accelerating the initial screening, we can get back to qualified candidates faster and spend more human time on meaningful interactions (interviews) rather than document review.

This isn't about replacing human judgment—it's about making your judgment more reliable by giving you a structured process and removing the noise that leads to unfair evaluations.

---

## What It Does

A configurable framework of Claude Code agents that helps you:

- **Evaluate resumes** consistently using your customizable rubrics
- **Prepare interviews** with role-appropriate questions based on each candidate's background
- **Assess interviews** objectively with structured scoring
- **Compare candidates** side-by-side for final decisions
- **Generate reports** with batch summaries and CSV exports for your hiring pipeline

---

## Quick Start

### 1. Clone and Open

```bash
git clone https://github.com/yourorg/hiring-evaluation-framework.git
cd hiring-evaluation-framework
claude  # Open Claude Code
```

### 2. Run Setup Wizard

In Claude Code, say:

```
Run the setup wizard
```

The `setup-wizard` agent will guide you through:
- Creating your organization folder (`.org/your-org-name/`)
- Configuring role levels and internal grades
- Setting scoring thresholds
- Customizing evaluation rubrics

### 3. Add Resumes

Drop resumes in the appropriate folders:

```
resumes/developers/new/
├── Software Engineer (L1)/
│   └── john-doe-resume.pdf
├── Senior Software Engineer (L2)/
│   └── jane-smith-resume.pdf
└── Lead Software Engineer (L3)/
    └── bob-wilson-resume.pdf

resumes/managers/new/
├── Engineering Manager/
│   └── alice-johnson-resume.pdf
└── Director/
    └── charlie-brown-resume.pdf
```

**Important:** Folder names must match your configured role levels.

### 4. Run Evaluations

In Claude Code, use the command:

```
/evaluate-resumes
```

This runs the complete workflow:
1. Scans all resume folders
2. Evaluates each resume with the appropriate agent
3. Invokes regional advisors if applicable
4. Generates HR summaries
5. Runs fact-checker
6. Produces batch report and CSV

---

## Commands

| Command | What It Does |
|---------|--------------|
| `/evaluate-resumes` | **Resume workflow** - processes all resumes in `/new` folders |
| `/assess-interviews` | **Interview workflow** - processes all interview notes in evaluation folders |
| `Run the setup wizard` | First-time configuration |
| `Prepare interview for [name]` | Generate interview questions |
| `Compare candidates` | Side-by-side comparison |

---

## Directory Structure

```
├── .org/                        # Organization-specific (gitignored)
│   ├── example/                 # Template - DO NOT MODIFY
│   └── your-org/                # Your config, rubrics, matrices
├── resumes/                     # Resume intake (gitignored)
│   ├── developers/new/[Level]/
│   └── managers/new/[Level]/
├── evaluations/                 # Generated evaluations (gitignored)
│   └── developers/
│       ├── 2024-01-15_✅_john-doe_28-35/     # ✅ = Interview
│       ├── 2024-01-16_⚠️_jane-smith_22-35/   # ⚠️ = Borderline
│       └── 2024-01-17_❌_bob-wilson_15-35/    # ❌ = Does Not Meet Threshold
├── reports/                     # Batch reports & CSV (gitignored)
├── .claude/
│   ├── agents/                  # All evaluation agents
│   └── skills/                  # Commands like /evaluate-resumes
├── examples/                    # Sample outputs (committed)
├── docs/                        # Documentation
├── CLAUDE.md                    # Agent methodology
└── README.md                    # This file
```

**Folder naming:** `YYYY-MM-DD_[emoji]_[name]_[score]-[max]/` for at-a-glance status

---

## Configuration

All organization-specific files live in `.org/your-org-name/`:

```
.org/your-org/
├── config.yaml              # Role levels, thresholds, tech stack
├── rubrics/
│   ├── ic_rubric.md         # IC scoring criteria
│   └── manager_rubric.md    # Manager scoring criteria
├── matrices/
│   ├── ic_matrix.md         # IC competency expectations
│   └── manager_matrix.md    # Manager competency expectations
└── regional/
    └── india_context.md     # Regional context files
```

**Convention over configuration:** Just drop files in folders and agents load them automatically.
- Files prefixed with `ic_` load for IC evaluations
- Files prefixed with `manager_` load for manager evaluations
- All files in `regional/` load for regional analysis

---

## Agents

| Agent | Purpose |
|-------|---------|
| `ic-evaluator` | Evaluate developer/engineer resumes |
| `manager-evaluator` | Evaluate manager/director resumes |
| `interview-prep` | Generate customized interview questions |
| `interview-assessor` | Score interview responses |
| `candidate-comparer` | Compare multiple candidates |
| `candidate-summary-distiller` | Create HR-ready summaries |
| `regional-education-advisor` | Contextualize education backgrounds |
| `regional-resume-analyst` | Critical analysis with regional context |
| `cultural-bias-advisor` | Identify cultural vs. capability differences |
| `resume-optimizer` | Help improve resumes (for users) |
| `fact-checker` | Verify claims across evaluations |
| `batch-reporter` | Generate batch reports and CSV |
| `setup-wizard` | First-time setup guidance |

---

## Key Principles

### Always Evaluate
- Specific technical accomplishments with measurable outcomes
- Depth of experience with required technologies
- Evidence of code quality practices (testing, code review, CI/CD)
- Problem-solving examples with challenge/solution/outcome
- Growth trajectory and increasing responsibility
- Code samples and portfolio quality

### Always Ignore
- Photos, age, marital status, religion, personal details
- Resume length (1 page vs. 5 pages)
- Formatting style, colors, templates
- Tone (formal vs. casual, humble vs. confident)
- Generic accolades ("Employee of the Month")
- Job titles (focus on actual responsibilities)
- Grammar quirks if meaning is clear

---

## Scoring System

Default thresholds (customize in your config):

### IC Roles (35 points)
| Percentage | Action |
|------------|--------|
| 91%+ | Exceptional - Fast track |
| 80-89% | Strong - Interview |
| 68-79% | Acceptable - Interview if pipeline allows |
| 57-67% | Borderline |
| <57% | Does Not Meet Threshold |

### Manager Roles (50 points)
| Percentage | Action |
|------------|--------|
| 90%+ | Exceptional - Fast track |
| 80-89% | Strong - Interview |
| 64-79% | Acceptable - Careful evaluation |
| 52-63% | Borderline |
| <52% | Does Not Meet Threshold |

---

## Privacy

All sensitive data is gitignored:
- `.org/*` (except `.org/example/`)
- `resumes/`
- `evaluations/`
- `reports/`
- `decisions/`

Only templates, examples, agent definitions, and documentation are committed.

---

## Workflow Example

```
# Day 1: Setup (one time)
> Run the setup wizard
[Setup wizard guides through configuration]

# Day 2: Batch of resumes arrives
# Drop resumes in resumes/developers/new/[Level]/ folders

> /evaluate-resumes

[Evaluates all resumes]
[Generates HR summaries]
[Creates batch report: reports/BATCH_2024-01-15.md]
[Creates CSV: reports/BATCH_2024-01-15.csv]

# Day 3: Schedule interviews
> Prepare interview for John Doe
[Generates customized questions based on resume]

# Day 4: After interview
# Drop interview_notes.md into evaluations/developers/2024-01-15_john-doe/

> /assess-interviews

[Scores interview responses]
[Generates interview HR summary]

# Day 5: Final decision
> Compare John Doe and Jane Smith
[Side-by-side comparison with recommendation]
```

---

## Documentation

| Document | Purpose |
|----------|---------|
| [CLAUDE.md](CLAUDE.md) | Complete agent methodology |
| [docs/GETTING_STARTED.md](docs/GETTING_STARTED.md) | Detailed setup guide |
| [config/schema.md](config/schema.md) | Configuration reference |
| [examples/](examples/) | Sample evaluations |

---

## Contributing

Contributions welcome! Please:

1. Keep the core framework region-agnostic
2. Add regional contexts via the `.org/example/regional/` template
3. Don't commit organization-specific rubrics or thresholds
4. Include anonymized examples for new features
5. Never modify files in `.org/example/` - it's the reference template

---

## License

[Your License Here]

---

Built with [Claude Code](https://github.com/anthropics/claude-code) by Anthropic.
