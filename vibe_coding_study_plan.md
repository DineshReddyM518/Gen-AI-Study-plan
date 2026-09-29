# Your Study Plan: From Vibe Coding to Expert Agentic Engineer
### Goal app: A desktop productivity tracker that logs your work, lets you query it, and gives you AI-driven insights, tips, and real-time nudges ("you seem distracted", "take a break")

---

## How this plan is organized

The original research gave you a solid 6-week foundation (Phases 1–4). I've kept that structure but:
- Added **daily/weekly lesson breakdowns** so it's actually followable, not just phase headers
- Added a **new Phase 5** specifically for the behavioral-nudge features (distraction alerts, break reminders) — these need skills the base plan doesn't cover
- Added a **Phase 6: Path to Expert**, an open-ended track for after the app is built

Total: **~10 weeks** to a working v1, then ongoing mastery.

---

## Phase 1 — Vibe PMing & Spec-Driven Development (Week 1)

**Why first:** the #1 failure mode in AI-assisted coding is vague prompting leading to a mess of half-working code. Specs fix this.

| Day | Lesson | Practice |
|---|---|---|
| 1–2 | LLM mechanics: tokens, context windows, why models "forget" mid-session, why they hallucinate under ambiguity | Read 2–3 explainers on transformer context limits; watch how output quality degrades in a long chat you've already had |
| 3 | Vibe PMing: writing specs *before* code | Draft `PRD.md` for your tracker app — just the "what" and "why", no tech yet |
| 4 | Tech architecture thinking | Draft `tech.md` — pick your stack constraints (see Phase 4 for the recommended one) |
| 5 | Breaking work into steps | Draft `steps.md` — 10–15 small, testable milestones from empty repo to v0 |
| 6–7 | Plan-Act-Check loop | Practice: give an AI assistant one of your PRD features and require it to output a plan *before* any code. Review the plan like a PM would — reject and ask for revisions at least once on purpose, to build the muscle |

**Deliverable:** `/docs/PRD.md`, `/docs/tech.md`, `/docs/steps.md` for your actual app.

---

## Phase 2 — Environment, Tooling & Rules (Week 2)

| Day | Lesson | Practice |
|---|---|---|
| 1 | Survey the AI dev stack: IDE-native (Cursor/Windsurf) vs CLI/agentic (Claude Code) — when to use which | Install one of each; do a trivial task in both to feel the difference |
| 2–3 | Writing project rules (`CLAUDE.md` or `.cursor/rules/`) | Write rules for: code quality, "don't touch code outside scope", naming conventions, tech stack boundaries |
| 4 | Git as an AI safety net | Set up the repo, commit cadence: one commit per completed step, never a half-working commit |
| 5 | Prompt hygiene | Learn to start fresh sessions per bounded task; practice writing one prompt that includes goal, constraints, and "definition of done" |
| 6–7 | Dry run | Take one tiny throwaway feature (e.g., a CLI that logs a timestamp to a file) and build it end-to-end using everything above, just for practice |

**Deliverable:** configured repo with rules file, committed initial scaffold.

---

## Phase 3 — Vertical Slices on Small Web Projects (Weeks 3–4)

This is where you build real muscle before touching desktop complexity.

**Week 3 — mechanics of a vertical slice**
- Day 1–2: What "vertical slice" means (DB → backend → UI for *one* feature at a time) and why it beats building all tables first
- Day 3: Multimodal prompting — attach a wireframe/screenshot and ask the AI to match it
- Day 4: The "do not break existing functionality" instruction and why it matters as your codebase grows
- Day 5–7: **Build**: a minimal web habit tracker — just check-in buttons and a streak counter, one vertical slice at a time, one commit per slice

**Week 4 — debugging and hardening**
- Day 1–2: Defensive debugging — capturing exact console/terminal errors, feeding them back precisely instead of "it's broken"
- Day 3–4: Add heatmap calendar view (a good test of AI handling a moderately gnarly UI component)
- Day 5: Refactor pass — ask the AI to review its own code for duplication/dead code (a useful expert habit: never skip review)
- Day 6–7: Write a short retro doc: what prompts worked, what didn't, what you'd do differently

**Deliverable:** working web-based habit tracker (streaks, check-ins, heatmap) + a personal "prompting patterns that work for me" notes file. Keep this file — you'll keep adding to it.

---

## Phase 4 — Capstone: The Desktop App, Core Build (Weeks 5–6)

| Step | What you're building | Key concepts |
|---|---|---|
| 1 | **Scaffold**: Tauri app, React/TS frontend, Rust backend, local SQLite | Why Tauri over Electron (smaller, more secure, native system access); how frontend/backend talk over Tauri's IPC bridge |
| 2 | **Core UI**: dashboard, card-based categories, local persistence | Local-first data design — your data never has to leave the machine unless you want AI analysis on it |
| 3 | **Tracking engine**: streaks, milestone badges, activity heatmap | Same logic as your web version, ported — good test of your "translate a working feature to a new stack" skill |
| 4 | **AI insights layer v1**: connect an LLM API to summarize logs into weekly insights | Prompt design for data summarization; structured output (JSON) so the UI can render it reliably, not just prose |
| 5 | **Docs loop**: `/ai/docs/` — document what was built after each phase | This is what keeps a *future* AI session (or future you) from re-learning your own codebase from scratch |

**Deliverable:** v1 desktop app — tracks activity, shows streaks/heatmap, gives you a weekly AI-written summary.

---

## Phase 5 — The "Distraction Alert" & Query Layer (Weeks 7–9) — *not in the original plan*

This is the part of your idea (real-time nudges, "take a break", querying past logs conversationally) that needs skills beyond Phases 1–4. Three sub-skills, roughly a week each.

### Week 7 — Activity & idle detection
- What "detecting distraction" actually requires: active-window tracking, keyboard/mouse idle detection, and (optionally) app-category classification — **not** content reading or screenshots unless you explicitly want that (privacy tradeoff you should decide deliberately)
- Tauri/Rust APIs for OS-level window/idle detection (platform differences: macOS, Windows, Linux each need separate handling — plan for this early)
- Build: a background service that logs "active app + duration" every N seconds to SQLite

### Week 8 — Turning raw logs into signal
- Simple heuristics first: e.g., >15 min on a non-work app category, or >45 min with no break = trigger candidate. Don't reach for ML yet — heuristics you can explain are easier to trust and debug
- Notification system: native OS notifications from Rust, throttling so you don't get spammed
- Build: rule-based nudges ("you've been on X for 40 min — break?") with a simple settings UI so *you* control the thresholds, not a black box

### Week 9 — Querying your own history + smarter insights
- Retrieval-Augmented Generation (RAG) basics: why you can't just paste months of logs into a prompt, and how embeddings + a local vector store let you ask "how was my focus last week vs this week" and get a grounded answer
- Local embeddings (e.g., a small local model or an API embeddings call) + a lightweight vector DB (sqlite-vec or similar, keeping it local-first)
- Build: a query box in the app — natural language question → relevant log excerpts retrieved → LLM answers from *your real data*, not guesses

**Deliverable:** v2 — background tracking, rule-based break/distraction nudges, and a working "ask your own data" query feature.

---

## Phase 6 — Path to Expert (Week 10 onward, ongoing)

Once v1/v2 work, expertise is about depth and judgment, not new features. Rough order of what to layer in:

1. **Evaluation habits** — stop eyeballing whether AI output is "good." Write small test suites for critical logic (streak math, idle-detection thresholds) so regressions are caught automatically, not by vibes.
2. **Multi-agent / orchestration patterns** — when and why to split a task across multiple AI calls with distinct roles (planner, coder, reviewer) instead of one big prompt. Useful once your app is complex enough that one-shot prompting starts failing.
3. **Fine-grained prompt engineering** — few-shot examples, structured output schemas, chain-of-thought scaffolding for the insights feature specifically (your weekly reports should get noticeably better here).
4. **From heuristics to lightweight ML** — once you have months of your own data, revisit distraction detection: a small classifier on your own logged patterns will beat hand-tuned thresholds, but only worth it after the rule-based version has taught you what actually matters.
5. **Packaging & distribution** — code signing, auto-update, cross-platform builds for Tauri. Necessary once you want to run this on more than one machine or share it.
6. **Privacy & security hardening** — since this app watches what you do all day: encryption at rest, a clear data-retention policy, and a "pause tracking" control you trust.
7. **Telemetry on yourself, carefully** — dashboards for your own productivity trends (weekly/monthly), but watch for the trap of over-optimizing to a metric instead of how you actually feel.
8. **Staying current** — agentic tooling moves fast; budget ~1 hr/week to skim what's new in Claude Code / Cursor / agent frameworks rather than relearning from scratch every few months.

This phase has no fixed end date — treat it as a rotating backlog you pull from as your app and your own workflow evolve.

---

## Weekly rhythm (suggested, adjust to your schedule)

- **3 sessions/week** during Phases 1–4 (foundations + build)
- **4–5 sessions/week** during Phase 5 (more experimental, more debugging)
- Keep the "prompting patterns that work for me" notes file going the whole way through — by week 10 it'll be a genuinely useful personal playbook, more valuable than any generic course.

---

*Want me to draft the actual `PRD.md`, `tech.md`, and `steps.md` for your app next, so you can start Phase 1 today?*
