---
name: sdd
description: >-
  Spec-driven deliverable pipeline under {workspace}/spec/.
  Use when defining requirements, building from a Spec, or verifying against Specs.
  Concept chain:  PRD → AC → (ADR | Direct Input) → ASR → Spec → review → implement → verify (→ feedback).
  Spec is grounded in user-approved decisions: identify what the user must decide (AC), review alternatives (ADR)
  or take Direct Input, fold approvals into ASR, then write Spec.
  Pair with the todo(scrum) skill from the start (Initialize Spec→backlog / sprint-00 before ① PRD;
  Scope = initial or delta product backlog — never run ①–⑥ without TODO.md).
  Use the adr skill when a concern needs option analysis and user approval.
---

# SDD: Spec-Driven Development Skill

Stage-gated pipeline under `{workspace}`. Load **this file first**, then load the matching `workflow/*.md` before writing that stage’s artifacts. Advance only after the stage’s **user go-ahead** (gate): accept the recommendation and authorize the next step. Natural language (ok, continue, 계속해줘, 승인할게, …) counts — do **not** require the literal words “approve”. Silence or questions without proceed intent do not.

**Same-turn advance:** On go-ahead, record the gate **and** start the next stage/step in that same turn. Do not stop after only marking approval.

Concrete procedures live only in `workflow/*.md`. This file: concepts → stages → artifacts (paths) → Readiness Gate → scrum mapping.

---

## 1. Core concepts

### Spec is grounded in what the user decided

A Spec is **not** invented from agent preference or chat improvisation. It is written from **decisions the user has made** (plus PRD must-haves that need no further design choice).

The pipeline **identifies → decides → consolidates → encodes** those decisions:

```
PRD → AC → (ADR | Direct Input) → ASR → Spec → ⑥ review → ⑦ implement → ⑧ verify → ⑨ if needed
```

| Step | Role in the decision chain |
|------|----------------------------|
| **PRD** | Product definition the user owns (goals, constraints, success criteria, deliverable form). |
| **AC** | **Identify** structural questions the user **must settle** before Spec is complete. |
| **ADR \| Direct Input** | **Decide** each AC: **adr** skill when trade-offs need recorded options; otherwise Direct Input on the AC (straightforward default, clear preference, or fails adr “When to Create an ADR”). Finish **all** blocking ACs in Dependency Order before consolidating. |
| **ASR** | **Integrate** user-approved outcomes (PRD must-haves + **all** designed AC results) only after every blocking AC is `designed`/`deferred`. Skeleton for Spec writing — not SSOT in ⑦⑧. |
| **Spec** | **Encode** those outcomes as self-contained `R-NNN` / `C-NNN` (design substance included; optional ASR/AC/ADR provenance). **SSOT** for ⑦⑧. |
| **⑥ → ⑨** | Approve Spec → implement → verify; **⑨** remediates via Spec-first alignment. |

### Do not let the user miss consequential decisions

AC stages are **decision-coverage**, not paperwork:

1. **Surface** every blocking structural concern (②) before Spec writing.
2. **Force a settle path** per concern (③): ADR or explicit Direct Input — never silent agent defaults for consequential trade-offs. Settle ACs **one at a time**; do not jump to ④ after a subset.
3. **Consolidate** only after the full blocking AC set is closed (④), then write Spec from that basis (⑤⑥).

If a consequential design question is still open (`identified` / `reviewing` AC, or `proposed` ADR), Spec readiness — and ASR consolidation — is **not** complete. Prefer an explicit AC (and ADR when warranted) over burying the choice in draft Spec or partial ASR prose.

### Operating principles

1. **Spec is SSOT** for build and verify. Not the chat prompt, not AC/ASR/ADR, not `docs/` or `AGENTS.md`.
2. User **approves Spec** (⑥) before implementation.
3. **Implement / verify** only against the approved Spec (and current sprint Scope when scrum is active).
4. **Changes** → update Spec → user **re-confirms** → then implement / re-verify.
5. Spec **deep-copies** structurally important decisions into `R-NNN` / `C-NNN` (not link-only pointers). Optional `←` provenance to ASR/AC/ADR is allowed for tracing.
6. **Sprint state** → scrum skill — boards first (approved Spec→backlog plan + `TODO.md`) before ①. **Option analysis** → adr skill (③). Boards and ADR files are not implementation SSOT.

---

## 2. Workflow stages

| Stage | Workflow | Gate artifact(s) |
|-------|----------|------------------|
| — | *(this file)* | Readiness Gate (§4) |
| ① PRD | [`01-prd.md`](workflow/01-prd.md) | PRD |
| ② AC identification | [`02-ac-identification.md`](workflow/02-ac-identification.md) | AC |
| ③ AC review (ADR \| Direct Input) | [`03-ac-review.md`](workflow/03-ac-review.md) | AC; optional ADR |
| ④ ASR consolidation | [`04-asr-consolidation.md`](workflow/04-asr-consolidation.md) | ASR |
| ⑤ Spec writing | [`05-spec-writing.md`](workflow/05-spec-writing.md) | Spec draft (formal approval is ⑥) |
| ⑥ Spec review | [`06-spec-review.md`](workflow/06-spec-review.md) | Spec as SSOT + review record |
| ⑦ Implementation | [`07-implementation.md`](workflow/07-implementation.md) | Deliverable(s); `AGENTS.md` (create if missing; keep structure/guides/Notes in sync) |
| ⑧ Verification | [`08-verification.md`](workflow/08-verification.md) | Verification record |
| ⑨ Feedback | [`09-feedback-loop.md`](workflow/09-feedback-loop.md) | Spec + deliverable aligned |

①–⑥ establish user-approved inputs; ⑦⑧ consume the approved Spec only; ⑨ remediates via Spec-first alignment (edit Spec → user re-confirms → implement / re-verify) when Spec and deliverable diverge.

---

## 3. Artifacts (paths & conventions)

| Artifact | Path | Conventions |
|----------|------|-------------|
| **PRD** | `spec/PRD.md` | User-approved product definition. |
| **AC** | `spec/AC.md` | Living registry — **append**; never wholesale replace. Must-haves with no open design question are not ACs (they enter ASR from PRD). |
| **ADR** | `adr/{concern-slug}.md` | One concern; procedure via **adr** skill. Deep-copy into Spec. |
| **Direct Input** | *(on the AC)* | Resolution without an ADR file. |
| **ASR** | `spec/ASR.md` | Consolidation for Spec writing; not SSOT in ⑦⑧. |
| **Spec** | `spec/{requirement-slug}.md` | Implementation/verification SSOT. |
| **Spec review** | `spec-review/{requirement-slug}.md` | Gate record for ⑥. |
| **Verification** | `verification/{requirement-slug}.md` | Gate record for ⑧. |

**Supporting (not requirements SSOT):** `docs/`; `AGENTS.md` (mandatory in ⑦ — create if missing; update when folders/modules/build/verify change; put durable development context in **Notes**); scrum boards (`TODO.md`, `scrum/`).

Filenames for ADR and requirements: English kebab-case. `{workspace}` / project slug may use the user’s language. Templates: each stage’s `workflow/*.md`. Counts are not fixed (one Spec or several; zero or many ACs).

---

## 4. Readiness Gate

Run **once** at session start (or when resuming an open defect). Jump to the first failing gate, then advance **forward**. Do not re-run the full gate until a **new** build request or a **⑨** remediation that needs re-routing. On a **continuation** (requirement add/change), re-evaluate gates for the **delta** only; do not treat missing first-pass sprint-00 as a reason to re-Initialize.

**Before gates (scrum bootstrap):** Unless the user explicitly skips scrum, do **not** start ①–⑥ (or jump to G1) while progress boards are missing. If `{workspace}/TODO.md` or the current Spec→backlog `sprint-NN.md` is absent → load the **scrum** skill, **Initialize** / **Plan** that Spec→backlog sprint, get **Sprint plan approval**, seed `TODO.md`, then resume gates / Execute (**§5**). Ensure `{workspace}/spec/` exists (create `{workspace}` only with user confirmation if missing).

Evaluate **G1 → G7 in order**; stop at the first failure.

| Gate | Pass when | On fail → |
|------|-----------|-----------|
| **G1** | PRD exists and user-approved | ① |
| **G2** | Blocking ACs registered; **Dependency Order** set (`none` if none); user approved the list | ② |
| **G3** | **Every** blocking AC is `designed` or `deferred` (none `identified` / `reviewing`). One settled AC is not enough — ④ is blocked until the full blocking set is closed | ③ |
| **G4** | No linked ADR remains `proposed` (Direct Input–only ACs need no ADR) | ③ |
| **G5** | ASR lists PRD + **all** designed AC outcomes; user-approved | ④ |
| **G6** | In-scope Spec file(s) encode all blocking ASRs (AC/ASR are not Specs) | ⑤ |
| **G7** | User approved Spec as SSOT; review record exists; no open blocking SSOT/consistency issues | ⑥ |

**All pass →** finish Spec→backlog (`PB-*`) if not done, then Boundary → **⑦** in the next implementation sprint (**§5**).

G2–G5 block Spec writing while consequential user decisions remain open or unapproved.

**Stage ③ → ④:** Do not offer or start ASR consolidation until G3 and G4 pass. After each individual AC settles, the next step is the **next unsettled AC** in Dependency Order — not ④. See `workflow/03-ac-review.md` (per-AC gate vs stage-exit gate).

---

## 5. Scrum mapping

Use the **scrum** skill for every non-trivial deliverable (skip only if the user says so). SDD owns Spec/ADR content; scrum owns board state and when sprints advance.

**Hard rule:** Do **not** begin ① PRD (or any later stage) until the current Spec→backlog sprint has an approved plan and a live `TODO.md`. Without those boards, progress cannot be tracked or resumed accurately — chat memory is not a substitute.

### Sprint kinds

Every sprint is **exactly one** of:

| Kind | Purpose | Stages | Exit |
|------|---------|--------|------|
| **Spec→backlog** | Produce / update the **product backlog** from approved Spec | ①–⑥ + backlog encode | Spec SSOT + `PB-*` ready; **no** product ⑦ |
| **Backlog→Implementation** | Implement/verify committed `PB-*` | ⑦ (⑧/⑨ as scoped) | Committed Scope done → Boundary → next plan |

Do not mix kinds: no product ⑦ in Spec→backlog; no Spec/AC redesign as main Scope of an implementation sprint (small in-sprint Sync only; large change → new Spec→backlog sprint).

**Spec→backlog Scope is the backlog itself** (not product implementation):

| Case | Sprint Scope (what the sprint commits to deliver) |
|------|---------------------------------------------------|
| **First setup** (`sprint-00`) | **Create the initial product backlog** — run ①–⑥, then encode approved Spec as the first `PB-*` set. There is no prior product backlog to commit; the Outcome *is* that backlog. |
| **Feature add / change** (later Spec→backlog) | **Change the product backlog for the additional / revised requirements** — delta ①–⑥, then append (never renumber) `PB-*` for the delta; leave unrelated open stories per scrum skill. |

Seed live `T-*` from that Scope + stages ①–⑥ (gates as Demo Checklist items) + a trailing boundary task. Product `PB-*` encoding is the sprint’s exit work after G7 — still inside Spec→backlog, before Boundary.

### Bootstrap order (mandatory)

```
1. Load sdd + scrum skills
2. Initialize / Plan Spec→backlog sprint (sprint-00 or next NN)
   → write scrum/product-backlog.md + scrum/sprint-NN.md
   → present for Sprint plan approval → STOP
3. After go-ahead → seed TODO.md (①–⑥ tasks + boundary) **in the same turn**, then Execute
4. Execute → ① PRD → … → ⑥ → encode / update PB-* → Boundary
5. Plan Backlog→Implementation sprint → plan approval → seed → ⑦…
```

Never invert steps 2–4 (no PRD/AC/ADR draft run as the “real” start while `TODO.md` / `sprint-NN.md` are still missing).

### Lifecycle

```
[no scrum] → Initialize sprint-00 Spec→backlog (Scope: initial product backlog)
         → plan approval → TODO.md → ①–⑥ + encode PB-*
         → Boundary → plan sprint-01 Backlog→Implementation
         → (scale) more implementation sprints until backlog drained / Goal met

[new feature / large change] → Plan next Spec→backlog (Scope: backlog change for the delta)
                             → plan approval → TODO.md → delta ①–⑥ + append PB-*
                             → Boundary → plan following implementation sprint(s)
```

| Case | Kind | Notes |
|------|------|-------|
| **First setup** (`sprint-00`) | Spec→backlog | **Initialize** with Goal/Scope/DoD/Demo = *initial product backlog via Spec pipeline*. Present **Sprint plan approval** and **stop**. After approval, seed `TODO.md` from ①–⑥ (+ backlog-encode task) ending with a boundary task — **then** start ①. After G7: split Spec into small product `PB-*` (Story / Spec path+section / DoD) in `product-backlog.md` (close or supersede the bootstrap Scope story as the scrum skill directs). Optionally draft next `sprint-NN.md`; do not rewrite live `TODO.md` for it until Boundary **and** **Sprint plan approval**. |
| **Implementation** (`sprint-01+`) | Backlog→Implementation | Starts only after a Spec→backlog Boundary. Commit a **subset** of open product `PB-*` — never “finish the whole backlog” in one sprint. **Sprint plan approval** before Execute. |
| **Continuation** (existing board) | Spec→backlog for the **delta** | Keep `scrum/` history and continuous `sprint-NN` numbering — do **not** re-Initialize as sprint-00 or wipe Completed. Scope = **product-backlog change** for the add/change. Append `PB-*` (never renumber/reuse); carry unchanged open stories. At Boundary, plan the next implementation scope from the updated backlog. |
