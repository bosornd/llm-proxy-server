# Stage ④ — ASR Consolidation (`spec/ASR.md`)

**Prerequisites:** Readiness Gate **G3** and **G4** pass — every blocking AC is `designed` or `deferred`, and no linked ADR remains `proposed` — **or** there are no blocking ACs. Partial progress (e.g. only AC-001/AC-002 `designed` while later ACs stay `identified` / `reviewing`) is **not** enough.

**Gate artifact:** `{workspace}/spec/ASR.md` — complete ASR registry approved by the user.

**Next stage:** `05-spec-writing.md` (after user approves `ASR.md`)

**Refuse early entry:** If any blocking AC is still `identified` or `reviewing`, **do not** draft or update `ASR.md` as stage work. Return to `03-ac-review.md` and finish those ACs (or get explicit user `deferred` on each remaining one). Do not “pre-consolidate” designed ACs into ASR while the registry is incomplete, unless the user explicitly ordered a partial consolidate **and** remaining open ACs are marked `deferred`.

---

## Procedure

1. **Re-check G3/G4** against `AC.md` (and linked ADR statuses). On fail → stop; resume ③.
2. **Create or open `ASR.md`** using the template below.
3. **From PRD:** For each goal, constraint, deliverable form, or success criterion that is architecturally significant and not already covered by an AC outcome, register an ASR with **Source:** `PRD` and a testable **Statement**.
4. **From designed ACs:** For each blocking AC at `designed` (ADR **or** Direct Input), register or merge an ASR with **Source:** `AC-00x`, copying **Resolution** into **Statement** / **Decision summary**. Link **Related AC** and **Related ADRs** on the ASR; set the **ASR** back-link on the AC in `AC.md`.
5. **Deduplicate:** One ASR must not contradict another; merge overlapping PRD and AC outcomes.
6. Fill **Dependency Order** for Spec writing (which ASRs must appear in which `{requirement-slug}.md` first).
7. Present `ASR.md` for user go-ahead before stage ⑤. On go-ahead **in the same turn**: record approval **and** start `05-spec-writing.md`.

Do not write `{requirement-slug}.md` content here — only the ASR registry.

---

## ASR.md Template

```markdown
# Architecturally Significant Requirements

Consolidated registry for {project-slug}. Approved in stage ④.

## Summary

| ID | Title | Source | Related AC | Related ADRs |
|----|-------|--------|------------|--------------|
| ASR-001 | {short title} | PRD \| AC-00x | AC-00x | adr/{slug}.md |

## Dependency Order (recommended Spec encoding path)

1. ASR-00x → ASR-00y → …

## ASR Detail

### ASR-001 — {Title}

- **Source:** PRD | AC-00x — {section or AC title}
- **Statement:** {Testable architecturally significant requirement}
- **Decision summary:** {If from AC/ADR/Direct Input — plain-language outcome}
- **Depends on:** ASR-00x (optional)
- **Related AC:** AC-00x (optional)
- **Related ADRs:**
  - `{workspace}/adr/{concern-slug}.md` — approved
- **Notes:** {optional}
```
