---
doc_id: RES-SHR-001
title: Reverse-Recovery Workflow-State Transition Reference
status: draft
version: 0.6.1
related_docs:
  - SPEC-SHR-002
  - SPEC-SHR-003
  - SPEC-SHR-004
  - SPEC-SHR-005
  - SPEC-SHR-006
  - APP-SHR-003
  - APP-SHR-005
---

# Reverse-Recovery Workflow-State Transition Reference

## Purpose and boundary

This document is an informative reading aid for `SPEC-SHR-002`. It is auxiliary, sits outside the minimum initial packet named in `APP-SHR-003`, and is not part of either short cold-start route defined by `SPEC-SHR-005` or `SPEC-SHR-006`.

It does not restate `SPEC-SHR-002` Recovery Definition of Done, promoted-output representation rules, or local lifecycle semantics from `SPEC-SHR-000`. It also does not define governed-artifact schema minima. Those surfaces live in `SPEC-SHR-004`. Stable authority-closure and promotion-handoff operating guidance lives in `APP-SHR-005`. This reference compresses workflow-reading routes only.

This document remains `draft` because it compresses workflow-reading routes from `SPEC-SHR-002` and should track later section and trace changes conservatively rather than pretending to be more stable than its governing source.

## Reading rule

1. This reference is not part of the minimum initial packet for a taxonomy-naive repository and is not part of either short cold-start path. Use it only after the authoritative transition rules in `SPEC-SHR-002` are already understood or when `APP-SHR-003` routes you here for fast recall.
2. `workflow_state`, `disposition`, and `support_level` are different records. They are also distinct from `SPEC-SHR-003` workspace structural posture. Shared vocabulary does not collapse their meanings.
3. The authoritative owner of default transition routes, unspecified-transition policy, and revalidation-triggered recomputation is `SPEC-SHR-002`, not this reference.
4. A transition or recomputation route not summarized here is neither allowed nor disallowed by this reference. Readers must consult `SPEC-SHR-002`.
5. Treat anchor change through `SPEC-SHR-002 / Workflow Overview / Target-anchor normalized form and comparison rules`. Free-form anchor prose is not a sufficient basis for recomputation under this reference.
6. When a workflow-reading route turns on packet, disposition, snapshot, or other governed-artifact field minima, consult `SPEC-SHR-004` for schema details after this reference has identified the controlling workflow section.
7. When the question is no longer only about workflow-state reading, but about packet-first authority intake, same-operator disclosure, or promotion-bearing handoff, leave this reference and use `APP-SHR-005`.

## Route-family index for workflow-state reading

Use the table below when the question is "where in `SPEC-SHR-002` do I read the controlling rule?"

| Workflow-reading need | Go to this `SPEC-SHR-002` section first | Why |
|---|---|---|
| ordinary slice opening, validation entry, blocking, authority routing, closure, and re-open routes | `Default workflow-state transitions` | this is the authoritative transition matrix |
| tempting shortcut routes such as `planned -> rejected` or `in-progress -> closed` without `validation-pending` | `Unspecified workflow-state transitions` | this is the authoritative unspecified-transition policy |
| recomputation after validator defects, continuity conflicts, or material anchor change | `Revalidation-triggered workflow-state recomputation` | this is the authoritative recomputation table |
| why a slice moved to `authority-required` rather than `blocked` | `Authority review operating model` together with `Revalidation-triggered workflow-state recomputation` | one section owns packet and authority-routing rules; the other owns state recomputation; use `SPEC-SHR-004` only if you need packet or disposition minima |
| why a slice is closed but not promotion-ready | `Validation / Slice advancement and revalidation`; `Promoted-output completeness and representation`; `Promotion Gate` | closure, promotion readiness, and ratification are different surfaces |
| where stable authority packet practice and promotion handoff now live | `APP-SHR-005 / Packet-first authority intake`; `APP-SHR-005 / Promotion-trace assembly and review checks` | those seams are not workflow-state reading and should not be compressed into this reference |

## Transition-family summary (Informative)

The family labels below are summary labels only. They are not additional workflow states.

| Transition family | Typical question answered by this family |
|---|---|
| opening and active-work | how a slice enters active work and reaches validation |
| scope-bounding | when explicit scope decisions move a slice to `out-of-scope` |
| validation-entry and validation-exit | when validation sends a slice back for refinement, rejects the current clause, or closes it |
| blocker and unblocker | when technical blockers move a slice into or out of `blocked` |
| authority-routing | when only a normative decision can close the live question |
| closure and revalidation | when later evidence reopens a previously closed slice |

For the actual `from_state` and `to_state` rules, use `SPEC-SHR-002 / Default workflow-state transitions`.

## Revalidation-reading summary (Informative)

Use the table below when the question is "what kind of impairment is this, and where does the authoritative recomputation rule live?"

| Impairment type | Read this authoritative section | Practical reminder |
|---|---|---|
| material technical impairment | `SPEC-SHR-002 / Revalidation-triggered workflow-state recomputation` | technical impairment usually routes to `blocked` unless unaffected evidence still satisfies the active contract |
| material normative impairment | `SPEC-SHR-002 / Revalidation-triggered workflow-state recomputation` and `Authority review operating model` | normative impairment usually routes to `authority-required` |
| anchor-driven instability | `SPEC-SHR-002 / Workflow Overview / Target-anchor normalized form and comparison rules` and `Revalidation-triggered workflow-state recomputation` | `materially-different` and `non-comparable` are the trigger outcomes |
| continuity conflict | `SPEC-SHR-002 / Recovery Memory and Negative Evidence / Continuity conflicts` and `Revalidation-triggered workflow-state recomputation` | preserve both artifacts first; do not silently overwrite |

## Disposition and support-level interaction

`Disposition` records current review judgment. `Support_level` records evidentiary grounding. They are orthogonal. A strong support level does not close a normative question by itself, and a negative or open disposition does not automatically negate the observed behavior that produced the current support record.

| `disposition` by `support_level` | `observed` or `corroborated` | `runtime-validated` or `externally-depended-on` | `authority-ratified` |
|---|---|---|---|
| `approved` | The current clause is judged acceptable at its present grounding, but later strengthening may still occur. | The clause is technically strong and presently approved, but authority closure is still separate unless ratified support is also recorded. | The clause is both approved and explicitly ratified. This is the stable fully closed pairing. |
| `needs-refinement` | The clause is not yet good enough even at early grounding. | Strong evidence may exist, but the clause framing, scope, or policy question still needs revision. | This pairing should be rare and should be explained explicitly if it appears. Ratified support ordinarily should not stay paired with an open refinement state. |
| `rejected` | The current clause or framing fails at its present grounding. | Strong evidence may still exist for a neighboring or narrower behavior. Rejection is about the current claim, not automatic falsity of every nearby claim. | Authority has rejected the candidate claim or carve-out. The support record still describes the basis strength that was reviewed. |
| `authority-required` | Evidence is still open and judgment is explicitly reserved. | The technical basis may already be strong, but future binding force is owned by authority rather than by ordinary workflow closure. | This pairing should be transient at most. Once explicit authority action ratifies the question, the slice should move to an ordinary closed judgment. |

The default reading rules are:

- `disposition` records current review judgment.
- `support_level` records evidentiary grounding.
- Neither field implies the other.
- `approved` does not, by itself, imply `authority-ratified`.
- `rejected` disposition does not necessarily mean the behavior was disproven. It may mean that the current clause, framing, or requested contractual force failed.
- `authority-required` may coexist with `runtime-validated` or `externally-depended-on` when the evidence is strong but the future contract still depends on authority judgment.
- `authority-ratified` support requires explicit authority action. It cannot be inferred from `workflow_state = closed`, from the existence of a packet, or from the absence of objections.

## Maintenance rule

If `SPEC-SHR-002` adds or changes any workflow literal, section title, trace label, or governing rule used by this reference, this document must be updated before readers rely on it. If this reference explicitly points to a paired schema section in `SPEC-SHR-004`, changes to that section title or governing schema rule are also update triggers. Changes to `SPEC-SHR-005`, `SPEC-SHR-006`, `APP-SHR-003`, or `APP-SHR-005` are likewise update triggers when they change whether this document is in or out of a routed reading path, or when they change the section names this reference points to for adjacent routing. In particular, changes to `workflow_state`, `support_level`, `disposition`, revalidation handling, target-anchor comparison rules, authority-review semantics, or the section names named in the route-family tables should be treated as update triggers for this reference rather than as details that readers are expected to reconcile mentally.
