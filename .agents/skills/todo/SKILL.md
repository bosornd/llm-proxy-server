---
name: todo
description: >-
  Manages deliverable progress with Scrum boards — initialize, track sprint tasks
  in TODO.md, sync with workspace artifacts, and pick the next actionable task.
  Maintains scrum/product-backlog.md (story status), sprint-NN.md (plans),
  and scrum/TODO-NN.md (closed-sprint archives). Sprint plan approval and sprint
  completion (execution approval) are required user gates before Execute and
  Sprint Boundary. Use when starting or resuming a deliverable session, progress
  or next-step questions, sprint/backlog planning, or alongside any deliverable
  workflow.
---

# TODO — Deliverable Progress Tracking

On-disk boards track progress so sessions can resume, report status, and pick the next task without chat memory.

**Progress SSOT:** `{workspace}/TODO.md` — current sprint board only. Always read it first for “where are we?” / “what’s next?”. Do not use the agent’s built-in `todowrite` tool.

**Ownership:** Companion skills own artifact *content*. This skill owns task/board *state* and when to advance.

---

## 1. Concepts

### Core loop

```
Small unit → validate → repeat
```

Work advances only after something checkable is done. Prefer units that unlock the next step over one mega-story. If a story cannot be demoed in one sprint, split it (append IDs; never renumber).

### Scrum operation

| Layer | What it is | How it moves |
|-------|------------|--------------|
| **Product backlog** (`PB-*`) | Deliverable-scoped stories with acceptance-oriented Story / DoD | Append-only IDs. Committed into a sprint via Scope. Closed when DoD is met — at **Sprint Boundary**, or when the user explicitly confirms DoD mid-sprint. Closing all related `T-*` does **not** auto-close the PB. |
| **Sprint plan** | Goal, Scope (`PB-*` subset), DoD, Demo Checklist | Light Scope: enough to meet Goal + Demo — not “finish the backlog.” Plan fields are not edited during execution to “mark done.” |
| **Execution board** (`T-*`) | Current-sprint tasks derived from Scope + companion workflow | Live only on `TODO.md`. Heavy work (refactors, rewrites, migrations) must appear as explicit `PB-*` / `T-*`, not silent side work. |

Flow: backlog → light Scope (after **Sprint plan approval**) → seed `TODO.md` → Execute / Sync → **Sprint completion** → **Sprint Boundary** (archive, update backlog, load next approved plan).

### User gates

```
Sprint plan approval → Execute → Sprint completion → Sprint Boundary
```

Both gates need **go-ahead** for the named gate (accept the recommended plan/outcome and authorize the next step). Natural language is enough — e.g. ok, continue, next, 계속해줘, 승인할게 — do **not** require the literal words “approve”. Silence, unrelated chat, or picking the next task without proceed intent is **not** approval. On go-ahead: record the gate **and** run the authorized next action in the **same turn** (seed Execute / Boundary / companion draft) — do not stop after approval alone. Task-level companion gates (draft → gate `[x]`) still apply inside Execute; they do not replace these sprint gates.

| Gate | When | Present | Until approved, do **not** |
|------|------|---------|----------------------------|
| **Sprint plan approval** | After drafting or revising `sprint-NN.md` | Goal, Scope, DoD, Demo Checklist | Seed/rewrite live `TODO.md` for that sprint, or **Execute** on it |
| **Sprint completion** | Boundary task is Current focus / Recommended next (or Recovery) | Goal vs outcome, Scope/DoD/Demo status, carry-over / skip proposals | Run **Sprint Boundary** |

Revising Goal/Scope/DoD/Demo after approval requires a fresh **Sprint plan approval**.

### Boundary task

The **boundary task** is the last open task on every seeded/rewritten live board. It is the sprint’s exit ramp — not ordinary scope work.

| Rule | Guidance |
|------|----------|
| **Always last** | No `T-*` after it. Mid-sprint additions go before it; extend its Depends. |
| **When selected** | Run **Sprint completion** first; after approval, **Sprint Boundary**. Mark `[x]` on the **archived** copy only. |
| **Depends** | Prefer every other planned `T-*`. If only the last scope/demo gate is listed, Sync must not leave earlier open tasks when that gate closes. |
| **Final sprint** | Title `Sprint Boundary → complete`. Archive + close backlog; do not invent a next sprint. |
| **Recovery** | Live board has no open tasks and no boundary task → treat as end-of-sprint (completion → Boundary). Re-seed always includes a boundary task. |

---

## 2. Process

### Situation → workflow

| Situation | Action |
|-----------|--------|
| New deliverable (no `TODO.md`) | **Initialize** → plan approval → seed board → **Execute** (companion stages only after seed) |
| Feature add / large Spec change | **Plan** next Spec→backlog sprint (Scope = backlog change) → plan approval → seed → **Execute** |
| Resume / mid-session | **Orient** |
| Draft done or gate approved | **Execute** → **Sync** → if Recommended next is a **draft**, continue the companion stage in the same turn |
| "where are we?" / "what's next?" | **Report** from `TODO.md` (+ backlog if product-wide) |
| Scope change within sprint | **Sync** |
| Backlog / sprint plan needed | **Plan** / **Plan Next** → plan approval before activate/Execute |
| Boundary task is Current focus / Recommended next | **Sprint completion** → after approval, **Sprint Boundary** |

### Initialize

Use when `{workspace}/TODO.md` is missing (new deliverable). Companion skills (e.g. sdd) must **not** start stage work until this completes through plan approval + seed.

1. Resolve `{workspace}` and companion workflow (sdd → Spec→backlog first).
2. Create `scrum/` + `product-backlog.md` + first `sprint-00.md` (light Scope) → set **Sprint:** on scoped PB items only.
   - **Spec→backlog (first setup):** product `PB-*` do not exist yet. Scope = **create the initial product backlog** (run Spec pipeline ①–⑥, then encode approved Spec as `PB-*`). Seed backlog with that bootstrap story (or a small set covering pipeline → backlog encode) — not with guessed product features.
   - **Spec→backlog (later / continuation):** do **not** re-Initialize as sprint-00; use **Plan** with continuous `sprint-NN` and Scope = **product-backlog change** for the delta (see sdd **Scrum mapping**).
3. Present Goal / Scope / DoD / Demo for **Sprint plan approval**. **Stop** until go-ahead — do not draft PRD/AC/ADR/Spec as the session’s primary work while waiting. On go-ahead **in the same turn**: record approval, seed `TODO.md`, and begin **Execute**.
4. After approval → seed `TODO.md` from Scope + companion stages (trailing boundary task; empty Completed sprints) → Current focus = first open task → **Report** / **Execute**.

### Orient

1. Read `TODO.md` (missing → Initialize; do not resume companion stages without a board).
2. Optionally skim current `sprint-NN.md` Goal/DoD; read backlog when product-wide status matters.
3. Drift vs disk → **Sync**.
4. **Report**.

### Execute

**Prerequisite:** Live board exists (seeded only after **Sprint plan approval**). Do not invent or activate a sprint board to “keep going.” Do not run companion pipeline stages (PRD → …) off-board.

1. Exactly one Current focus. **Recommended next** = Depends satisfied + gate over draft + ID order within phase; keep Current focus if still open and unblocked.
2. Draft done → `[x]`; move to gate or next.
3. Gate → `[x]` only with user go-ahead (batch: flip matching headlines in place).
4. After a gate `[x]` **in the same turn**: continue the authorized next step (companion **draft**, Sync, or Boundary when go-ahead covers it) and produce the next gate/draft artifact unless the user asked to pause. Approval-only replies with no next-step work are incomplete.
5. Refresh Snapshot → **Sync**.
6. If Current focus / Recommended next is the boundary task → **Sprint completion** (do not jump to Boundary).

Research may run in parallel while a gate waits **only if** the user asks (or the companion skill requires it) — it does not close or bypass the gate.

### Sync

1. Compare disk artifacts to current `## Tasks`.
2. Align with **committed sprint scope** only: drop removed scope with `[x]` + `_(skipped)_`.
3. Append or reshape `T-*` only when (a) the user explicitly requested it, or (b) Initialize / Plan / Boundary seeded from an **approved** Scope. Keep the boundary task last; update its Depends if earlier tasks change. No filler tasks.
4. Recompute Snapshot.
5. Unscoped / future work stays off this board until after Boundary.

### Plan / Plan Next

**Plan:** Draft/revise `sprint-NN.md` (light Scope) → set PB **Sprint:** → present for **Sprint plan approval**. **Stop.** After approval, seed/switch live `TODO.md` when that sprint is active (trailing boundary task) → **Execute**.

- **Spec→backlog:** Scope is backlog creation (sprint-00) or backlog change for added/revised requirements (later NN) — see sdd **Scrum mapping**. Tasks cover companion stages ①–⑥ + `PB-*` encode/append; no product ⑦.
- **Backlog→Implementation:** Scope is a **subset** of open product `PB-*` after a Spec→backlog Boundary. Companion artifacts already exist; do not re-open Spec redesign as main Scope.

**Plan Next:** If next `sprint-NN.md` is missing (or user pre-plans): pick open/carried Stories (or draft the next Spec→backlog Scope when requirements change) → create the plan → present for **Sprint plan approval** (now or defer until Boundary — silence ≠ approval). Do not rewrite live `TODO.md` until that sprint starts **and** the plan is approved.

### Sprint completion

**When:** Boundary task is Current focus / Recommended next, or Recovery.

1. Present completion review: Goal outcome, each Scope `PB-*` (met DoD / carry-over / skip), Demo evidence, any incomplete open `T-*`.
2. Propose backlog actions — do not apply them yet.
3. **Stop** for execution go-ahead. Do not run Boundary in the same turn unless the user already gave go-ahead in that message.
4. Do not invent filler `T-*` or auto-`[x]` remaining work. Incomplete scope → carry-over or explicit skip only with user intent.
5. After approval → **Sprint Boundary**.

### Sprint Boundary

**When:** After **Sprint completion** only.

1. Archive: copy `TODO.md` → `scrum/TODO-{NN}.md`; mark boundary task `[x]` on the archived copy.
2. Backlog: met DoD → `[x]`; intentionally dropped → `[x]` + `_(skipped)_`; incomplete → carry-over (leave `[ ]`, set **Sprint** to next or `—`, note in both boards' Notes). Bump backlog **Updated:**.
3. Append **Completed sprints** row (Goal, Closed date, Archive link).
4. Next plan **exists and approved:** rewrite live `TODO.md` from that plan (new trailing boundary task); **keep** Completed sprints; do not leave the closed task list on the live board.
5. Next plan **missing or unapproved:** **Plan Next** / present for plan approval; rewrite only after approval.

After Boundary (and next-plan approval when needed), Current focus / Recommended next come from the new sprint’s seeded tasks.

### Report

```markdown
### Progress — {Title}

- **Sprint:** {sprint-NN} — {goal}
- **Phase:** {phase}
- **Done:** {done}/{total} ({percent}%)
- **Current focus:** {T-id} — {title}
- **Recommended next:** {T-id} — {title} ({why})
- **Backlog:** {done}/{total} stories closed   # when product-wide status matters
```

---

## 3. Artifacts

| Role | Path | Authority / rules |
|------|------|-------------------|
| **Progress SSOT** | `{workspace}/TODO.md` | Live sprint tasks, Snapshot, Current focus, Recommended next. Current sprint only. Seed/rewrite only after **Sprint plan approval**. Preserve **Completed sprints** across rewrites. |
| Product backlog | `{workspace}/scrum/product-backlog.md` | Story status (`PB-*`) — not day-to-day progress. Append-only IDs. **Story:** required (acceptance-oriented). **Spec:** path+section when Specs exist; else `—`. **Sprint:** set when committed; keep after done. **Depends:** other `PB-*` or `—`. |
| Sprint plan | `{workspace}/scrum/sprint-NN.md` | Goal, Scope, DoD, Demo — plan only. Zero-pad (`sprint-00.md`, …). Scope = committed `PB-*` only. Not a second progress board. Do not edit Goal/DoD/Demo during execution to “mark done.” |
| Closed board | `{workspace}/scrum/TODO-NN.md` | Exact copy of `TODO.md` at Boundary. Immutable after a successful copy. |

Demo Checklist items on the plan should appear as gate tasks on `TODO.md`. Seed tasks from Scope + companion workflow; end with a boundary task.

---

## 4. Templates

### Task model

| Signal | Meaning |
|--------|---------|
| `- [ ]` | Open |
| `- [x]` | Closed (done, or `_(skipped)_`) |
| Snapshot **Current focus** | At most one in-progress open task |
| `_(blocked: …)_` | Open but blocked |
| `_(skipped)_` | Closed without doing the work |

Use checkboxes + Current focus only — no `pending` / `in_progress` / `done` tokens on headlines. Same signals apply to backlog `PB-*`.

| Kind | Form | Stability |
|------|------|-----------|
| Task | `T-{nnn}` | Stable within one board; never renumber/reuse. May restart at `T-001` when `TODO.md` is rewritten for a new sprint. |
| Story | `PB-{nnn}` | Stable for the deliverable; append only — never renumber/reuse. |

**Story → tasks:** Each scoped `PB-*` becomes one or more `T-*`. Prefer Notes or title to show the parent (e.g. `PB-012`).

### Task entry

```text
- [ ] **T-00n** — {short title}
  - Phase: {phase or "cross-cutting"}
  - Artifact: `{path}` or —
  - Depends: T-00m, T-00k
  - Notes: {optional}
```

### Boundary task entry

```text
- [ ] **T-00n** — Sprint Boundary → sprint-{NN+1}
  - Phase: cross-cutting
  - Artifact: `scrum/TODO-{NN}.md`
  - Depends: {prefer every other planned T-*; else the last scope/demo gate}
  - Notes: archive board, update backlog, load next sprint
```

### TODO.md

```markdown
# TODO: {Title}

> **Project root:** `{workspace}`
> **Current sprint:** sprint-{NN} — [{goal}](./scrum/sprint-{NN}.md)
> **Product Backlog:** [`scrum/product-backlog.md`](./scrum/product-backlog.md)

## Snapshot

| Done | Open | Blocked | Skipped |
|------|------|---------|---------|
| {n}  | {n}  | {n}     | {n}     |

**Sprint goal:** {one line}
**Current focus:** {T-id or "—"}
**Recommended next:** {T-id} — {one-line description}

## Tasks

{task entries — current sprint only}

## Completed sprints   # keep across rewrites — never wipe

| Sprint | Goal | Closed | Archive |
|--------|------|--------|---------|
| 00 | {one-liner} | {YYYY-MM-DD} | [`TODO-00.md`](./scrum/TODO-00.md) |

## Notes
…
```

### product-backlog.md

```markdown
# Product Backlog — {Title}

> **Execution board:** [`../TODO.md`](../TODO.md)

## Stories

- [ ] **PB-001** — {short title}
  - Story: {concrete Done-when / who-wants-what}
  - Spec: `{spec/….md}` §{section}   # or —
  - Sprint: {sprint-NN or —}
  - Depends: —
  - Notes: …
```

### sprint-NN.md

```markdown
# Sprint {NN} — {short goal title}

> **Product Backlog:** [`product-backlog.md`](./product-backlog.md)
> **Execution board:** [`../TODO.md`](../TODO.md)

## Sprint Goal
{1–3 sentences}

## Scope

| ID | Story |
|----|-------|
| PB-xxx | {short title — match backlog headline} |

**Out of sprint:** {next slice or —}

## Definition of Done
1. …
2. …

## Demo Checklist
- …   # mirror as gate tasks on TODO.md
```
