---
name: collaborator
description: Manages parallel work between two Claude Code instances. Generates self-contained prompts for the teammate's Claude Code at every build step. Invoke at the start of the build and at every step that needs teammate work.
---

# COLLABORATOR
## Protocol for two Claude Code instances working in parallel
## Shrey's CC is the orchestrator. Teammate's CC executes handed-off tasks.

---

## HOW THIS WORKS

```
YOUR CC (orchestrator)         TEAMMATE'S CC (executor)
──────────────────────         ─────────────────────────
Works on its task              Waits for a prompt from you
Outputs a TEAMMATE PROMPT  →   You copy-paste it in
                               They execute it on their files
                               They say "done"
You tell your CC: "done"   ←
Your CC gives next prompt  →   Repeat
```

**Each teammate prompt is self-contained.** It includes all context needed. Teammate's CC never needs to read your CC's session history.

**File ownership is strictly split to prevent conflicts:**
```
YOUR CC owns:       src/App.jsx
                    src/utils/claude.js
                    src/utils/parse.js (if needed)
                    .env

TEAMMATE'S CC owns: src/components/Header.jsx
                    src/components/InputPanel.jsx
                    src/components/OutputPanel.jsx
                    src/index.css
                    index.html (title only)
```
Neither CC touches the other's files. If you need a component to accept new props, you write the usage in App.jsx and include the prop spec in the teammate prompt — they update the component definition.

---

## HOW TO START A SESSION WITH YOUR CC

Paste this at the start of every hackathon Claude Code session so it knows the protocol:

```
We are at a hackathon. Two-person team, two Claude Code instances on the same repo.
I am the orchestrator — I own App.jsx, src/utils/claude.js, src/utils/parse.js, and .env.
My teammate owns all files in src/components/ and src/index.css.

Rules:
- You only ever write to files I own. Never touch src/components/.
- Whenever you start a task, also output a ready-to-paste TEAMMATE PROMPT block at the end.
  The teammate prompt must be completely self-contained — full context, exact file targets,
  no references to our conversation. Teammate's CC has no context from our session.
- Format teammate prompts as a fenced block labelled: TEAMMATE PROMPT (copy this exactly)
- When I say "teammate done with [X]", treat that as new input and continue to the next step.
- If a step doesn't require teammate work, say "No teammate task for this step."

The app uses React (Vite) + Tailwind CSS + Claude API (direct browser fetch, claude-sonnet-4-6).
All state lives only in App.jsx. No routing, no Context, no extra libraries.
Read SKILL.md and agents/architect.md in this repo for full context before starting.
```

---

## HOW TO START TEAMMATE'S SESSION

Teammate pastes this once at the start of their Claude Code session:

```
We are at a hackathon. Two-person team, two Claude Code instances on the same repo.
I am the component developer — I own all files in src/components/ and src/index.css.
My teammate owns App.jsx, src/utils/, and .env. I never touch those files.

Rules:
- Only write to files in src/components/ and src/index.css.
- Each task I receive is a self-contained prompt. I execute it and report "done with [task name]".
- I do not make decisions about app logic, state shape, or API calls — that's my teammate's domain.
- If a prompt is unclear about what props a component should accept, I ask before writing.
- Stack: React (Vite) + Tailwind CSS. No TypeScript. No extra libraries. All styling via Tailwind.

Read collaborator.md in this repo for context. Then wait for the first task prompt.
```

After this, they wait. You drive.

---

## THE TEAMMATE PROMPT FORMAT

Every time your CC gives you a teammate prompt, it will look like this:

```
─────────────────────────────────────────────
TEAMMATE PROMPT — [Task Name]
File: src/components/[ComponentName].jsx
─────────────────────────────────────────────

[Full self-contained instructions here. No assumed context.]

Props this component receives:
  [exact prop list]

Do not touch any files outside src/components/.
Report back: "done with [Task Name]"
─────────────────────────────────────────────
```

You copy everything between the lines and paste it to your teammate. They paste it into their CC. Done.

---

## SYNC POINTS — WHEN BOTH STOP AND CHECK

After these moments, stop and verify together before continuing:

**Sync 1 — After scaffold:**
Both run `npm run dev`. Both see the app load. No console errors. Then split.

**Sync 2 — After first API call works (your side) + all 3 components exist (their side):**
The app must be able to call Claude and render some output (even raw string). If one side is blocked, the other waits rather than building on a broken foundation.

**Sync 3 — T+90:**
Core feature works end-to-end. Both open polisher.md. You assign: one person runs the full overhaul prompt, the other starts pitcher.md.

---

## CONFLICT RESOLUTION

**"I need to change a component's props"**
→ Tell your teammate what new props you need and why. They update the component signature.
→ You update the usage in App.jsx. Never update both files yourself.

**"My CC accidentally edited a component file"**
→ Run `git diff src/components/` to see what changed.
→ Undo just those changes: `git checkout -- src/components/[file]`
→ Your CC's change goes into the teammate prompt instead.

**"We both edited the same file"**
→ Read both versions. Merge manually (or have one CC merge it with both versions shown).
→ Agree on the final version. Move on.

---

## WHAT YOUR CC WILL DO AUTOMATICALLY

Once you give your CC the session-start prompt above, it will:

1. Work on App.jsx / utils tasks
2. At the end of each step, output a formatted TEAMMATE PROMPT
3. Wait for you to report "teammate done with [X]" before moving forward
4. Adjust teammate prompts based on what's been built so far
5. Keep a mental model of what each component currently looks like so prompts stay accurate

You never need to manually coordinate what files each person touches — the session-start prompt handles that rule. Just execute your side, hand off the teammate prompt, and keep building.
