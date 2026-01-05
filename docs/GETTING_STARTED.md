# Getting Started

This guide walks you through setting up the Hiring Evaluation Framework for your organization.

---

## Prerequisites

- [Claude Code CLI](https://github.com/anthropics/claude-code) installed
- Git repository cloned locally

---

## Quick Start (5 minutes)

### 1. Create Your Configuration

```bash
cp config/config.yaml.example config/config.yaml
```

Edit `config/config.yaml` to customize:
- Your organization name
- Role levels that match your leveling system
- Scoring thresholds
- Enable/disable regional contexts

### 2. Create Your Rubrics

```bash
# For IC (developer) roles
cp rubrics/templates/ic_rubric_template.md rubrics/developers/EVALUATION_RUBRIC.md

# For manager roles
cp rubrics/templates/manager_rubric_template.md rubrics/managers/MANAGER_EVALUATION_RUBRIC.md
```

Edit each rubric to define your scoring criteria for each category.

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
2. Open Claude Code in the project directory
3. Say: "Process new resumes"

---

## Detailed Setup

### Configuration Deep Dive

See `config/schema.md` for complete documentation of all configuration options.

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

Each rubric category should have:
- **Clear criteria** for each score level (5=exceptional through 0=absent)
- **What to look for** - signals of quality
- **What to ignore** - cultural differences, formatting
- **Red flags** - warning signs

Example:

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

1. Enable in config.yaml:
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

2. Create regional context files in `config/regional/` with:
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

- See `CLAUDE.md` for the full methodology
- See `config/schema.md` for configuration options
- See `rubrics/README.md` for rubric customization
- See `examples/` for sample evaluations
