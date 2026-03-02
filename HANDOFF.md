# Mission Control — Developer Handoff
**Repo:** `alleyestodos/mission-control` (public)
**Live URL:** https://alleyestodos.github.io/mission-control/
**Last working version:** `mission-control-v12.html` (deployed as `index.html`)
**Owner:** Gary | **Orchestrator:** Harriet (Mac Air M4, HarrietSkull)

---

## How to resume work

### On HarrietSkull (Harriet or any OpenClaw session)
Working files: `/Users/air3/.openclaw/workspace/mission-control-vN.html`
Deploy repo: `/tmp/mc-deploy/` (if gone after reboot, clone fresh: `git clone git@github.com:alleyestodos/mission-control.git /tmp/mc-deploy`)

Deploy command:
```
cp /Users/air3/.openclaw/workspace/mission-control-vN.html /tmp/mc-deploy/index.html
cd /tmp/mc-deploy && git add . && git commit -m "VN: description"
GIT_SSH_COMMAND="ssh -o StrictHostKeyChecking=no" git push
```

### From GitHub (any machine, any model)
```
git clone https://github.com/alleyestodos/mission-control.git
```
`index.html` = live version. All `mission-control-vN.html` files = full history for rollback.

---

## What this is
Single-file dashboard (pure HTML/CSS/JS, no framework, no backend).
8 tabs: Dashboard · Projects (Kanban) · Timeline · Notes (Journal) · Revenue · Agents · Family · Intel
Deployed to GitHub Pages. Goal: white-label SaaS product via Gumroad.

---

## Current state (v12, session 2026-03-02)

### Working well
- Full 8-tab layout with all content
- Projects 5-column kanban (Recurring/Backlog/In Progress/Review/Done)
- Notes: immutable journal, localStorage persistence, 20 AI auto-tags, direct voice capture, Copy for Agent export
- Top Priorities: voice input, inline edit, complete → trophy modal
- Family tab: grade cards, modal with voice note
- Tasks: push counter (frustrated emoji), delete confirmation (vanquish flow — task removed completely)
- Complete → Review gate (no skipping to Done)
- Review → Done requires Touch ID (WebAuthn)
- Avatar images at /avatars/ subfolder in repo
- Local Whisper server at localhost:8765, auto-starts on boot via LaunchAgent

### Open issues (Gary's feedback 2026-03-02)
1. Smart Voice Capture modal (SVC, v12) — concept right, implementation glitchy. Needs:
   - Reliable audio capture (Web Speech + MediaRecorder dual-track)
   - Better parser edge cases
   - Chrome required for Web Speech (Safari wont work)
2. Notes — verify addNoteEntry wires from all modal contexts
3. Activity Feed — needs two independent columns (Humans left, Agents right)
4. Telegram to Notes — not yet wired
5. Gary personal avatar — no photo, using G circle
6. Goals Progress — display only, not wired to live data
7. Responsive nav — collapse to icons at small sizes

---

## Hard constraints
- Single HTML file, no external JS libraries (Google Fonts OK)
- Never use escaped HTML entities (&lt; etc) — write real < > " ' characters
- Colors: bg #0d0d11, nav #13131a, cards #16161e, borders #2d2d44, accent #a78bfa/#6366f1
- Font: Inter via Google Fonts, base 21.6px
- White-label: company name in id="company-name"

---

## Key functions
- openSVC('task'|'project') — Smart Voice Capture modal
- addNoteEntry(tag, name, text, color, initial) — adds to Notes + localStorage
- autoTag(text, sourceTag) — returns category string
- showToast(msg) — bottom notification
- markAsDone() — Touch ID gate

## Local services on HarrietSkull
- Whisper server: localhost:8765, POST audio to /transcribe, returns {transcript:"..."}
- LaunchAgent: ~/Library/LaunchAgents/ai.openclaw.whisper-server.plist
- Script: /Users/air3/.openclaw/workspace/whisper-server.py

## Agents (colors for reference)
Harriet #a78bfa · Oz #10b981 · Banksy #ec4899 · Clu #8b5cf6 · Andy #3b82f6
Marissa #ec4899 · Matlock #10b981 · Spencer #6366f1 · Phineas #f59e0b · Chris #f97316

Last updated: 2026-03-02 · Harriet · Sonnet 4-6
