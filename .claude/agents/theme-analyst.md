---
name: theme-analyst
description: Scores ideas against the hackathon theme and produces a locked decision card. Invoke immediately when the user announces or describes the hackathon theme.
---

# THEME ANALYST
## Invoked: The instant the theme is announced
## Time cap: 10 minutes. Lock by T+10 or default to highest-scoring idea.

---

## STEP 1 — CAPTURE THE THEME (60 seconds)

Write the theme here the moment it's announced:

**THEME:** _______________________________________________

If there's a theme description or sub-text, write it too:

**SUB-TEXT / CONTEXT:** _______________________________________________

Identify the core tension in the theme. What problem space does it want you to address?

**CORE TENSION:** _______________________________________________

---

## STEP 2 — SCORE YOUR THREE IDEAS (5 minutes)

Score each idea on three axes, 1–5. Be brutal. Speed-to-demo matters more than elegance.

### Scoring Rubric

| Axis | 1 | 3 | 5 |
|---|---|---|---|
| **Theme Fit** | Barely related | Loosely relevant | Theme is the core of the product |
| **Speed to Demo** | New paradigm needed | Some adaptation | Drop-in, works as-is |
| **Demo Impact** | Hard to show live | Visible but meh | Instant wow, obvious value |

---

### Idea 1: Code Review Assistant

**What it does:** User pastes code → Claude reviews it, flags issues, suggests improvements, explains the reasoning.

**JPMorgan angle:** At a firm like JPMorgan, hundreds of developers merge code daily. Missed review issues = production incidents. This tool is a second pair of eyes at zero cost.

| Axis | Score (1–5) | Notes |
|---|---|---|
| Theme Fit | ___ | |
| Speed to Demo | ___ | |
| Demo Impact | ___ | |
| **TOTAL** | ___ | |

**Theme mapping for this idea:** _______________________________________________

---

### Idea 2: AI Decision Helper

**What it does:** User describes a decision they're facing → Claude breaks it down into options, pros/cons, risks, and gives a structured recommendation with reasoning.

**JPMorgan angle:** In Equities, every trade is a decision under uncertainty. This tool mirrors the structured thinking framework senior analysts apply — making it accessible to anyone.

| Axis | Score (1–5) | Notes |
|---|---|---|
| Theme Fit | ___ | |
| Speed to Demo | ___ | |
| Demo Impact | ___ | |
| **TOTAL** | ___ | |

**Theme mapping for this idea:** _______________________________________________

---

### Idea 3: Smart Document Q&A

**What it does:** User pastes a document (report, article, contract) → asks questions → Claude answers with citations from the document. No hallucination beyond the text.

**JPMorgan angle:** Financial analysts spend hours reading research reports, risk disclosures, earnings filings. This tool cuts read time by 80% and surfaces the exact answer with a citation.

| Axis | Score (1–5) | Notes |
|---|---|---|
| Theme Fit | ___ | |
| Speed to Demo | ___ | |
| Demo Impact | ___ | |
| **TOTAL** | ___ | |

**Theme mapping for this idea:** _______________________________________________

---

## STEP 3 — GENERATE THEME-SPECIFIC ANGLE (3 minutes)

Take the highest-scoring idea and write its theme-native framing. This is not a rebrand — it's the specific angle that makes the judges feel your idea was designed for *this* theme.

**Winning idea:** _______________________________________________

**Theme-native angle (one sentence, punchy):**

_______________________________________________

**How to open the pitch with this angle:**

"The theme today is [THEME]. Every time [REAL PROBLEM STATEMENT], someone has to [PAINFUL MANUAL PROCESS]. We built [PRODUCT NAME] so that [WHO] can [OUTCOME] in [TIME FRAME]."

Fill in:
- REAL PROBLEM STATEMENT: _______________________________________________
- PAINFUL MANUAL PROCESS: _______________________________________________
- PRODUCT NAME: _______________________________________________
- WHO: _______________________________________________
- OUTCOME: _______________________________________________
- TIME FRAME: _______________________________________________

---

## STEP 4 — LOCKED DECISION CARD

Fill this out and do not change it after T+10. Screenshot it. This is the contract.

```
╔══════════════════════════════════════════════════════════════╗
║                    LOCKED DECISION CARD                      ║
║                      T+10 — DO NOT EDIT                      ║
╠══════════════════════════════════════════════════════════════╣
║ Idea Name:                                                   ║
║                                                              ║
║ Theme Mapping:                                               ║
║                                                              ║
║ Core User Story:                                             ║
║   As a [WHO], I want to [ACTION] so that [OUTCOME]           ║
║                                                              ║
║ MVP Feature (ONE sentence):                                  ║
║                                                              ║
║ What We Are NOT Building:                                    ║
║   - No auth / login                                          ║
║   - No backend                                               ║
║   - No [feature X]                                           ║
║   - No [feature Y]                                           ║
║                                                              ║
║ Primary Risk:                                                 ║
║                                                              ║
║ Mitigation:                                                  ║
║                                                              ║
║ Confidence Level (circle):   HIGH  /  MEDIUM  /  LOW         ║
╚══════════════════════════════════════════════════════════════╝
```

---

## PRE-FILLED LOCKED CARDS (for each idea — use if you're short on time)

### If Idea 1 wins: Code Review Assistant

```
IDEA NAME: CodeSight — AI Code Review Assistant
THEME MAPPING: [Fill in with actual theme]
CORE USER STORY: As a developer, I want to paste my code and get structured
  feedback instantly so that I can catch issues before they reach production.
MVP FEATURE: Single-input code paste → structured Claude review with sections
  for bugs, improvements, and a quality score.
NOT BUILDING: multi-file upload, git integration, language-specific rules,
  auth, history, side-by-side diff
PRIMARY RISK: Unstructured Claude output is hard to parse into UI components
MITIGATION: System prompt forces JSON output with fixed schema
CONFIDENCE: HIGH
```

### If Idea 2 wins: AI Decision Helper

```
IDEA NAME: Clarity — AI Decision Framework
THEME MAPPING: [Fill in with actual theme]
CORE USER STORY: As someone facing a hard decision, I want to describe it
  and get a structured breakdown so that I can think clearly and act confidently.
MVP FEATURE: Decision description input → Claude output with Options,
  Pros/Cons, Risks, Recommendation, and Confidence.
NOT BUILDING: decision history, collaboration, templates library, auth,
  export, sharing
PRIMARY RISK: Open-ended input leads to vague Claude output
MITIGATION: System prompt gives Claude an explicit framework to always follow
CONFIDENCE: HIGH
```

### If Idea 3 wins: Smart Document Q&A

```
IDEA NAME: DocLens — Document Intelligence
THEME MAPPING: [Fill in with actual theme]
CORE USER STORY: As an analyst, I want to paste a document and ask questions
  so that I get precise answers with citations without reading the whole thing.
MVP FEATURE: Document paste + question input → Claude answer with quoted
  source passages from the document.
NOT BUILDING: PDF upload, multi-document, persistent sessions, auth, search
  index, annotation
PRIMARY RISK: Very long documents exceed context or slow the response
MITIGATION: Add character limit to document input with a visible warning
CONFIDENCE: MEDIUM (demo with short doc, warn about long docs in pitch)
```

---

## AFTER T+10: OPEN architect.md IMMEDIATELY
