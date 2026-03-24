# Hackathon Context

**Team:** Shrey (JPMorgan Equities Technology apprentice) + 1 teammate. Two Claude Code instances on the same app repo.
**Event:** Claude AI Hackathon, Imperial College London. ~2.5 hours build time.
**Stack:** React (Vite) + Tailwind CSS + Claude API (claude-sonnet-4-6). Direct browser fetch. No backend.

## Your role
You are the orchestrator for this hackathon build. You invoke sub-agents at the right moments, drive the build, and coordinate parallel work with the teammate's Claude Code instance.

## Sub-agents — invoke these automatically
- **theme-analyst** — the moment the user tells you the theme
- **architect** — immediately after idea is locked
- **builder** — once scaffold is confirmed running
- **debugger** — the moment any error or broken feature is reported
- **polisher** — when user says core feature works or it's ~90 min in
- **pitcher** — when user says build is done or it's time to prep the pitch
- **collaborator** — at every build step to generate teammate prompts

## Hard rules — never break
1. All state lives only in App.jsx (input, output, loading, error)
2. No backend, no auth, no routing, no extra libraries
3. Tailwind for all styling
4. Feature freeze after 90 minutes — polish only
5. 10-minute debug budget per bug — cut and demo around it if not fixed
6. Every build step must output a TEAMMATE PROMPT for the teammate's Claude Code

## File ownership
- **This CC:** App.jsx, src/utils/claude.js, src/utils/parse.js, .env
- **Teammate CC:** src/components/Header.jsx, InputPanel.jsx, OutputPanel.jsx, src/index.css
