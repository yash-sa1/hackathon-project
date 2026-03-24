---
name: builder
description: Drives the full build sprint step by step. Invoke once the app scaffold is confirmed running. Outputs a teammate prompt at every step.
---

# BUILDER
## Invoked: Once scaffold prompt is ready (T+15)
## Drives the build sprint from T+15 to T+90

---

## BUILD STATE TRACKER

Check off each step as it's VERIFIED WORKING — not just "Claude Code wrote the code", but "it works in the browser."

```
PHASE A — FOUNDATION (T+15 to T+30)
[ ] A1. Scaffold generated, npm run dev running at localhost:5173
[ ] A2. No console errors on startup
[ ] A3. callClaude() tested, raw API response logs to console
[ ] A4. App.jsx handleSubmit wired: calls callClaude, sets output state
[ ] A5. Raw output string renders on screen (ugly is fine)

PHASE B — CORE FEATURE (T+30 to T+60)
[ ] B1. JSON parsing works — output is an object, not a string
[ ] B2. OutputPanel receives parsed output and renders structured sections
[ ] B3. All key fields rendered (varies by idea — see below)
[ ] B4. Loading state: spinner shown, button disabled during API call
[ ] B5. Error state: error message shown in red when API fails

PHASE C — UX LAYER (T+60 to T+90)
[ ] C1. Input validation — submit disabled when input is empty
[ ] C2. Output panel styled — readable, scannable, good hierarchy
[ ] C3. Input panel styled — clean form, good labels, proper sizing
[ ] C4. Header styled — app name prominent, tagline readable
[ ] C5. Full page layout polished — no awkward gaps, no overflow

** T+60 CHECKPOINT: If B1–B5 are NOT all checked, activate FALLBACK PLAN **
** T+90 CHECKPOINT: Hand off to polisher.md **
```

---

## CHECKPOINT: T+60 — DEMOABLE STATE REQUIRED

At exactly T+60, assess:

**If B1–B5 are all checked:** You're on track. Continue to Phase C.

**If B3 or B4 is not checked (structured output not rendering):**
→ Execute the FALLBACK PLAN below. Do not attempt to fix the parsing. Render raw string.

**If B1 or B2 is not checked (API not working):**
→ Open debugger.md immediately. This is a P0.

---

## FALLBACK PLAN (activate if behind at T+60)

The fallback goal: have a working demo even if output isn't perfectly structured.

**Paste this prompt into Claude Code:**

```
The app currently shows raw JSON string in the output panel. The JSON parsing is broken or incomplete.
Do the following quick fix:
1. In OutputPanel.jsx, render the output prop as a <pre> tag with overflow-auto and a monospace font
2. Apply syntax highlighting using text colors: wrap property names in spans with text-blue-400,
   string values in text-green-300, numbers in text-yellow-300
3. Do not attempt to parse the JSON into components — just make the raw JSON look good
4. Result should look like a developer tool output — clean, readable, impressive in its own way

This is intentional minimalism, not a bug. Make it look like a deliberate design choice.
```

A polished raw JSON display is demoable and actually looks technical/impressive.

---

## EXACT BUILD PROMPTS — PASTE DIRECTLY INTO CLAUDE CODE

Execute these in order. Each prompt is self-contained.

---

### PROMPT A1 — SCAFFOLD (use the one from architect.md — do not re-paste here)

After pasting the scaffold prompt from architect.md:
1. Wait for Claude Code to generate all files
2. Run `npm install` if it wasn't auto-run
3. Run `npm run dev`
4. Check browser: you should see the app layout with no errors

---

### PROMPT A3 — VERIFY API CONNECTION

```
In src/utils/claude.js, verify the callClaude function is correctly calling the Anthropic Messages API.
The function should:
1. Accept (systemPrompt, userMessage) parameters
2. Use fetch to POST to https://api.anthropic.com/v1/messages
3. Headers must include:
   - "Content-Type": "application/json"
   - "x-api-key": import.meta.env.VITE_CLAUDE_API_KEY
   - "anthropic-version": "2023-06-01"
   - "anthropic-dangerous-direct-browser-access": "true"
4. Body must be JSON with: model, max_tokens, system, messages array
5. Return the text content from response.content[0].text
6. Throw a descriptive error if the response is not ok

Then in App.jsx, add a temporary test button "Test API" that calls:
  callClaude("You are a helpful assistant.", "Say 'API connection successful' and nothing else")
  and console.logs the result.

Show me the updated claude.js and the test button.
```

After this: open browser console, click Test API, confirm you see "API connection successful".
Then delete the test button (or just leave it — you can remove it in polish phase).

---

### PROMPT A4 — WIRE UP HANDLESUBMIT

**For Code Review Assistant:**
```
In App.jsx, implement the handleSubmit function that:
1. Takes the input (code string) and language from InputPanel state
2. Calls callClaude with this system prompt:
   [PASTE THE FULL SYSTEM PROMPT FROM architect.md → Code Review section]
3. User message format: "Please review the following [language] code:\n\n[code]"
4. Sets output state to the raw response string
5. Wraps everything in try/catch: sets loading true before, false in finally
6. Sets error state in catch block
7. Clears previous output and error at the start of each submit

Pass handleSubmit, loading to InputPanel. Pass output, loading, error to OutputPanel.
Do not parse the JSON yet — just display output as a string in OutputPanel.
```

**For AI Decision Helper:**
```
In App.jsx, implement the handleSubmit function that:
1. Takes the input (decision description string) from InputPanel
2. Calls callClaude with this system prompt:
   [PASTE THE FULL SYSTEM PROMPT FROM architect.md → Decision Helper section]
3. User message: the raw decision description as-is
4. Sets output state to the raw response string
5. Wraps in try/catch: loading true before, false in finally, error set in catch
6. Clears previous output and error at the start of each submit

Pass handleSubmit, loading to InputPanel. Pass output, loading, error to OutputPanel.
Display output as raw string for now.
```

**For Smart Document Q&A:**
```
In App.jsx, implement the handleSubmit function that:
1. Takes document text and question from InputPanel (two separate state values)
2. Calls callClaude with this system prompt:
   [PASTE THE FULL SYSTEM PROMPT FROM architect.md → DocLens section]
3. User message format:
   "DOCUMENT:\n[document text]\n\nQUESTION:\n[question]"
4. Sets output state to the raw response string
5. Wraps in try/catch: loading true before, false in finally, error set in catch
6. Clears previous output and error at the start of each submit
7. Submit button disabled if either document or question is empty

Pass handleSubmit, loading, document, setDocument, question, setQuestion to InputPanel.
Pass output, loading, error to OutputPanel. Display output as raw string for now.
```

---

### PROMPT B1-B2 — PARSE AND RENDER STRUCTURED OUTPUT

**For Code Review Assistant:**
```
In App.jsx, add a function parseReviewOutput(rawString) that:
1. Tries JSON.parse(rawString)
2. If that fails, returns null (we'll handle null in OutputPanel)

Update OutputPanel.jsx to render the parsed review data with these sections:
- Top bar: quality score as a colored badge (green >=7, yellow 4-6, red <=3) + summary text
- "Bugs Found" section: list of bugs, each showing severity badge + issue + fix suggestion
  Severity colors: high=red-500, medium=yellow-500, low=blue-500
- "Improvements" section: list with category tag + suggestion
- "Security Issues" section: only show if array is non-empty, same structure as bugs
- "What's Working" section: positive_aspects as a simple list with checkmarks
- Bottom bar: verdict text in italic

If output is a string (not parsed JSON), render it in a <pre> tag with monospace font.
If output is null (parse failed), show "Processing response..." or re-render as string.

Use Tailwind for all styling. Each section should have a subtle border and padding.
Background: slate-800 cards on slate-900 background.
```

**For AI Decision Helper:**
```
In App.jsx, add a function parseDecisionOutput(rawString) that:
1. Tries JSON.parse(rawString)
2. Returns null if parsing fails

Update OutputPanel.jsx to render the parsed decision data with these sections:
- Header: decision_title as h2 + summary as paragraph
- Options grid: 2-column grid of option cards, each showing:
  * Option name as card title
  * Pros (green checkmarks) and Cons (red X marks) as lists
  * Risk level badge: low=green, medium=yellow, high=red
- "Key Factors" section: horizontal row of pill badges
- Recommendation box: highlighted box (blue-950 bg, blue-500 border) showing:
  * "Recommendation:" label + choice name bold
  * Reasoning text
  * Confidence score as a progress bar (0-100%) with colored fill
- Risks section: collapsible or just a list, each with likelihood badge
- Next steps: numbered list

If output is a string, render as <pre>. Tailwind only.
```

**For Smart Document Q&A:**
```
In App.jsx, add a function parseDocOutput(rawString) that:
1. Tries JSON.parse(rawString)
2. Returns null if parsing fails

Update OutputPanel.jsx to render the parsed Q&A data with these sections:
- Question display: restated question in a subtle header bar (slate-700 bg)
- Confidence badge: high=green, medium=yellow, low=red — positioned top-right
- Answer: large, clear answer text in white, 18px
- Evidence section: each evidence item as a quote card:
  * Left border accent (blue-500)
  * Quote text in italic
  * Relevance explanation in smaller text below
- Caveats: only shown if non-null, in amber-900/amber-200 styling
- Related Topics: row of pill badges

If output is a string, render as <pre>. Tailwind only.
```

---

### PROMPT B4 — LOADING STATE

```
Add a proper loading state to the app:

1. In OutputPanel.jsx, when loading=true show a loading skeleton:
   - A pulsing spinner SVG (animate-spin, 40x40, blue-500)
   - Text below: "Claude is thinking..." in slate-400
   - Three gray placeholder bars (animate-pulse) to suggest content is coming
   - Center everything in the panel

2. In InputPanel.jsx, when loading=true:
   - The submit button text changes to "Analysing..." (or equivalent for the app)
   - The submit button is disabled and shows a small inline spinner
   - The textarea/input becomes read-only or slightly grayed out

3. Make sure the loading overlay completely replaces the output content area,
   not overlapping it.

Use only Tailwind. No external spinner libraries.
```

---

### PROMPT B5 — ERROR STATE

```
Add a proper error state to the app:

In OutputPanel.jsx, when error is a non-null string and loading=false:
- Show a red error card (red-950 bg, red-500 border, red-200 text)
- Icon: a red X circle SVG inline
- Title: "Something went wrong"
- Body: the error message string
- A "Try Again" note: "Check your API key and try submitting again."

The error card should replace the output content (not appear alongside it).
When output is set again after a new submit, the error clears automatically
because loading starts and output replaces the error.

Make sure the error card has the same padding and layout rhythm as the output content.
```

---

### PROMPT C1 — INPUT VALIDATION

```
Add input validation to InputPanel:

For Code Review Assistant:
- Submit button disabled if code textarea is empty (trim().length === 0)
- Add a character counter below the textarea showing "X characters"
- If code is over 8000 characters, show warning: "Large input may be slow"

For AI Decision Helper:
- Submit button disabled if decision textarea is empty
- Add placeholder hint text below input: "Be specific about constraints, timeline, and what matters most to you"

For Smart Document Q&A:
- Submit button disabled if either document OR question is empty
- Add character counter for document: "X / 10000 characters"
- Warn if document > 8000 characters

All validation must be purely state-derived — no refs, no DOM queries.
```

---

### PROMPT C2-C4 — FULL VISUAL POLISH PASS

Use this if you're at T+85 and want one final pass before handing off to polisher.md:

```
Do a full visual review of the entire app and make these improvements:

1. LAYOUT: Ensure the two-panel layout fills the full viewport height.
   Input panel and output panel should both be independently scrollable if content overflows.
   No horizontal scrollbars on the page.

2. OUTPUT PANEL: Every card/section should have:
   - Consistent padding (p-4 minimum)
   - Clear section headers in slate-300 or white
   - Subtle dividers between sections (border-slate-700)
   - Good font size hierarchy (base text-sm or text-base, headings text-lg)

3. INPUT PANEL:
   - Textarea should have focus ring in blue-500
   - Submit button should be full-width at bottom of panel
   - Button hover state should be visible (darker shade)
   - Good label typography (text-slate-400 for labels, white for input text)

4. HEADER:
   - App name prominent (text-xl or text-2xl, font-bold, white)
   - Tagline subtle (text-slate-400, text-sm)
   - Slight bottom border (border-slate-800) separating from content

5. COLORS: Audit the entire file for any leftover white/light backgrounds.
   Everything should be on slate-900, slate-800, or slate-950.

6. EMPTY STATE: When output is null and loading is false, show a centered
   placeholder in OutputPanel: icon + "Submit your [code/decision/document] to see results"
   in slate-500.

Show me all modified files.
```

---

## TIME CHECKPOINTS AND FALLBACKS

| Time | Expected state | If behind |
|---|---|---|
| T+30 | API call working, raw output on screen | Skip language selector / extra fields, simplify input |
| T+45 | Structured output rendering in sections | Use fallback raw JSON display from above |
| T+60 | Loading + error states working | Skip error state, only add loading spinner |
| T+75 | Input validation + basic polish | Skip validation, go straight to polish |
| T+90 | Hand off to polisher.md | Hand off anyway — polish with what you have |

**At T+90, regardless of state: open polisher.md. Stop adding features.**

---

## EMERGENCY FIXES

If things are broken and you need a quick working demo, paste this:

```
The app is broken and I need a working demo in 5 minutes.
Strip back to the absolute minimum:
1. App.jsx has one textarea input and one button
2. On submit, call Claude API with a simple system prompt: "You are a helpful assistant."
3. Show the raw response in a div below
4. Loading state: button says "Thinking..." and is disabled
5. Error state: show error message in red text

Make this work in one go. Delete any broken components. This is the MVP.
After this works, we'll layer back the styling.
```

This nuclear option gives you a working, demoable app in 5 minutes. It's not beautiful but it proves the concept.
