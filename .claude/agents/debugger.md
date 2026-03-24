---
name: debugger
description: Triages and fixes errors, bugs, and broken features. Invoke immediately whenever the user reports an error, something is broken, or the app stops working.
---

# DEBUGGER
## On standby the entire event
## Time budget: 10 minutes maximum per issue. After 10 minutes, cut the feature.

---

## THE ONE RULE

**If you have been staring at a bug for more than 10 minutes: cut the feature and demo around it.**

A working demo without one feature beats a broken demo every time. Judges cannot award points for code that doesn't run.

---

## TRIAGE TABLE — IDENTIFY YOUR ERROR FIRST

Read the error in the browser console or terminal. Find your category below, then go to that section.

| Symptom | Category | Section |
|---|---|---|
| `Cannot read properties of undefined (reading 'X')` | JS runtime | Section 1 |
| `Failed to fetch` / `NetworkError` / `CORS` | Network / CORS | Section 2 |
| `401 Unauthorized` / `403 Forbidden` | Auth / API key | Section 3 |
| White screen, nothing renders | React crash | Section 4 |
| Styles not applying / Tailwind not working | CSS/Tailwind | Section 5 |
| State change happens but UI doesn't update | React state | Section 6 |
| JSON parse error / `Unexpected token` | Output parsing | Section 7 |
| `VITE_` env var is undefined | Env config | Section 8 |
| App works but output looks wrong/garbled | Prompt/API | Section 9 |
| Infinite re-render / frozen browser | useEffect loop | Section 10 |

---

## SECTION 1 — Cannot read properties of undefined

**Cause:** You're trying to access a property on something that is null or undefined.
Usually: the output hasn't loaded yet, or the JSON parse returned unexpected structure.

**Diagnosis prompt:**
```
I'm getting: "Cannot read properties of undefined (reading '[PROPERTY]')"
The error points to [FILE]:[LINE].
Here is the relevant component code:
[paste the component or function]
What is undefined and how do I fix it with a safe guard?
```

**Common fixes:**
1. **Optional chaining:** `output?.bugs?.map(...)` instead of `output.bugs.map(...)`
2. **Default fallback:** `const bugs = output?.bugs ?? []`
3. **Conditional render:** `{output && output.bugs && output.bugs.map(...)}`
4. **Guard at top of component:**
```js
if (!output) return <EmptyState />;
if (!output.bugs) return <div>Loading structure...</div>;
```

**Fix prompt:**
```
In [COMPONENT].jsx, I'm getting "Cannot read properties of undefined".
The component receives an output prop that may be null, undefined, or missing nested properties.
Add defensive checks throughout: use optional chaining (?.) and nullish coalescing (??)
for every property access on output. Add a null check at the top of the component
that returns an empty state if output is null.
```

---

## SECTION 2 — CORS / Failed to fetch / Network Error

**Cause:** The browser is blocking the API request. This is a CORS policy issue.

**Diagnosis:** Open browser console, look for:
- `Access-Control-Allow-Origin` in the error
- `blocked by CORS policy`
- `ERR_FAILED` alongside a fetch to api.anthropic.com

**Fix — Add required header to claude.js:**
```
In src/utils/claude.js, the fetch call to the Anthropic API is failing with a CORS error.
Add this header to the fetch request headers:
"anthropic-dangerous-direct-browser-access": "true"

This header is required by Anthropic for direct browser-based API calls.
Show me the corrected callClaude function with all required headers:
- "Content-Type": "application/json"
- "x-api-key": import.meta.env.VITE_CLAUDE_API_KEY
- "anthropic-version": "2023-06-01"
- "anthropic-dangerous-direct-browser-access": "true"
```

**If that doesn't fix it:** The issue might be the URL. Confirm you're fetching:
`https://api.anthropic.com/v1/messages`
Not `/api/v1/messages` or any other variation.

---

## SECTION 3 — 401 Unauthorized / 403 Forbidden

**Cause:** Your API key is missing, wrong, or not being sent correctly.

**Step 1 — Check the key exists:**
Open browser console and run:
```js
console.log(import.meta.env.VITE_CLAUDE_API_KEY)
```
- If it prints `undefined` → the `.env` file is wrong (see Section 8)
- If it prints `your-key-here` → you forgot to replace the placeholder
- If it prints the key starting with `sk-ant-` → the key is loaded, move to Step 2

**Step 2 — Check the key works:**
Go to console.anthropic.com → API Keys → verify the key exists and is enabled.

**Step 3 — Check the header name:**
The header must be exactly `x-api-key` (lowercase), not `Authorization` or `API-Key`.

**Fix prompt:**
```
In src/utils/claude.js, the API call is returning 401.
Double-check:
1. The API key header is named exactly "x-api-key" (lowercase)
2. The key is read from import.meta.env.VITE_CLAUDE_API_KEY
3. Add a console.log before the fetch that prints: "Calling API with key:", import.meta.env.VITE_CLAUDE_API_KEY?.slice(0,10) + "..."
   so I can verify the key is being read correctly without exposing the full key.
Show me the corrected claude.js.
```

---

## SECTION 4 — White Screen / Nothing Renders

**Cause:** A JavaScript error during render is crashing the component tree. React swallows the error and shows blank.

**Step 1 — Check the console.** There will be a red error. Read it first.

**Step 2 — Find the crash.** Look for:
- `Error: X is not a function`
- `TypeError: Cannot read properties of null`
- `ReferenceError: X is not defined`
- `Minified React error`

**Step 3 — Divide and conquer:**

```
My app shows a white screen. The browser console shows this error:
[paste the exact error]

The last change I made was:
[describe what you just changed]

Here is App.jsx:
[paste App.jsx]

What caused the white screen and what is the fix?
```

**Nuclear reset — if you can't find the cause:**
```
My app shows a white screen after recent changes.
Reset App.jsx to a minimal working state:
1. Import Header, InputPanel, OutputPanel
2. State: input, output, loading, error — all initialized to empty/null/false
3. handleSubmit as an async function with try/catch that calls callClaude
4. Return a simple two-panel layout: Header on top, InputPanel left, OutputPanel right
5. No other logic

Do not touch claude.js, Header.jsx. Only reset App.jsx and the affected component.
```

---

## SECTION 5 — Tailwind Not Applying / Styles Missing

**Symptom:** You write `className="bg-slate-900 text-white"` and the div is white or unstyled.

**Check 1 — index.css has Tailwind directives:**
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
If these are missing: paste them at the top of index.css.

**Check 2 — tailwind.config.js content paths include your files:**
```js
content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"]
```
If `./src/**/*.{js,ts,jsx,tsx}` is missing → Tailwind doesn't scan your files → no classes generated.

**Fix prompt:**
```
Tailwind CSS classes are not being applied.
Check and fix:
1. src/index.css must start with @tailwind base; @tailwind components; @tailwind utilities;
2. tailwind.config.js content array must include "./src/**/*.{js,ts,jsx,tsx}"
3. postcss.config.js must export { plugins: { tailwindcss: {}, autoprefixer: {} } }
4. vite.config.js must not have anything overriding CSS processing

Show me the corrected versions of all config files.
After this fix I will need to restart npm run dev.
```

**CDN Fallback (nuclear option — if config is hopeless):**
If Tailwind config is completely broken and you're short on time, add this to `index.html` `<head>`:
```html
<script src="https://cdn.tailwindcss.com"></script>
```
Then delete the `@tailwind` directives from index.css. CDN Tailwind works instantly, no build step.
The tradeoff: slightly slower page load, but it works.

---

## SECTION 6 — State Not Updating / UI Not Re-rendering

**Symptom:** You call `setState(value)` but the UI doesn't change. Or the old value still shows.

**Most common causes:**
1. You mutated state directly instead of returning a new value
2. You're reading the old state value in a callback (stale closure)
3. You updated a nested object without spreading it

**Diagnosis prompt:**
```
My state update is not triggering a re-render.
Here is the relevant code:
[paste the handleSubmit or the setState call]

I'm calling [setState] with [value] but the component shows [old/wrong value].
What is wrong and how do I fix it?
```

**Common fix patterns:**
```js
// WRONG — mutating state
output.bugs.push(newBug);
setOutput(output);

// RIGHT — new object/array
setOutput({ ...output, bugs: [...output.bugs, newBug] });

// WRONG — reading stale state in async
const handleSubmit = async () => {
  const result = await callClaude(input); // input might be stale
};

// RIGHT — use the latest value
const handleSubmit = async () => {
  const currentInput = input; // capture before async
  const result = await callClaude(currentInput);
};
```

---

## SECTION 7 — JSON Parse Error / Unexpected Token

**Symptom:** `JSON.parse(output)` throws. The output isn't valid JSON.

**First, console.log the raw response:**
```js
const raw = await callClaude(systemPrompt, userMessage);
console.log("RAW RESPONSE:", raw);
```

Look at what Claude actually returned. Common issues:
- Claude wrapped the JSON in markdown fences: ` ```json {...} ``` `
- Claude added preamble text before the JSON: "Here is the analysis: {...}"
- Claude returned partial JSON (response cut off at max_tokens)

**Fix prompt:**
```
Claude is returning JSON wrapped in markdown code fences or with preamble text.
In App.jsx, update the output parsing to:
1. Strip markdown code fences: remove ```json and ``` from start/end
2. Find the first { and last } in the string, extract just that substring
3. Then try JSON.parse on the extracted substring
4. If it still fails, catch the error and set output as the raw string instead

Here is the current parsing code: [paste it]
```

**Better long-term fix — update the system prompt:**
Add this to the end of your system prompt:
`"IMPORTANT: Respond ONLY with the raw JSON object. No markdown, no code fences, no explanation before or after the JSON."`

---

## SECTION 8 — VITE_* env var is undefined

**Symptom:** `import.meta.env.VITE_CLAUDE_API_KEY` is `undefined` in the browser.

**Check 1 — File name:** The file must be `.env` in the root of the project (same level as `package.json`). Not `env`, not `.env.local` unless you're using Vite's env file conventions.

**Check 2 — Variable prefix:** The variable MUST start with `VITE_`. Variables without this prefix are not exposed to the browser by Vite.

**Check 3 — File contents (correct format):**
```
VITE_CLAUDE_API_KEY=sk-ant-api03-...
```
No spaces around `=`. No quotes. No `export`. Just `KEY=VALUE`.

**Check 4 — Restart required:** After creating or editing `.env`, you MUST restart `npm run dev`. Vite does not hot-reload env files.

**Fix steps:**
1. Confirm `.env` exists at project root: `ls -la` in terminal
2. Confirm content: `cat .env` (make sure key starts with `VITE_`)
3. `Ctrl+C` to stop dev server
4. `npm run dev` to restart
5. Check console: `console.log(import.meta.env.VITE_CLAUDE_API_KEY)`

---

## SECTION 9 — App Works but Output is Wrong/Garbled

**Symptom:** API returns something, it renders, but the content is bad — too short, ignoring the structure, missing fields.

**Likely causes:**
1. System prompt wasn't passed correctly (it's going as user message or getting dropped)
2. `max_tokens` is too low and response is being cut off
3. The model is a cheaper/different one than expected

**Diagnosis prompt:**
```
The Claude API is returning but the response doesn't follow the format I specified.
Here is my callClaude function: [paste claude.js]
Here is the system prompt I'm using: [paste it]
Here is a sample raw response: [paste console.log of raw response]

What is wrong with how I'm calling the API?
```

**Quick fixes:**
- Increase `max_tokens` to `2000`
- Make sure `system` is at the top level of the body, not inside `messages`
- Correct API body structure:
```json
{
  "model": "claude-sonnet-4-6",
  "max_tokens": 2000,
  "system": "[your system prompt here]",
  "messages": [{ "role": "user", "content": "[user message]" }]
}
```

---

## SECTION 10 — Infinite Re-render / Frozen Browser

**Symptom:** The page freezes, CPU spikes, or React throws "too many re-renders."

**Cause:** Almost always a `useEffect` with a missing or incorrect dependency array, or setting state inside a render function.

**Fix prompt:**
```
My app has an infinite re-render loop. The browser is freezing.
Here is App.jsx: [paste it]

Find any useEffect hooks and check:
1. If a useEffect has no dependency array [], it runs after every render
2. If a useEffect sets state without a condition, it triggers another render → loop
3. If a function is in the dependency array but defined inside the component, it creates a new reference every render

Fix all useEffect dependency arrays. If setting state in a useEffect, add a condition to prevent the loop.
If there are no useEffects, check if I'm calling setState during render (not in a handler).
```

---

## 10-MINUTE RULE — ENFORCED

If you opened this file more than 10 minutes ago and the issue is not resolved:

1. **Can you demo without this feature?** → Yes: Comment out the broken code. Demo around it.
2. **Is this the core feature?** → Yes: Apply the nuclear reset (Section 4) and rebuild from scratch.
3. **Is this a style issue?** → Yes: Skip it. Polisher will fix it or you'll live without it.

**Cutting a feature is not failure. Shipping a broken app is.**
