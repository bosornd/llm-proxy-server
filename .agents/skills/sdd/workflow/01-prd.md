# Stage ① — PRD (`spec/PRD.md`)

**Prerequisites:** Readiness Gate G1 failed, or new project with no approved PRD. Unless the user explicitly skips scrum: current Spec→backlog sprint plan is **approved** and `{workspace}/TODO.md` is seeded (SDD **Scrum mapping** / scrum **Initialize**) — do **not** start this stage while those boards are missing.

**Gate artifact:** `{workspace}/spec/PRD.md`

**Next stage:** `02-ac-identification.md` (after user approves PRD)

---

## Procedure

1. **Confirm board:** If scrum is active and `TODO.md` / current `sprint-NN.md` are missing → stop; run scrum Initialize / Plan + **Sprint plan approval** first (sprint-00 Scope = initial product backlog).
2. **Identify Goal:** Artifact type, target audience, and core function.
3. **Create Project Folder:** Ensure `{workspace}` and `{workspace}/spec/` exist (confirm location/name with user before creating).
4. **Document PRD:** Write or update **`{workspace}/spec/PRD.md`** using the template below. Sync the matching `T-*` on `TODO.md`.
5. **Obtain PRD Approval:** Present the PRD and wait for user go-ahead. On go-ahead in the same turn: record approval **and** start `02-ac-identification.md` (do not stop after approval alone).

---

## PRD Content Guidelines

- State **what** deliverable(s) the project produces and **for whom**.
- Capture global **constraints** and **success criteria** at product level.
- PRD is **product context**, not the per-requirement SSOT. Detailed functional/non-functional requirements and design decisions belong in `{requirement-slug}.md` (stage ⑤).
- Use concise bullets over prose.

---

## PRD.md Template

```markdown
# PRD: {Product / Deliverable Title}

## Summary
{One or two sentences — what this project produces and for whom.}

## Goals
- {Primary goal}
- {Secondary goal}

## Audience
- {Who uses or consumes the deliverable}

## Deliverables
- {Artifact type(s), format, and output location — e.g. Flutter app, markdown report, PNG set}

## Scope
### In Scope
- {High-level capability or deliverable included in this project}

### Out of Scope
- {Explicit non-goals at product level — prevents silent scope creep}

## Constraints
- {Global technical, legal, time, format, or quality limits that apply across all specs}

## Success Criteria
- {How the user will judge the project successful — measurable where possible}
```
