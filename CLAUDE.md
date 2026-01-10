# CLAUDE.md - Claude Code CLI Instructions

**[CRITICAL] Always reference @AI.md for core methodology, principles, and agent definitions.**

This document contains **Claude Code CLI specific** instructions for using the Hiring Evaluation Framework. For the shared methodology, evaluation principles, and agent system overview, see [AI.md](AI.md). For detailed agent personas, see [`.claude/agents/*.md`](.claude/agents/).

---

## Table of Contents

1. [Quick Links](#quick-links)
2. [Configuration Quick Reference](#configuration-quick-reference)
3. [Role Routing](#role-routing)
4. [Agent Dispatch Table](#agent-dispatch-table)
5. [Claude Code CLI Specific Features](#claude-code-cli-specific-features)
   - [Context Management](#context-management-critical)
   - [Parallel Processing](#parallel-processing-up-to-5-at-a-time)
   - [Agent Return Messages](#agent-return-messages)
6. [Commands](#commands)
   - [/evaluate-resumes](#evaluate-resumes---run-full-evaluation-workflow)
   - [/assess-interviews](#assess-interviews---run-interview-assessment-workflow)
7. [Example Interactions](#example-interactions)
8. [Getting Help](#getting-help)

---

## Quick Links

- **Shared Methodology:** [AI.md](AI.md) - Core evaluation principles, agent definitions, workflows
- **Setup Guide:** [docs/GETTING_STARTED.md](docs/GETTING_STARTED.md)
- **Configuration:** [config/schema.md](config/schema.md)

---

## Configuration Quick Reference

Organization-specific data lives in `.org/[org-name]/` (gitignored). See [AI.md - Configuration System](AI.md#configuration-system) for complete details.

**Key points for Claude agents:**
- Agents automatically load all files from `.org/[org-name]/rubrics/` with appropriate prefix (`ic_*` or `manager_*`)
- **ALWAYS IGNORE `.org/example/`** when any other org folder exists
- Use `setup-wizard` agent for first-time setup

---

## Role Routing

**See [AI.md - Agent System](AI.md#agent-system) for the complete agent definitions.**

**Quick routing:**
- "Manager", "Director", "VP", "Head of" → Use `manager-evaluator` agent
- "Engineer", "Developer", "Architect" → Use `ic-evaluator` agent

---

## Agent Dispatch Table

**See [AI.md - Agent System](AI.md#agent-system) for the complete agent definitions, personas, and methodologies.**

| Task | Agent File | When to Use |
|------|------------|-------------|
| **Setup & Config** |
| Initial setup | `.claude/agents/setup-wizard.md` | First-time setup or new org configuration |
| **Resume Evaluation** |
| Evaluate IC resume | `.claude/agents/ic-evaluator.md` | Any developer/engineer resume |
| Evaluate manager resume | `.claude/agents/manager-evaluator.md` | Any EM/Director resume |
| Regional education context | `.claude/agents/regional-education-advisor.md` | Candidates from configured regions |
| Critical resume analysis | `.claude/agents/regional-resume-analyst.md` | Candidates from configured regions |
| Cultural bias check | `.claude/agents/cultural-bias-advisor.md` | When presentation feels "off" |
| **Interview** |
| Prepare interview questions | `.claude/agents/interview-prep.md` | Before interviewing a candidate |
| Assess interview responses | `.claude/agents/interview-assessor.md` | After interview, with notes or transcription |
| **Post-Evaluation** |
| Compare candidates | `.claude/agents/candidate-comparer.md` | When choosing between multiple candidates |
| Create HR summaries | `.claude/agents/candidate-summary-distiller.md` | After evaluation (phase=resume) or interview (phase=interview) |
| Verify factual claims | `.claude/agents/fact-checker.md` | After completing all evaluations in batch |
| Generate batch reports & CSV | `.claude/agents/batch-reporter.md` | After completing batch of evaluations |

---

## Claude Code CLI Specific Features

### Context Management (CRITICAL)

Processing many candidates can overflow agent context. Follow these rules:

**Before Large Batches:**
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
