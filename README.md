# Hiring Evaluation Framework

A bias-aware, consistent methodology for evaluating software developers and engineering managers using agents.

**Works with:**
- **Claude Code CLI** (original)
- **VS Code + GitHub Copilot**

---

## Table of Contents

- [Why This Exists](#why-this-exists)
- [What It Does](#what-it-does)
- [Quick Start](#quick-start)
- [Commands](#commands)
- [Directory Structure](#directory-structure)
- [Features](#features)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [Requirements](#requirements)

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

A configurable framework powered by AI assistants that helps you:

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
# Claude: claude-code
# VS Code: code .
```

### 2. Run Setup

**Claude Code CLI:**
Use the `setup wizard` to create your configuration:
- Guides you through organization setup
- Creates `.org/your-org-name/` folder
- Configures role levels and internal grades
- Sets scoring thresholds
- Helps customize evaluation rubrics

**VS Code + Copilot:**
Say "Set up the framework for my organization" in Copilot Chat (Ctrl+I or Cmd+I)

Or use VS Code tasks: Ctrl+Shift+P → "Tasks: Run Task" → "Setup: Create Organization Folder"

> **Note:** Both tools create the same `.org/your-org/` structure and use the same evaluation methodology from AI.md

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

**Claude Code CLI:**
```
/evaluate-resumes
```

**VS Code + Copilot:**
Say "Process new resumes" in Copilot Chat (Ctrl+I or Cmd+I)

This runs the complete workflow:
1. Scan all resume folders
2. Load your organization's rubrics and config
3. Evaluate each resume (IC or Manager rubric)
4. Create evaluation folders with scores
5. Generate HR summaries
6. Run fact-checker
7. Produce batch report and CSV

**VS Code users** can also use tasks to list pending resumes before evaluating.

---

## Commands


| Command | Or say | What It Does |
|---------|--------|--------------|
| `/evaluate-resumes` | "Process new resumes" | **Resume workflow** - processes all resumes in `/new` folders |
| `/assess-interviews` | "Assess interviews" | **Interview workflow** - processes all interview notes in evaluation folders |
| Run the setup wizard | "Set up the framework" | First-time configuration |
| Prepare interview for [name] | "Prepare interview for [name]" | Generate interview questions |
| Compare candidates | "Compare candidates" | Side-by-side comparison |

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
│       ├── 2024-01-15_⭐_john-doe_32-35/     # ⭐ = Strong/Exceptional (Fast Track)
│       ├── 2024-01-16_✅_jane-smith_25-35/   # ✅ = Interview
│       ├── 2024-01-17_⚠️_bob-wilson_22-35/   # ⚠️ = Borderline
│       └── 2024-01-18_❌_alex-jones_15-35/   # ❌ = Does Not Meet Threshold
├── reports/                     # Batch reports & CSV (gitignored)
├── .github/
│   └── copilot-instructions.md  # GitHub Copilot instructions
├── .claude/
│   └── agents/                  # Claude Code CLI agents
    └── commands/                # Commands like /evaluate-resumes
├── .vscode/
│   └── tasks.json               # VS Code tasks for quick workflows
├── examples/                    # Sample outputs (committed)
├── docs/                        # Documentation
├── AI.md                        # Core methodology (shared)
├── CLAUDE.md                    # Claude-specific instructions
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

## Architecture

This framework uses a **shared methodology** approach to prevent drift between AI providers:

- **[AI.md](AI.md)** - Single source of truth for evaluation principles, scoring rubrics, agent personas, and workflows
- **[CLAUDE.md](CLAUDE.md)** - Claude Code CLI implementation (agent dispatch, commands)
- **[.github/copilot-instructions.md](.github/copilot-instructions.md)** - GitHub Copilot implementation (tool mappings)

Both providers reference AI.md for all evaluation logic, ensuring consistent assessments regardless of which tool you use.

## How It Works

**Claude Code CLI:** Uses specialized agents in `.claude/agents/` folder that implement AI.md methodology  
**VS Code + Copilot:** Uses VS Code tools to implement AI.md methodology via copilot-instructions.md

Both tools load your organization's config from `.org/your-org/` and apply identical rubrics and scoring.

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

**Day 1: Setup (one time)**

**Claude Code CLI:**
```
Run the setup wizard
```

**VS Code + Copilot:**
Say "Set up the framework for my organization" in Copilot Chat (Ctrl+I or Cmd+I)

Tool guides through configuration, creates `.org/mycompany/` folder

**Day 2: Batch of resumes arrives**

Drop resumes in `resumes/developers/new/[Level]/` folders

**Claude Code CLI:**
```
/evaluate-resumes
```

**VS Code + Copilot:**
Say "Process new resumes" in Copilot Chat (Ctrl+I or Cmd+I), or use VS Code Task: "Resumes: List Pending Evaluations"

✓ Evaluates all resumes with appropriate rubric  
✓ Creates evaluation folders with scores  
✓ Generates HR summaries  
✓ Creates batch report: `reports/BATCH_2024-01-15.md`  
✓ Creates CSV: `reports/BATCH_2024-01-15.csv`

**Day 3: Schedule interviews**
Say "Prepare interview for John Doe"

✓ Generates `INTERVIEW_PREP.md` (study material)  
✓ Generates `INTERVIEW_NOTES.md` (note-taking template)

**Day 4: After interview**

Fill in `INTERVIEW_NOTES.md` during/after interview, then say "Assess interview for John Doe"

✓ Scores interview responses  
✓ Generates `INTERVIEW_ASSESSMENT.md`  
✓ Generates interview HR summary

**Day 5: Final decision**
Say "Compare John Doe and Jane Smith"

✓ Side-by-side comparison  
✓ Hiring recommendation with confidence level

---

## Documentation

| Document | Purpose |
|----------|---------|
| **[AI.md](AI.md)** | **🎯 SHARED FOUNDATION - All evaluation logic, scoring, principles** |
| [CLAUDE.md](CLAUDE.md) | Claude Code CLI: how to implement AI.md (agent dispatch) |
| [.github/copilot-instructions.md](.github/copilot-instructions.md) | GitHub Copilot: how to implement AI.md (tool usage) |
| [docs/GETTING_STARTED.md](docs/GETTING_STARTED.md) | Detailed setup guide |
| [config/schema.md](config/schema.md) | Configuration reference |
| [.claude/agents/](.claude/agents/) | Claude agent definitions (implement AI.md methodology) |
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

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Requirements

**For Claude Code CLI:**
- [Claude Code CLI](https://github.com/anthropics/claude-code) installed

**For VS Code + GitHub Copilot:**
- [VS Code](https://code.visualstudio.com/) installed
- [GitHub Copilot](https://github.com/features/copilot) subscription and extension enabled

---

**Built with AI assistants:** [Claude](https://www.anthropic.com/claude) and [GitHub Copilot](https://github.com/features/copilot)
