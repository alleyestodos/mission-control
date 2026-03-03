# Mission Control — Handoff Document
Last updated: 2026-03-02 21:32 AST

## Live URL
https://alleyestodos.github.io/mission-control/

## Current Version
**V14** — `mission-control-v14.html` = `index.html` in repo

## What V14 Has
- 8-tab dashboard: Projects, Tasks, Timelines, Activity, Family, Goals, Agents, Notes
- Projects kanban: 4 columns (Backlog / In Progress / Review / Done), each independently scrollable, oldest-first sort with toggle
- Project modal: locked Start Date (birthdate/immutable), editable Due Date with audit log, Priority dropdown, full 13-person assignee roster (gold rings + checkmarks), Progress Notes feed, column-aware action buttons
- Column gates: Backlog→In Progress only; In Progress→Review or ↩ Backlog (+1 push); Review→Done requires Touch ID (WebAuthn)
- Times Pushed counter: trips on (1) return to Backlog, (2) due date extended — cannot be gamed
- Timelines tab: active projects only (In Progress + Review) as horizontal progress bars, click for detail modal
- Notes tab: localStorage journal, AI auto-tagging (20 categories), voice note, category filters, immutable source-of-truth (words never changed)
- Voice input: MediaRecorder + local Whisper server at localhost:8765 (dual-track with Web Speech API fallback)
- Smart Voice Capture (SVC): unified modal for Add Task / Add Project — **still glitchy, paused**
- All 13 agents/family as avatar pills across modals
- Touch ID (WebAuthn) gate on Review→Done

## Repo Structure
```
/ (root)
  index.html              ← current live version (V14)
  mission-control-v2.html through mission-control-v14.html
  HANDOFF.md              ← this file
  avatars/
    harriet.png, banksy.png, andy.png, clu.png, marissa.png,
    matlock.png, oz.png, spencer.png, phineas.png, ed-lewis.png
```

## Open Issues (Pick Up Here)
1. **SVC glitchy** — Smart Voice Capture modal needs parser polish; V11 is stable fallback
2. **Activity Feed** — needs two independent scrollable columns (Humans left / Agents right)
3. **Telegram → Notes bridge** — prefix-tagged Telegram messages should auto-log to Notes tab
4. **Gary personal avatar** — no photo yet, using G circle; Banksy to create
5. **Goals tab** — display only, not wired to live data
6. **Responsive nav** — collapse to icons at smaller screen sizes
7. **Stats bar counts** — still shows old numbers from before Recurring column removal
8. **Wire real kanban cards to Timelines** — replace DEMO_PROJECTS seed data with actual localStorage cards

## Key localStorage Keys
- `mc_notes_v1` — Notes journal entries
- `mc_projects_v1` — Project state store (priority, dates, assignees, progress notes)

## Key JS Functions
`openSVC('task'|'project')`, `addNoteEntry(tag, name, text, color, initial)`, `autoTag(text, sourceTag)`, `loadSavedNotes()`, `showToast(msg)`, `refreshDashboard()`, `markAsDone()`, `vanquishTask()`, `renderTimelines()`, `openTimelineDetail(p)`, `renderProjectAssignees(assigned, projectId)`, `renderProgressNotes(notes)`, `toggleAddProgressNote()`, `saveProgressNote()`, `openDueDateEdit()`, `saveDueDate()`, `savePriority(val)`, `moveCardToColumn(col)`, `revertToBacklog()`, `getProjectStore()`, `saveProjectStore(store)`, `getProjectState(id)`, `saveProjectState(id, state)`, `toggleKanbanSort()`, `sortAllKanbanColumns()`

## Key Element IDs
`notesJournal`, `directVoiceBtn`, `svcModal`, `taskModal`, `projectModal`, `familyModal`, `toast`, `timelinesTable`, `timelinesEmpty`, `timelineDetailModal`, `pmTitle`, `pmDesc`, `pmStart`, `pmDue`, `pmDueInput`, `pmDueEditArea`, `pmDueAudit`, `pmPrioritySelect`, `pmPush`, `pmAssignees`, `pmProgressNotes`, `pmAddNoteArea`, `pmNoteInput`, `pmContributorHistory`, `pmHistoryList`, `pmMoveProgressBtn`, `pmMoveReviewBtn`, `pmMarkDoneBtn`, `pmRevertBtn`, `tdTitle`, `tdMeta`, `tdNotes`, `kanbanSortBtn`

## Color Spec
- bg: `#0d0d11` | nav: `#13131a` | cards: `#16161e` | borders: `#2d2d44`
- accent purple: `#a78bfa` / `#6366f1` | active tab bg: `#1e1b4b` | active tab text: `#c4b5fd`
- green: `#22c55e` | red: `#ef4444` | amber: `#f59e0b`
- Font: Inter (Google Fonts), base 21.6px

## Roster (ALL_ROSTER order)
gary (#6366f1 G), nardellly (#8b5cf6 N), alan (#0ea5e9 A), milo (#f59e0b M),
harriet (#a78bfa), oz (#10b981), clu (#8b5cf6), banksy (#ec4899),
andy (#3b82f6), marissa (#ec4899), matlock (#10b981), spencer (#6366f1), phineas (#f59e0b)

## Infrastructure (as of 2026-03-02)
- Harriet (Air M4, HarrietSkull): Claude Code Max ✅, acpx plugin ✅
- Oz (Mac Studio, 100.79.21.58 Tailscale / 10.0.0.9 LAN): Claude Code Max ✅, acpx plugin ✅
- Both machines use `alleyestodoslosojos@gmail.com` Max subscription for heavy builds
- API credits ($48 Harriet / $240 Oz) reserved for conversation only
- Whisper server: `localhost:8765` on Harriet, LaunchAgent `ai.openclaw.whisper-server`
- Deploy: `cd /tmp/mc-deploy && cp /path/to/vN.html index.html && git add . && git commit -m "VN: desc" && GIT_SSH_COMMAND="ssh -o StrictHostKeyChecking=no" git push`
- Re-clone deploy dir if missing: `git clone git@github.com:alleyestodos/mission-control.git /tmp/mc-deploy`

## Design Rules
- Pure HTML + CSS + JS, single file, no external JS libraries (Google Fonts OK)
- Write raw HTML characters — NO HTML entities (`<` not `&lt;`, `"` not `&quot;`)
- Company name "Todos los OjOs" in `id="company-name"` div (white-label ready)
- "Online" pill fixed bottom-left, NOT in nav bar
- Notes tab = immutable journal — original words never changed; API keys/tokens auto-redacted
