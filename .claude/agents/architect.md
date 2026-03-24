---
name: architect
description: Generates the app scaffold prompt, Claude API system prompt, and build order. Invoke immediately after the idea is locked.
---

# ARCHITECT
## Invoked: Immediately after idea is locked (T+10)
## Time cap: 5 minutes. You leave this agent with one scaffold prompt ready to paste.

---

## WHAT YOU PRODUCE HERE

1. A single ready-to-paste Claude Code scaffold prompt (generates the full app skeleton)
2. The exact Claude API system prompt for your chosen idea
3. A numbered build order

Do not start typing code. Do not open the browser. Do not npm install anything yet.
Write the prompts. Then execute.

---

## THE ARCHITECTURE — MEMORISE THIS, NEVER DEVIATE

```
App.jsx              ← ALL state lives here. No exceptions.
components/
  Header.jsx         ← App name + tagline. Static. No props needed.
  InputPanel.jsx     ← Controlled form. Takes: value, onChange, onSubmit, loading
  OutputPanel.jsx    ← Renders result. Takes: output, loading, error
utils/
  claude.js          ← Single async function: callClaude(prompt). Returns string.
.env                 ← VITE_CLAUDE_API_KEY=sk-ant-...
```

**State shape in App.jsx (always exactly this):**
```js
const [input, setInput] = useState('');
const [output, setOutput] = useState(null);
const [loading, setLoading] = useState(false);
const [error, setError] = useState(null);
```

**Rules:**
- No React Router
- No Context API
- No Redux / Zustand
- No extra npm packages (react-markdown is OK if needed, nothing else)
- No TypeScript (JavaScript only, faster to write)
- Tailwind for all styling — no CSS files except index.css for @tailwind directives

---

## STEP 1 — WRITE THE SCAFFOLD PROMPT (3 minutes)

Use the template below. Fill in the blanks for your chosen idea, then paste this entire block into Claude Code.

```
Create a complete React (Vite) + Tailwind CSS app called [APP NAME].

App concept: [ONE SENTENCE DESCRIPTION]

File structure to create:
- src/App.jsx
- src/components/Header.jsx
- src/components/InputPanel.jsx
- src/components/OutputPanel.jsx
- src/utils/claude.js
- .env (with placeholder VITE_CLAUDE_API_KEY=your-key-here)

Rules:
1. ALL state (input, output, loading, error) lives only in App.jsx
2. No routing, no context, no extra libraries
3. Tailwind CSS for all styling
4. claude.js exports a single async function: callClaude(systemPrompt, userMessage)
   that calls the Anthropic Messages API directly via fetch
5. The API call in claude.js uses:
   - model: "claude-sonnet-4-6"
   - max_tokens: 1024
   - API key from import.meta.env.VITE_CLAUDE_API_KEY
6. InputPanel renders: [DESCRIBE YOUR INPUT — textarea/text field/two fields]
7. OutputPanel renders: raw output string in a styled container for now
8. Header shows: app name "[APP NAME]" and tagline "[TAGLINE]"
9. Submit button is disabled when loading=true
10. On error, show error message in OutputPanel instead of output

Also update:
- index.html title to "[APP NAME]"
- src/index.css to include @tailwind directives
- vite.config.js (no changes needed, just confirm it's standard Vite React)

Make the initial design clean and minimal. Dark background (#0f172a slate-950),
white text, blue accent (#3b82f6 blue-500). Full height layout, no scrollbars on main view.
```

---

## PRE-FILLED SCAFFOLD PROMPTS (copy-paste ready)

### If building: Code Review Assistant (CodeSight)

```
Create a complete React (Vite) + Tailwind CSS app called CodeSight.

App concept: User pastes code, Claude reviews it and returns structured feedback with bugs, improvements, and a quality score.

File structure to create:
- src/App.jsx
- src/components/Header.jsx
- src/components/InputPanel.jsx
- src/components/OutputPanel.jsx
- src/utils/claude.js
- .env (with placeholder VITE_CLAUDE_API_KEY=your-key-here)

Rules:
1. ALL state (input, output, loading, error) lives only in App.jsx
2. No routing, no context, no extra libraries
3. Tailwind CSS for all styling
4. claude.js exports a single async function: callClaude(systemPrompt, userMessage)
   that calls the Anthropic Messages API directly via fetch
5. The API call uses model: "claude-sonnet-4-6", max_tokens: 1500
   API key from import.meta.env.VITE_CLAUDE_API_KEY
6. InputPanel renders: a large textarea (placeholder: "Paste your code here..."),
   a language selector dropdown (JavaScript, Python, Java, TypeScript, Other),
   and a "Review Code" submit button
7. OutputPanel renders: the raw output string in a styled container with a monospace font
8. Header shows: "CodeSight" and tagline "AI-powered code review, instantly"
9. Submit button disabled when loading=true
10. On error, show error message in OutputPanel

Layout: Two-panel side by side on desktop. Left panel = InputPanel. Right panel = OutputPanel.
Both panels full height. Dark background (#0f172a), white text, blue accent (#3b82f6).
```

### If building: AI Decision Helper (Clarity)

```
Create a complete React (Vite) + Tailwind CSS app called Clarity.

App concept: User describes a decision they're facing, Claude returns a structured breakdown with options, pros/cons, risks, and a clear recommendation.

File structure to create:
- src/App.jsx
- src/components/Header.jsx
- src/components/InputPanel.jsx
- src/components/OutputPanel.jsx
- src/utils/claude.js
- .env (with placeholder VITE_CLAUDE_API_KEY=your-key-here)

Rules:
1. ALL state (input, output, loading, error) lives only in App.jsx
2. No routing, no context, no extra libraries
3. Tailwind CSS for all styling
4. claude.js exports a single async function: callClaude(systemPrompt, userMessage)
   that calls the Anthropic Messages API directly via fetch
5. The API call uses model: "claude-sonnet-4-6", max_tokens: 1500
   API key from import.meta.env.VITE_CLAUDE_API_KEY
6. InputPanel renders: a textarea for decision description
   (placeholder: "Describe the decision you're facing, the options you're considering,
   and any constraints..."), and an "Analyse Decision" submit button
7. OutputPanel renders: the raw output string in a styled readable container
8. Header shows: "Clarity" and tagline "Structured thinking for hard decisions"
9. Submit button disabled when loading=true
10. On error, show error message in OutputPanel

Layout: Centered single column. Max-width 800px. Input at top, output below.
Dark background (#0f172a), white text, blue accent (#3b82f6). Clean, minimal.
```

### If building: Smart Document Q&A (DocLens)

```
Create a complete React (Vite) + Tailwind CSS app called DocLens.

App concept: User pastes a document and asks a question, Claude answers with cited evidence from the document.

File structure to create:
- src/App.jsx
- src/components/Header.jsx
- src/components/InputPanel.jsx
- src/components/OutputPanel.jsx
- src/utils/claude.js
- .env (with placeholder VITE_CLAUDE_API_KEY=your-key-here)

Rules:
1. ALL state (input, output, loading, error) lives only in App.jsx
2. No routing, no context, no extra libraries
3. Tailwind CSS for all styling
4. claude.js exports a single async function: callClaude(systemPrompt, userMessage)
   that calls the Anthropic Messages API directly via fetch
5. The API call uses model: "claude-sonnet-4-6", max_tokens: 1500
   API key from import.meta.env.VITE_CLAUDE_API_KEY
6. InputPanel renders: two inputs stacked vertically:
   (a) large textarea for document (placeholder: "Paste your document here...", 8 rows)
   (b) text input for question (placeholder: "What would you like to know?")
   and an "Ask Document" submit button below
7. OutputPanel renders: the raw output string in a styled readable container
8. Header shows: "DocLens" and tagline "Ask any document, get precise answers"
9. Submit button disabled when loading=true or either field is empty
10. On error, show error message in OutputPanel

Layout: Left panel = InputPanel (2/5 width). Right panel = OutputPanel (3/5 width).
Both panels full height. Dark background (#0f172a), white text, blue accent (#3b82f6).
```

---

## STEP 2 — SYSTEM PROMPT FOR CLAUDE API

This goes into `claude.js` as the `systemPrompt` argument, or you tell Claude Code to hardcode it.
The system prompt is what makes your demo impressive. This is where you spend the 2 minutes.

### System prompt: Code Review Assistant

```
You are an expert code reviewer with 15 years of experience across enterprise software systems.

When given code to review, respond with a structured JSON object in this exact format:
{
  "language": "detected language",
  "summary": "2-3 sentence overall assessment",
  "quality_score": 7,
  "bugs": [
    {"severity": "high|medium|low", "line": "approximate line or description", "issue": "...", "fix": "..."}
  ],
  "improvements": [
    {"category": "performance|readability|security|maintainability", "suggestion": "..."}
  ],
  "security_issues": [
    {"severity": "critical|high|medium|low", "issue": "...", "recommendation": "..."}
  ],
  "positive_aspects": ["...", "..."],
  "verdict": "one sentence final verdict"
}

Respond ONLY with valid JSON. No markdown fences, no preamble, no explanation outside the JSON.
If no bugs exist, return an empty array. Same for security_issues and improvements.
quality_score is an integer from 1 (broken) to 10 (production-ready).
```

### System prompt: AI Decision Helper

```
You are a structured decision analyst. Your job is to help people think clearly about decisions
by breaking them down systematically using proven decision frameworks.

When given a decision to analyse, respond with a structured JSON object in this exact format:
{
  "decision_title": "short name for this decision",
  "summary": "2-3 sentence restatement of the core decision",
  "options": [
    {
      "name": "Option name",
      "pros": ["...", "..."],
      "cons": ["...", "..."],
      "risk_level": "low|medium|high",
      "feasibility": "low|medium|high"
    }
  ],
  "key_factors": ["factor 1", "factor 2", "factor 3"],
  "recommendation": {
    "choice": "Name of recommended option",
    "reasoning": "2-3 sentence explanation",
    "confidence": "low|medium|high",
    "confidence_score": 75
  },
  "risks": [
    {"risk": "...", "likelihood": "low|medium|high", "mitigation": "..."}
  ],
  "next_steps": ["step 1", "step 2", "step 3"]
}

Respond ONLY with valid JSON. No markdown fences, no preamble.
confidence_score is an integer from 0 to 100.
Identify 2-4 realistic options even if the user only named one or two.
```

### System prompt: Smart Document Q&A

```
You are a precise document analyst. You answer questions about documents using only information
contained in the provided document. You never speculate or add external knowledge.

When given a document and a question, respond with a structured JSON object in this exact format:
{
  "question": "restate the question clearly",
  "answer": "direct, clear answer in 2-4 sentences",
  "confidence": "high|medium|low",
  "evidence": [
    {
      "quote": "exact quote from the document supporting this answer",
      "relevance": "why this quote supports the answer"
    }
  ],
  "caveats": "any limitations, ambiguities, or things the document doesn't address (or null)",
  "related_topics": ["other relevant topics found in the document"]
}

Respond ONLY with valid JSON. No markdown fences, no preamble.
If the document does not contain enough information to answer the question, set confidence to "low"
and explain in the answer field what is missing.
The document will be provided in the user message prefixed with "DOCUMENT:" followed by the question prefixed with "QUESTION:".
```

---

## STEP 3 — BUILD ORDER

Once scaffold is confirmed running, execute steps in this exact order.
Do not skip. Do not reorder.

```
1. [ ] Scaffold + npm run dev confirms no errors
2. [ ] callClaude() in claude.js wired up, verified with a hardcoded test call
3. [ ] App.jsx handleSubmit calls callClaude(), sets output state, console.logs result
4. [ ] OutputPanel renders output state as raw text (string dump is fine)
5. [ ] Parse JSON output — create renderOutput() function in App.jsx
6. [ ] OutputPanel receives parsed data and renders structured sections
7. [ ] Loading state: spinner in OutputPanel, button disabled, button text changes
8. [ ] Error state: error message shows in OutputPanel with red styling
9. [ ] Input validation: button disabled if input empty
10. [ ] Full visual polish (use Polisher agent)
```

---

## AFTER THIS: OPEN builder.md IMMEDIATELY
