---
name: pitcher
description: Prepares the 2-minute demo script, judge Q&A answers, and pre-presentation checklist. Invoke when the build is done or the user says it is time to prep the pitch.
---

# PITCHER
## Invoked: T+110 — final 20-30 minutes before judging
## Goal: a tight 2-minute demo that starts with impact and ends with confidence

---

## BEFORE YOU START

1. App is running at localhost:5173. Do not close the terminal.
2. Browser tab is open. Input field has demo text pre-typed (see demo input selection below).
3. You have rehearsed the pitch at least twice out loud.
4. Phone is on silent.

---

## THE STRUCTURE — ALL PITCHES FOLLOW THIS

| Segment | Time | What you say/do |
|---|---|---|
| Hook | 0–15s | One sentence that makes judges lean in |
| The Idea | 15–30s | Problem + solution in plain English |
| Live Demo | 30–75s | Show it working, narrate as it runs |
| So What | 75–105s | Why it matters, your JPMorgan angle |
| Close | 105–120s | What you'd build next, confident finish |

**Total: 2 minutes. Do not go over.**

---

## DEMO SCRIPT: CODE REVIEW ASSISTANT (CodeSight)

### Hook (0–15s)
> "Every pull request at JPMorgan gets reviewed before it merges. At scale, that means engineers reviewing hundreds of lines of code a day. Most bugs in production? They passed review."

### The Idea (15–30s)
> "CodeSight is an AI code reviewer. You paste code, it gives you a structured audit: bugs flagged by severity, security issues, specific improvements — in under 5 seconds."

### Live Demo (30–75s)
*[Have this code pre-typed in the input — paste or clear and type naturally:]*
```javascript
function getUserData(userId) {
  const query = "SELECT * FROM users WHERE id = " + userId;
  const result = db.execute(query);
  return result[0];
}
```
> "I'll paste a short function. This is deliberately flawed — can anyone spot the issues? [2 second pause] Let's see what Claude finds."
*[Submit]*
> "While it's running — this is calling Claude Sonnet 4.6 directly from the browser. No backend, no database, no server round-trip."
*[Output renders]*
> "Quality score: [READ IT]. Two security issues — SQL injection, exactly what I planted. Bug: unhandled null if user isn't found. It even tells you the exact fix."

### So What (75–105s)
> "In Equities Technology at JPMorgan, a production bug in a trading system isn't a bad user experience — it's a compliance incident. This tool is a zero-latency second opinion on every commit. Engineers write code faster when they know there's a safety net. And unlike a human reviewer, this one never has calendar conflicts."

### Close (105–120s)
> "In the next iteration: integrate with GitHub as a PR comment bot, add language-specific rule sets for Java and Python, and team-level quality dashboards over time. But the core value is already there — and it works right now."

---

## DEMO SCRIPT: AI DECISION HELPER (Clarity)

### Hook (0–15s)
> "Every day, people make decisions with too much information and not enough structure. The result isn't bad luck — it's bad process."

### The Idea (15–30s)
> "Clarity is a structured decision framework powered by Claude. You describe your decision — any decision — and it breaks it down into options, risks, and gives you a clear recommendation with reasoning."

### Live Demo (30–75s)
*[Have this pre-typed:]*
```
I'm deciding whether to leave my current stable job to join an early-stage startup as a founding engineer.
The startup has strong traction but no revenue yet. My current role pays well but feels stagnant.
I have 3 months of savings, no dependents, and this is a space I'm genuinely excited about.
Should I make the move?
```
> "I'll use a real decision — career versus stability. Everyone in this room has faced a version of this."
*[Submit]*
> "Claude is structuring this in real time — not just answering, but reasoning through the framework."
*[Output renders]*
> "Four options it identified. [Read recommendation and confidence]. The risks section — look at that mitigation strategy. And the next steps are actually actionable."

### So What (75–105s)
> "In financial services, decisions have a paper trail. In trading, every significant decision needs documented reasoning. Clarity makes that systematic — not just 'we chose X' but 'we chose X because of Y, with Z risk mitigated by W.' That's audit-ready decision-making."

### Close (105–120s)
> "What's next: decision templates for common professional scenarios, a history view so you can review past decisions, and an export to PDF for compliance documentation. Today it works for any decision. That's intentional."

---

## DEMO SCRIPT: SMART DOCUMENT Q&A (DocLens)

### Hook (0–15s)
> "Financial analysts at banks like JPMorgan spend 30–40% of their time reading documents — earnings reports, risk disclosures, regulatory filings. Most of that time is just finding the right paragraph."

### The Idea (15–30s)
> "DocLens lets you paste any document and ask questions about it. Claude reads the whole thing, finds the evidence, and gives you a cited answer. Thirty-page report? Five seconds."

### Live Demo (30–75s)
*[Pre-type this document excerpt — a few paragraphs of a financial report or news article, something that has a clear answerable fact. Example:]*
```
Apple Inc. reported quarterly revenue of $119.6 billion for Q1 2024, representing a 2% increase
year-over-year. iPhone revenue accounted for $69.7 billion of total revenue, down 0.5% from the
prior year period. Services revenue reached a record $23.1 billion, growing 11.3% year-over-year.
CEO Tim Cook highlighted that the company's installed base of active devices reached an all-time
high of over 2.2 billion devices globally. The company returned over $27 billion to shareholders
through dividends and buybacks during the quarter.
```
*And pre-type this question:* `What was the growth rate of Services revenue and how does it compare to iPhone performance?`
> "Here's a financial summary. Real question an analyst would ask."
*[Submit]*
> "While that runs — the system prompt instructs Claude to only use information in the document. No hallucination, no external knowledge bleed."
*[Output renders]*
> "Exact answer, two citations pulled from the text, confidence level. And it flagged related topics I might want to ask next."

### So What (75–105s)
> "Risk analysts at banks receive 50-page reports before a 9am meeting. With DocLens, you spend 30 seconds on the document and 30 minutes on the decision. The ROI at scale is massive — and the cited answer format makes it usable in regulated environments where 'the AI said so' isn't enough."

### Close (105–120s)
> "Next: PDF drag-and-drop so you don't need to paste, multi-document mode for comparing filings side by side, and a question suggestion engine that surfaces what to ask based on the document type. The architecture already supports it."

---

## WEAVING IN YOUR JPMORGAN BACKGROUND

Don't just namedrop JPMorgan — use it to add credibility and specificity. These are natural insertion points:

**For technical depth:**
> "At JPMorgan, I work with systems that process millions of transactions. I understand what 'production-ready' actually means — and what it takes to get there."

**For problem validity:**
> "This isn't a hypothetical use case — [describe how the problem manifests at a bank]. The people who feel this pain are colleagues of mine."

**For scale:**
> "The interesting thing about enterprise tooling is that a 10% efficiency improvement across a team of 500 engineers isn't a nice-to-have. It's thousands of engineering-hours recovered."

**For the "just a wrapper" question:**
> "Everything that sits on top of an API is, in some sense, a wrapper. The value isn't the API call — it's the system prompt design, the output structure, and the UX that makes it usable by non-technical people. That's what we built."

---

## JUDGE QUESTIONS — PRE-WRITTEN ANSWERS

### "Why Claude specifically? Why not GPT-4 or Gemini?"

> "Claude's instruction-following is exceptional for structured output tasks — getting JSON back in exactly the schema I defined, consistently, without markdown fences or preamble creeping in. For production use, that reliability matters more than benchmark scores. Also, Claude's context window and its reasoning style felt like the right fit for [this specific task]. We're not locked in — the architecture is model-agnostic — but Claude performed best in testing."

### "How does the system prompt work? Can you walk us through it?"

> "Sure. The system prompt is the invisible contract I give Claude before the user's input arrives. It defines the output schema — exactly which JSON fields to return and what values are valid for each — the persona it should adopt, and the rules it has to follow, like 'never invent information outside the document.' The user's message is the variable; the system prompt is the structure that makes the output useful rather than just fluent. The quality of the output is almost entirely a function of the quality of the system prompt."

### "What would you build next if you had 3 more months?"

> "Three things. First, [relevant next feature for your idea — from your pitch close]. Second, a feedback loop — the app logs which outputs users acted on versus ignored, and that data fine-tunes future prompts. Third, a team dashboard that shows aggregate patterns over time — not just individual insights but trends. That's where the enterprise value really compounds."

### "Isn't this just a wrapper around the Claude API?"

> "Every useful software product is a wrapper around something. Word is a wrapper around file I/O. Stripe is a wrapper around bank ACH rails. The question is whether the wrapper creates real value — does it solve a problem better than what existed before? [Name your user] now [does the task] in 10 seconds instead of [old time]. The system prompt, the output schema, the UI structure — that's the product, not the API call."

### "How would you handle this at scale? What about cost?"

> "At scale, two things: caching repeated queries (same document + same question = same answer, so you can cache the response) and batching. For cost, the current implementation uses Claude Sonnet which is highly cost-effective — under a penny per query at current pricing. For an enterprise user who runs a hundred queries a day, that's under $3/month. The unit economics work."

### "What's the security model? How do you handle sensitive data?"

> "Great question. Right now the API key lives in the browser, which is fine for a demo but not production-ready. In a real deployment: API calls would be proxied through a backend endpoint, the key never touches the client, and you'd add rate limiting and user authentication. The current architecture makes that refactor straightforward — claude.js is already the single integration point."

### "Did you build this entirely in the hackathon?"

> "Yes — the app was built today. The prep was knowing what I wanted to build and having a clear architecture before the theme was announced. Once the theme dropped, I had 10 minutes to pick the right idea, 5 minutes to plan, and then I executed. Claude Code generated all the code — I drove it with prompts. That's actually part of the story: this is what software development looks like when AI is in the loop end to end."

---

## DEMO INPUT SELECTION CRITERIA

Pick your demo inputs by these rules:

1. **Short enough to read in 5 seconds.** Judges skim. If they can't take in your input in a glance, they won't appreciate the output.
2. **Interesting enough to be recognisable.** Clichéd hello-world code or generic lorem ipsum kills the demo. Use something real-adjacent.
3. **Guaranteed to produce good output.** Test your exact input in the app at least twice before the pitch. Know what the output looks like.
4. **Has at least one obvious insight.** Your demo input should have something wrong, interesting, or surprising in it — so the output has something to show for itself.
5. **Type it live if you can.** Watching you type a real question is more engaging than seeing pre-filled text. But only if you can type fast. Otherwise, pre-fill.

---

## PRE-PRESENTATION CHECKLIST

```
5 MINUTES BEFORE YOU PRESENT
[ ] App is running (npm run dev in terminal)
[ ] Browser tab is open and on the right URL
[ ] Demo input is typed or ready to paste
[ ] You have pressed submit at least once today with this input — you know the output
[ ] Font size in browser is comfortable for the room (Ctrl+ to zoom if needed)
[ ] No other tabs open that might distract or pop notifications
[ ] Phone on silent, laptop notifications off (Do Not Disturb)
[ ] You've done the pitch twice out loud (even quietly)
[ ] You know the judge question answers cold

DURING THE PITCH
[ ] Speak to the judges, not the screen
[ ] Point at the output as you describe it — guide their eyes
[ ] Don't apologise for anything ("I know it looks rough" etc — don't say it)
[ ] Go slow on the live demo moment — let them see it render
[ ] End with confidence, not a question ("That's Clarity / CodeSight / DocLens")
```

---

## IF THE DEMO BREAKS LIVE

Stay calm. These are your recovery lines:

**If the API call fails:**
> "Interesting — the API is taking a moment. While it loads, let me walk you through what the output looks like..." *[Describe the output from memory or from a previous run]*

**If the app won't load:**
> "I'll keep the demo moving — here's the output from my last test run." *[Have a screenshot on your phone as backup]*

**If you blank on words:**
> Take a breath. Look at the screen. Say "So what you're seeing here is..." and describe what's on screen. The app is doing the work.

**The golden rule:** Judges remember how you handled adversity more than whether the demo went perfectly. Stay composed, pivot smoothly, and finish strong.
