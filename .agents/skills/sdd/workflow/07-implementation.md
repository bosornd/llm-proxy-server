# Stage ⑦ — Implementation (artifact production)

**Prerequisites:** Readiness Gate **G7** pass (Spec approved in stage ⑥). Scrum: current sprint is **sprint-01+** (sprint-00 Boundary done), unless the user explicitly skips scrum for a tiny one-shot.

**Gate artifacts:** Deliverable(s) at the path(s) named in Spec/PRD (or agreed with the user when Spec names only a form); `{workspace}/AGENTS.md` **created if missing, and kept consistent with the tree/build/verify method** in the same turn (including **Notes** for durable development context).

**Next stage:** `08-verification.md`

---

## Authority

| Source | Role |
|--------|------|
| `spec/{requirement-slug}.md` + `spec/PRD.md` | **SSOT** — what to build |
| Live sprint Scope (`TODO.md` / `sprint-NN.md`) when scrum is active | **Which** Spec bullets / `PB-*` to implement now |
| Existing codebase + project conventions | Placement and style when Spec is silent |
| `AGENTS.md` | Orientation and method — **not** requirements |
| Chat / original prompt | Must not override Spec; if conflict, ask which source wins |

**Not inputs for requirements:** `docs/`, `adr/`, `AC.md`, `ASR.md` (already deep-copied into Spec). Do not open them as a substitute for Spec content.

---

## User gates vs agent autonomy

Stage ⑦ consumes an **already approved** Spec. Design decisions belong in ①–⑥ (and Spec re-confirm via ⑨). Do **not** invent mid-implementation user approvals for code style, module layout, or “PB done?” checks.

| Who | Owns |
|-----|------|
| **User** | Sprint **plan** approval (Scope/DoD/Demo), Spec as SSOT (⑥ / ⑨), Sprint **completion** (boundary) |
| **Agent** | Implementation plan on the board, code/tests, per-task self-check, Demo Checklist **execution**, board Sync |

Sprint user gates (scrum skill):

```
Sprint plan approval → Execute (⑦ agent-driven) → Sprint completion → Sprint Boundary
```

Inside Execute for this stage: keep moving through open `T-*` without stopping for user go-ahead **unless** a **Blockers** row applies (Spec ambiguity / missing decision). Natural “keep going” is already granted by plan approval.

---

## Procedure

### 1. Scope & load Spec(s)

| Request | Load |
|---------|------|
| Named slug | `PRD.md` + that `{requirement-slug}.md` |
| "All specs" / full project | `PRD.md` + every in-scope `{requirement-slug}.md` |
| Unspecified | Ask which deliverable set, then load it |

When scrum is active, **narrow** to bullets referenced by the current sprint’s committed `PB-*` / tasks — do not implement Spec scope outside that sprint.

Keep in working memory: Requirement title/statement, every in-scope `R-NNN` / `C-NNN`, Out of Scope. Spec overrides conversation history. Ignore provenance links as requirements — implement from the statement text.

### 2. Scrum check

Load the **scrum** skill. Implement **only** the live board’s committed Scope.

- Skip the board only when the user explicitly allows a tiny one-shot without scrum.
- Never implement product scope while sprint-00 is current (SDD skill **Scrum mapping**).
- Live board must already exist from **Sprint plan approval** + seed. Do not invent a parallel off-board worklist.

### 3. Implementation plan (seed / reshape board)

Before writing product code (or as the first work when seeding after plan approval), turn Scope into an executable board. This is **agent work** — present a short plan summary in Report if useful, but **do not** add a user gate to “approve the implementation task breakdown.”

#### 3.1 Derive tasks from Scope

| Input | Board output |
|-------|----------------|
| Each scoped `PB-*` | One or more `T-*` with Phase `implementation` (title/Notes name the parent `PB-*`) |
| Sprint **Demo Checklist** | `T-*` with Phase `demo` — agent **runs** and records evidence; **not** per-item user gates |
| Sprint exit | Trailing **Sprint Boundary** task (scrum skill) |

Order by backlog **Depends** and technical prerequisites. Prefer the smallest slice that can be self-checked before the next `T-*`. Parallel `T-*` only when Depends allow.

#### 3.2 What to put on each implementation `T-*`

- **Artifact:** concrete paths to create/change (or `—` until discovered, then Sync).
- **Notes:** which Spec `R-NNN` / `C-NNN` this slice covers, and the self-check the agent will run before closing the task.
- Close a `T-*` only after that self-check passes (build/tests/smoke as applicable). Closing the task is **not** a user approval event.

#### 3.3 Do **not** seed (anti-patterns)

| Anti-pattern | Why |
|--------------|-----|
| Per-`PB-*` “완료 게이트” / `implementation gate` waiting on user go-ahead | User already approved Spec + sprint plan; code-level sign-off is agent self-check + Sprint completion |
| Separate user gate after every file/module/PR-sized chunk | Same — fragments Execute into needless stops |
| Re-opening ADR/AC/Spec review as board gates inside ⑦ | Design gates belong in Spec→backlog (①–⑥) or ⑨ |
| Treating Demo Checklist rows as user `gate` tasks that block until chat approval | Agent executes Demo; user reviews outcomes at **Sprint completion** |
| Silent heavy refactors/migrations not represented as `PB-*` / `T-*` | Must be explicit board work (scrum skill) |

Spec→backlog companion gates (PRD/AC/Spec draft → user gate) apply **only** in those stages — do not copy that pattern into Backlog→Implementation boards.

#### 3.4 Demo & handoff tasks

- Seed Demo Checklist as agent-runnable `demo` tasks (or fold into the last implementation `T-*` Notes when the checklist is tiny).
- Evidence from demo tasks feeds the **Sprint completion** presentation — that is the user review of outcomes, not a mid-sprint pause per demo bullet.
- Formal Spec verification remains stage **⑧** (`08-verification.md`); demo/self-check does not replace it.

#### 3.5 After plan approval

On Sprint plan go-ahead **in the same turn** (scrum): seed/rewrite `TODO.md` with the structure above, set Current focus to the first unblocked implementation `T-*`, and start **§4 Implement** — do not stop after seeding alone.

### 4. Implement

Produce artifacts **strictly from the Spec** (and current sprint Scope when scrum is active):

1. Locate existing code and conventions before writing; match neighboring modules rather than inventing a new layout. **Read `{workspace}/AGENTS.md` first** when it exists — it is the orientation SSOT for how this repo is structured and how to work in it.
2. Honor every **in-scope** `R-NNN`; respect every in-scope `C-NNN` and Out of Scope. Add nothing the Spec does not call for.
3. When Spec is silent on style or placement, follow the repository’s patterns and `AGENTS.md` Implementation guide — choose and proceed; do not ask the user to pick among equivalent code layouts.
4. Write to path(s) Spec/PRD names. If Spec only describes form (e.g. “HTTP API”) and not a path, agree the path with the user before creating new top-level trees.
5. Add or update automated tests when the Spec or project conventions require them for the slices you touch.
6. After each `T-*` (or cohesive batch in one turn): self-check → **sync `AGENTS.md` (§5)** → `[x]` → Sync Snapshot → continue to Recommended next without waiting for user go-ahead.

**Hard rule:** Closing an implementation `T-*` without an up-to-date `AGENTS.md` is incomplete. Structural product work and context maintenance are one unit of work — not a deferred cleanup.

### 5. Maintain `AGENTS.md` (mandatory context)

`AGENTS.md` is the **living orientation map** for later sessions (and for the next `T-*`). Spec is still requirements SSOT; `AGENTS.md` is how a cold start finds structure, build method, and non-obvious facts without re-walking the tree or replaying chat.

**Mandatory in every implementation turn that changes the deliverable:**

1. If `{workspace}/AGENTS.md` is missing → **create it** in this turn (use the template below). Do not finish product edits and leave the file absent.
2. If it exists → **read it**, then **update any section that drifted** from what you just built. Keep sections and notes that are still true; never wipe unrelated sections.
3. Treat drift as a defect of the same turn: folder added/removed/renamed, package/module introduced, dependency direction changed, build/run/verify commands changed, or a hard-won implementation fact appeared — all require an `AGENTS.md` edit **before** marking the `T-*` done.

| Section | Contents | Update when (non-exhaustive) |
|---------|----------|------------------------------|
| **Repository structure** | One short paragraph on what the repo is; tree of **major folders only** with one-line roles; one or two sentences on how those folders depend. No file lists inside folders. | A major folder / package / app target is **added, removed, renamed, or its role changes**; new top-level or workspace package appears; dependency direction between major folders changes |
| **Working principles** | Standing rules (skills to follow, where research goes, what belongs in notes) | A standing rule changes |
| **Implementation guide** | Module design/structure, then steps/commands/paths/conventions to build or change the deliverable | Module layout, entry points, dependency wiring, scripts, packaging, or conventions change |
| **Verification guide** | How to check against the Spec (commands, fixtures, evidence) so stage ⑧ can follow it. Pass/fail record stays in `verification/`. | The verification method, smoke path, or required commands change |
| **Notes** | Durable facts a later implementer must know that are **not** already in `spec/`, `adr/`, `docs/`, or `TODO.md` | Such a fact appears, changes, or becomes false — see **Notes duty** below |

#### Notes duty (development context)

Use **Notes** to preserve implementation context that Spec does not carry. Prefer short, actionable bullets a future agent can apply without re-discovery.

**Do record** (examples):

- Runtime / packaging constraints (asar, unpack paths, config dirs, FUSE, native modules)
- Non-obvious path or wiring rules (where extensions live vs apps; which app depends on which extension)
- Commands or flags that differ by target (browser vs electron vs next) when not obvious from the Implementation guide alone
- Pitfalls fixed during this work (wrong assumption, broken default, required postinstall/patch step)
- Pointers to deeper research in `docs/` when the note itself must stay brief

**Do not** dump chat narration, Spec requirement restatements, or one-off task status (that belongs on the scrum board). Remove or rewrite a Note when it becomes false.

Do **not** edit `AGENTS.md` only because a file inside `spec/`, `adr/`, or `verification/` changed. Research write-ups go in `docs/` (a note may point at a doc). Write headings and body in the user’s language (or the language already used in that file). If the file is new or sections are missing, add them without wiping unrelated existing sections:

~~~markdown
## Repository structure

{One paragraph: what this repository is}

```text
{workspace}/
├── {major-dir}/    # {one-line role}
├── spec/           # requirements and feature specs
├── adr/            # architecture decisions
├── docs/           # research
└── verification/   # verification records against the Spec
```

{How major folders depend on each other. Do not list files.}

## Working principles

1. {Standing rule for this repository}

## Implementation guide

### Module design and structure

{How modules are split, how they depend, and where they live}

### How to implement

{Steps, commands, paths, and conventions to create or change the deliverable}

## Verification guide

{How to check the deliverable against the Spec}

## Notes

- {Fact a later session must know}
~~~

### 6. Blockers

| Situation | Action |
|-----------|--------|
| Spec ambiguous, contradictory, or missing a needed `R-NNN` / `C-NNN` | **Pause** artifact edits that would encode a guess. Ask the user; agree a Spec update, then **user re-confirms Spec** (⑥ or scoped Spec approval per ⑨) before continuing |
| Consequential design trade-off not settled in Spec | Do **not** pick silently — route via ⑨ / AC·ADR; not a code-level “which refactor do you prefer?” gate |
| Environment/tooling blocks progress | Document briefly; if it changes how to implement or verify, record a still-true note in `AGENTS.md` |
| Defect after ⑧, or bug mid-build | Load `09-feedback-loop.md` and follow its routing — do not invent a parallel process |

Non-blockers (handle without user gate): equivalent module splits, naming within repo conventions, test file placement, minor dependency choice when Spec is silent and project norms exist.

### 7. Pre-handoff check

Lightweight self-check **before** recommending ⑧ — not a substitute for formal verification:

1. Spot-check that in-scope `R-NNN` / `C-NNN` have corresponding evidence in the artifact; fix obvious gaps.
2. Run automated tests the Spec or project defines for the touched slices; fix failures you introduced.
3. Confirm structure/behavior do not contradict Spec `R-NNN` / `C-NNN`, and Out of Scope does not appear in the artifact.
4. **`AGENTS.md` consistency:** file exists; Repository structure matches major folders/packages you touched; Implementation / Verification guides match current commands; **Notes** include any non-obvious facts learned this sprint that a later session still needs (drop stale notes).
5. If scrum: ensure Demo Checklist `T-*` evidence is recorded so Sprint completion can cite it.

### 8. Report

State deliverable paths **and** whether `AGENTS.md` was created/updated (call out structure or Notes changes when material); brief Requirements checklist (done / deferred to a later sprint); recommend stage ⑧. If scrum: note board progress (Current focus / Recommended next). When the boundary task is next → run scrum **Sprint completion** (user gate) — do not invent extra implementation gates before that.
