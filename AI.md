# AI.md - Hiring Evaluation Framework

**Shared methodology for evaluating software developers and engineering managers**

This document is the **single source of truth** for the Hiring Evaluation Framework's core methodology, evaluation principles, and agent system overview. Both Claude Code CLI and GitHub Copilot implementations reference this document to ensure consistent evaluations across AI providers.

---

## Provider-Specific Instructions

- **Claude Code CLI users:** See [CLAUDE.md](CLAUDE.md) for agent dispatch and commands
- **GitHub Copilot users:** See [.github/copilot-instructions.md](.github/copilot-instructions.md) for tool usage

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Configuration System](#configuration-system)
   - [Organization-Specific Configuration](#organization-specific-configuration)
   - [Critical Org Discovery Rules](#critical-org-discovery-rules)
   - [Protected Files](#protected-files-do-not-modify)
3. [Evaluation Principles](#evaluation-principles)
   - [Contextualize & Filter](#contextualize--filter-presentation-vs-substance)
   - [Always Focus On](#always-focus-on-evaluate-strictly)
   - [Red Flags That Matter](#red-flags-that-matter)
   - [Green Flags That Matter](#green-flags-that-matter)
   - [Regional Context](#regional-context)
4. [Scoring & Thresholds](#scoring--thresholds)
   - [IC Candidates](#ic-candidates-default-35-points-7-categories)
   - [Manager Candidates](#manager-candidates-default-50-points-10-categories)
   - [Interview Scoring (IC)](#interview-scoring-ic)
   - [Interview Scoring (Manager)](#interview-scoring-manager---higher-standard)
5. [Agent System](#agent-system)
   - [Available Agents](#available-agents)
   - [Agent Implementation](#agent-implementation)
6. [Team Collaboration Patterns](#team-collaboration-patterns)
   - [Resume Evaluation Flow (IC)](#resume-evaluation-flow-ic)
   - [Resume Evaluation Flow (Manager)](#resume-evaluation-flow-manager)
   - [Interview Flow](#interview-flow)
   - [Comparison Flow](#comparison-flow)
   - [Batch Reporting Flow](#batch-reporting-flow)
7. [File Organization](#file-organization)
   - [Directory Structure](#directory-structure)
   - [Folder Naming Convention](#folder-naming-convention)
   - [File Contents](#file-contents)
8. [Role Level Mapping](#role-level-mapping)
9. [Quality Checklist (CRITICAL REMINDERS)](#quality-checklist-critical-reminders)
10. [Getting Help](#getting-help)

---

## Project Overview

This framework helps evaluate candidates consistently using structured rubrics, reducing bias and ensuring fair assessment. It supports:

- **Resume evaluation** for IC (Individual Contributor) and Manager roles
- **Interview preparation** with gap-targeted questions
- **Interview assessment** with structured scoring
- **Candidate comparison** for decision-making
- **Batch reporting** and tracking

### Key Differentiators

- **Bias-aware:** Filters cultural presentation styles, focuses on substance
- **Structured scoring:** Quantified rubrics with clear criteria
- **Agent-based:** Specialist "team members" with defined roles
- **Regional context:** Interprets educational/cultural differences
- **Consistent:** Same methodology regardless of AI provider

---

## Configuration System

### Organization-Specific Configuration

All organization-specific data lives in `.org/[org-name]/` (gitignored). The system uses **convention over configuration** - drop files in folders and they'll be loaded automatically.

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

### Critical Org Discovery Rules **[CRITICAL]**

**When loading organization config, ALWAYS follow this order:**

1. **Scan `.org/` for folders** - List all directories in `.org/`
2. **ALWAYS IGNORE `.org/example/`** when any other folder exists
3. **Use the first non-example folder** (e.g., `.org/mycompany/`)
4. **If ONLY `.org/example/` exists** → STOP and ask the user to run setup

**Why this matters:**
- `.org/example/` contains generic templates with placeholder values
- Real evaluations require org-specific rubrics, levels, and tech stack
- Using example config will produce inaccurate scoring

**Never:**
- Load rubrics/matrices from `.org/example/` when another org folder exists
- Silently fall back to example config
- Mix files from example and org-specific folders

### Config Contents (config.yaml)

- **organization.name**: Your company name
- **internal_levels**: Your organization's internal grade system (G5, G6, G7, etc.)
- **role_levels**: External hiring roles mapped to internal grades with expected years of experience
- **competency_matrices**: Dimensions and expectations per level
- **thresholds**: Pass/fail scoring thresholds (percentage-based)
- **tech_stack**: Technology priorities for alignment scoring
- **regional**: Regional context configuration (enabled/disabled, triggers)

### Protected Files (DO NOT MODIFY)

The following files are **templates** for other users. **NEVER modify these:**

- `.org/example/` (entire folder - reference template)
- `config/config.yaml.example`
- `config/schema.md`
- `examples/*` (sample output files)

**When creating org-specific config:**
1. Copy `.org/example/` to `.org/[your-org-name]/`
2. Modify only the copy in `.org/[your-org-name]/`
3. Never touch `.org/example/` - it stays as a reference

---

## Evaluation Principles

### Contextualize & Filter (Presentation vs Substance)

**DO NOT penalize or comment on:**
- Photos, age, marital status, religion, personal details
- Resume length (1 page vs. 5 pages)
- Formatting style, colors, templates
- Tone (formal vs. casual, humble vs. confident)
- Generic accolades ("Employee of the Month", "Star Performer")
- Job title variations (focus on actual responsibilities)
- Educational institution prestige (focus on practical experience)
- Grammar quirks if meaning is clear
- Cultural presentation differences (verbose vs. concise)

### Always Focus On (Evaluate Strictly)

- **Specific technical accomplishments** with measurable outcomes
- **Depth of experience** with required technologies
- **Evidence of code quality practices** (testing, code review, CI/CD)
- **Problem-solving examples** with clear challenge/solution/outcome
- **Growth trajectory** and increasing responsibility
- **Code samples and portfolio quality**
- **Consistency and honesty** of claims

### Red Flags That Matter

- Cannot identify specific projects or contributions
- Vague descriptions throughout ("worked on various projects")
- Inconsistent timelines or impossible claims
- No code samples for senior positions
- Excessive job hopping without explanation (>3 jobs in 2 years)
- Technology time travel (5 years with 2-year-old framework)
- **For Managers:** Ticket jockey pattern (passive execution, no ownership)

### Green Flags That Matter

- **Specific metrics** ("reduced load time from 3s to 800ms")
- **Problem-solving stories** with challenge/solution/outcome format
- **Active GitHub** with meaningful projects (not just tutorials)
- **Clear progression** in responsibility over time
- **Quality focus** without prompting (mentions testing, architecture)
- **Teaching/sharing** (blogs, mentoring, open source contributions)
- **For Managers:** Systemic improvements, team growth, pushback on leadership

### Regional Context

If candidate is from a configured region:
- Load regional context files from `.org/[org-name]/regional/`
- Use context to interpret presentation styles and educational credentials
- Note cultural differences without penalizing
- Focus on substance over style

**Never include cultural/regional commentary in HR summaries** - keep those observations in full evaluations only.

---

## Scoring & Thresholds

### IC Candidates (Default: 35 points, 7 categories)

**Scoring:** Each category scored 0-5, total 35 points

**Default Thresholds (configure in `.org/[org]/config.yaml`):**

| Score | Percentage | Emoji | Action |
|-------|------------|-------|--------|
| 32-35 | 91%+ | ⭐ | Exceptional - Fast track |
| 28-31 | 80-89% | ⭐ | Strong - Interview |
| 24-27 | 68-79% | ✅ | Acceptable - Interview |
| 20-23 | 57-67% | ⚠️ | Borderline |
| 0-19 | <57% | ❌ | Does Not Meet Threshold |

### Manager Candidates (Default: 50 points, 10 categories)

**Scoring:** Each category scored 0-5, total 50 points

**Default Thresholds (configure in `.org/[org]/config.yaml`):**

| Score | Percentage | Emoji | Action |
|-------|------------|-------|--------|
| 45-50 | 90%+ | ⭐ | Exceptional - Fast track |
| 40-44 | 80-89% | ⭐ | Strong - Interview |
| 32-39 | 64-79% | ✅ | Acceptable - Careful evaluation |
| 26-31 | 52-63% | ⚠️ | Borderline |
| 0-25 | <52% | ❌ | Does Not Meet Threshold |

**Manager Additional Requirements:**
- Interview average must be 4.0+ (out of 5)
- No scores of 2 or below on leadership questions
- No ticket jockey pattern detected

### Interview Scoring (IC)

**Per Question: 1-5**
- **5**: Specific example + clear reasoning + learning + growth
- **4**: Specific example + clear reasoning + solid outcome
- **3**: Example provided but lacking depth
- **2**: Vague or hypothetical answer
- **1**: Cannot answer or fundamentally wrong

**Recommendation:**
- Average 4.0+ → Strong hire
- Average 3.5-3.9 → Hire
- Average 3.0-3.4 → Borderline
- Average <3.0 → No hire

### Interview Scoring (Manager - HIGHER STANDARD)

**Per Question: 1-5**
- **5**: Org/multi-team impact + systemic improvement + influences up/down/across
- **4**: Multi-team impact + systemic thinking
- **3**: Team-level impact (IC-level "good" answer)
- **2**: Individual scope only
- **1**: Cannot answer, blames others, ticket jockey pattern

**Recommendation:**
- Average 4.0+ AND no leadership scores ≤2 AND no ticket jockey → Hire
- Average 3.5-3.9 OR any leadership score =2 → Borderline
- Average <3.5 OR any leadership score ≤1 OR ticket jockey → No hire

---

## Agent System

The framework uses specialized agents with defined roles and responsibilities. Each agent has specific expertise and follows consistent methodologies.

### Available Agents

| Agent | Purpose | Implementation |
|-------|---------|----------------|
| **IC Evaluator** | Evaluate developer/IC resumes | `.claude/agents/ic-evaluator.md` |
| **Manager Evaluator** | Evaluate manager/EM resumes | `.claude/agents/manager-evaluator.md` |
| **Regional Education Advisor** | Translate educational credentials | `.claude/agents/regional-education-advisor.md` |
| **Regional Resume Analyst** | Challenge resume claims | `.claude/agents/regional-resume-analyst.md` |
| **Cultural Bias Advisor** | Identify evaluation biases | `.claude/agents/cultural-bias-advisor.md` |
| **Interview Prep** | Generate interview questions | `.claude/agents/interview-prep.md` |
| **Interview Assessor** | Score interview responses | `.claude/agents/interview-assessor.md` |
| **Candidate Summary Distiller** | Create HR summaries | `.claude/agents/candidate-summary-distiller.md` |
| **Candidate Comparer** | Compare finalists | `.claude/agents/candidate-comparer.md` |
| **Fact Checker** | Verify claims | `.claude/agents/fact-checker.md` |
| **Batch Reporter** | Generate reports | `.claude/agents/batch-reporter.md` |
| **Setup Wizard** | Initial configuration | `.claude/agents/setup-wizard.md` |

### Agent Implementation

**For Claude Code CLI:**
- Agent definitions live in `.claude/agents/[agent-name].md`
- Each file contains complete persona, methodology, and prompts
- See [CLAUDE.md](CLAUDE.md) for dispatch instructions

**For GitHub Copilot:**
- Implement agent behaviors using VS Code tools
- See [.github/copilot-instructions.md](.github/copilot-instructions.md) for tool mappings
- Follow same evaluation principles and scoring rubrics

**Shared Methodology:**
- All agents follow evaluation principles from this document
- All agents use scoring thresholds from this document
- All agents follow file organization conventions from this document

---

## Team Collaboration Patterns

### Resume Evaluation Flow (IC)

```
1. ic-evaluator reads resume
   ├─ Loads .org/[org]/ config and rubrics
   ├─ Scores 7 categories (35 points)
   └─ IF candidate from configured region:
      ├─ Invokes regional-education-advisor → translates credentials
      ├─ Invokes regional-resume-analyst → challenges claims
      └─ ic-evaluator adjusts scores based on context

2. ic-evaluator creates evaluation folder
   ├─ Format: evaluations/developers/YYYY-MM-DD_[emoji]_[name]_[score]-35/
   ├─ Saves [name]-evaluation.md
   └─ MOVES resume from resumes/ to evaluation folder

3. candidate-summary-distiller creates HR summary
   └─ Saves HR_SUMMARY_RESUME.md (exactly 2 sections)

4. IF candidate scored ✅ or ⭐:
   └─ interview-prep generates interview questions
      ├─ Saves INTERVIEW_PREP.md (study guide)
      └─ Saves INTERVIEW_NOTES.md (clean template)
```

### Resume Evaluation Flow (Manager)

```
1. manager-evaluator reads resume
   ├─ Loads .org/[org]/ config and manager rubrics
   ├─ Scores 10 categories (50 points)
   ├─ Checks for ticket jockey patterns
   └─ IF candidate from configured region:
      ├─ Invokes regional-education-advisor → translates credentials
      ├─ Invokes regional-resume-analyst → challenges claims
      └─ manager-evaluator adjusts scores based on context

2. manager-evaluator creates evaluation folder
   ├─ Format: evaluations/managers/YYYY-MM-DD_[emoji]_[name]_[score]-50/
   ├─ Saves [name]-evaluation.md
   └─ MOVES resume from resumes/ to evaluation folder

3. candidate-summary-distiller creates HR summary
   └─ Saves HR_SUMMARY_RESUME.md (exactly 2 sections)

4. IF candidate scored ✅ or ⭐:
   └─ interview-prep generates interview questions
      ├─ Saves INTERVIEW_PREP.md (study guide)
      └─ Saves INTERVIEW_NOTES.md (clean template)
```

### Interview Flow

```
1. [Human conducts interview using INTERVIEW_NOTES.md template]

2. interview-assessor reads transcription and/or notes
   ├─ Parses Q&A pairs with exact quotes
   ├─ Identifies role type (IC vs Manager)
   ├─ Applies appropriate scoring rubric (IC: 1-5, Manager: higher bar)
   ├─ For Managers: Checks ticket jockey patterns
   └─ Calculates average and determines recommendation

3. interview-assessor saves INTERVIEW_ASSESSMENT.md
   └─ Includes scores, exact quotes, average, recommendation

4. candidate-summary-distiller creates interview HR summary
   └─ Saves HR_SUMMARY_INTERVIEW.md (exactly 2 sections)
```

### Comparison Flow

```
1. candidate-comparer reads all candidate evaluation files
   ├─ Resume scores
   ├─ Interview scores (if available)
   └─ HR summaries

2. candidate-comparer creates comparison table
   ├─ Side-by-side scores, stack match, experience
   ├─ Category-by-category breakdown
   ├─ Identifies key differentiators (2+ point gaps)
   └─ Applies decision framework

3. candidate-comparer saves comparison analysis
   └─ decisions/YYYY-MM-DD_[role]_comparison.md
```

### Batch Reporting Flow

```
1. fact-checker validates all evaluations in batch
   └─ Saves reports/FACT_CHECK_REPORT_YYYY-MM-DD.md

2. batch-reporter summarizes batch
   ├─ Checks for VACANCY_CODES.md
   ├─ Extracts key data from all evaluations
   ├─ Creates markdown summary with stack ranking
   └─ Creates CSV tracking file

3. batch-reporter saves both files
   ├─ reports/BATCH_YYYY-MM-DD.md
   └─ reports/BATCH_YYYY-MM-DD.csv
```

---

## File Organization

### Directory Structure

```
/
├── AI.md                        # This file - shared methodology
├── CLAUDE.md                    # Claude-specific instructions
├── .github/
│   └── copilot-instructions.md  # Copilot-specific instructions
├── .org/                        # Organization-specific (gitignored)
│   └── [org-name]/
│       ├── config.yaml          # All org settings
│       ├── matrices/
│       │   ├── ic_matrix.md
│       │   └── manager_matrix.md
│       ├── rubrics/
│       │   ├── ic_*.md          # All IC rubrics/guides (auto-loaded)
│       │   └── manager_*.md     # All manager rubrics/guides (auto-loaded)
│       └── regional/
│           └── *.md             # Regional context (auto-loaded)
├── config/
│   ├── config.yaml.example      # Template (committed)
│   └── schema.md                # Configuration docs
├── resumes/                     # All candidate data (gitignored)
│   ├── developers/
│   │   ├── new/
│   │   │   └── [Grade Subfolder]/  # e.g., "Senior Engineer (L2)"
│   │   └── processed/
│   └── managers/
│       ├── new/
│       │   └── [Grade Subfolder]/
│       └── processed/
├── evaluations/                 # All evaluations (gitignored)
│   ├── developers/
│   │   ├── 2026-01-15_⭐_john-doe_32-35/
│   │   │   ├── john-doe-evaluation.md
│   │   │   ├── john-doe-resume.pdf
│   │   │   ├── HR_SUMMARY_RESUME.md
│   │   │   ├── INTERVIEW_PREP.md
│   │   │   ├── INTERVIEW_NOTES.md
│   │   │   ├── INTERVIEW_ASSESSMENT.md
│   │   │   └── HR_SUMMARY_INTERVIEW.md
│   │   ├── 2026-01-16_✅_jane-smith_25-35/
│   │   ├── 2026-01-17_⚠️_bob-wilson_22-35/
│   │   └── 2026-01-18_❌_alex-jones_15-35/
│   └── managers/ (same structure, scores out of 50)
├── reports/                     # Batch reports (gitignored)
│   ├── BATCH_YYYY-MM-DD.md
│   ├── BATCH_YYYY-MM-DD.csv
│   └── FACT_CHECK_REPORT_YYYY-MM-DD.md
├── decisions/                   # Candidate comparisons (gitignored)
│   └── YYYY-MM-DD_[role]_comparison.md
└── examples/                    # Anonymized examples (committed)
    ├── sample_evaluation.md
    └── sample_hr_summary.md
```

### Folder Naming Convention

**Format:** `YYYY-MM-DD_[emoji]_[candidate-name]_[score]-[max]/`

**Status Emoji by Score:**
- ⭐ = 80%+ (IC: 28+/35, Manager: 40+/50) - Strong/Exceptional
- ✅ = 68-79% IC, 64-79% Manager (IC: 24-27, Manager: 32-39) - Interview
- ⚠️ = 57-67% IC, 52-63% Manager (IC: 20-23, Manager: 26-31) - Borderline
- ❌ = <57% IC, <52% Manager (IC: <20, Manager: <26) - No Match

**Examples:**
- `2026-01-15_⭐_john-doe_32-35/`
- `2026-01-15_✅_jane-smith_25-35/`
- `2026-01-15_⚠️_bob-wilson_22-35/`
- `2026-01-15_❌_alex-jones_15-35/`

**Rules:**
- ALWAYS include emoji and score for at-a-glance status
- ALWAYS move resume into candidate's evaluation folder (don't leave in resumes/)
- NEVER create consolidated summary files - individual summaries per candidate per phase

### File Contents

#### [name]-evaluation.md
Team-based evaluation structure with individual agent sections:

```markdown
# RESUME EVALUATION

**Candidate:** Name
**Role:** Position
**Date:** YYYY-MM-DD

---

# AGENT ASSESSMENTS

## IC Evaluator Assessment (or Manager Evaluator Assessment)
[Primary scoring and analysis]

## Regional Education Advisor Notes
[Educational context - if applicable]

## Regional Resume Analyst Notes
[Critical analysis - if applicable]

## Cultural Bias Advisor Notes
[Cultural context - if invoked]

---

# CONSOLIDATED EVALUATION

## Final Score
## Recommendation
## Summary (Strengths, Concerns, Interview Focus)
## Role Level Assessment
```

#### HR_SUMMARY_RESUME.md
```markdown
# HR Summary - Resume Evaluation

**Candidate:** [Name]
**Role:** [Position]
**Evaluation Date:** [Date]
**Phase:** Resume Screening

---

## Strengths

[Single paragraph, 3-5 sentences, concrete evidence]

---

## Opportunities

[Single paragraph, 3-5 sentences, constructive framing]

---

## Resume Score

**Score:** X/35 (X%)
**Recommendation:** [Interview / Borderline / Does Not Meet Threshold]
```

#### HR_SUMMARY_INTERVIEW.md
Same format but with:
- **Phase:** Interview Assessment
- **Interview Score:** X.X/5.0 average
- Strengths/Opportunities from interview responses

---

## Role Level Mapping

Role levels are configured in `.org/[org-name]/config.yaml`. Create subfolders in `resumes/` matching your configured `subfolder` values.

**Example mapping:**

| Subfolder | Internal Level | Expectations |
|-----------|----------------|--------------|
| Software Engineer (L1) | G5 | Entry level, 0-2 years, learning |
| Senior Software Engineer (L2) | G6 | Independent delivery, 2-5 years |
| Lead Software Engineer (L3) | G7 | Technical leadership, 5+ years |
| Engineering Manager | G7 | People management + technical |
| Senior Engineering Manager | G8 | Multi-team leadership |
| Director | G9 | Org-level impact |

**The subfolder name determines the grade level** used for competency expectations.

---

## Quality Checklist (CRITICAL REMINDERS)

Before finalizing any evaluation:
- [ ] Scores add up correctly (verify math!)
- [ ] Folder name matches calculated score
- [ ] Resume moved (not copied) to evaluation folder
- [ ] HR summary has exactly 2 sections (no extras)
- [ ] No cultural/regional commentary in HR summaries
- [ ] Exact quotes pulled from transcriptions (not notes)
- [ ] All file paths use correct folder structure
- [ ] Bias mitigation applied (presentation vs substance)
- [ ] For managers: Ticket jockey check completed

---

## Getting Help

For detailed setup instructions:
- See [docs/GETTING_STARTED.md](docs/GETTING_STARTED.md)

For configuration options:
- See [config/schema.md](config/schema.md)

For sample outputs:
- See [examples/](examples/) folder

For provider-specific usage:
- **Claude Code CLI:** See [CLAUDE.md](CLAUDE.md)
- **GitHub Copilot:** See [.github/copilot-instructions.md](.github/copilot-instructions.md)
