# SkillGraph — AI Career Gap Analyzer

An **AI-powered skill assessment and career gap analyzer** that maps your technical skills against target job roles, generates radar chart comparisons, identifies skill gaps, and produces a 3-phase learning roadmap — all in the browser, no backend required.

## Live Demo

[Open SkillGraph](https://sneha-chandra.github.io/skillgraph) &nbsp;·&nbsp; [GitHub](https://github.com/Sneha-chandra/skillgraph)

---

## System Architecture

```
┌────────────────────────────────────────────────────────────────────┐
│                   SkillGraph — 3-Step SPA                          │
│                                                                    │
│  ┌──────────────────────────────────────────────────────────────┐ │
│  │                     Data Model Layer                         │ │
│  │                                                              │ │
│  │  SKILL_CATEGORIES{}   8 categories × ~10 skills each        │ │
│  │    Frontend, Backend, DevOps, Data, ML/AI,                   │ │
│  │    Security, Cloud, Soft Skills                              │ │
│  │                                                              │ │
│  │  ROLES{}              8 target roles                        │ │
│  │    Each role: { required[], preferred[], description }       │ │
│  │    Required  = must-have for the JD                          │ │
│  │    Preferred = nice-to-have, differentiator                  │ │
│  └──────────────────────────┬───────────────────────────────────┘ │
│                              │                                     │
│  ┌───────────────────────────▼──────────────────────────────────┐ │
│  │                   3-Step UI Flow                             │ │
│  │                                                              │ │
│  │  Step 1: Skill Selection                                     │ │
│  │    Render checkbox grid per SKILL_CATEGORIES                 │ │
│  │    User selects skills they have                             │ │
│  │    selectedSkills = Set of checked skill names               │ │
│  │                                                              │ │
│  │  Step 2: Role Selection                                      │ │
│  │    Radio buttons for 8 ROLES                                 │ │
│  │    selectedRole → lookup ROLES[role]                         │ │
│  │                                                              │ │
│  │  Step 3: Analysis & Results                                  │ │
│  │    analyzeSkills() → all computations + render              │ │
│  └──────────────────────────┬───────────────────────────────────┘ │
│                              │                                     │
│  ┌───────────────────────────▼──────────────────────────────────┐ │
│  │                 Analysis Engine                              │ │
│  │                                                              │ │
│  │  computeMatchScore()                                         │ │
│  │    requiredHave    = selectedSkills ∩ role.required          │ │
│  │    preferredHave   = selectedSkills ∩ role.preferred         │ │
│  │    score = (requiredHave×2 + preferredHave) /               │ │
│  │            (required×2 + preferred)  × 100                  │ │
│  │                                                              │ │
│  │  buildRadarDatasets()                                        │ │
│  │    Per category: your coverage % vs role needs %            │ │
│  │    8 axes → radar chart comparison                          │ │
│  │                                                              │ │
│  │  buildGapTable()                                            │ │
│  │    missing_required  = role.required - selectedSkills        │ │
│  │    missing_preferred = role.preferred - selectedSkills       │ │
│  │    Sort required gaps first → table with priority column    │ │
│  │                                                              │ │
│  │  buildRoadmap()                                             │ │
│  │    Phase 1 (0-30d)  → top 3 required gaps                   │ │
│  │    Phase 2 (30-60d) → next required gaps + top preferred    │ │
│  │    Phase 3 (60-90d) → remaining preferred + stretch goals   │ │
│  │                                                              │ │
│  │  buildInsights()                                            │ │
│  │    score > 80% → "Strong Match" message                     │ │
│  │    score > 60% → "Good Foundation" message                  │ │
│  │    score < 40% → "Upskilling Recommended" message           │ │
│  └──────────────────────────┬───────────────────────────────────┘ │
│                              │                                     │
│  ┌───────────────────────────▼──────────────────────────────────┐ │
│  │              Chart.js Radar Visualization                   │ │
│  │                                                              │ │
│  │  Two datasets on one radar:                                 │ │
│  │    Dataset 1 (blue)  → Your skill coverage per category     │ │
│  │    Dataset 2 (amber) → Role requirement coverage %          │ │
│  │  8 axes = 8 SKILL_CATEGORIES                                │ │
│  │  Visual gap = area where role > your coverage              │ │
│  └──────────────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

---

## Project File Structure

```
skillgraph/
├── index.html          # Complete app — skill data, analysis engine, Chart.js radar, UI
└── README.md
```

---

## Skill Matrix Design

### Categories (8 total, 80+ skills)

| Category | Example Skills |
|---|---|
| Frontend | React, Vue, Angular, TypeScript, CSS3, Tailwind, Webpack |
| Backend | Node.js, Python, Java, Go, REST API, GraphQL, FastAPI |
| DevOps | Docker, Kubernetes, CI/CD, Terraform, GitHub Actions, AWS |
| Data Engineering | SQL, PostgreSQL, MongoDB, Redis, Kafka, Spark, ETL |
| ML/AI | Machine Learning, TensorFlow, PyTorch, NLP, LLM APIs, Pandas |
| Security | Penetration Testing, OWASP, Network Security, SIEM |
| Cloud | AWS, Azure, GCP, Serverless, IAM, S3, Lambda |
| Soft Skills | Project Management, Agile, Technical Writing, System Design |

### Roles (8 total)

| Role | Focus |
|---|---|
| Full Stack Developer | Frontend + Backend + DB |
| ML Engineer | ML/AI + Python + MLOps |
| DevOps / SRE | Cloud + Docker + Kubernetes + Monitoring |
| Data Engineer | SQL + ETL + Spark + Cloud |
| Backend Developer | API design + DB + System design |
| Security Engineer | Pentesting + SIEM + Network |
| Frontend Developer | React/Vue + CSS + UX |
| Product Engineer | Full stack + Agile + Design thinking |

---

## Match Score Algorithm

```
Required skills  → weighted 2× (must-have)
Preferred skills → weighted 1× (differentiator)

score = (requiredHave × 2 + preferredHave × 1)
        ─────────────────────────────────────────  × 100
        (totalRequired × 2 + totalPreferred × 1)

Thresholds:
  score ≥ 80% → "Strong Match — Ready to Apply"
  score ≥ 60% → "Good Foundation — Minor Gaps"
  score ≥ 40% → "Developing — Targeted Upskilling"
  score < 40% → "Significant Gap — Structured Plan Needed"
```

---

## Technical Knowledge Implemented

### Algorithm / Data Science Concepts
| Concept | Implementation |
|---|---|
| **Set operations** | `A ∩ B` (intersection) for skill gap computation |
| **Weighted scoring** | Required skills weighted 2× preferred in match score |
| **Multi-dimensional comparison** | 8-axis radar chart for category-level gap visualization |
| **Priority ranking** | Required gaps ranked above preferred in gap table |
| **Phased planning** | 3-phase roadmap derived algorithmically from gap priority |
| **Coverage %** | Per-category: your skills / role's skills × 100 |

### Chart.js Advanced Features
| Feature | Usage |
|---|---|
| **Radar chart** | `type: 'radar'`, 8 axes (one per skill category) |
| **Dual dataset** | "Your Skills" vs "Role Requirements" on same radar |
| **Custom scales** | `min: 0, max: 100`, `ticks: { stepSize: 25 }` |
| **Point styles** | `pointBackgroundColor`, `pointRadius` per dataset |
| **Area fill** | `fill: true`, semi-transparent RGBA colors |
| **Legend positioning** | `legend.position: 'top'` |

### JavaScript Patterns
| Pattern | Where Used |
|---|---|
| **Set data structure** | `selectedSkills` — O(1) lookup for has/hasn't skill |
| **Set intersection** | `new Set([...a].filter(x => b.has(x)))` |
| **Step indicator** | CSS class toggling on numbered step circles |
| **Form validation** | Step progression blocked if no skills / no role selected |
| **Dynamic DOM** | Checkbox grids generated from `SKILL_CATEGORIES` data |
| **Data-driven UI** | All content generated from data constants, not hardcoded HTML |

---

## User Flow

```
Step 1: Select Your Skills
   └── Check boxes across 8 categories (80+ skills)
          │
          ▼
Step 2: Choose Target Role
   └── Pick from 8 job roles (radio buttons)
          │
          ▼
Step 3: View Analysis
   ├── Match Score badge  (weighted %)
   ├── Radar Chart        (you vs role, per category)
   ├── Skill Gap Table    (required → preferred, priority order)
   ├── 3-Phase Roadmap    (30/60/90 day plan)
   └── AI Insights        (score-based personalized message)
          │
          ▼
      Reset All → Start Over
```

---

## How to Run

```bash
open index.html
# No server, no API calls, works offline
```

---

## Skills Demonstrated

`Algorithm Design` · `Set Theory (Intersection, Difference)` · `Weighted Scoring Models` · `Chart.js Radar Chart` · `Multi-dimensional Data Visualization` · `Career Analytics` · `Data-Driven UI` · `Dynamic DOM Generation` · `Multi-Step Form UX` · `Gap Analysis` · `Algorithmic Roadmap Generation` · `Vanilla JavaScript (ES6+)`
