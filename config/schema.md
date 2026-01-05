# Configuration Schema

This document describes all configuration options for the hiring framework.

## Setup

Organization-specific configuration lives in `.org/[org-name]/`:

1. Create your org folder: `.org/my-company/`
2. Copy structure from `.org/default/` or use `config.yaml.example` as starting point
3. The `.org/` folder is gitignored - all org data stays private

### Folder Structure
```
.org/[org-name]/
├── config.yaml          # Main configuration
├── matrices/
│   ├── ic_matrix.md     # IC competency expectations
│   └── manager_matrix.md
├── rubrics/
│   ├── ic_rubric.md     # IC evaluation rubric
│   └── manager_rubric.md
└── regional/
    └── india_context.md
```

Agents automatically discover the first org folder in `.org/` and load all files within it.

---

## Organization

```yaml
organization:
  name: "Your Company Name"
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Your organization name, used in reports and summaries |

---

## Internal Levels

Define your organization's internal grade/leveling structure. These are the actual internal grades that hiring roles map to.

### IC Internal Levels

```yaml
internal_levels:
  ic:
    - grade: "G5"
      title: "Software Engineer"
      experience: "0-4 years"
      hiring_status: "not_hiring"
      description: "Entry level, learning fundamentals"
    - grade: "G6"
      title: "Senior Software Engineer"
      experience: "2-6 years"
      hiring_status: "hiring"
      description: "Independent contributor, owns features"
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `grade` | string | Yes | Internal grade code (e.g., "G5", "G6", "G7") |
| `title` | string | Yes | Internal title at this grade |
| `experience` | string | No | Typical experience range |
| `hiring_status` | string | No | One of: `not_hiring`, `hiring`, `priority` |
| `description` | string | No | Brief description of the level |

### Manager Internal Levels

Same structure as IC. Managers may share grade codes with IC (e.g., G7 can be Staff Engineer or Technology Manager).

---

## Competency Matrices

Define the competency dimensions and level expectations. These drive evaluation scoring.

### Structure

```yaml
competency_matrices:
  ic:
    dimensions:
      - name: "Technical Scope"
        question: "Am I building knowledge and recognized as an expert?"
        weight: 1.0
    levels:
      G6:
        technical_scope:
          - "Debugs moderately complex bugs"
          - "Implements medium/large features"
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `dimensions` | array | Yes | List of competency dimensions |
| `dimensions[].name` | string | Yes | Display name for the dimension |
| `dimensions[].question` | string | No | Guiding question for evaluation |
| `dimensions[].weight` | float | No | Relative importance (1.0 = standard) |
| `levels` | object | Yes | Expectations keyed by internal grade |
| `levels.[grade].[dimension]` | array | Yes | List of expected competencies |

**Note:** Dimension keys in `levels` should be snake_case versions of dimension names.

### Matrix Files

For detailed competency descriptions, use markdown files in `.org/[name]/matrices/`:
- `ic_matrix.md` - Full IC competency expectations with examples
- `manager_matrix.md` - Full manager competency expectations

These files provide richer context than YAML allows and are referenced by agents during evaluation.

---

## Role Levels

Define your organization's leveling system. Each role level maps to a subfolder in the resumes directory.

### IC (Individual Contributor) Levels

```yaml
role_levels:
  ic:
    - code: "L1"
      name: "Software Engineer"
      subfolder: "Software Engineer (L1)"
      internal_grade: "G5"
      years_experience: "0-2"
      expectations: "Learning, needs guidance"
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | string | Yes | Short code for reports (e.g., "L1", "L2") |
| `name` | string | Yes | Display name for the hiring role |
| `subfolder` | string | Yes | Must match subfolder name in `resumes/developers/new/` |
| `internal_grade` | string | Yes | Maps to an `internal_levels` grade (e.g., "G6") |
| `years_experience` | string | No | Expected years of experience range |
| `expectations` | string | No | Brief description of level expectations |

### Manager Levels

```yaml
role_levels:
  manager:
    - code: "M1"
      name: "Engineering Manager"
      subfolder: "Engineering Manager"
      internal_grade: "G7"
      team_size: "5-8"
      expectations: "Single team management"
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | string | Yes | Short code (e.g., "M1", "M2") |
| `name` | string | Yes | Display name |
| `subfolder` | string | Yes | Must match subfolder in `resumes/managers/new/` |
| `internal_grade` | string | Yes | Maps to an `internal_levels` grade |
| `team_size` | string | No | Expected team size at this level |
| `expectations` | string | No | Brief description |

---

## Thresholds

Define scoring thresholds for resume evaluations and interviews.

### IC Thresholds

```yaml
thresholds:
  ic:
    max_score: 35
    categories: 7
    exceptional: 32    # 91%+
    strong: 28         # 80%+
    acceptable: 24     # 68%+
    borderline: 20     # 57%+
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `max_score` | integer | Yes | Maximum possible score (categories x 5) |
| `categories` | integer | Yes | Number of evaluation categories |
| `exceptional` | integer | Yes | Score threshold for "fast track" recommendation |
| `strong` | integer | Yes | Score threshold for "recommend interview" |
| `acceptable` | integer | Yes | Score threshold for "interview if pipeline allows" |
| `borderline` | integer | Yes | Score threshold for "consider for lower level" |

Candidates scoring below `borderline` are rejected.

### Manager Thresholds

Same structure as IC, but typically:
- 10 categories
- 50 max score
- Higher absolute thresholds due to more categories

### Interview Thresholds

```yaml
thresholds:
  interview:
    max_score: 65
    manager_max_score: 55
    pass: 0.70
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `max_score` | integer | Yes | Maximum IC interview score |
| `manager_max_score` | integer | Yes | Maximum manager interview score |
| `pass` | float | Yes | Minimum percentage to pass (0.0-1.0) |

---

## Paths

Configure directory paths relative to project root.

```yaml
paths:
  rubrics: "rubrics/"
  resumes:
    developers_new: "resumes/developers/new/"
    developers_processed: "resumes/developers/processed/"
    managers_new: "resumes/managers/new/"
    managers_processed: "resumes/managers/processed/"
  evaluations:
    developers: "evaluations/developers/"
    managers: "evaluations/managers/"
```

All paths should be relative to the project root and end with `/`.

---

## Regional Configuration

Enable regional context advisors for candidates from specific regions.

```yaml
regional:
  enabled: true
  contexts:
    india:
      enabled: true
      context_file: "regional/india_context.md"  # Relative to .org/[name]/
      triggers:
        - "india"
        - "bangalore"
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `enabled` | boolean | Yes | Master switch for regional features |
| `contexts.[region].enabled` | boolean | Yes | Enable this specific region |
| `contexts.[region].context_file` | string | Yes | Path relative to `.org/[name]/` |
| `contexts.[region].triggers` | array | Yes | Keywords that trigger this context |

### Creating Regional Context Files

Place context files in `.org/[name]/regional/`. Each file should contain:
- Educational institution rankings and context
- Common resume presentation patterns
- Cultural communication norms
- Interview interpretation guidance

---

## Technology Stack

Optional: Define your organization's technology priorities for alignment scoring.

```yaml
tech_stack:
  primary:
    - ".NET"
    - "React"
  secondary:
    - "Python"
    - "AWS"
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `primary` | array | No | Core technologies (weighted higher) |
| `secondary` | array | No | Acceptable technologies |

---

## Interview Configuration

```yaml
interview:
  min_questions_per_category: 2
  ticket_jockey_detection: true
  require_specifics: true
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `min_questions_per_category` | integer | No | Minimum questions per interview category |
| `ticket_jockey_detection` | boolean | No | Flag candidates who execute but don't lead |
| `require_specifics` | boolean | No | Require concrete examples, not hypotheticals |

---

## Complete Example

See `config.yaml.example` for a complete working example that you can copy and customize.
