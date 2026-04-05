---
doc_id: APP-SHR-005
title: Authority-Closure and Promotion-Handoff Baseline for Reverse Recovery
status: active
version: 0.1.0
related_docs:
  - SPEC-SHR-000
  - SPEC-SHR-001
  - SPEC-SHR-002
  - SPEC-SHR-003
  - SPEC-SHR-004
  - SPEC-SHR-005
  - SPEC-SHR-006
  - APP-SHR-001
  - APP-SHR-002
  - APP-SHR-003
  - APP-SHR-004
---

# Authority-Closure and Promotion-Handoff Baseline for Reverse Recovery

## Purpose and boundary

This document is an informative operational baseline for stable authority closure and promotion handoff after the first-loop and repository-shaped baselines are already understood.

It stabilizes eight operational surfaces that were previously available only inside the broader `draft` companion:

- packet-first authority intake
- review-mode choice in practice
- copy-adapt exemplar for explicit local same-operator authority designation
- copy-adapt exemplar for packet and disposition disclosure of that path
- minimum viable Authority Disposition
- boundary-clean promotion transformation
- recovered-behavior representation mapping
- promotion-trace assembly and review checks

This APP does not create new workflow semantics, new governed-artifact schema minima, new authority rules, or a second Promotion Gate. `SPEC-SHR-002` remains the owner of authority review, promoted-output representation requirements, and Promotion Gate behavior. `SPEC-SHR-004` remains the owner of Authority Review Packet and Authority Disposition field minima. `SPEC-SHR-000` remains the owner of local-authority validity and same-operator designation authority. `APP-SHR-001` remains the broader `draft` companion for recovery memory, partial recovery, later-stage failure patterns, composition examples, and other wider or more revisable operating seams.

## When to use this document

Use this document when the engagement needs stable authority-review intake, same-operator disclosure, disposition practice, or promotion-handoff assembly without relying first on the broader `draft` APP.

Use `APP-SHR-001` only when the engagement also needs broader draft material such as recovery-memory realization, multi-slice composition, partial-recovery handling, or later-stage correction patterns.

## Stable-baseline extraction crosswalk

The table below records the stable subset extracted from `APP-SHR-001`.

| Historical `APP-SHR-001` section | Stable baseline now lives here |
|---|---|
| `APP-SHR-001 / Authority review in practice / Packet-first intake` | `APP-SHR-005 / Packet-first authority intake` |
| `APP-SHR-001 / Authority review in practice / Choosing review mode` | `APP-SHR-005 / Review-mode choice in practice` |
| `APP-SHR-001 / Authority review in practice / Same-operator explicit local authority path` | `APP-SHR-005 / Copy-adapt template: explicit local same-operator authority designation` and `APP-SHR-005 / Copy-adapt template: same-operator packet and disposition disclosure` |
| `APP-SHR-001 / Authority review in practice / Minimum viable Authority Disposition` | `APP-SHR-005 / Minimum viable Authority Disposition` |
| `APP-SHR-001 / Appendix C: Promotion handoff and recovered-behavior representation` | `APP-SHR-005 / Boundary-clean promotion transformation`, `APP-SHR-005 / Recovered-behavior representation mapping`, and `APP-SHR-005 / Promotion-trace assembly and review checks` |

## Reuse and non-extension rule

This baseline reuses owner surfaces exactly. It does not introduce new packet fields, new disposition fields, new `representation_class` literals, or a new lineage surface.

| Surface | Reuse rule | Default carried through this baseline |
|---|---|---|
| `review_mode` | use the closed vocabulary owned by `SPEC-SHR-002` exactly as declared there | omission means `per-slice` |
| `authority_independence` and `local_authority_basis_ref` | use the packet fields owned by `SPEC-SHR-002` and `SPEC-SHR-004` exactly as declared there | omission means `independent-reviewer`; same-operator disclosure is explicit only |
| `promotion_trace_matrix` | use the packet-local promotion handoff surface owned by `SPEC-SHR-004` exactly as declared there | one row per promoted unit is the operational default |
| `representation_class` | use the closed vocabulary owned by `SPEC-SHR-002` exactly as declared there | preserved accidental behavior is placed explicitly, not silently normalized |
| `derived_from` | keep document-level lineage in the existing `SPEC-SHR-000` field | `derived_from` remains document-level lineage only and does not replace clause-level provenance |

## Packet-first authority intake

A practical packet should answer a small set of questions without forcing the reviewer to read raw code, broad conversational history, or unrelated slices. The packet is a bounded review surface, not a second source of truth.

| Packet field or section | What the reviewer learns | Operational note |
|---|---|---|
| `packet_ref` | which packet is under review | use a stable identifier that can be cited later from the disposition |
| `scope` | exactly what slice, batch, or clause set is being decided | keep the scope narrow enough that rejection has a clear blast radius |
| `recovery_objective` and `compatibility_target` | why this question matters | the reviewer should not have to infer the objective from the evidence |
| `authority_independence` and `local_authority_basis_ref` when applicable | whether review is independent or using an explicit same-operator local-authority path | same-operator review should be visible immediately rather than inferred later from names or repository size |
| `questions_for_authority` | the normative questions still open | phrase these as decisions, not as open-ended essay prompts |
| `candidate_decisions` | the live alternatives | include the most plausible narrower or defer option, not only approve or reject |
| `basis_refs` | which witnesses and governed artifacts matter most | link the decisive basis, cite chain refs when clause provenance rather than one raw witness is under review, and do not inline the whole corpus |
| `open_objections` | what the critic still disputes | empty is allowed, but say so explicitly |
| `compatibility_and_security_impact` | what breaks, leaks, or drifts if the wrong decision is taken | this keeps the review grounded in consequence rather than narrative plausibility |
| `recommended_disposition` | what the coordinator or proposer believes should happen | recommendations are useful as long as the evidence and objections stay visible |
| `follow_up_if_rejected` | what the workflow does next if the reviewer says no or bounds the scope | a packet that cannot answer this is not ready |
| `promotion_trace_matrix` when `review_mode = promotion-batch` | how each promoted clause, compatibility clause, exclusion, or explicit out-of-scope note traces back to slice and basis refs | treat this as the clause-level handoff surface, not as an optional editorial convenience |

A practical first-pass packet should fit into one short artifact plus links to decisive witnesses. If it requires the reviewer to absorb the whole recovery tree before they can even state the question, the packet is not ready.

## Review-mode choice in practice

| Review mode | Use when | Avoid when | Operational cue |
|---|---|---|---|
| `per-slice` | the question is new, contentious, or repository-local | throughput pressure is the only reason to batch | default for first packet examples and for repositories new to the vocabulary |
| `bounded-batch` | several slices share one authority owner, one policy question, one compatibility target, and one sensitivity class | any slice has unresolved critic disagreement, a different compatibility target, or a different confidentiality boundary | keep one packet plus clear per-slice annexes or per-slice dispositions |
| `promotion-batch` | a coherent Level 3 handoff is ready except for final ratification | the batch still contains unresolved non-authority technical work | use only when the handoff is actually promotion-ready and the clause-level `promotion_trace_matrix` is ready with it |

Per-slice review remains the operational default. A taxonomy-naive repository should stay in `per-slice` mode until there is at least one accepted local packet example and one stable local vocabulary note or glossary.

## Copy-adapt template: explicit local same-operator authority designation

Use the same-operator path only when an authoritative local basis exists and names that path explicitly. The local basis must be a scanned local active `SPEC` under the governed document roots. It is not satisfied by an `APP`, an `RFC`, a `RES`, or an arbitrary Markdown note.

A minimal local designation surface is:

```markdown
# /specs/SPEC-005-local-authority-designation.md
---
doc_id: SPEC-005
title: Local authority designation for reverse-recovery ratification
status: active
related_docs:
  - SPEC-SHR-000
  - SPEC-SHR-002
  - APP-SHR-005
---

## Purpose and boundary

This local specification designates the release-engineering owner as the local authority for reverse-recovery ratification of machine-parsed release-automation surfaces recovered in this repository.

## Local rule

Same-operator authority is permitted only when all of the following are true:

1. the authority question is packeted through an Authority Review Packet
2. the packet records `authority_independence: same-operator-explicit-local-authority`
3. the packet records `local_authority_basis_ref: SPEC-005`
4. the resulting Authority Disposition preserves packet, basis, and normative-effect records required by the adopted reverse-recovery workflow
5. Promotion Gate handling remains unchanged

This rule does not authorize silent local closure, does not waive packet-first review, and does not waive Authority Disposition or Promotion Gate requirements.
```

The numeric identifier in this example is illustrative only. Use the local repository's own numbering rule. In an adjacent governed workspace, the same rule applies inside that workspace's scanned local `SPEC` set rather than inside the external target repository.

## Copy-adapt template: same-operator packet and disposition disclosure

When the same-operator path is valid locally, disclose it in the packet and disposition rather than by narrative implication.

### Authority Review Packet excerpt

```yaml
packet_ref: ARP-0003
scope: slice:cli-summary-line
recovery_objective: backward-compatible reimplementation
compatibility_target: preserve current release automation contract
review_mode: per-slice
authority_independence: same-operator-explicit-local-authority
local_authority_basis_ref: SPEC-005
questions_for_authority:
  - Should stable key order be preserved as part of the recovered machine-parsed interface?
candidate_decisions:
  - approve inclusion of stable key order in the recovered contract
  - bound the clause to stable prefix and key presence only
basis_refs:
  - SC-0003
  - EB-0003:chain:summary-line-contract
open_objections: []
compatibility_and_security_impact: field-order drift would break current release automation; no additional sensitive material is required for first-pass review
recommended_disposition: approved-with-exceptions
follow_up_if_rejected: narrow the clause to stable prefix and key presence and reopen order dependence only if new evidence appears
```

### Authority Disposition excerpt

```yaml
artifact_ref: slice:cli-summary-line
packet_ref: ARP-0003
decision: approved-with-exceptions
review_mode: per-slice
scope: include stable prefix, key names, and key order in the recovered contract
basis_refs:
  - SC-0003
  - EB-0003:chain:summary-line-contract
normative_effect: promote
rationale: the decisive runtime and consumer evidence show a load-bearing machine-parsed interface surface
remaining_exceptions:
  - failure-path optionality of the output field remains a separate slice
follow_up_actions:
  - open a separate slice for failure-path optionality of the output field
ratifier: release-engineering owner
```

If the same-operator path is not valid locally, omit `authority_independence` and `local_authority_basis_ref` and allow the workflow default to remain `independent-reviewer`.

## Minimum viable Authority Disposition

A useful Authority Disposition is short, explicit, and mechanically traceable back to the packet it closes.

| Disposition field | Why it matters |
|---|---|
| `artifact_ref` | names what the decision applies to |
| `packet_ref` | shows exactly which packet was reviewed when the decision is promotion-bearing or materially alters contractual force |
| `decision` | records the authority judgment in plain terms |
| `review_mode` | shows whether the reviewer saw a slice, a bounded batch, or a promotion packet |
| `basis_refs` | keeps the decisive basis visible on the disposition itself |
| `normative_effect` | records whether the decision promotes, bounds, excludes, defers, or carves out |
| `remaining_exceptions` | prevents silent over-closure |
| `follow_up_actions` | keeps the workflow moving after the decision |
| `ratifier` | names the authority source that closed the question |

The operational failure mode is a disposition that says only `approved` or `rejected` without naming the decisive basis or normative effect. That is a record of sentiment, not a reviewable closure.

## Boundary-clean promotion transformation

Promotion is a transformation, not a copy. The promoted `SPEC` should carry boundary-clean contractual prose for the declared scope. The recovery tree keeps witness detail, negative evidence, mechanistic reasoning, live alternatives, and provisional classification history.

Use the following defaults unless a domain-specific reason requires a narrower local rule. These defaults mirror `SPEC-SHR-002 / Promoted-output completeness and representation` and the packet-level schema minima in `SPEC-SHR-004` rather than replacing them.

- preserved accidental behavior defaults to an explicit compatibility clause or compatibility appendix, not to silent normalization
- unresolved or authority-pending behavior defaults to non-promotion
- scope-bounded behavior defaults to an explicit out-of-scope note, not to silent omission
- mechanistic explanation defaults to the recovery record rather than the promoted contract prose
- `derived_from` remains document-level lineage only and does not replace clause-level provenance

## Recovered-behavior representation mapping

| `representation_class` | Recovered behavior class | Recommended promoted representation |
|---|---|---|
| `main_contract_clause` | intended and load-bearing | main contract clause |
| `compatibility_clause_or_appendix` | externally depended-on accidental behavior that will be preserved | compatibility clause or compatibility appendix |
| `explicit_exclusion_or_carve_out` | externally depended-on accidental behavior that will not be preserved | explicit exclusion, carve-out, or sunset note |
| `explicit_out_of_scope_note` | behavior outside the declared scope | explicit out-of-scope note or bounded exclusion |

Behavior that is still unresolved or still authority-pending is not promotion-ready and should remain Level 2 working state.

## Promotion-trace assembly and review checks

For `review_mode = promotion-batch`, treat `promotion_trace_matrix` as the required clause-level handoff surface rather than as an editorial aid.

A minimal promotion-trace excerpt is:

```yaml
promotion_trace_matrix:
  - promoted_unit_ref: clause:cli-summary-line-order
    representation_class: compatibility_clause_or_appendix
    slice_refs:
      - slice:cli-summary-line
    basis_refs:
      - EB-0003:chain:summary-line-contract
      - ARP-0003
      - AD-0003
```

The operational default is one row per promoted unit. Do not collapse several clauses into one row merely because they came from the same packet.

### Promotion assembly order

1. Freeze the promoted scope, `recovery_objective`, `compatibility_target`, and normalized `target_anchor` for the handoff packet.
2. Build `promotion_trace_matrix` so every promoted unit has reviewable slice and basis provenance.
3. Place preserved accidental behavior, exclusions, and out-of-scope material using the representation mapping table above rather than by ad hoc prose placement.
4. Populate `derived_from` using the recommended recovery-origin lineage form when the governed output is materially derived from slice-backed recovery.
5. Remove mechanistic residue from the promoted prose and leave the supporting analysis in the recovery record.

### Operational review checks

A promotion handoff is operationally ready when all of the following are true.

- every promoted unit can be traced back through `promotion_trace_matrix` without reading raw conversational history
- preserved accidental behavior lands in a predictable compatibility location rather than being silently normalized into the main contract
- unresolved or out-of-scope behavior is explicitly bounded rather than silently dropped
- the final prose is black-box clean even when the recovery record necessarily contains structural reasoning

## Least-context and security discipline

Authority review is a least-context function. First-pass packets should prefer witness refs, redacted summaries, and boundary-centered consequences over raw secrets, raw credentials, unredacted production payloads, or unnecessary personally identifying information.

| Prefer in first-pass packets | Keep out of first-pass packets unless decisive and separately handled |
|---|---|
| stable witness refs | raw credentials |
| redacted summaries | private keys |
| boundary-centered consequence statements | unrelated production payloads |
| explicit scope and compatibility notes | unnecessary PII |
| named open objections and next moves | broad conversational history used as a substitute for packet provenance |

If no authority reviewer exists, or the designated domain-authorized reviewer is unavailable for the question at hand, keep recovery moving at Level 1 or Level 2, record the blocked normative question, and emit a Recovery Snapshot before the work goes cold. Do not infer approval from silence, delay, or reviewer fatigue.

## Operational acceptance checks

This baseline is operationally sufficient when all of the following are true.

1. A small team can copy-adapt the same-operator local-authority path without consulting `APP-SHR-001`.
2. An authority reviewer can assemble packet, disposition, and promotion handoff from this document plus the governing `SPEC` documents without reading the whole recovery tree first.
3. Every example in this document reuses existing workflow vocabularies and existing packet, disposition, and promotion-trace surfaces rather than inventing new ones.
4. Stable authority-closure and promotion-handoff routing no longer depends on the broader `draft` APP.
