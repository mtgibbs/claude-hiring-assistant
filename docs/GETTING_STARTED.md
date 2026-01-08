# Getting Started

This guide walks you through setting up the Hiring Evaluation Framework for your organization.

**This framework works with:**
- **Claude Code CLI** (original)
- **VS Code + GitHub Copilot**

---

## Table of Contents

1. [Core Documentation](#core-documentation)
2. [Prerequisites](#prerequisites)
3. [Quick Start (5 minutes)](#quick-start-5-minutes)
   - [1. Create Your Configuration](#1-create-your-configuration)
   - [2. Create Your Rubrics](#2-create-your-rubrics)
   - [3. Set Up Resume Folders](#3-set-up-resume-folders)
   - [4. Start Evaluating](#4-start-evaluating)
4. [Detailed Setup](#detailed-setup)
   - [Configuration Deep Dive](#configuration-deep-dive)
   - [Rubric Customization](#rubric-customization)
   - [Regional Context](#regional-context)
5. [Workflow Overview](#workflow-overview)
   - [Resume Evaluation Flow](#resume-evaluation-flow)
   - [Interview Flow](#interview-flow)
   - [Comparison Flow](#comparison-flow)
6. [Common Commands](#common-commands)
7. [File Organization](#file-organization)
8. [Tips](#tips)
   - [Setting Your Quality Bar](#setting-your-quality-bar)
   - [Calibrating Scores](#calibrating-scores)
   - [Cultural Considerations](#cultural-considerations)
9. [Need Help?](#need-help)

---

## Core Documentation

**Architecture:** This framework uses a shared methodology approach:
- **[AI.md](../AI.md)** 🎯 **SHARED FOUNDATION** - All evaluation logic, scoring rubrics, agent personas (read this first!)
- **[CLAUDE.md](../CLAUDE.md)** - How Claude Code CLI implements AI.md (agent dispatch, commands)
- **[.github/copilot-instructions.md](../.github/copilot-instructions.md)** - How GitHub Copilot implements AI.md (tool mappings)
- **[config/schema.md](../config/schema.md)** - Configuration options reference

> Both providers reference AI.md for all evaluation decisions, ensuring no drift between tools.

---

## Prerequisites

**For Claude Code CLI:**
- [Claude Code CLI](https://github.com/anthropics/claude-code) installed
- Git repository cloned locally

**For VS Code + GitHub Copilot:**
- [VS Code](https://code.visualstudio.com/) installed
- [GitHub Copilot](https://github.com/features/copilot) extension enabled
- Git repository cloned locally

---

## Quick Start (5 minutes)

### 1. Create Your Configuration

**Using the setup wizard (recommended):**

**Claude Code CLI:**
```
Run the setup wizard
```

**VS Code + Copilot:**
Say "Set up the framework for my organization" in Copilot Chat (Ctrl+I or Cmd+I)

The wizard will:
- Create `.org/your-org-name/` folder
- Copy templates from `.org/example/`
- Guide you through configuration
- Set up role levels and thresholds

**Manual alternative:**
```bash
cp -r .org/example .org/your-org-name
cd .org/your-org-name
# Edit config.yaml, rubrics/, matrices/
```

Edit `config/config.yaml` to customize:
- Your organization name
- Role levels that match your leveling system
- Scoring thresholds
- Enable/disable regional contexts

### 2. Create Your Rubrics


The setup wizard handles this automatically, or you can manually customize:

```bash
# After copying .org/example to .org/your-org-name
cd .org/your-org-name/rubrics/

# Customize IC rubrics (any file starting with ic_*)
# Edit ic_rubric.md - define your IC scoring criteria

# Customize manager rubrics (any file starting with manager_*)  
# Edit manager_rubric.md - define your manager scoring criteria
```

**Convention:** Files prefixed with `ic_*` auto-load for IC evaluations, `manager_*` for manager evaluations.

### 3. Set Up Resume Folders

Create subfolders in `resumes/` that match your configured role levels:

```bash
# Example for IC roles (adjust names to match your config)
mkdir -p "resumes/developers/new/Software Engineer (L1)"
mkdir -p "resumes/developers/new/Senior Software Engineer (L2)"
mkdir -p "resumes/developers/new/Lead Software Engineer (L3)"

# Example for manager roles
mkdir -p "resumes/managers/new/Engineering Manager"
mkdir -p "resumes/managers/new/Director"

# Processed folders
mkdir -p resumes/developers/processed
mkdir -p resumes/managers/processed
```

### 4. Start Evaluating

1. Place resume PDFs in the appropriate `resumes/.../new/[role-level]/` folder
2. Say: "Process new resumes"

---

## Detailed Setup

### Configuration Deep Dive

**All organization-specific config lives in `.org/your-org-name/`** (gitignored)

See [config/schema.md](../config/schema.md) for complete documentation.

**Key settings to customize:**

```yaml
# Your organization
organization:
  name: "Acme Corp"

# Role levels (create matching resume subfolders)
role_levels:
  ic:
    - code: "L1"
      name: "Software Engineer"
      subfolder: "Software Engineer (L1)"  # Must match folder name
      years_experience: "0-2"
      expectations: "Learning, needs guidance"

# Scoring thresholds (adjust to your quality bar)
thresholds:
  ic:
    max_score: 35      # 7 categories x 5 points
    exceptional: 32    # 91%+ = fast track
    strong: 28         # 80%+ = interview
    acceptable: 24     # 68%+ = maybe
    borderline: 20     # 57%+ = unlikely
```

### Rubric Customization

**Rubrics live in `.org/your-org-name/rubrics/`**

**Convention:** Files prefixed with `ic_*` load for IC evaluations, `manager_*` for managers

Each rubric category should have:
- **Clear criteria** for each score level (5=exceptional through 0=absent)
- **What to look for** - signals of quality
- **What to ignore** - cultural differences, formatting
- **Red flags** - warning signs

Example from `.org/your-org/rubrics/ic_rubric.md`:

```markdown
## Category 1: Technical Stack Alignment (0-5)

**What to look for:**
- 3+ years production experience with our core stack
- Recent usage (within last 1-2 years)
- Depth, not just surface exposure

**Scoring:**
- **5**: 3+ years with React + TypeScript + Node.js in production
- **4**: 2+ years with core stack
- **3**: 1-2 years OR strong adjacent experience
- **2**: Limited but transferable
- **1**: Tangential only
- **0**: No relevant experience
```

### Regional Context

If you hire from specific regions, configure regional advisors:

1. Enable in `.org/your-org-name/config.yaml`:
```yaml
regional:
  enabled: true
  contexts:
    india:
      enabled: true
      context_file: "config/regional/india_context.md"
      triggers:
        - "india"
        - "bangalore"
```

2. Create regional context files in `.org/your-org-name/regional/`:
- Educational institution rankings
- Grading system interpretation
- Resume presentation norms
- Interview communication patterns

See `config/regional/india_context.md` for an example.

---

## Workflow Overview

### Resume Evaluation Flow

```
1. Add resumes to resumes/.../new/[role-level]/
2. "Process new resumes"
   → ic-evaluator or manager-evaluator runs
   → Regional agents invoked if configured
   → Evaluation saved to evaluations/.../
3. "Create HR summaries"
   → candidate-summary-distiller generates summaries
4. "Run fact-checker"
   → Verifies factual claims
5. "Generate batch report"
   → Creates summary report and CSV
```

### Interview Flow

```
1. "Prepare interview for [candidate]"
   → interview-prep generates customized questions
2. Conduct interview, take notes
3. "Assess interview for [candidate]"
   → interview-assessor evaluates responses
4. "Create interview HR summary"
   → candidate-summary-distiller generates summary
```

### Comparison Flow

```
1. "Compare candidates for [role]"
   → candidate-comparer analyzes all evaluations
   → Produces side-by-side comparison
```

---

## Common Commands

**How to use:** 
- **Claude:** Say commands directly
- **Copilot:** Open Copilot Chat (Ctrl+I or Cmd+I), then say commands

| What you say | What happens |
|--------------|--------------|
| "Process new resumes" | Evaluates all resumes in new/ folders |
| "Evaluate this resume" | Evaluates a specific resume |
| "Create HR summaries" | Generates summaries for all candidates |
| "Prepare interview for [name]" | Creates interview questions |
| "Assess interview" | Evaluates interview notes |
| "Compare candidates" | Side-by-side comparison |
| "Run fact-checker" | Verifies claims in evaluations |
| "Generate batch report" | Creates summary report |

---

## File Organization

After processing, files are organized as:

```
evaluations/
└── developers/
    └── 2024-01-15_jane-doe/
        ├── jane-doe-evaluation.md    # Full evaluation
        ├── HR_SUMMARY_RESUME.md       # HR summary from resume
        ├── INTERVIEW_ASSESSMENT.md    # Interview scoring
        ├── HR_SUMMARY_INTERVIEW.md    # HR summary from interview
        └── jane-doe-resume.pdf        # Copy of resume
```

---

## Tips

### Setting Your Quality Bar

Start with default thresholds and adjust based on your hiring funnel:
- Too many candidates passing? Raise thresholds
- Too few? Lower or add more candidates to pipeline

### Calibrating Scores

After a few evaluations, compare scores to interview performance:
- Candidates scoring 28+ should do well in interviews
- If not, your rubric criteria may need adjustment

### Cultural Considerations

If evaluating candidates from unfamiliar regions:
1. Enable regional contexts if available
2. Use the cultural-bias-advisor when something feels "off"
3. Focus on substance, not presentation style

---

## Need Help?

**Core Methodology:**
- See [AI.md](../AI.md) 🎯 for ALL evaluation logic, scoring, principles (shared foundation)

**Configuration:**
- See [config/schema.md](../config/schema.md) for all configuration options
- See `.org/example/` for configuration templates
- See `examples/` for sample evaluations

**Provider-Specific Implementation:**
- **Claude Code CLI:** See [CLAUDE.md](../CLAUDE.md) for agent dispatch and `.claude/agents/` for agent definitions
- **VS Code + Copilot:** See [.github/copilot-instructions.md](../.github/copilot-instructions.md) for tool mappings
