# GitHub Copilot Instructions: Hiring Evaluation Framework

**[CRITICAL] Always reference the [AI.md](../AI.md) file (attached below) for core methodology, evaluation principles, and agent system.**

This document provides **GitHub Copilot-specific** instructions for using the Hiring Evaluation Framework. For the shared methodology, agent personas, and workflows, see [AI.md](../AI.md).

---

## Table of Contents

1. [Quick Links](#quick-links)
2. [Copilot-Specific Guidelines](#copilot-specific-guidelines)
   - [Key Principles (CRITICAL)](#key-principles-critical)
   - [Tool Usage Patterns](#tool-usage-patterns)
3. [Agent Role Mapping](#agent-role-mapping)
4. [Task Categories](#task-categories)
   - [1. Setup and Configuration](#1-setup-and-configuration)
   - [2. Resume Evaluation (IC Roles)](#2-resume-evaluation-ic-roles)
   - [3. Resume Evaluation (Manager Roles)](#3-resume-evaluation-manager-roles)
   - [4. HR Summary Generation](#4-hr-summary-generation)
   - [5. Interview Preparation](#5-interview-preparation)
   - [6. Interview Assessment](#6-interview-assessment)
   - [7. Candidate Comparison](#7-candidate-comparison)
   - [8. Batch Reporting](#8-batch-reporting)
   - [9. Fact Checking](#9-fact-checking)

---

## Quick Links

- **Shared Methodology:** See attached AI.md below for:
  - [Configuration System](../AI.md#configuration-system)
  - [Evaluation Principles](../AI.md#evaluation-principles)
  - [Agent System](../AI.md#agent-system)
  - [Team Collaboration Patterns](../AI.md#team-collaboration-patterns)
  - [Scoring & Thresholds](../AI.md#scoring--thresholds)
  - [File Organization](../AI.md#file-organization)
- **Setup Guide:** [docs/GETTING_STARTED.md](../docs/GETTING_STARTED.md)
- **Configuration:** [config/schema.md](../config/schema.md)

---

## Copilot-Specific Guidelines

### Key Principles (CRITICAL)

**See [AI.md](../AI.md) for complete methodology:**
- **Agent System:** 12 specialist agents with defined roles ([AI.md - Agent System](../AI.md#agent-system))
- **Scoring Systems:** IC (35 pts), Manager (50 pts, higher bar), Interview (1-5 scale) ([AI.md - Scoring & Thresholds](../AI.md#scoring--thresholds))
- **Bias Mitigation:** Filter presentation vs. substance ([AI.md - Evaluation Principles](../AI.md#evaluation-principles))
- **Regional Context:** Use `.org/[org]/regional/` files, NO commentary in HR summaries ([AI.md - Regional Context](../AI.md#regional-context))
- **File Organization:** Structured folders with emoji status indicators ([AI.md - File Organization](../AI.md#file-organization))

### Tool Usage Patterns

**Map agent behaviors to Copilot tools:**
- `read_file` - Load org config, rubrics, matrices, evaluations
- `create_file` - Generate evaluations, HR summaries, reports
- `create_directory` - Create evaluation folders
- `run_in_terminal` - Move resume files (not copy!), check file counts
- `list_dir` - Scan `.org/` for org folders, scan `resumes/` for pending files
- `file_search` - Find `.vtt` transcription files
- `grep_search` - Find candidates from specific regions

---

## Agent Role Mapping

**See [AI.md - Agent System](../AI.md#agent-system) for complete agent persona definitions.** Map user requests to these agent behaviors:

| User Request | Agent Persona (from [AI.md](../AI.md#agent-system)) | What to Implement |
|--------------|----------------------------|-------------------|
| "Set up the framework" | Setup Wizard | Guide through org config creation |
| "Evaluate this resume" (IC) | IC Evaluator | Apply 7-category rubric (35 pts) |
| "Evaluate this resume" (Manager) | Manager Evaluator | Apply 10-category rubric (50 pts) |
| "Create HR summary" | Candidate Summary Distiller | Generate exactly 2 sections |
| "Prepare interview" | Interview Prep | Generate PREP + NOTES files |
| "Assess interview" | Interview Assessor | Score responses with exact quotes |
| "Compare candidates" | Candidate Comparer | Side-by-side with decision framework |
| "Create batch report" | Batch Reporter | Markdown + CSV output |
| "Check facts" | Fact Checker | Verify quantifiable claims |
| "Process new resumes" | IC/Manager Evaluator (batch) | Evaluate all resumes in `resumes/.../new/` |

---

## Task Categories

**All evaluation logic is in [AI.md](../AI.md). This section shows ONLY which VS Code tools to use.**

### 1. Setup and Configuration

**Trigger:** "Set up the framework", "Configure for my organization", "First time setup"

**Implementation:**
1. Use `list_dir` to check `.org/` for non-example folders
2. If only `.org/example/` exists → Guide user through setup (see [AI.md - Setup Wizard](../AI.md#agent-system))
3. Use `run_in_terminal` to copy: `cp -r .org/example .org/[company-name]`
4. Use `read_file` and `replace_string_in_file` to update config.yaml

**Critical:** ALWAYS ignore `.org/example/` when other org folders exist

---

### 2. Resume Evaluation (IC Roles)

**Tools:**
1. `list_dir` → `.org/` (ignore example folder)
2. `read_file` → config.yaml, `ic_*.md` rubrics, ic_matrix.md
3. Apply rubric (see [AI.md - IC Evaluator](../AI.md#agent-system) for scoring)
4. `create_directory` → `evaluations/developers/YYYY-MM-DD_[emoji]_[name]_[score]-35/`
5. `create_file` → evaluation file
6. `run_in_terminal` → `mv resumes/.../file.pdf evaluations/.../` (MOVE not copy!)

**Note:** If candidate is from a configured region, automatically apply regional context:
- **Regional Education Advisor**: Interpret educational credentials (institution quality, grading systems, test scores)
- **Regional Resume Analyst**: Challenge claims critically (tenure patterns, technical impact, quantifiable outcomes)
- **Cultural Bias Advisor**: Contextualize presentation style (resume length, formality, communication patterns)
- Document regional context in evaluation, but exclude from HR summaries

**Regional Agent Behaviors:**
- **Education Advisor**: Classify institutions (Elite/Top-tier/Strong/Mid-tier), interpret grading systems (CGPA/percentage conversions), explain test score significance, identify credential quality signals
- **Resume Analyst**: Flag tenure <12 months, demand specific impact metrics, challenge vague statements, verify timeline consistency, distinguish presentation vs. substance
- **Bias Advisor**: Separate cultural norms (verbosity, formality, deference) from genuine red flags (vagueness, inability to provide examples, inconsistencies)

---

### 3. Resume Evaluation (Manager Roles)

**Tools:**
1. `read_file` → config.yaml, `manager_*.md` rubrics, manager_matrix.md
2. Apply rubric + check ticket jockey patterns (see [AI.md - Manager Evaluator](../AI.md#agent-system))
3. `create_directory` → `evaluations/managers/YYYY-MM-DD_[emoji]_[name]_[score]-50/`
4. `run_in_terminal` → `mv` resume to evaluation folder

**Note:** If candidate is from a configured region, automatically apply regional context (see Task 2 for regional agent behavior definitions):
- **Regional Education Advisor**: Interpret educational credentials
- **Regional Resume Analyst**: Challenge claims critically
- **Cultural Bias Advisor**: Contextualize presentation style
- Document regional context in evaluation, but exclude from HR summaries

---

### 4. HR Summary Generation

**Tools:**
1. `read_file` → evaluation file
2. `create_file` → `HR_SUMMARY_RESUME.md` or `HR_SUMMARY_INTERVIEW.md`

**Format:** Exactly 2 sections (Strengths + Opportunities). See [AI.md - Candidate Summary Distiller](../AI.md#agent-system) for exclusions.

---

### 5. Interview Preparation

**Tools:**
1. `read_file` → evaluation + `.org/[org]/rubrics/*_interview_questions.md`
2. `create_file` → `INTERVIEW_PREP.md` (study guide)
3. `create_file` → `INTERVIEW_NOTES.md` (template)

See [AI.md - Interview Prep](../AI.md#agent-system) for question selection logic.

---

### 6. Interview Assessment

**Tools:**
1. `file_search` → `.vtt` files (preferred) or `read_file` → `INTERVIEW_NOTES.md`
2. Parse Q&A with exact quotes
3. Apply scoring (see [AI.md - Interview Assessor](../AI.md#agent-system) for IC vs Manager scales)
4. `create_file` → `INTERVIEW_ASSESSMENT.md`

---

### 7. Candidate Comparison

**Tools:**
1. `read_file` → all candidate evaluation files
2. Create comparison table (see [AI.md - Candidate Comparer](../AI.md#agent-system) for framework)
3. `create_file` → `decisions/YYYY-MM-DD_[role]_comparison.md`

---

### 8. Batch Reporting

**Tools:**
1. `file_search` → `VACANCY_CODES.md` (optional)
2. `list_dir` → evaluation folders
3. `read_file` → extract data from evaluations
4. `create_file` → `reports/BATCH_YYYY-MM-DD.md` + `.csv`

See [AI.md - Batch Reporter](../AI.md#agent-system) for report structure.

---

### 9. Fact Checking

**Tools:**
1. `read_file` → evaluation + original resume
2. Extract quantifiable claims (dates, durations, counts)
3. Cross-reference
4. `create_file` → `reports/FACT_CHECK_REPORT_YYYY-MM-DD.md`

See [AI.md - Fact Checker](../AI.md#agent-system) for severity levels.
