# Stage ③ — AC Review (`spec/AC.md`)

**Prerequisites:** ACs identified in `AC.md` with Dependency Order (Readiness Gate G2 pass).

**Gate artifacts:** Updated `{workspace}/spec/AC.md`; approved `{workspace}/adr/{concern-slug}.md` when the ADR route is used.

**Next stage:** Stay in ③ until **every** blocking AC is `designed` or `deferred` **and** no linked ADR remains `proposed` (Readiness Gate G3 + G4). Only then → `04-asr-consolidation.md`.

This stage owns **AC resolution** in `AC.md`. It does **not** define how to write an ADR. When the **ADR Analysis** route applies, load the **adr** skill and follow it. Do not restate or improvise that procedure.

---

## Procedure

Review blocking ACs in **Dependency Order**, **one AC (or dependency branch) at a time**. Completing one AC does **not** exit stage ③.

1. Pick the next unsettled blocking AC (`identified` or `reviewing`) in Dependency Order whose Depends are settled (`designed` / `deferred` / none).
2. Set that AC to `reviewing`.
3. Route via **AC Decision Pathways** (below).
4. Record its resolution in `AC.md` as it settles — do not wait to batch every AC before writing, and do **not** start ④ mid-loop.
5. **Per-AC gate** (below): get explicit approval for **this** AC’s outcome, then continue the loop.

When a single AC’s resolution path is complete:

- If any Related ADR is still `proposed`: **pause for user ADR approval** via the **adr** skill. Do **not** invent approval.
- When all linked ADRs are `approved` (or the AC used Direct Input only): set **that** AC to `designed` with a concrete **Resolution** (from ADR **Decision** or Direct Input).

Status transitions: `identified` → `reviewing` → `designed`. Use `deferred` only with explicit user agreement.

**Hard rule — stay in ③:** While any blocking AC is still `identified` or `reviewing`, do **not** offer, draft, or enter stage ④ (ASR). Do **not** phrase the per-AC gate as “approve and proceed to ASR.” The only valid “next” after an early AC settles is the **next unsettled AC** in Dependency Order (or ADR follow-ups still open for the active AC). Exception: the user **explicitly** defers remaining ACs (`deferred`) or **explicitly** asks to consolidate a partial set — then re-check G3/G4 before naming ④.

---

## AC Decision Pathways

Do not force an ADR for every concern. Apply the **adr** skill filters under **When to Create an ADR** before opening a file.

| Route | When | Action |
|-------|------|--------|
| **Direct Input** | Straightforward problem, natural default, or clear user preference; or the concern fails the adr filters | Ask direct questions; user chooses; set AC to `designed`; fill **Resolution**. Record the disqualification label from the adr skill (`out of scope`, `directly resolved`, `covered`, `deferred assumption`) in **Resolution** or **Notes** — no new ADR file |
| **ADR Analysis** | User unsure, or complex trade-offs; concern passes the adr filters | Set AC to `reviewing`; run the **adr** skill for this concern; link ADR(s) per **ADR ↔ AC links** |

---

## ADR ↔ AC links

When using the ADR Analysis route:

1. Create or update `{workspace}/adr/{concern-slug}.md` via the **adr** skill.
2. On the AC: set **Resolution path** to `adr` (or `mixed` if Direct Input plus ADR), and list the ADR under **Related ADRs** with its status.
3. After the user **approves** the ADR: copy the ADR **Decision** into AC **Resolution** (full outcome text — not a path-only pointer). Set AC **Status** to `designed`. Return control to this stage’s **per-AC gate / loop** — ADR approval alone is not permission to start ④.
4. If the ADR is `rejected` or abandoned: keep or revert the AC to `reviewing`/`identified` and choose another route. Never leave an AC `designed` without a concrete **Resolution**.

One AC may link multiple ADRs when follow-ups appear (adr skill chaining). The AC stays `reviewing` until every **blocking** linked ADR is `approved`, or the remaining follow-up is `deferred` with user agreement.

---

## Gate

**Two different stops — never merge them into one question.**

### Per-AC gate (still inside ③)

After settling one AC (Direct Input or approved ADR → `designed` / agreed `deferred`):

1. Present the updated row for **that AC** (and its ADR if any).
2. Ask only whether the user gives go-ahead on **this AC’s Resolution** (or deferral).
3. Name the **next unsettled blocking AC** (Dependency Order) as the continuation — or state that remaining ACs must still be reviewed.
4. On go-ahead **in the same turn**: mark the gate and continue the next unsettled AC (or stage-exit when G3/G4 would pass). Do not stop after recording approval alone.
5. **Do not** mention ASR / stage ④ / `04-asr-consolidation.md` in this gate while G3 would still fail.

### Stage-exit gate (③ → ④)

Only when Readiness Gate **G3** and **G4** would pass (every blocking AC is `designed` or `deferred`; no linked ADR is `proposed`):

1. Present the full `AC.md` summary (all blocking ACs settled).
2. Ask for go-ahead to **leave ③** and run ASR consolidation (④).
3. On go-ahead **in the same turn**: record approval **and** start `04-asr-consolidation.md`.
