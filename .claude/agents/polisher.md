---
name: polisher
description: Runs a visual polish pass on the app UI. Invoke when the core feature is working and it is time to improve the design. No logic changes, style only.
---

# POLISHER
## Invoked: Once core feature works (~T+90)
## Time cap: 20 minutes. Hard stop at T+110.

---

## THE ONLY RULE

**No new features. No logic changes. Style only.**

If you think of something functional to add right now — write it on a sticky note for the "what I'd build next" pitch section. Do not build it.

---

## VISUAL AUDIT CHECKLIST

Before running any prompts, spend 2 minutes auditing the app visually. Check each item:

```
LAYOUT
[ ] No horizontal scrollbars on the page
[ ] Both panels fill the full viewport height
[ ] No awkward empty whitespace in either panel
[ ] Content in output panel is independently scrollable if long
[ ] Header is clearly separated from the main content area

TYPOGRAPHY
[ ] App name in header is prominent (large, bold, white)
[ ] Tagline is visible but secondary (smaller, slate-400)
[ ] Section headers in output are clearly distinct from body text
[ ] Body text is readable (min 14px, adequate line height)
[ ] Monospace font used for code/JSON, readable font for prose

COLORS
[ ] No default white backgrounds remaining
[ ] Consistent dark palette throughout (slate-900, slate-800, slate-950)
[ ] Accent color (blue-500) used consistently for CTAs and highlights
[ ] Error states are red, success/high-confidence are green
[ ] Loading state is visually distinct (muted, animated)

INTERACTIVE STATES
[ ] Submit button has visible hover state (cursor-pointer, darker shade)
[ ] Submit button shows loading state (spinner + "Analysing...")
[ ] Textarea has visible focus ring (blue-500 outline)
[ ] Disabled state is visually obvious (opacity-50 or grayed out)

OUTPUT QUALITY
[ ] Each section in output has clear visual hierarchy
[ ] Badges/pills are properly sized and colored
[ ] Lists have adequate spacing between items
[ ] No raw string dump visible when structured output is working
[ ] Empty state message is shown before first submission
```

Mark each as passed or note what's wrong. Then use the targeted prompts below.

---

## PROMPT 1 — FULL VISUAL OVERHAUL (use this first)

Run this before any surgical fixes. It handles the most common issues in one pass.

```
Do a comprehensive visual polish pass on the entire React app.
Do NOT change any logic, API calls, or state management.
Style improvements only.

Requirements:
1. OVERALL LAYOUT
   - Full viewport height layout: html, body, #root all h-full
   - Main container: flex flex-col h-screen overflow-hidden bg-slate-950
   - Header: fixed height, subtle bottom border border-slate-800
   - Content area: flex-1 flex overflow-hidden (for side-by-side panels)
   - Each panel: flex flex-col overflow-hidden with its own internal overflow-y-auto

2. HEADER
   - bg-slate-900 with border-b border-slate-800
   - App name: text-xl font-bold text-white tracking-tight
   - Tagline: text-sm text-slate-400 mt-0.5
   - Padding: px-6 py-4
   - Optional: small logo/icon using an SVG or emoji as accent

3. INPUT PANEL
   - bg-slate-900 with border-r border-slate-800
   - Inner padding: p-6
   - Section label above textarea: text-xs font-semibold text-slate-400 uppercase tracking-wider mb-2
   - Textarea: w-full bg-slate-800 border border-slate-700 rounded-lg text-white
     placeholder-slate-500 p-3 text-sm resize-none focus:outline-none focus:ring-2
     focus:ring-blue-500 focus:border-transparent transition-all
   - Submit button: w-full mt-4 bg-blue-600 hover:bg-blue-500 disabled:opacity-50
     disabled:cursor-not-allowed text-white font-semibold py-3 px-4 rounded-lg
     transition-colors flex items-center justify-center gap-2

4. OUTPUT PANEL
   - bg-slate-950 or bg-slate-900
   - When empty: centered placeholder with slate-600 icon + "Results will appear here" text
   - When loading: centered spinner (animate-spin, text-blue-500) + "Analysing..." text
   - When populated: overflow-y-auto with p-6

5. SECTION CARDS in output
   - Each section: bg-slate-800 rounded-xl p-4 mb-3
   - Section title: text-xs font-semibold text-slate-400 uppercase tracking-wider mb-3
   - Content: text-sm text-slate-200

6. BADGES AND PILLS
   - Severity high: bg-red-900 text-red-300 border border-red-700
   - Severity medium: bg-yellow-900 text-yellow-300 border border-yellow-700
   - Severity low: bg-blue-900 text-blue-300 border border-blue-700
   - All badges: text-xs font-medium px-2 py-0.5 rounded-full

Apply these systematically. Show me all modified .jsx files.
```

---

## PROMPT 2 — OUTPUT SECTION DEEP POLISH

If the output section still looks rough after Prompt 1:

**For Code Review Assistant:**
```
Polish the OutputPanel for CodeSight specifically:
1. Quality score: large number (text-5xl font-bold) centered in a circle,
   colored green (>=7), yellow (4-6), red (<=3). Show the /10 in smaller text.
   Circle should be 80px, border-4, colored border matching the number color.
2. Summary text directly below the score in text-base text-slate-300 italic, centered.
3. Bug list items: each bug gets a left-border colored card.
   Border color matches severity. "FIX:" prefix in text-blue-400 font-mono text-sm
4. Improvement items: simple list with a right-arrow icon (→) in blue-400
5. Security section: only visible when non-empty. Amber warning banner at top.
6. Positive aspects: green checkmark icon ✓ before each item
7. Verdict: bottom card with subtle gradient bg, italic quote styling
Do not change any logic. Style only.
```

**For AI Decision Helper:**
```
Polish the OutputPanel for Clarity specifically:
1. Options grid: 2-column on desktop, 1-column on mobile (grid grid-cols-1 md:grid-cols-2 gap-3)
   Each option card: bg-slate-800 rounded-xl p-4 border border-slate-700
   Hover state: border-blue-500/30 (subtle glow on hover)
2. Pros: each item prefixed with a green "+" in font-bold
3. Cons: each item prefixed with a red "–" in font-bold
4. Recommendation box: gradient bg from blue-950 to slate-900,
   border-l-4 border-blue-500, prominent recommendation text,
   confidence score as a visual progress bar (h-2, rounded-full, blue fill)
5. Confidence score percentage shown as large text next to the bar
6. Risk items: each with a colored circle indicator (low=green, medium=yellow, high=red)
   and the mitigation in a lighter color below
7. Next steps: numbered with large circular step numbers in slate-700
Do not change any logic. Style only.
```

**For Smart Document Q&A:**
```
Polish the OutputPanel for DocLens specifically:
1. Question header: bg-slate-800 px-4 py-3 rounded-lg mb-4 with a "?" icon
   Question text in text-slate-200 font-medium
   Confidence badge positioned right: text-xs pill
2. Answer: large, prominent. text-white text-base leading-relaxed.
   Light left border (border-l-2 border-blue-500) with pl-4.
3. Evidence cards: quote styling with:
   - Left border: border-l-3 border-blue-500/50
   - Quote text in italic text-slate-300
   - Quotation marks (") as decorative element in text-slate-600 text-4xl
   - Relevance text below in text-xs text-slate-500
4. Caveats section: amber warning card with ⚠ icon, only when non-null
5. Related topics: horizontal scrollable row of pill tags (bg-slate-800 text-slate-300)
Do not change any logic. Style only.
```

---

## PROMPT 3 — SUBMIT BUTTON

If the submit button looks flat or unimpressive:

```
Redesign the submit button in InputPanel to be more polished:
1. Full width at the bottom of the input panel
2. Background: gradient from blue-600 to blue-700, or solid blue-600
3. Hover: background transitions to blue-500 (transition-colors duration-150)
4. Active/click: scale-95 transform (active:scale-95)
5. When loading: show an inline spinner SVG (animate-spin, 16px, white)
   + button text changes to match the app's verb ("Reviewing...", "Analysing...", "Searching...")
6. When disabled (empty input): opacity-40 cursor-not-allowed
7. Font: font-semibold text-white text-sm py-3 rounded-lg
8. Add a subtle icon on the left: a wand/sparkle/arrow SVG that matches the app theme

Show me the updated InputPanel.jsx only.
```

---

## PROMPT 4 — HEADER UPGRADE

If the header looks like a default Vite app:

```
Redesign the Header component to look polished and intentional:
1. Background: bg-slate-900 with border-b border-slate-800
2. Layout: flex items-center justify-between px-6 py-4
3. Left side: app name + tagline stacked vertically
   - Name: text-xl font-bold text-white (or gradient: bg-gradient-to-r from-blue-400 to-blue-600 bg-clip-text text-transparent)
   - Tagline: text-xs text-slate-500 mt-0.5
4. Right side: a subtle badge: "Powered by Claude" in text-xs text-slate-600
   with a small "A" icon or similar — makes the tech stack part of the brand
5. Optional divider: a 1px vertical line (w-px h-6 bg-slate-700) between name and any right content

Show me the updated Header.jsx only. No logic changes.
```

---

## PROMPT 5 — MICRO-INTERACTIONS

Add these only if you have time remaining and everything else looks good:

```
Add subtle micro-interactions to improve the feel of the app:
1. Output sections: fade in with opacity and translateY when they appear
   (use Tailwind's animate-in or add a simple CSS animation in index.css:
    @keyframes fadeInUp { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }
    .fade-in-up { animation: fadeInUp 0.3s ease-out forwards; }
   )
2. Submit button: ripple effect on click (add a click handler that briefly adds a ring class)
3. Textarea: smooth border color transition on focus (already handled by Tailwind transition)
4. Output panel scroll: smooth scroll to top when new output arrives
   (add useEffect that scrolls the output panel ref on output change)

Keep all animations subtle and fast (under 300ms). Do not use any animation libraries.
Show me all modified files.
```

---

## WHAT TO SKIP

These are common temptations at this stage. Skip them:

- **Dark/light mode toggle** — Not worth the time. Keep it dark.
- **Copy to clipboard button** — Nice to have. Not essential. Skip unless 5 min to spare.
- **Export as PDF** — Way too complex. Skip.
- **Keyboard shortcut (Ctrl+Enter to submit)** — Actually quick to add if you have 3 min:
  ```
  In InputPanel, add an onKeyDown handler to the textarea:
  onKeyDown={(e) => { if (e.ctrlKey && e.key === 'Enter') onSubmit(); }}
  This adds Ctrl+Enter to submit. One line of code.
  ```
- **Mobile responsiveness** — Your demo will be on a laptop. Skip.
- **History/previous results** — Feature, not polish. Skip.

---

## HARD STOP: T+110

At T+110, stop all edits. Open `pitcher.md`.

The app is as good as it's going to be. The pitch is where you win.
