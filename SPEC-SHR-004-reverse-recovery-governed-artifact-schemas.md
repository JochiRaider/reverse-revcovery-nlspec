---
doc_id: SPEC-SHR-004
title: Reverse-Recovery Governed Artifact Schemas
status: active
version: 0.13.0
related_docs:
  - SPEC-SHR-000
  - SPEC-SHR-001
  - SPEC-SHR-002
  - SPEC-SHR-003
  - SPEC-SHR-005
  - APP-SHR-001
  - APP-SHR-002
  - APP-SHR-003
  - APP-SHR-004
---

# Reverse-Recovery Governed Artifact Schemas

## Purpose, Artifact Class, Audience, and Authority Boundary

This document is the normative governed-artifact schema companion to `SPEC-SHR-002`.

It governs field presence, conditionality, omission semantics, empty-value meaning, continuity keys, governed-artifact identity, provenance-coordinate defaults, overlay field minima, and worked examples for the governed reverse-recovery artifact families. It does not govern workflow-state semantics, transition semantics, revalidation routing, authority-review ownership, evidence weighting, black-box discipline, or Promotion Gate behavior. Those surfaces are owned by `SPEC-SHR-002`.

This document follows the same define-once discipline that governs the shared corpus. Workflow meaning lives in `SPEC-SHR-002`. Canonical artifact shape lives here. When both documents mention the same artifact family, read `SPEC-SHR-002` for why and when the artifact is used, and read this document for what fields it must carry and what omission means.

## Relation to SPEC-SHR-002 and import model

`SPEC-SHR-002` and this document are intended to be adopted together. `SPEC-SHR-002` defines the reverse-recovery workflow and the authority model. This document defines the canonical governed-artifact shapes used by that workflow.

The owner boundary is:

| Artifact family or surface | Workflow-owned meaning | Schema-owned minima |
|---|---|---|
| artifact-role inventory | entry condition, inventory-first workflow expectation, protected-surface treatment | row shape, field status, omission semantics, and linkage rules |
| repository-surface-family record | requirement to classify repository surface families explicitly and close promotion-bearing repository families reviewably | row completeness, field minima, omission semantics, linkage rules, and closure-linkage minima |
| Specification Plan | queue meaning, slice lifecycle role, and advancement semantics | canonical field table, step-list minima, and omitted-field semantics |
| Slice Contract | local validation contract meaning and discriminating-question role | canonical field table, executable-action minima, and non-executable-resolution minima |
| Validation Report | criterion-relative closure meaning, validator semantics, and revalidation implications | canonical field table, criterion-result minima, interpretation minima, and validator-record minima |
| Critique Report, Evidence Bundle, Authority Review Packet, Authority Disposition | critique, evidence, packet-first authority review, and normative closure meaning | canonical field tables and omitted-field semantics |
| Recovery Snapshot | resumable handoff meaning and promotion-readiness interpretation | canonical field table and omitted-field semantics |
| overlays | overlay-local meaning and overlay matrix | overlay field minima and worked examples |

## Normative language

In this document, `must` introduces a conformance requirement. `may` introduces a permitted option or an explicit implementation freedom. `should` is advisory only and does not determine artifact validity.

The canonical field-minimum tables in this document and the overlay matrix in `Overlays` are authoritative for field presence, conditionality, and omission semantics. When a field is marked `required`, `required-with-empty-allowed`, `conditional`, or `optional-with-default`, that status has the same binding force as prose using `must`.

When this document says that a rule is `default and binding`, `default` identifies the baseline that governs unless a consuming domain overrides it under `Consuming-domain overrides`. It does not make the rule advisory.

## Consuming-domain overrides

Unless a local section says otherwise, a consuming domain may override a specific schema rule, field-local obligation, omission meaning, or overlay row without replacing this whole document.

A valid override must be declared once at an authoritative site in the consuming domain and must name the affected governing document id, section, table, row, and field together with the replacement rule, stricter rule, or narrowed scope.

All requirements in this document that are not explicitly named by that override remain binding.

Silence, implication, examples, local convenience templates, or looser prose do not override normative requirements.

## Imported vocabulary and authoritative-site rule

Closed and extensible literal declarations remain owned by `SPEC-SHR-002 / Vocabulary Declarations`. This document imports those declarations by reference for use in its field tables and worked examples. It does not create a third shared vocabulary specification.

When a field in this document uses a vocabulary declared in `SPEC-SHR-002`, the vocabulary owner remains `SPEC-SHR-002` and this document owns only the field-local presence and omission semantics.

## Artifact-role inventory minimum shape and linkage

For workflow entry conditions, inventory-first recovery expectations, and the semantic meaning of `control-plane prose`, `framing metadata`, and `protected surface`, use `SPEC-SHR-002 / Recovery Inputs and Artifact Roles / Control-plane prose and artifact-role inventory`. This section owns the canonical minimum row shape, field status, omission semantics, and linkage rules for the artifact-role inventory.


The artifact-role inventory is a required entry record. It is not a new top-level governed artifact family. Unless a consuming domain defines a stricter carrier rule, any stable reviewable recovery-local artifact may carry it. `SPEC-SHR-003` governs the corpus-default carrier coordinate and any non-default topology declaration for that carrier. Under the current shared-corpus default topology, the resolved carrier is `<recovery_root>/recon/artifact-roles.yaml`.

Draft status is allowed, but draft status does not waive minimum entry shape. A draft inventory item may carry provisional wording or provisional classification, yet it must still preserve stable identity, explicit scope classification, mutation authority, mutation conditions, and violation effect.

Omission semantics are strict. Omission means a surface is not yet inventoried. Omission must not be read as `out-of-scope` or `not-boundary-bearing`. If a currently known surface is intentionally out of scope or intentionally classified as not boundary-bearing, the inventory must say so explicitly through `scope_classification` rather than by silence. A currently known boundary-bearing surface must not be omitted.

Each structured inventory item must preserve at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `inventory_item_ref` | required | always | invalid inventory item | not allowed |
| `surface` | required | always | invalid inventory item | not allowed |
| `role` | required | always | invalid inventory item | not allowed |
| `scope_classification` | required | always | invalid inventory item | not allowed |
| `protected_surface` | optional-with-default | always | omission means the surface is not declared protected | explicit `false` means the default was chosen deliberately |
| `protection_note` | conditional | required when `protected_surface = true` | omission means no protected-surface claim is being made | empty value is not allowed when present |
| `mutation_authority` | required | always | invalid inventory item | not allowed |
| `mutation_conditions` | required | always | invalid inventory item | not allowed |
| `violation_effect` | required | always | invalid inventory item | not allowed |

`Scope_classification` uses the closed vocabulary declared in `SPEC-SHR-002 / Vocabulary Declarations`. `Protected_surface = true` is the explicit declaration form for protected-surface treatment; leaving the field absent or `false` does not waive the duty to preserve mutation authority for every item.

The artifact-role inventory is the authoritative source of global surface classification for the recovery effort. Slice-local `artifact_roles` records in the Specification Plan may narrow the surface for the current slice, but they must not silently contradict the inventory item they cite. When a new boundary-bearing surface is discovered after the inventory exists, the recovery coordinator must add the new inventory item before or in the same change that advances the first slice depending materially on that surface.

## Repository-surface-family record for repository-shaped targets

For the workflow requirement to classify major repository surface families explicitly rather than leaving them to silence, use `SPEC-SHR-002 / Recovery Inputs and Artifact Roles / Repository-surface-family record for repository-shaped targets`. This section owns the row-complete schema, field minima, omission semantics, linkage rules, and closure-linkage minima for the repository-surface-family record.

When the target is repository-shaped, the entry record must also carry a row-complete `repository_surface_families` record. This record is part of the artifact-role inventory carrier for repository-shaped recovery. It is not a second top-level governed artifact family.

The corpus-default repository surface-family set is closed and exhaustive for the shared default.

| `surface_family` | Meaning |
|---|---|
| `build_graph_and_packaging_boundary` | build graph, packaging boundary, release artifact shape, and caller-visible package identity |
| `deployment_unit_and_release_topology` | deployment-unit boundary, release topology, and materially caller-visible deployment shape |
| `configuration_plane_and_default_source` | configuration inputs, precedence, default source, and operator-facing default behavior |
| `generated_artifact_or_codegen_boundary` | generated artifacts, code-generation seams, regeneration invariants, and generated-surface contracts |
| `extension_plugin_or_hook_surface` | extension points, plugin boundaries, hook surfaces, and registration or invocation semantics |
| `data_store_schema_and_migration_seam` | data-store schema, migration seam, durable storage contract, and migration compatibility boundary |
| `service_topology_and_cross_service_coordination_seam` | cross-service coordination, queue or event seams, retry or status translation, and multi-service boundary behavior |

Each repository-surface-family row must preserve at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `surface_family` | required | always | invalid repository-surface-family row | not allowed |
| `assessment` | required | always | invalid repository-surface-family row | not allowed |
| `inventory_item_refs` | conditional | required when `assessment` begins with `present-` | omission means the family is not being asserted present | explicit `[]` is not allowed when present |
| `assessment_note` | conditional | required when the assessment is not self-explanatory or when promotion later depends on the bounding judgment | omission means no additional explanation is required | empty value is not allowed when present |
| `closure_basis_refs` | optional-with-default | always | omission means reviewable promotion-bearing closure has not yet been linked for this family | explicit `[]` means the family row was reviewed but no closure basis ref has yet been attached; this is invalid when promotion-bearing closure is being claimed |

`Assessment` uses the closed vocabulary declared in `SPEC-SHR-002 / Vocabulary Declarations`.

- `present-in-scope` means the family is present and materially in scope for the declared recovery objective.
- `present-out-of-scope` means the family is present but intentionally excluded from the declared scope.
- `present-not-boundary-bearing` means the family is present but currently judged non-contract-bearing for the declared objective.
- `not-present` means the family was checked and is not present in the target.
- `inspection-pending` means the family is suspected, partially seen, or not yet bounded enough for the stronger judgments above.

The repository-surface-family record is row-complete by default. Omission of any family row is invalid for a repository-shaped target. `Inspection-pending` is allowed at entry, but any family that materially intersects promoted scope must not remain `inspection-pending` at promotion time. Control-plane prose, fixed boundary artifacts, and the ordinary artifact-role inventory still govern clause-bearing surfaces directly; the repository-surface-family record exists to prevent major repository surface families from disappearing by silence.

### Family-specific closure facts and use of `closure_basis_refs`

The row itself remains the family-classification surface. Do not duplicate full recovered semantics inline in `assessment_note`. Instead, use `closure_basis_refs` to point to the governed records that collectively close the minimum facts below when the family is `present-in-scope` and materially intersects promoted scope.

`Closure_basis_refs` may point to stable governed refs such as plan entries, contracts, validation reports, evidence bundles, packets, dispositions, or promoted-unit trace rows. During reconnaissance and early slice work, omission or explicit `[]` means the family has not yet reached reviewable promotion-bearing closure. Before promotion-bearing review, a `present-in-scope` family that materially intersects promoted scope must carry non-empty `closure_basis_refs` whose linked records collectively close the minimum facts below.

| `surface_family` | Minimum facts that `closure_basis_refs` must collectively close when the family is `present-in-scope` and materially intersects promoted scope |
|---|---|
| `build_graph_and_packaging_boundary` | authoritative build inputs or roots, caller-visible package or release artifact identity, versioning or packaging boundary, and any regeneration or reproducibility invariant that is load-bearing |
| `deployment_unit_and_release_topology` | deployed unit or release-family boundary, materially caller-visible topology split or variant boundary, rollout or rollback invariant when load-bearing, and caller-visible divergence between variants when such divergence exists |
| `configuration_plane_and_default_source` | authoritative configuration inputs, precedence order, default source, and omitted-value behavior |
| `generated_artifact_or_codegen_boundary` | authoritative generator input surface, generated output contract, regeneration invariant, and downstream dependency shape |
| `extension_plugin_or_hook_surface` | registration surface, invocation or ordering rule, failure or isolation rule, and compatibility boundary for supported extensions |
| `data_store_schema_and_migration_seam` | authoritative schema surface, migration direction or invariant, compatibility or rollback rule, and operator-visible durable-state consequence when such consequence is in scope |
| `service_topology_and_cross_service_coordination_seam` | participating seam, retry rule, deduplication or idempotence rule, status or error translation rule, and any operator-visible coordination invariant that affects the declared scope |

A reviewer should be able to answer two questions from the row plus its linked closure basis: where the family was closed, and which minimum facts are already explicit. If those answers still depend on silence or on narrative plausibility alone, the family is not yet ready for promotion-bearing review.

### Worked closure example (Non-Normative)

The partial snippet below illustrates completed closure linkage for three high-risk families.

```yaml
repository_surface_families:
  - surface_family: configuration_plane_and_default_source
    assessment: present-in-scope
    inventory_item_refs:
      - ARI-0021
      - ARI-0022
    assessment_note: default source and precedence are contract-bearing for operator-visible startup behavior
    closure_basis_refs:
      - plan:config-defaults
      - slice:config-default-precedence
      - EB-0042:chain:config-default-source
  - surface_family: generated_artifact_or_codegen_boundary
    assessment: present-in-scope
    inventory_item_refs:
      - ARI-0031
    assessment_note: downstream consumers compile generated client stubs directly
    closure_basis_refs:
      - slice:generated-client-surface
      - VR-0019
      - EB-0051:chain:generated-client-regeneration
  - surface_family: service_topology_and_cross_service_coordination_seam
    assessment: present-in-scope
    inventory_item_refs:
      - ARI-0040
      - ARI-0044
    assessment_note: queue retry and status translation are caller-visible through the job-status surface
    closure_basis_refs:
      - slice:job-status-translation
      - ARP-0007
      - AD-0007
```

This example is illustrative only. The governing rule is the row-complete schema plus the closure-linkage rule above.

## Default slice artifact continuity and assembly

Workflow meaning for when each governed artifact family is required remains in `SPEC-SHR-002 / Governed Working Artifacts`. This section owns the canonical continuity keys, default assembly rules, and route-family minima for the governed artifact chain.


This subsection is the authoritative continuity and assembly view for the ordinary reverse-recovery artifact chain. It states when each governed artifact family is required, which continuity keys connect them, and which routes require new artifacts rather than in-place updates. It does not restate the field-minima tables.

| Artifact family | When it is required | Continuity keys or reference surface | Assembly note |
|---|---|---|---|
| Specification Plan entry | before the first Slice Contract for a slice and throughout active recovery | `slice_id`; `current_step_id` when step decomposition is active | The plan is the queue and continuity spine. |
| Slice Contract | before nontrivial critique, validation, or non-executable closure on a slice | `slice_id`; `contract_id`; optional `locator` or default carrier coordinate | The current contract is the slice-local validation contract. |
| Evidence Bundle | when new or reused decisive witnesses or chain-capable evidence records are recorded at bundle scope | `bundle_id`; witness `ref` values; `chain_ref` values | Evidence Bundles may be created before witness capture or chain normalization completes, but decisive clause provenance must resolve through stable chain-capable records before promotion. |
| Validation Report | after executable validation and after any non-executable closure | `slice_id`; `contract_ref` | The report closes the active contract attempt. |
| Critique Report | when objections remain after critique | `artifact_ref` | A critique blocks advancement until revision or authority disposition. |
| Authority Review Packet | when authority review begins or is refreshed | `packet_ref` | The packet is the bounded review surface, not a duplicate corpus. |
| Authority Disposition | when authority closes a normative question or materially alters contractual force | `artifact_ref`; `packet_ref` when packet-backed | A disposition records the decision that closes the authority-owned question. |
| Recovery Snapshot | when work pauses, is handed off, or loses authority availability before promotion | `snapshot_ref`; `plan_ref` | A snapshot records resumable state rather than promotion. |

The default continuity rules are:

1. `slice_id` ties the Specification Plan entry, the Slice Contract, the Validation Report, and any slice-scoped packet or disposition surface together.
2. `contract_ref` always targets `Slice Contract.contract_id`, not a path or session-local coordinate.
3. `basis_refs`, `witness_refs`, packet refs, and snapshot refs must use stable governed references already carried by the recovery corpus.
4. Moving an artifact to a new carrier coordinate does not, by itself, create a new governed artifact when the governed artifact identity stays fixed.
5. Promotion reuses this continuity chain. It does not invent a separate ad hoc handoff bundle outside the governed artifacts already defined here.

The default route families are:

| Route family | Minimum governed path |
|---|---|
| straight-line slice closure | Plan entry -> Slice Contract -> Validation Report -> plan update |
| critique-driven revision | Plan entry -> Slice Contract -> Critique Report -> plan update -> revised Slice Contract |
| authority-routed closure | Plan entry -> Slice Contract and/or Validation Report -> Authority Review Packet -> Authority Disposition -> plan update |
| paused or handed-off work | current governed artifacts -> Recovery Snapshot -> later plan refresh and revalidation as needed |

## Governed artifact identity, provenance, and attempt metadata

This section owns governed artifact identity, provenance-coordinate, and attempt-metadata rules for the governed artifact schemas used by reverse recovery.


This section is the authoritative definition site for `governed artifact identity`, `provenance coordinate`, and `attempt metadata`.

A governed artifact identity names the logical governed artifact across relocation, compaction, retry, replay, revalidation, or storage migration. It is the continuity key for supersession, conflict detection, and stable cross-artifact reference.

A provenance coordinate identifies where a current artifact instance may be retrieved, inspected, or replayed. A provenance coordinate may change while the governed artifact identity stays fixed.

Every governed artifact must have a reviewable provenance coordinate. A field-local schema may carry that coordinate explicitly through a `locator` field, or may define the authoritative current carrier coordinate of the artifact inside the governing workspace as the default provenance coordinate when `locator` is omitted.

Attempt metadata records execution-local, review-local, or storage-local facts about one production or handling episode. Examples include timestamps, temporary paths, session-local cursors, reviewer identities, validator versions, or tool-run identifiers. Attempt metadata is volatile unless a higher-authority domain rule explicitly elevates part of it into stable identity semantics.

These three facts must not be collapsed. Continuity, supersession, `contract_ref`, `artifact_ref`, `packet_ref`, and `snapshot_ref` operate on governed artifact identity rather than on provenance coordinates or attempt metadata. A relocation, retranscription, or retry that preserves governed artifact identity and content after excluding declared attempt metadata is not, by itself, a new governed artifact.

When a governed artifact identity or comparison digest is derived from structured content, the consuming domain must define the canonicalization rule, the excluded volatile fields, and the versioning rule explicitly. Implementers must not infer those rules ad hoc from current tooling or current storage layout.

This document introduces explicit governed artifact identities for the Slice Contract and for lighter artifacts that later review must cite stably, such as Authority Review Packets and Recovery Snapshots. It does not require every governed artifact class to carry a standalone identifier when stable reference can be supplied by an enclosing namespace or another already-governed reference form.

## Canonical schema status legend

This legend is authoritative for field presence, conditionality, and omitted-field semantics within this schema companion.


The base-artifact tables below are authoritative for field presence and absent-field semantics.

| Status | Meaning |
|---|---|
| `required` | The field must be present. Omission makes the artifact invalid. |
| `required-with-empty-allowed` | The field must be present. An explicit empty value has governed meaning. |
| `conditional` | The field must be present when the stated condition holds and must be omitted otherwise. |
| `optional-with-default` | The field may be omitted; omission applies the stated default meaning. |

Unless a local section says otherwise, additional local fields are allowed on governed working artifacts. They must not change the meaning of defined fields, satisfy a required field by implication, or silently alter stated omission semantics. Tooling that implements only this specification must ignore unknown local fields after it has validated the fields defined here.

## Compatibility and migration note for schema companion versions 0.11.0 through 0.13.0

Schema companion version `0.11.0` externalized governed-artifact schema authority from inline ownership in `SPEC-SHR-002@0.10.0` into this document. Schema companion version `0.12.0` preserved that owner split and added promotion-bearing repository-surface-family closure linkage through `closure_basis_refs`. Schema companion version `0.13.0` preserves the field-minimum tables from `0.12.0` and adds a worked-example fidelity rule together with repaired full examples.

This version line is structural first. Unless another adopted specification or consuming-domain override says otherwise, workflow meaning remains in `SPEC-SHR-002`, field minima remain here, and older draft inventory rows remain readable when they omit `closure_basis_refs`. The omission default means reviewable promotion-bearing family closure has not yet been linked.

A workspace that adopts `SPEC-SHR-002@0.13.0` SHOULD also adopt `SPEC-SHR-003@0.7.0` or a later compatible topology specification so that the effective schema companion can be resolved explicitly or by the paired companion default.

Use the migration table below when normalizing a workspace from inline schema ownership, from pre-closure-linkage repository-family rows, or from copied full examples that no longer satisfy the current field tables, to the current companion.

| Legacy authority surface | New authority surface | Required action before further advancement under the current line |
|---|---|---|
| `SPEC-SHR-002@0.10.0` inline artifact-role inventory minima | `SPEC-SHR-004 / Artifact-role inventory minimum shape and linkage` | future advancing work under the split release must resolve the effective schema companion and treat this document as the canonical owner of inventory minima |
| `SPEC-SHR-002@0.10.0` inline repository-surface-family minima | `SPEC-SHR-004 / Repository-surface-family record for repository-shaped targets` | repository-shaped workspaces must validate row completeness against this document before further advancement, revalidation, or promotion-bearing review |
| `SPEC-SHR-004@0.11.0` repository-surface-family rows without `closure_basis_refs` | `SPEC-SHR-004@0.13.0` repository-surface-family rows with closure linkage when promotion-bearing closure is claimed | older draft rows remain readable unchanged; add non-empty `closure_basis_refs` before promotion-bearing review for any `present-in-scope` family materially intersecting promoted scope |
| full copied examples or local templates derived from `SPEC-SHR-004@0.12.0` worked appendices | `SPEC-SHR-004@0.13.0` full worked examples and partial-snippet rule | refresh every copied full example so required fields are present, or relabel the copied material as a partial snippet that does not claim full artifact shape |
| `SPEC-SHR-002@0.10.0` inline plan, contract, and validation-report tables | `SPEC-SHR-004 / Specification Plan`, `Slice Contract`, and `Validation Report` | current advancing artifacts must conform to the resolved schema companion rather than to historical inline copies |
| `SPEC-SHR-002@0.10.0` inline packet, disposition, evidence-bundle, and snapshot tables | `SPEC-SHR-004 / Other governed artifacts` and `Recovery snapshots and incremental resumption` | packet-backed review, authority closure, evidence-bundle carriage, and snapshot refresh must use the schema companion as the authoritative owner of field minima |
| `SPEC-SHR-002@0.10.0` inline overlay matrix and worked appendices | `SPEC-SHR-004 / Overlays` and Appendices A through C | overlay-bearing work and example-driven local templates must route here rather than to the historical inline site |

Historical artifacts may remain in the corpus for provenance. They do not become invalid merely because this schema companion exists. The question is conformance for future advancing work under the adopted workflow and topology version line.

## Specification Plan

For workflow meaning of the Specification Plan, use `SPEC-SHR-002 / Governed Working Artifacts / Specification Plan`. This section owns the canonical field-minimum table, field-local default semantics, linkage rules, and step-list record minima for the Specification Plan.


The Specification Plan is the queue and state record of unresolved slices.

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `slice_id` | required | always | invalid artifact | not allowed |
| `slice_summary` | required | always | invalid artifact | not allowed |
| `recovery_objective` | required | always | invalid artifact | not allowed |
| `compatibility_target` | required | always | invalid artifact | not allowed |
| `target_anchor` | required | always | invalid artifact | not allowed |
| `completeness_dimensions` | required | always | invalid artifact | not allowed |
| `artifact_roles` | required | always | invalid artifact | not allowed |
| `candidate_clause` | required | always | invalid artifact | not allowed |
| `evidence_chain_refs` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no chain-capable evidence references have been attached yet and evidence-chain duties remain open |
| `support_level` | required | always | invalid artifact | not allowed |
| `workflow_state` | required | always | invalid artifact | not allowed |
| `disposition` | required | always | invalid artifact | not allowed |
| `revalidation_required` | optional-with-default | always | omission means `false` | explicit `false` means no revalidation is currently required |
| `ambiguity_status` | required | always | invalid artifact | not allowed |
| `ambiguity_note` | conditional | required when `ambiguity_status` is `known-open`, `bounded-by-scope`, or `unreviewed` | omission is allowed only when `ambiguity_status` is `none-known` | empty string is not allowed |
| `risk_impact` | required | always | invalid artifact | not allowed |
| `regime_or_environment_status` | required | always | invalid artifact | not allowed |
| `regime_or_environment_note` | conditional | required when `regime_or_environment_status` is `captured`, `capture-pending`, or `unknown` | omission is allowed only when `regime_or_environment_status` is `not-material` | empty string is not allowed |
| `negative_evidence_status` | required | always | invalid artifact | not allowed |
| `negative_evidence_refs` | conditional | required when `negative_evidence_status` is `found` | omission means either `not-searched` or `searched-none-found` as declared by `negative_evidence_status` | explicit `[]` is not allowed when present |
| `next_corrective_action` | required | always | invalid artifact | use an explicit sentinel such as `none; slice closed` rather than an empty value |
| `stop_condition` | required | always | invalid artifact | not allowed |
| `authority_disposition_needed` | required | always | invalid artifact | not allowed |
| `active_overlays` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no overlays are active |
| `step_list` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no decomposed iterative steps are currently recorded |
| `current_step_id` | optional-with-default | always | omission means no decomposed step is currently selected as the governing next move | explicit `null` means no current step is selected even though step records are preserved |

`target_anchor` uses the normalized form and comparison rules in `SPEC-SHR-002 / Workflow Overview / Entry minima before first Slice Contract`. A material anchor change must be recorded explicitly rather than inferred from later witnesses.

The artifact-role inventory defined in Recovery Inputs and Artifact Roles / Artifact-role inventory minimum shape and linkage is the authoritative source of global surface classification for the recovery effort.

Each `artifact_roles` entry must name at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `inventory_item_ref` | required | always | invalid artifact-role entry | not allowed |
| `surface` | required | always | invalid artifact-role entry | not allowed |
| `role` | required | always | invalid artifact-role entry | not allowed |
| `mutation_authority` | required | always | invalid artifact-role entry | not allowed |
| `mutation_conditions` | required | always | invalid artifact-role entry | not allowed |
| `violation_effect` | required | always | invalid artifact-role entry | not allowed |

`Inventory_item_ref` must resolve to exactly one item in the authoritative artifact-role inventory. The plan-local `surface` may equal or narrow the inventoried surface for slice purposes. It must not broaden or silently contradict the inventoried surface.

Every ref in `evidence_chain_refs` must resolve to a stable `chain_ref` carried by a chain-capable carrier as defined in `SPEC-SHR-002 / Evidence and Black-Box Discipline / Evidence chains`. Under the default and binding carrier rule in this document, that means `chain_ref` values recorded in `Evidence Bundle.chains`. Multiple entries in `evidence_chain_refs` are conjunctive and ordered as listed.

If `current_step_id` is present, it must name exactly one `step_id` in the local `step_list`. If `step_list` is `[]`, `current_step_id` must be omitted.

Each `step_list` entry must name at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `step_id` | required | always | invalid step record | not allowed |
| `description` | required | always | invalid step record | not allowed |
| `proposed_action` | required | always | invalid step record | explicit `TBD` means the action has not yet been designed |
| `status` | required | always | invalid step record | not allowed |
| `basis_refs` | required-with-empty-allowed | always | invalid step record | explicit `[]` means no stable governed basis ref has been attached yet |
| `superseded_by_step_id` | conditional | required when `status` is `superseded` | omission means the step was not superseded by a named successor | empty value is not allowed when present |
| `result_note` | required-with-empty-allowed | always | invalid step record | explicit `null` means no result is available yet |

The closed `step_list.status` vocabulary is declared in `SPEC-SHR-002 / Vocabulary Declarations`. `Discarded` means abandoned without a named replacement. `Superseded` means replaced by a named successor step and therefore requires `superseded_by_step_id`.

`Basis_refs` must use stable governed references already defined elsewhere in the recovery corpus. They may point to witnesses, reports, packets, memory items, or other governed artifacts. They are not limited to Evidence Bundles.

`Result_note` may remain `null` only while the step has no outcome yet, which is ordinarily when `status` is `planned` or `in-progress`. A step whose `status` is `done`, `blocked`, `discarded`, or `superseded` must carry a non-empty `result_note` explaining the outcome.

After each execution or review event, the just-run step must be updated from evidence. When a corrective successor replaces rather than merely follows an earlier path, the replaced step must be marked `superseded`, linked through `superseded_by_step_id`, and the plan's `current_step_id` must move to the successor.

## Slice Contract

For workflow meaning of the Slice Contract and for the semantic definitions of `discriminating question`, `discriminating evidence`, and `decisive witness`, use `SPEC-SHR-002 / Governed Working Artifacts / Slice Contract`. This section owns the canonical field-minimum table, executable-action minima, non-executable-resolution minima, and field-local omission semantics for the Slice Contract.


This section is the authoritative definition site for `discriminating_question`, `discriminating evidence`, and `decisive witness`.

The Slice Contract is the local validation contract and local Definition of Done for the next recovery action on a slice. It bridges a high-level objective and a checkable action.

The discriminating question is the specific question whose answer would materially change the current clause by confirming it, narrowing it, splitting it, or refuting it.

Evidence is discriminating when it can separate the current candidate clause from at least one live alternative explanation or nearby clause.

A witness or witness set is decisive when it answers the active discriminating question strongly enough to confirm, narrow, split, or refute the candidate clause.

Each Slice Contract must carry `contract_id`. `contract_id` is the governed artifact identity of the contract as defined in Governed artifact identity, provenance, and attempt metadata. It must remain stable across relocation, retranscription, or storage changes to the same contract artifact and must not encode transient attempt metadata.

A Slice Contract may also carry `locator` as its explicit provenance coordinate. When `locator` is omitted, the contract's authoritative current carrier coordinate inside the governing recovery workspace is the provenance coordinate by default.

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `slice_id` | required | always | invalid artifact | not allowed |
| `contract_id` | required | always | invalid artifact | not allowed |
| `locator` | optional-with-default | always | omission means the contract's authoritative current carrier coordinate inside the governing recovery workspace is its provenance coordinate | explicit path means the provenance coordinate is recorded directly |
| `recovery_objective` | required | always | invalid artifact | not allowed |
| `compatibility_target` | required | always | invalid artifact | not allowed |
| `target_anchor` | required | always | invalid artifact | not allowed |
| `candidate_clause` | required | always | invalid artifact | not allowed |
| `discriminating_question` | required | always | invalid artifact | not allowed |
| `required_criteria` | required | always | invalid artifact | not allowed |
| `validation_mode` | required | always | invalid artifact | not allowed |
| `execution_preconditions` | conditional | required when `validation_mode` is `executable` | omission means the slice is not closing through executable validation | explicit `[]` means there are no additional executable preconditions beyond the contract itself |
| `environment_assumptions` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no additional environment assumptions are being claimed |
| `memory_refs_consulted` | conditional | required when `SPEC-SHR-002 / Recovery Memory and Negative Evidence / Memory consultation protocol` says consultation must be recorded | omission means the contract did not meet the consultation-record trigger | explicit `[]` means consultation occurred and no relevant recovery-memory item applied |
| `mutation_policy` | required | always | invalid artifact | not allowed |
| `reset_method` | conditional | required when `validation_mode` is `executable` | omission means the slice is not closing through executable validation | explicit `none` means reset is unnecessary because the executable action is demonstrably non-mutating and non-stateful |
| `expected_observables` | conditional | required when `validation_mode` is `executable` | omission means the slice is not closing through executable validation | explicit `[]` is not allowed when execution is claimed |
| `witness_capture_path` | required | always | invalid artifact | explicit `[]` means no new witness artifacts are expected and only existing cited witnesses are being reused |
| `corrective_next_action` | required | always | invalid artifact | use an explicit sentinel such as `none; contract closes on current evidence` rather than an empty value |
| `active_overlays` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no overlays are active |
| `executable_action` | conditional | required when `validation_mode` is `executable` | omission means the slice is not closing through executable validation | not allowed when present |
| `non_executable_resolution` | conditional | required when `validation_mode` is `non-executable` | omission means the slice is not closing through non-executable resolution | not allowed when present |
| `cross_overlay_dependencies` | conditional | required when more than one overlay is active | omission means no cross-overlay dependency is claimed because at most one overlay is active | explicit `[]` means overlays are active together but have no declared dependencies beyond conjunctive satisfaction |
| `framing_ablation` | conditional | required when `SPEC-SHR-002 / Validation / Framing-ablation` says semantics-first ablation is required | omission means framing-ablation is not required for this slice | not allowed when present |

`Target_anchor` uses the normalized form and comparison rules in `SPEC-SHR-002 / Workflow Overview / Entry minima before first Slice Contract`. A Slice Contract must not mix decisive witnesses from materially different or non-comparable anchors under one unrevised contract.

When `locator` is present, it must use a stable reviewable provenance coordinate rather than transient attempt metadata.

Each `required_criteria` entry must name at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `criterion_id` | required | always | invalid criterion record | not allowed |
| `description` | required | always | invalid criterion record | not allowed |
| `observable_or_witness_class` | required | always | invalid criterion record | not allowed |
| `success_threshold` | required | always | invalid criterion record | not allowed |
| `refuting_condition` | required | always | invalid criterion record | not allowed |

When `validation_mode` is `executable`, the `executable_action` record must name at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `action_kind` | required | always | invalid executable-action record | not allowed |
| `execution_harness` | required | always | invalid executable-action record | not allowed |
| `entrypoint` | conditional | required when the action invokes a stable executable, script, module, or harness target rather than inline payload alone | omission means the inline payload alone is the executable instruction surface | empty value is not allowed when present |
| `argument_vector` | conditional | required when `entrypoint` is present | omission is invalid when `entrypoint` is present | explicit `[]` means the entrypoint receives no additional arguments |
| `working_directory` | optional-with-default | always | omission means the harness default working directory for the declared validation environment | explicit path means a narrower working directory was chosen deliberately |
| `input_refs` | required-with-empty-allowed | always | invalid executable-action record | explicit `[]` means no additional named input artifact is required beyond the contract, environment capture, and already-cited witnesses |
| `runnable_payload` | required | always | invalid executable-action record | the payload must be machine-actionable for the named harness; narrative prose is not allowed |
| `intent` | required | always | invalid executable-action record | not allowed |
| `expected_observable` | required | always | invalid executable-action record | not allowed |
| `declared_assumptions` | required-with-empty-allowed | always | invalid executable-action record | explicit `[]` means the executable action claims no assumptions beyond the parent contract |

`Execution_harness` names a capability class rather than a branded tool. `EntryPoint`, `working_directory`, and `input_refs` must use stable reviewable coordinates and must not encode secrets, credentials, temporary handles, or session-local state. When `entrypoint` is present, `argument_vector` preserves argument order exactly as executed.

When `validation_mode` is `non-executable`, the `non_executable_resolution` record must name at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `reason` | required | always | invalid non-executable-resolution record | not allowed |
| `decisive_basis` | required | always | invalid non-executable-resolution record | not allowed |
| `closure_rule` | required | always | invalid non-executable-resolution record | not allowed |

## Validation Report

For workflow meaning of the Validation Report and for criterion-closure, validator, and revalidation semantics, use `SPEC-SHR-002 / Governed Working Artifacts / Validation Report` and `SPEC-SHR-002 / Validation`. This section owns the canonical field-minimum table, criterion-result minima, interpretation minima, validator-record minima, and field-local omission semantics for the Validation Report.


Every executed Slice Contract must emit a Validation Report. A Slice Contract that closes through `validation_mode = non-executable` must also emit a Validation Report recording the decisive non-executable basis. This report is criterion-local and expectation-relative. It is not replaced by a broad narrative that the system "basically worked."

The `contract_ref` field must reference `Slice Contract.contract_id` as defined in Governed artifact identity, provenance, and attempt metadata. It must not be a path, filename, or session-local cursor.

The Validation Report is interpreted against the `target_anchor` declared by the referenced Slice Contract. It does not introduce a second top-level anchor field by default. If target-anchor comparison returns `materially-different` or `non-comparable`, issue a new Slice Contract or explicitly revalidate the existing one under the new anchor.

For a single assembled view of the full Validation Report surface, including validator fields, framing-ablation fields, and overlay extensions, see Validation Report assembly view (Non-Normative) immediately below. The field tables and overlay matrix remain authoritative.

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `slice_id` | required | always | invalid artifact | not allowed |
| `contract_ref` | required | always | invalid artifact | not allowed |
| `witness_refs` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means the report relies only on already-cited contract-local witness references and introduces no new top-level witness list |
| `criterion_results` | required | always | invalid artifact | not allowed |
| `interpretation` | required | always | invalid artifact | not allowed |
| `active_overlays` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no overlays are active |
| `cross_overlay_findings` | conditional | required when more than one overlay is active | omission means no cross-overlay finding is required because at most one overlay is active | explicit `[]` means multiple overlays were active but no additional cross-overlay finding arose beyond the base criteria |
| `framing_ablation_result` | conditional | required when the Slice Contract carried `framing_ablation` | omission means framing-ablation was not required for this slice | not allowed when present |
| `validator_record` | conditional | required when a validator was used | omission means no validator was used | not allowed when present |

At report scope, `witness_refs` is the report-level witness list. It does not replace criterion-local decisive-basis recording.

Each `criterion_results` entry must name at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `criterion_id` | required | always | invalid criterion-result record | not allowed |
| `witness_ref` | required-with-empty-allowed | always | invalid criterion-result record | explicit `none` means the criterion remained blocked or untested without a decisive witness |
| `witness_refs` | conditional | required when more than one decisive witness materially supports the criterion | omission means `witness_ref` is the sole decisive witness, or no decisive witness exists because the criterion remained blocked or untested | explicit `[]` is not allowed when present |
| `outcome` | required | always | invalid criterion-result record | not allowed |
| `observed_divergence` | required | always | invalid criterion-result record | explicit `none` means no divergence was observed |
| `corrective_next_action` | required | always | invalid criterion-result record | explicit `none` means no corrective action is required for that criterion |

Each `criterion_results.witness_ref` names the primary decisive witness for that criterion. When a decisive witness set exists, `criterion_results.witness_refs` must enumerate the full set and the primary witness must be selected according to Other governed artifacts / Witness locator stability and decisive-witness selection.

The required `interpretation` record must name at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `action_reference` | required | always | invalid interpretation record | use `none; non-executable closure` when `validation_mode` is `non-executable` |
| `expected_outcome` | required | always | invalid interpretation record | not allowed |
| `observed_outcome` | required | always | invalid interpretation record | not allowed |
| `expectation_delta` | required | always | invalid interpretation record | explicit `none` means expectation and observation matched |
| `contractual_significance` | required | always | invalid interpretation record | not allowed |
| `next_move` | required | always | invalid interpretation record | explicit `none; slice closed` means no further move is required |

When a validator was used, the `validator_record` must name at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `validator_mode` | required | always | invalid validator record | not allowed |
| `validator_outcome` | required | always | invalid validator record | not allowed |
| `validator_ref` | required | always | invalid validator record | not allowed |
| `validator_version` | required | always | invalid validator record | not allowed |
| `known_limitations` | required-with-empty-allowed | always | invalid validator record | explicit `[]` means no additional known limitations were recorded beyond the declared validator mode |

### Validation Report assembly view (Non-Normative)

This subsection assembles the full Validation Report surface in one place. It is derivative. If a row here and an authoritative field table or overlay row differ, the authoritative field table or overlay row controls. A revision that changes the Validation Report surface must update the authoritative site first and this assembly view in the same change set.

| Path | When present | Governing site |
|---|---|---|
| `slice_id` | always | Validation Report field table |
| `contract_ref` | always | Validation Report field table |
| `witness_refs` | always | Validation Report field table |
| `criterion_results` | always | Validation Report field table |
| `criterion_results.*` | for every emitted criterion result | Validation Report criterion-results table |
| `interpretation` | always | Validation Report field table |
| `interpretation.*` | for every emitted interpretation record | Validation Report interpretation table |
| `active_overlays` | always | Validation Report field table |
| `cross_overlay_findings` | when more than one overlay is active | Validation Report field table |
| `framing_ablation_result` | when the Slice Contract carried `framing_ablation` | Validation Report field table and `SPEC-SHR-002 / Validation / Framing-ablation` |
| `framing_ablation_result.*` | when `framing_ablation_result` is present | `SPEC-SHR-002 / Validation / Framing-ablation` |
| `validator_record` | when a validator was used | Validation Report field table |
| `validator_record.*` | when `validator_record` is present | Validation Report validator-record table |
| `concept_overlay.*` | when `concept_overlay` is active | Overlays / Overlay field minima matrix, rows `OV-C09` and `OV-C10` |
| `abstract_witness_overlay.*` | when `abstract_witness_overlay` is active | Overlays / Overlay field minima matrix, rows `OV-A10` through `OV-A12` |
| `lifecycle_overlay.*` | when `lifecycle_overlay` is active | Overlays / Overlay field minima matrix, row `OV-L17` |

## Validation and framing-ablation subrecord minima

Workflow semantics for semantics-first review, framing-ablation triggers, framing-sensitive advancement, and validator interpretation remain in `SPEC-SHR-002 / Validation / Framing-ablation` and `SPEC-SHR-002 / Validation / Criterion status and validator semantics`. This section owns the canonical subrecord minima for `Slice Contract.framing_ablation` and `Validation Report.framing_ablation_result`.

### Slice Contract `framing_ablation`

The `framing_ablation` subrecord must preserve at least the following fields.

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `required` | required | always | invalid framing-ablation record | explicit `true` confirms the ablation pass is mandatory for this slice |
| `ablated_surfaces` | required-with-empty-allowed | always | invalid framing-ablation record | explicit `[]` means the record concludes that no non-binding surface was actually available to ablate after review |
| `fixed_witnesses` | required | always | invalid framing-ablation record | explicit `[]` is not allowed |
| `material_flip_rule` | required | always | invalid framing-ablation record | empty value not allowed |

### Validation Report `framing_ablation_result`

The `framing_ablation_result` subrecord must preserve at least the following fields.

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `ablated_surfaces` | required-with-empty-allowed | always | invalid framing-ablation-result record | explicit `[]` means the ablation requirement was reviewed but no non-binding surface remained to remove |
| `fixed_witnesses` | required | always | invalid framing-ablation-result record | explicit `[]` is not allowed |
| `stability_outcome` | required | always | invalid framing-ablation-result record | empty value not allowed |
| `notes` | required-with-empty-allowed | always | invalid framing-ablation-result record | explicit empty value means there are no additional notes beyond the structured fields |


## Other governed artifacts

For workflow meaning of critique, evidence handling, packet-first review, authority disposition, and decisive-witness selection, use `SPEC-SHR-002 / Governed Working Artifacts`, `SPEC-SHR-002 / `SPEC-SHR-002 / Authority review operating model``, and `SPEC-SHR-002 / Evidence and Black-Box Discipline`. This section owns the canonical field-minimum tables and field-local omission semantics for the lighter governed artifact families.


This section gives canonical field-minima treatment to the lighter governed artifacts used by the workflow. The tables below are authoritative for field presence, conditionality, omission semantics, and empty-value meaning.

### Critique Report

A Critique Report records structured objections against a proposed clause or Slice Contract. `artifact_ref` must be a stable governed-artifact reference as defined in Governed artifact identity, provenance, and attempt metadata. It must not be a path, filename, temporary handle, or session-local cursor.

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `artifact_ref` | required | always | invalid artifact | not allowed |
| `objections` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no remaining objection is being asserted in this field |
| `missing_criteria` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no missing criterion is being asserted in this field |
| `requested_revisions` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no concrete revision is being requested in this field |
| `critic` | required | always | invalid artifact | not allowed |
| `recommended_disposition` | required | always | invalid artifact | not allowed |

At least one of `objections`, `missing_criteria`, or `requested_revisions` must be non-empty on an emitted Critique Report.

### Evidence Bundle

An Evidence Bundle stores the witnesses and chain-capable evidence records referenced by the slice record.

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `bundle_id` | required | always | invalid artifact | not allowed |
| `witnesses` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means the bundle identity was created before witness capture completed |
| `chains` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no chain records are yet carried at bundle scope |
| `environment_notes` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no additional environment note is being asserted at bundle scope |

Each witness record must name at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `ref` | required | always | invalid witness record | not allowed |
| `kind` | required | always | invalid witness record | not allowed |
| `source_role` | required | always | invalid witness record | not allowed |
| `locator` | required | always | invalid witness record | not allowed |
| `note` | required | always | invalid witness record | not allowed |

Each chain record must name at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `chain_ref` | required | always | invalid chain record | not allowed |
| `clause_focus` | required | always | invalid chain record | not allowed |
| `boundary_stimulus_or_precondition` | required | always | invalid chain record | not allowed |
| `implicated_artifact_roles_or_source_classes` | required | always | invalid chain record | explicit `[]` is not allowed |
| `structural_or_documentary_corroboration_refs` | required-with-empty-allowed | always | invalid chain record | explicit `[]` means no structural or documentary corroboration is being asserted |
| `semantic_basis_refs` | conditional | required when the clause depends materially on recovered concepts or structured absences | omission means no additional semantic basis is being asserted beyond the other chain fields | explicit `[]` is not allowed when present |
| `execution_conditions` | required-with-empty-allowed | always | invalid chain record | explicit `[]` means no execution occurred or no additional execution condition is being asserted |
| `runtime_witness_refs` | required-with-empty-allowed | always | invalid chain record | explicit `[]` means no runtime witness is being asserted |
| `observable_effect` | required | always | invalid chain record | not allowed |
| `external_dependence_refs` | required-with-empty-allowed | always | invalid chain record | explicit `[]` means no external dependence evidence is being asserted |
| `current_disposition` | required | always | invalid chain record | not allowed |
| `assertion_basis` | required | always | invalid chain record | not allowed |

`Source_role` names the local source surface. The evidentiary tier used for weighting and decisive-witness selection is the source class defined in `SPEC-SHR-002 / Evidence and Black-Box Discipline / Source hierarchy`. Each witness `ref` must be unique within its containing bundle. Each `chain_ref` must be stable and unique within the recovery corpus. `Specification Plan.evidence_chain_refs` resolves to these `chain_ref` values by default. This document does not treat Validation Reports, Authority Review Packets, or Authority Dispositions as chain-capable carriers by default. When those artifacts summarize chain material, the summary does not replace the controlling chain record.


### Authority Review Packet

An Authority Review Packet records the bounded decision surface presented to the authority reviewer.

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `packet_ref` | required | always | invalid artifact | not allowed |
| `scope` | required | always | invalid artifact | not allowed |
| `recovery_objective` | required | always | invalid artifact | not allowed |
| `compatibility_target` | required | always | invalid artifact | not allowed |
| `review_mode` | optional-with-default | always | omission means `per-slice` | explicit `per-slice` means the default was chosen deliberately |
| `authority_independence` | optional-with-default | always | omission means `independent-reviewer` | explicit `independent-reviewer` means the default was chosen deliberately |
| `local_authority_basis_ref` | conditional | required when `authority_independence = same-operator-explicit-local-authority` | omission means no same-operator local-authority path is being asserted | empty value is not allowed when present |
| `questions_for_authority` | required | always | invalid artifact | explicit `[]` is not allowed |
| `candidate_decisions` | required | always | invalid artifact | explicit `[]` is not allowed |
| `basis_refs` | required | always | invalid artifact | explicit `[]` is not allowed |
| `open_objections` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no unresolved objection remains after packet normalization |
| `compatibility_and_security_impact` | required | always | invalid artifact | not allowed |
| `recommended_disposition` | required | always | invalid artifact | not allowed |
| `follow_up_if_rejected` | required | always | invalid artifact | use an explicit sentinel such as `none; packet closes on approval only` rather than an empty value |
| `promotion_trace_matrix` | conditional | required when `review_mode = promotion-batch` | omission means the packet is not a promotion batch | explicit `[]` is not allowed when present |

When `promotion_trace_matrix` is present, each row must preserve at least:

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `promoted_unit_ref` | required | always | invalid promotion-trace row | not allowed |
| `representation_class` | required | always | invalid promotion-trace row | not allowed |
| `slice_refs` | required | always | invalid promotion-trace row | explicit `[]` is not allowed |
| `basis_refs` | required | always | invalid promotion-trace row | explicit `[]` is not allowed |

`Representation_class` uses the closed vocabulary declared in `SPEC-SHR-002 / Vocabulary Declarations`. Every promoted main contract clause, compatibility clause or appendix unit, explicit exclusion or carve-out, and explicit out-of-scope note must have exactly one promotion-trace row. `Basis_refs` must use stable governed refs only. Raw conversational history is not a valid required basis surface.

### Authority Disposition

An Authority Disposition records the explicit normative decision.

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `artifact_ref` | required | always | invalid artifact | not allowed |
| `decision` | required | always | invalid artifact | not allowed |
| `scope` | required | always | invalid artifact | not allowed |
| `rationale` | required | always | invalid artifact | not allowed |
| `remaining_exceptions` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no remaining exception is being preserved |
| `ratifier` | required | always | invalid artifact | not allowed |
| `packet_ref` | conditional | required when the decision is promotion-bearing or materially alters contractual force | omission means the decision is non-promotion-bearing and non-scope-altering | not allowed when present |
| `basis_refs` | conditional | required when `packet_ref` is required | omission means the decision is non-promotion-bearing and non-scope-altering | explicit `[]` is not allowed when present |
| `review_mode` | conditional | required when `packet_ref` is required | omission means the decision is non-promotion-bearing and non-scope-altering | not allowed when present |
| `normative_effect` | conditional | required when the decision is promotion-bearing or materially alters contractual force | omission means the decision does not alter contractual force beyond bounded review commentary | not allowed when present |
| `follow_up_actions` | conditional | required when `packet_ref` is required | omission means the decision is non-promotion-bearing and non-scope-altering | explicit `[]` means no follow-up action remains after the disposition |

### Witness locator stability and decisive-witness selection

For workflow meaning of witness locator stability, decisive-witness selection, and source-class ranking, use `SPEC-SHR-002 / Governed Working Artifacts / Witness locator stability and decisive-witness selection`. This section is schema-local only.

Any field in this document that carries a witness `locator`, `witness_ref`, or `witness_refs` must preserve stable reviewable coordinates or stable governed refs rather than session-local handles, temporary paths, or prose-only descriptions.

When a criterion closes on more than one decisive witness, `criterion_results.witness_refs` must enumerate the full decisive set and `criterion_results.witness_ref` must name the primary decisive witness selected under the deterministic workflow rule in `SPEC-SHR-002`. This document does not restate that selection rule.

## Recovery snapshots and incremental resumption

For workflow meaning of resumable state, pause and handoff obligations, resume sequencing, and promotion-readiness interpretation, use `SPEC-SHR-002 / Recovery snapshots and incremental resumption`. This section owns the canonical Recovery Snapshot field-minimum table and the field-local omission semantics below. It does not restate the workflow resume procedure.

| Field | Status | Condition | Omission meaning | Empty-value meaning |
|---|---|---|---|---|
| `snapshot_ref` | required | always | invalid artifact | not allowed |
| `scope` | required | always | invalid artifact | not allowed |
| `recovery_objective` | required | always | invalid artifact | not allowed |
| `compatibility_target` | required | always | invalid artifact | not allowed |
| `target_anchor` | required | always | invalid artifact | not allowed |
| `workflow_spec_ref` | optional-with-default | always | omission means the snapshot may predate this field and the historical governing workflow version is not recorded here | explicit value records the workflow version governing the snapshot when it was created or last authoritatively refreshed |
| `promotion_readiness` | optional-with-default | always | omission means `slice-backed` when any slice reference is present and `recon-only` otherwise | explicit `slice-backed` means the default was chosen deliberately |
| `recon_refs` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no reconnaissance artifact is being cited at snapshot scope |
| `plan_ref` | required | always | invalid artifact | not allowed |
| `closed_slice_refs` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no slice is currently closed in this snapshot |
| `bounded_slice_refs` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no slice is currently bounded in this snapshot |
| `open_slice_refs` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no slice is currently open in this snapshot |
| `authority_gaps` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no current authority gap is being recorded |
| `resume_order` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no next governed action has yet been prioritized |
| `memory_refs` | required-with-empty-allowed | always | invalid artifact | explicit `[]` means no recovery-memory item is currently being cited at snapshot scope |
| `stop_reason` | required | always | invalid artifact | not allowed |

`Target_anchor` in a Recovery Snapshot uses the normalized form and comparison rules in `SPEC-SHR-002 / Workflow Overview / Entry minima before first Slice Contract`.

`Workflow_spec_ref`, when present, records the workflow version that governed the snapshot at creation time or last authoritative refresh. The current `SPEC-SHR-003` discovery file continues to name the workflow version that governs future work. The two records must not be conflated.

Promotion readiness uses the closed vocabulary declared in `SPEC-SHR-002 / Vocabulary Declarations`. `Gate-candidate` means the recorded recovery state appears ready for Promotion Gate review but has not yet been ratified.

## Overlays

This section owns overlay-local meaning, overlay field minima, and worked overlay examples for governed artifacts. Workflow choice about whether an overlay is justified in a given slice remains governed by `SPEC-SHR-002 / Overlays`, `SPEC-SHR-002 / Completeness and Criteria`, and `SPEC-SHR-002 / Validation`.


The semantic definitions of the overlays live in the subsection-specific prose below. The authoritative field-minima map for all three overlays lives in the single overlay matrix in this section.

Activating an overlay is a binding commitment to satisfy the corresponding matrix rows across the affected artifacts. Before an overlay is activated, the recovery author must review the relevant matrix rows and confirm that the slice actually needs the added fields. If the added field burden is not yet justified, leave the overlay inactive and keep the slice narrower.

### Concept Recovery and Semantic Bases

This section is the authoritative definition site for `recovered concept` and `concept-bearing slice`.

Some slices are semantically dense enough that raw witnesses do not yet make the candidate clause intelligible or reviewable. This is common in materially code-derived slices, control-plane-heavy systems, lifecycle-dense behavior, and metric-governed logic. In such cases, the working record must recover the intermediate semantics explicitly before asking a reviewer or validator to treat the clause as reviewable.

A recovered concept is a human-legible intermediate semantic property that is more abstract than a raw witness and less normative than a promoted clause. Examples include `allocation occurs`, `operator action is authority-gated`, `duplicate trigger is ignored rather than re-executed`, or `default path is non-mutating`.

A slice whose clause depends materially on one or more such intermediate semantic properties is a concept-bearing slice.

The `concept_overlay` block is optional and slice-scoped. It is a Level 2 overlay, not a fourth recovery level and not a new artifact family.

When concept recovery is active, the plan records the concept vocabulary, semantic basis, coverage note, and any structured absences that were checked. The contract records how concepts are extracted, which witnesses bear them, and any concept probe or perturbation-stability check used to separate concept-local uncertainty from clause-local uncertainty. The report records concept-local outcomes and remaining concept-level coverage limits.

Concept recovery improves clause reviewability. It does not create a separate support ladder. Clauses still advance through the ordinary support vocabulary.

### Abstract Witness Families and Instantiation Preservation

This section is the authoritative definition site for `abstract witness family`.

Some slices are best recovered not as one clause per witness but as a reusable witness family. An abstract witness family is a parameterized witness form whose placeholders, role constraints, consistency rules, and admissible instantiation scope are explicit enough that many concrete witnesses become reviewable as members of one family instead of as unrelated examples.

The `abstract_witness_overlay` block is optional and slice-scoped. It is a Level 2 overlay, not a new recovery level and not a new artifact family.

When an abstract witness family is active, the plan records the family form, instantiation scope, and any admissibility or preservation gaps. The contract records placeholder roles, placeholder constraints, cross-placeholder consistency, admissibility checks, and preservation conditions. The report records the separate judgments for family validity, concrete admissibility, and preservation under concretization.

Abstract-family validation does not automatically transfer to every concrete instance. A concrete witness inherits support from the family only when it satisfies the declared roles, constraints, consistency rules, instantiation scope, and preservation conditions under the captured circumstances.

### Recovered Lifecycle Semantics

This section is the authoritative definition site for `promotion-bearing lifecycle model`, `state_snapshot`, `transition_trace`, and the lifecycle witness-classification literal `derived_summary`.

Some slices are about the lifecycle of an identifiable subject across time rather than about one-shot inputs and outputs. This applies when correctness depends on state identity, transition eligibility, sequencing, restart or resume behavior, refusal behavior, or terminal interpretation. Trivial linear flows do not need lifecycle scaffolding.

The `lifecycle_overlay` block is optional and slice-scoped. It is not a new recovery level and it does not create parallel artifacts.

A lifecycle model uses the closed `model_mode` vocabulary declared in `SPEC-SHR-002 / Vocabulary Declarations`. The authoritative conceptual definitions of those modes are:

- `descriptive`: a planning aid that may summarize observed flow, likely phases, or example sequences, but must not introduce new contractual states, guards, or transitions and must not be the sole basis of promotion
- `promotion-bearing`: a lifecycle model strong enough to support conformance-critical judgments and promotion decisions for a lifecycle-bearing slice

Within lifecycle witness classification, the closed literals have the following meanings:

- `state_snapshot`: a witness showing that a particular lifecycle state held at an inspection point
- `transition_trace`: a witness showing that a specific lifecycle transition occurred under stated conditions
- `derived_summary`: a summary or aggregation derived from stronger lifecycle witnesses; it may corroborate stronger lifecycle witnesses, but it does not outrank them

`derived_summary` in this field-local classification names a lifecycle witness class. It does not relabel every generic derived analysis elsewhere in the document.

When lifecycle recovery is active, the plan records the lifecycle subject, instance key, selected model mode, and any open lifecycle gaps. A `promotion-bearing` contract must record canonical states, events, derivation logic, transition rules, guard precedence, and the relevant observable consequences of each claimed state or transition. The report records lifecycle witness classification entries, each naming the witness, its classification, and the states or transitions it supports.

### Overlay field minima matrix

The overlay prose in the preceding subsections defines overlay semantics and field-local meaning. The matrix below alone is authoritative for overlay field minima, conditionality, and omission semantics. Derived checklists, local templates, and examples are non-normative unless they restate a matrix row by ID without changing meaning.

Legend: `R` = required when the overlay is active; `C` = conditional; `—` = not applicable.

| Row ID | Overlay field | Specification Plan | Slice Contract | Validation Report | Condition | Omission or empty semantics |
|---|---|---|---|---|---|---|
| `OV-C01` | `concept_overlay.concept_vocabulary` | R | R | — | `concept_overlay` active | Omission invalid where marked `R`; empty list is not allowed. |
| `OV-C02` | `concept_overlay.concept_basis` | R | — | — | `concept_overlay` active | Omission invalid; empty value not allowed. |
| `OV-C03` | `concept_overlay.concept_coverage_note` | R | R | — | `concept_overlay` active | Omission invalid where marked `R`; empty value not allowed. |
| `OV-C04` | `concept_overlay.structured_absences_checked` | R | — | — | `concept_overlay` active | Omission invalid; explicit `[]` means no structured absences were confirmed as checked. |
| `OV-C05` | `concept_overlay.concept_extraction_method` | — | R | — | `concept_overlay` active | Omission invalid; empty value not allowed. |
| `OV-C06` | `concept_overlay.concept_witnesses` | — | R | — | `concept_overlay` active | Omission invalid; explicit `[]` is not allowed. |
| `OV-C07` | `concept_overlay.concept_probe` | — | C | — | required when a feasible concept-local probe exists | Omit only when no feasible concept-local probe exists; the feasibility limit must be explained in `concept_coverage_note`. |
| `OV-C08` | `concept_overlay.perturbation_stability_check` | — | C | — | required when a relevant perturbation-stability check exists | Omit only when perturbation stability is not relevant or not feasible; the limit must be explained in `concept_coverage_note`. |
| `OV-C09` | `concept_overlay.concept_criteria_outcomes` | — | — | R | `concept_overlay` active | Omission invalid; explicit `[]` means no concept-local criterion split was feasible and the report must say so in `concept_coverage_findings`. |
| `OV-C10` | `concept_overlay.concept_coverage_findings` | — | — | R | `concept_overlay` active | Omission invalid; empty value not allowed. |
| `OV-A01` | `abstract_witness_overlay.abstract_witness_form` | R | R | — | `abstract_witness_overlay` active | Omission invalid where marked `R`; empty value not allowed. |
| `OV-A02` | `abstract_witness_overlay.instantiation_scope` | R | R | — | `abstract_witness_overlay` active | Omission invalid where marked `R`; empty value not allowed. |
| `OV-A03` | `abstract_witness_overlay.admissibility_gaps` | R | — | — | `abstract_witness_overlay` active | Omission invalid; explicit `[]` means no admissibility gaps are currently known. |
| `OV-A04` | `abstract_witness_overlay.instantiation_preservation_gaps` | R | — | — | `abstract_witness_overlay` active | Omission invalid; explicit `[]` means no preservation gaps are currently known. |
| `OV-A05` | `abstract_witness_overlay.placeholder_roles` | — | R | — | `abstract_witness_overlay` active | Omission invalid; explicit `[]` is not allowed. |
| `OV-A06` | `abstract_witness_overlay.placeholder_constraints` | — | R | — | `abstract_witness_overlay` active | Omission invalid; explicit `[]` means no additional placeholder constraints exist beyond role identity. |
| `OV-A07` | `abstract_witness_overlay.cross_placeholder_consistency` | — | R | — | `abstract_witness_overlay` active | Omission invalid; explicit `[]` means no cross-placeholder consistency rule exists beyond those already stated. |
| `OV-A08` | `abstract_witness_overlay.admissibility_check` | — | R | — | `abstract_witness_overlay` active | Omission invalid; empty value not allowed. |
| `OV-A09` | `abstract_witness_overlay.instantiation_preservation_conditions` | — | R | — | `abstract_witness_overlay` active | Omission invalid; empty value not allowed. |
| `OV-A10` | `abstract_witness_overlay.family_validity` | — | — | R | `abstract_witness_overlay` active | Omission invalid; empty value not allowed. |
| `OV-A11` | `abstract_witness_overlay.concrete_admissibility` | — | — | R | `abstract_witness_overlay` active | Omission invalid; empty value not allowed. |
| `OV-A12` | `abstract_witness_overlay.preservation_under_concretization` | — | — | R | `abstract_witness_overlay` active | Omission invalid; empty value not allowed. |
| `OV-L01` | `lifecycle_overlay.model_mode` | R | R | — | `lifecycle_overlay` active | Omission invalid where marked `R`; empty value not allowed. |
| `OV-L02` | `lifecycle_overlay.lifecycle_subject` | R | R | — | `lifecycle_overlay` active | Omission invalid where marked `R`; empty value not allowed. |
| `OV-L03` | `lifecycle_overlay.instance_key` | R | R | — | `lifecycle_overlay` active | Omission invalid where marked `R`; empty value not allowed. |
| `OV-L04` | `lifecycle_overlay.open_lifecycle_gaps` | R | — | — | `lifecycle_overlay` active | Omission invalid; explicit `[]` means no open lifecycle gaps are currently known. |
| `OV-L05` | `lifecycle_overlay.canonical_state_vocabulary` | — | C | — | required when `model_mode = promotion-bearing` | Omission means the lifecycle model is descriptive only. Empty list is not allowed when present. |
| `OV-L06` | `lifecycle_overlay.observed_to_canonical_mapping` | — | C | — | required when `model_mode = promotion-bearing` and source vocabularies disagree materially | Omission means no material vocabulary disagreement was found. Explicit `{}` means agreement was checked and no mapping was needed. |
| `OV-L07` | `lifecycle_overlay.event_vocabulary` | — | C | — | required when `model_mode = promotion-bearing` | Omission means the lifecycle model is descriptive only. Empty list is not allowed when present. |
| `OV-L08` | `lifecycle_overlay.authoritative_state_surface` | — | C | — | required when `model_mode = promotion-bearing` | Omission means the lifecycle model is descriptive only. Empty value not allowed when present. |
| `OV-L09` | `lifecycle_overlay.state_derivation_rule` | — | C | — | required when `model_mode = promotion-bearing` | Omission means the lifecycle model is descriptive only. Empty value not allowed when present. |
| `OV-L10` | `lifecycle_overlay.transition_rules` | — | C | — | required when `model_mode = promotion-bearing` | Omission means the lifecycle model is descriptive only. Empty list is not allowed when present. |
| `OV-L11` | `lifecycle_overlay.guard_precedence` | — | C | — | required when `model_mode = promotion-bearing` | Omission means the lifecycle model is descriptive only. Explicit `[]` means no multi-guard conflict exists in the declared scope. |
| `OV-L12` | `lifecycle_overlay.boundary_effects_by_transition` | — | C | — | required when `model_mode = promotion-bearing` | Omission means the lifecycle model is descriptive only. Explicit `{}` means no additional boundary effects are claimed beyond state or transition observables. |
| `OV-L13` | `lifecycle_overlay.external_status_mapping` | — | C | — | required when an external status surface exists | Omission means no external status surface exists in scope. Explicit `{}` is not allowed when such a surface exists. |
| `OV-L14` | `lifecycle_overlay.illegal_transition_policy` | — | C | — | required when `model_mode = promotion-bearing` | Omission means the lifecycle model is descriptive only. Empty value not allowed when present. |
| `OV-L15` | `lifecycle_overlay.per_state_observables` | — | C | — | required when `model_mode = promotion-bearing` | Omission means the lifecycle model is descriptive only. Explicit `{}` means no additional per-state observable is claimed beyond the authoritative state surface. |
| `OV-L16` | `lifecycle_overlay.per_transition_observables` | — | C | — | required when `model_mode = promotion-bearing` | Omission means the lifecycle model is descriptive only. Explicit `{}` means no additional per-transition observable is claimed beyond the boundary effect record. |
| `OV-L17` | `lifecycle_overlay.witness_classification` | — | — | R | `lifecycle_overlay` active | Omission invalid; explicit `[]` means no decisive lifecycle witness was obtained and the report must remain bounded accordingly. |

### Overlay audit checklist (Non-Normative)

This checklist is a review aid derived mechanically from the authoritative matrix above. It does not add, remove, or modify any overlay requirement.

- If `concept_overlay` is active, review rows `OV-C01` through `OV-C10`. The plan must satisfy `OV-C01` through `OV-C04`, the contract must satisfy `OV-C01`, `OV-C03`, `OV-C05`, and `OV-C06`, and the report must satisfy `OV-C09` and `OV-C10`, with `OV-C07` and `OV-C08` applied when their stated conditions hold.
- If `abstract_witness_overlay` is active, review rows `OV-A01` through `OV-A12`. The plan must satisfy `OV-A01` through `OV-A04`, the contract must satisfy `OV-A01`, `OV-A02`, and `OV-A05` through `OV-A09`, and the report must satisfy `OV-A10` through `OV-A12`.
- If `lifecycle_overlay` is active, review rows `OV-L01` through `OV-L17`. The plan must satisfy `OV-L01` through `OV-L04`, the contract must satisfy `OV-L01` through `OV-L04` always and `OV-L05` through `OV-L16` when their stated conditions hold, and the report must satisfy `OV-L17`.
- If more than one overlay is active, also verify the base-artifact `cross_overlay_dependencies` and `cross_overlay_findings` requirements in Governed Working Artifacts.

### Overlay Composition

When multiple overlays are active at once, they compose by rule rather than by ad hoc interpretation.

1. Overlays extend the same slice-scoped artifacts. They do not create parallel artifact families.
2. Base criteria and all active overlay criteria are conjunctive unless authority explicitly narrows scope.
3. Any cross-overlay dependency must be declared explicitly in the Slice Contract and reflected in the Validation Report.
4. Absent an explicit dependency, overlays are interpreted independently.

When an abstract witness family is used inside a lifecycle-bearing slice, admissibility is either global to the declared lifecycle scope or state-local or transition-local. The Slice Contract must say which.

## Artifact Schema Definition of Done

A reverse-recovery workspace conforms to this schema companion when all of the following are true.

1. Governed artifact families that this document owns use the canonical field minima, condition rules, omission semantics, and empty-value meanings defined here.
2. Closed and extensible literal declarations imported from `SPEC-SHR-002` are used as declared there rather than being silently redefined here.
3. Any consuming-domain override that changes a schema rule names the governing document id plus the exact section, table, row, and field being replaced, narrowed, or made stricter.
4. Stable governed references, identity rules, provenance-coordinate defaults, and continuity keys are used exactly as defined by this document for the artifact families it governs.
5. Overlay-bearing artifacts satisfy the overlay matrix rows that apply to their active overlays.
6. Worked appendices remain non-normative examples. Full worked examples must still satisfy the active field tables unless they are explicitly labeled partial, and no partial snippet may silently contradict the imported workflow semantics from `SPEC-SHR-002`.

Nothing else is required by this specification.

## Worked examples and partial snippets

The appendices below remain non-normative for behavior, but a worked example that presents a full artifact or a full artifact chain is still expected to satisfy the active field tables in this document. A partial snippet must identify itself as partial and must not silently contradict a required field, a condition rule, or an omission meaning. When a field table changes or a required field was previously omitted, the field table remains authoritative and the affected example should be refreshed in the same change set.

## Appendix A

This appendix is a non-normative example of the base workflow without overlays. The target surface is a machine-parsed CLI summary line.

Non-normative note: this appendix exercises the new base-schema status fields. Because `active_overlays` is `[]`, no overlay blocks appear. Because `negative_evidence_status` is `found`, `negative_evidence_refs` is present. Because `validation_mode` is `executable`, `executable_action` is present and `non_executable_resolution` is omitted by rule.

Non-normative note: this appendix introduces the additional `witness.source_role` literals `consumer_code`, `incident_note`, and `framing_metadata` as appendix-local explicit additions to the extensible vocabulary. In this appendix, `consumer_code` and `incident_note` align to the target-matched corroborant source class, and `framing_metadata` aligns to the framing-and-commentary source class.

### Specification Plan entry

```yaml
slice_id: cli-summary-line
slice_summary: terminal stdout summary line consumed by downstream automation
recovery_objective: backward-compatible reimplementation
compatibility_target: preserve the current automation contract for release tooling
target_anchor: revision=repo@8f31c2 + consumer-version=parser@release-tool-2026-02
completeness_dimensions:
  - interface
  - boundary
artifact_roles:
  - inventory_item_ref: ARI-A1-summary-line
    role: fixed_boundary_artifact
    surface: terminal stdout summary line
    mutation_authority: producer-controlled; downstream tooling treats the surface as fixed
    mutation_conditions: authority-approved interface revision only
    violation_effect: automated publication fails if the summary shape changes
  - inventory_item_ref: ARI-A1-success-transcript
    role: runtime_ledger
    surface: captured stdout transcript
    mutation_authority: generated witness in the isolated replay
    mutation_conditions: append-only within the controlled run
    violation_effect: witness contamination or replay loss
  - inventory_item_ref: ARI-A1-parser-regression-report
    role: derived_analysis
    surface: parser regression report
    mutation_authority: analyst-edited summary
    mutation_conditions: may change as analysis is refined
    violation_effect: interpretive drift, not direct contract change
candidate_clause: >
  On successful completion, the CLI emits exactly one terminal summary line in the form
  EXPORT_SUMMARY<TAB>status=ok<TAB>records=<int><TAB>output=<path>.
  Downstream tooling depends on the stable prefix, key names, and key order.
evidence_chain_refs:
  - EB-A1:chain:summary-line-contract
support_level: corroborated
workflow_state: validation-pending
disposition: needs-refinement
revalidation_required: false
ambiguity_status: known-open
ambiguity_note: key presence is well supported; exact key order still needs compatibility confirmation
risk_impact: parser break would block automated publication
regime_or_environment_status: captured
regime_or_environment_note: run against fixture corpus v7 in a non-mutating clone
negative_evidence_status: found
negative_evidence_refs:
  - EB-A1:readme-describes-keys-but-not-order
next_corrective_action: replay the consumer parser against canonical and reordered summary lines
stop_condition: either order dependence is witnessed or the clause is narrowed to key presence only
authority_disposition_needed: true
active_overlays: []
current_step_id: step:test-order-dependence
step_list:
  - step_id: step:capture-success-transcript
    description: capture a success transcript from a controlled run
    proposed_action: run exporter against fixture corpus v7
    status: done
    basis_refs:
      - EB-A1:success-transcript
    result_note: terminal summary line stored in EB-A1:success-transcript
  - step_id: step:test-order-dependence
    description: test whether consumer parsing depends on field order
    proposed_action: replay parser against canonical and reordered lines
    status: planned
    basis_refs:
      - EB-A1:consumer-parser
    result_note: null
```

### Slice Contract

```yaml
slice_id: cli-summary-line
contract_id: SC-A1
locator: /recovery/slices/cli-summary-line/contract.yaml
recovery_objective: backward-compatible reimplementation
compatibility_target: preserve the current automation contract for release tooling
target_anchor: revision=repo@8f31c2 + consumer-version=parser@release-tool-2026-02
candidate_clause: >
  On successful completion, the CLI emits exactly one terminal summary line in the form
  EXPORT_SUMMARY<TAB>status=ok<TAB>records=<int><TAB>output=<path>.
  Downstream tooling depends on the stable prefix, key names, and key order.
discriminating_question: >
  Does downstream automation depend on the order of the key-value pairs, or only on their presence?
validation_mode: executable
required_criteria:
  - criterion_id: C1
    description: canonical summary line is emitted exactly once on success
    observable_or_witness_class: runtime_witness
    success_threshold: one terminal line matching the canonical prefix and key order
    refuting_condition: missing line, duplicate line, or different field order
  - criterion_id: C2
    description: consumer parser accepts the canonical line
    observable_or_witness_class: compatibility_check
    success_threshold: parser returns status, records, and output without error
    refuting_condition: parser rejects canonical output
  - criterion_id: C3
    description: consumer parser treats a reordered but key-equivalent line as incompatible
    observable_or_witness_class: compatibility_check
    success_threshold: reordered line fails or misparses while canonical line succeeds
    refuting_condition: reordered line parses identically, showing order is not contractual
execution_preconditions:
  - fixture corpus v7 available
  - release parser executable available
environment_assumptions:
  - no mutation of production outputs
  - parser version pinned to current release tool revision
memory_refs_consulted: []
mutation_policy: non-mutating clone only
reset_method: restore fixture corpus v7 snapshot before rerun
expected_observables:
  - one canonical success line in stdout
  - parser success on canonical line
  - parser failure or divergent parse on reordered line
witness_capture_path:
  - store stdout transcript under EB-A1:success-transcript
  - store parser replay output under EB-A1:parser-replay
corrective_next_action: >
  If C3 fails, narrow the clause to stable prefix and key presence, then reopen the slice on order dependence only if new evidence appears.
active_overlays: []
executable_action:
  action_kind: controlled_runtime_plus_compatibility_replay
  execution_harness: shell
  entrypoint: bash
  argument_vector:
    - -lc
  working_directory: /workspace
  input_refs: []
  runnable_payload: >
    1. run exporter against fixture corpus v7;
    2. capture terminal summary line;
    3. replay release parser against canonical output and a line with records/output swapped.
  intent: determine whether field order is contract-bearing
  expected_observable: parser succeeds only on canonical order
  declared_assumptions:
    - parser behavior is representative of current downstream tooling
    - exporter run is isolated and repeatable
```

### Evidence Bundle

```yaml
bundle_id: EB-A1
witnesses:
  - ref: EB-A1:success-transcript
    kind: runtime_witness
    source_role: runtime_ledger
    locator: artifacts/appendix-a/success-transcript.txt
    note: stdout transcript from controlled successful run
  - ref: EB-A1:consumer-parser
    kind: static_corroborant
    source_role: consumer_code
    locator: tools/release_parser.py#parse_summary_line
    note: parser logic expects fixed prefix and ordered tab-separated fields
  - ref: EB-A1:parser-replay
    kind: compatibility_check
    source_role: derived_analysis
    locator: artifacts/appendix-a/parser-replay.txt
    note: canonical line succeeds, reordered line fails with missing output field
  - ref: EB-A1:release-incident
    kind: support_history
    source_role: incident_note
    locator: incidents/2025-11-14-summary-line-order.md
    note: release job failed after a local output reorder
  - ref: EB-A1:readme-describes-keys-but-not-order
    kind: public_documentation
    source_role: framing_metadata
    locator: README.md#summary-line
    note: README names the stable keys but does not specify field order, so it is negative evidence against order being documented rather than against order being contractual
chains:
  - chain_ref: EB-A1:chain:summary-line-contract
    clause_focus: canonical success summary line shape and downstream order dependence
    boundary_stimulus_or_precondition: successful export run against fixture corpus v7 and parser replay against canonical and reordered lines
    implicated_artifact_roles_or_source_classes:
      - machine_parsed_operational_output
      - runtime-and-boundary source class
    structural_or_documentary_corroboration_refs:
      - EB-A1:consumer-parser
      - EB-A1:readme-describes-keys-but-not-order
    execution_conditions:
      - fixture corpus v7 restored before each run
      - release parser pinned to revision 8f31c2
    runtime_witness_refs:
      - EB-A1:success-transcript
      - EB-A1:parser-replay
    observable_effect: canonical summary line is emitted exactly once and reordered output is rejected by the current parser
    external_dependence_refs:
      - EB-A1:release-incident
    current_disposition: needs-refinement
    assertion_basis: direct-observation
environment_notes:
  - fixture corpus v7 restored before each run
  - release parser pinned to revision 8f31c2
```

### Validation Report

```yaml
slice_id: cli-summary-line
contract_ref: SC-A1
witness_refs:
  - EB-A1:success-transcript
  - EB-A1:parser-replay
criterion_results:
  - criterion_id: C1
    witness_ref: EB-A1:success-transcript
    outcome: pass
    observed_divergence: none
    corrective_next_action: none
  - criterion_id: C2
    witness_ref: EB-A1:parser-replay
    outcome: pass
    observed_divergence: none
    corrective_next_action: none
  - criterion_id: C3
    witness_ref: EB-A1:parser-replay
    outcome: pass
    observed_divergence: reordered line rejected by current parser
    corrective_next_action: none
interpretation:
  action_reference: SC-A1.executable_action
  expected_outcome: canonical output parses successfully and reordered output does not
  observed_outcome: canonical output parsed successfully; reordered output failed
  expectation_delta: none
  contractual_significance: stable key order is part of the machine-parsed interface surface and warrants inclusion in the recovered contract
  next_move: update support to runtime-validated and request authority disposition
active_overlays: []
```

### Authority Review Packet

```yaml
packet_ref: ARP-A1
scope: slice:cli-summary-line
recovery_objective: backward-compatible reimplementation
compatibility_target: preserve the current automation contract for release tooling
review_mode: per-slice
authority_independence: independent-reviewer
questions_for_authority:
  - Should stable key order be preserved as part of the recovered machine-parsed interface?
candidate_decisions:
  - approve inclusion of stable key order in the recovered contract
  - bound the clause to stable prefix and key presence only
basis_refs:
  - SC-A1
  - EB-A1:chain:summary-line-contract
  - EB-A1:release-incident
open_objections:
  - README-level documentation does not mention order, so documentary intent remains weaker than runtime and consumer evidence
compatibility_and_security_impact: field-order drift would break current release automation; no additional sensitive material is required for first-pass review
recommended_disposition: approved-with-exceptions
follow_up_if_rejected: narrow the clause to stable prefix and key presence and reopen order dependence as a separate cleanup question
```

### Authority Disposition

```yaml
artifact_ref: slice:cli-summary-line
packet_ref: ARP-A1
decision: approved-with-exceptions
review_mode: per-slice
scope: include stable prefix, key names, and key order in the recovered contract
basis_refs:
  - SC-A1
  - EB-A1:chain:summary-line-contract
  - EB-A1:release-incident
normative_effect: promote
rationale: runtime witness, consumer parser, and support history all indicate a load-bearing machine-parsed interface
remaining_exceptions:
  - failure-path optionality of output field remains a separate slice
follow_up_actions:
  - open a separate slice for failure-path optionality of the output field
ratifier: release-engineering owner
```


## Appendix B

This appendix is a non-normative example of a slice that is simultaneously concept-bearing, abstract-family-backed, and lifecycle-bearing. The target surface is a workflow engine keyed by `job_id`.

Non-normative note: this appendix exercises the multi-overlay path. Because more than one overlay is active, `cross_overlay_dependencies` and `cross_overlay_findings` are present. Because `negative_evidence_status` is `found`, `negative_evidence_refs` is present. Because `validation_mode` is `executable`, `executable_action` is present and `non_executable_resolution` is omitted by rule.

Non-normative note: this appendix introduces the additional `witness.source_role` literal `runtime_counter` as an appendix-local explicit addition to the extensible vocabulary. In this appendix, `runtime_counter` aligns to the runtime-and-boundary source class.

### Specification Plan entry

```yaml
slice_id: duplicate-start-after-terminal-success
slice_summary: duplicate start trigger for an already succeeded job
recovery_objective: backward-compatible reimplementation
compatibility_target: preserve current workflow engine behavior for queue and API callers
target_anchor: revision=repo@2026-02-11 + deployment-family=workflow-engine-2026-02
completeness_dimensions:
  - behavioral
  - interface
  - boundary
artifact_roles:
  - inventory_item_ref: ARI-B1-status-api
    role: fixed_boundary_artifact
    surface: status API surface
    mutation_authority: service-controlled boundary surface
    mutation_conditions: authority-reviewed interface change only
    violation_effect: caller-visible lifecycle contract drift
  - inventory_item_ref: ARI-B1-workflow-ledger
    role: runtime_ledger
    surface: workflow event ledger
    mutation_authority: system-generated execution record
    mutation_conditions: append-only within the isolated replay
    violation_effect: transition evidence becomes unreliable
  - inventory_item_ref: ARI-B1-retry-runbook
    role: control_plane_artifact
    surface: operator retry runbook
    mutation_authority: operator-edited policy surface
    mutation_conditions: governance-approved operational revision
    violation_effect: retry handling may be misclassified during recovery
  - inventory_item_ref: ARI-B1-duplicate-trigger-analysis
    role: derived_analysis
    surface: duplicate-trigger triage notebook
    mutation_authority: analyst-edited summary
    mutation_conditions: may change as interpretation improves
    violation_effect: interpretive drift, not direct contract change
candidate_clause: >
  When a start trigger is received for a job_id whose authoritative status surface already shows terminal succeeded,
  the system records a duplicate no-op event, leaves the job in succeeded, and does not emit a second execution attempt.
evidence_chain_refs:
  - EB-B1:chain:duplicate-start-after-terminal-success
support_level: corroborated
workflow_state: validation-pending
disposition: needs-refinement
revalidation_required: false
ambiguity_status: known-open
ambiguity_note: terminal-success handling is strongly suggested; terminal-failure handling remains a separate slice
risk_impact: duplicate execution would create double billing and incorrect downstream status
regime_or_environment_status: captured
regime_or_environment_note: replayed in isolated queue clone with fixed worker count and frozen ledger snapshot
negative_evidence_status: found
negative_evidence_refs:
  - EB-B1:stale-notebook-mislabels-terminal-state
next_corrective_action: execute duplicate-trigger replay across admissible instances from both queue and API entry paths
stop_condition: either the duplicate-no-op clause is validated for terminal success or the clause is split by trigger source or terminal state
authority_disposition_needed: true
active_overlays:
  - concept_overlay
  - abstract_witness_overlay
  - lifecycle_overlay
current_step_id: step:replay-duplicate-starts
concept_overlay:
  concept_vocabulary:
    - idempotent_terminal_handling
    - authoritative_status_projection
  concept_basis: >
    Terminal success is projected from the status surface and blocks re-entry into execution.
  concept_coverage_note: covers terminal succeeded only; terminal failed and refused remain out of scope
  structured_absences_checked:
    - no worker-claim path after authoritative succeeded state
abstract_witness_overlay:
  abstract_witness_form: duplicate_start_after_terminal_success(job_id, terminal_record, duplicate_trigger)
  instantiation_scope: jobs started through queue adapter v2 or public start API
  admissibility_gaps: []
  instantiation_preservation_gaps:
    - confirm that queue and API triggers preserve the same duplicate-no-op boundary effects
lifecycle_overlay:
  model_mode: promotion-bearing
  lifecycle_subject: workflow job
  instance_key: job_id
  open_lifecycle_gaps:
    - terminal failed duplicate handling is not yet covered
step_list:
  - step_id: step:validate-authoritative-status
    description: validate authoritative status projection for terminal success
    proposed_action: inspect status API and ledger alignment for selected jobs
    status: done
    basis_refs:
      - EB-B1:status-snapshot
    result_note: authoritative projection captured in EB-B1:status-snapshot
  - step_id: step:replay-duplicate-starts
    description: replay duplicate start triggers across admissible instances
    proposed_action: run duplicate-trigger suite in isolated queue clone
    status: planned
    basis_refs:
      - memory:technical-witness:isolated-queue-clone-replay-suite
    result_note: null
```

### Slice Contract

```yaml
slice_id: duplicate-start-after-terminal-success
contract_id: SC-B1
locator: /recovery/slices/duplicate-start-after-terminal-success/contract.yaml
recovery_objective: backward-compatible reimplementation
compatibility_target: preserve current workflow engine behavior for queue and API callers
target_anchor: revision=repo@2026-02-11 + deployment-family=workflow-engine-2026-02
candidate_clause: >
  When a start trigger is received for a job_id whose authoritative status surface already shows terminal succeeded,
  the system records a duplicate no-op event, leaves the job in succeeded, and does not emit a second execution attempt.
discriminating_question: >
  For admissible duplicate-start instances in terminal succeeded, is the boundary behavior a true no-op rather than a hidden re-execution or a source-dependent branch?
validation_mode: executable
required_criteria:
  - criterion_id: K1
    description: authoritative status surface projects terminal succeeded from boundary-observable artifacts
    observable_or_witness_class: state_snapshot_plus_derivation_check
    success_threshold: status API and ledger-derived status agree on succeeded
    refuting_condition: authoritative surface is ambiguous or disagrees with boundary artifacts
  - criterion_id: K2
    description: recovered concept idempotent_terminal_handling remains stable under admissible witness perturbation
    observable_or_witness_class: concept_probe_plus_perturbation_stability
    success_threshold: neutral trigger-label changes do not change the duplicate-no-op interpretation
    refuting_condition: concept interpretation flips under non-binding perturbation
  - criterion_id: F1
    description: abstract witness family is valid for duplicate_start_after_terminal_success
    observable_or_witness_class: family_level_witness_review
    success_threshold: placeholder roles and constraints describe the recurring duplicate-start shape
    refuting_condition: family form cannot cover observed duplicate-start cases without ad hoc exceptions
  - criterion_id: F2
    description: concrete instances are admissible under the family
    observable_or_witness_class: admissibility_check
    success_threshold: each instance uses the same job_id, terminal succeeded state, and recognized duplicate trigger source
    refuting_condition: any instance falls outside declared roles or scope
  - criterion_id: F3
    description: duplicate-no-op behavior is preserved under concretization across queue and API triggers
    observable_or_witness_class: preservation_check
    success_threshold: boundary-visible effects match across admissible instances
    refuting_condition: one trigger source re-executes or produces different terminal effects
  - criterion_id: L1
    description: duplicate start does not re-enter running from succeeded
    observable_or_witness_class: transition_trace
    success_threshold: no succeeded -> running transition is observed
    refuting_condition: any duplicate start produces a new running transition
  - criterion_id: L2
    description: duplicate start records a duplicate no-op boundary effect
    observable_or_witness_class: transition_trace_plus_boundary_output
    success_threshold: duplicate event is recorded and status remains succeeded
    refuting_condition: duplicate event missing, hidden, or status changes
  - criterion_id: L3
    description: worker attempt count does not increase on duplicate start
    observable_or_witness_class: runtime_counter_witness
    success_threshold: worker attempt counter is unchanged for the job_id
    refuting_condition: counter increments on duplicate start
execution_preconditions:
  - isolated queue clone available
  - workflow ledger snapshot frozen
  - worker pool pinned to one logical execution slot
environment_assumptions:
  - queue adapter v2 and public start API are both enabled
  - status API reads from the current authoritative projection path
memory_refs_consulted:
  - memory:contract-pattern:terminal-noop-after-duplicate-trigger
  - memory:strategy:establish-authoritative-state-before-replay
  - memory:technical-witness:isolated-queue-clone-replay-suite
mutation_policy: isolated replay only; no production mutation
reset_method: restore queue clone and ledger snapshot before each replay case
expected_observables:
  - authoritative succeeded status before duplicate trigger
  - duplicate no-op event after duplicate trigger
  - no new running transition
  - unchanged worker attempt count
witness_capture_path:
  - store state snapshots under EB-B1:status-snapshot
  - store duplicate-trigger traces under EB-B1:duplicate-trigger-trace
  - store worker attempt counters under EB-B1:worker-attempt-counter
corrective_next_action: >
  If preservation or lifecycle criteria fail, split the clause by trigger source or narrow it to the validated terminal-state subset.
active_overlays:
  - concept_overlay
  - abstract_witness_overlay
  - lifecycle_overlay
cross_overlay_dependencies:
  - admissibility is checked per job_id and requires terminal succeeded to be established by the lifecycle state-derivation rule
  - concept criterion K2 is evaluated only on instances that satisfy F2 admissibility
  - preservation criterion F3 must preserve both the lifecycle no-reentry rule and the duplicate no-op boundary effect
concept_overlay:
  concept_vocabulary:
    - idempotent_terminal_handling
    - authoritative_status_projection
  concept_extraction_method: compare status API projection with event ledger and duplicate-trigger traces
  concept_witnesses:
    - EB-B1:status-snapshot
    - EB-B1:duplicate-trigger-trace
  concept_probe: replay duplicate start after neutral renaming of non-binding trigger labels in the test harness
  concept_coverage_note: terminal succeeded only
  perturbation_stability_check: neutral trigger-label changes must not change the duplicate-no-op interpretation for the criterion to pass
abstract_witness_overlay:
  abstract_witness_form: duplicate_start_after_terminal_success(job_id, terminal_record, duplicate_trigger)
  placeholder_roles:
    - job_id identifies the lifecycle subject
    - terminal_record proves succeeded at the boundary
    - duplicate_trigger is a start request for the same job_id after terminal success
  placeholder_constraints:
    - terminal_record and duplicate_trigger must refer to the same job_id
    - duplicate_trigger must arrive after the terminal record
  cross_placeholder_consistency:
    - job_id equality across all placeholders
    - duplicate trigger source is either queue adapter v2 or public start API
  instantiation_scope: queue adapter v2 and public start API only
  admissibility_check: verify same job_id, terminal succeeded, and in-scope trigger source before counting the instance
  instantiation_preservation_conditions: duplicate-no-op boundary effect and no re-entry into running must hold for each admissible instance
lifecycle_overlay:
  model_mode: promotion-bearing
  lifecycle_subject: workflow job
  instance_key: job_id
  canonical_state_vocabulary:
    - queued
    - running
    - succeeded
    - failed
    - refused
  observed_to_canonical_mapping:
    done: succeeded
    error: failed
  event_vocabulary:
    - start
    - worker_claimed
    - worker_completed
    - worker_failed
    - duplicate_start
  authoritative_state_surface: status API projection backed by event ledger
  state_derivation_rule: >
    Current state is derived from the latest terminal or nonterminal ledger event according to guard precedence,
    with terminal events dominating duplicate start records.
  transition_rules:
    - queued -> running on worker_claimed
    - running -> succeeded on worker_completed
    - running -> failed on worker_failed
    - succeeded -> succeeded on duplicate_start
  guard_precedence:
    - terminal state dominates duplicate start
    - duplicate start after succeeded is handled before any new worker claim
  boundary_effects_by_transition:
    succeeded -> succeeded on duplicate_start: duplicate no-op event plus unchanged status API result
  external_status_mapping:
    succeeded: status=done
    failed: status=error
    refused: status=refused
  illegal_transition_policy: out-of-order duplicate start after succeeded is accepted as a no-op, not as a new execution
  per_state_observables:
    succeeded: status API returns done and worker attempt counter is stable
  per_transition_observables:
    succeeded -> succeeded on duplicate_start:
      - duplicate no-op event recorded
      - no new running transition
      - worker attempt counter unchanged
executable_action:
  action_kind: isolated_lifecycle_replay_suite
  execution_harness: shell
  entrypoint: bash
  argument_vector:
    - -lc
  working_directory: /workspace
  input_refs:
    - memory:technical-witness:isolated-queue-clone-replay-suite
  runnable_payload: >
    For three admissible jobs, confirm terminal succeeded, then issue duplicate start through queue adapter v2 and public start API,
    capture ledger events, status API outputs, and worker attempt counters.
  intent: determine whether duplicate start after terminal success is a boundary-visible no-op across admissible instances
  expected_observable: duplicate no-op event, unchanged succeeded status, and no new execution attempt
  declared_assumptions:
    - ledger snapshot and queue clone remain resettable between cases
    - duplicate trigger sources are representative of in-scope callers
```

### Evidence Bundle

```yaml
bundle_id: EB-B1
witnesses:
  - ref: EB-B1:status-snapshot
    kind: state_snapshot
    source_role: fixed_boundary_artifact
    locator: artifacts/appendix-b/status-snapshot.json
    note: status API results aligned with ledger-derived succeeded state
  - ref: EB-B1:duplicate-trigger-trace
    kind: transition_trace
    source_role: runtime_ledger
    locator: artifacts/appendix-b/duplicate-trigger-trace.jsonl
    note: duplicate start events for queue and API sources after terminal success
  - ref: EB-B1:worker-attempt-counter
    kind: runtime_witness
    source_role: runtime_counter
    locator: artifacts/appendix-b/worker-attempt-counter.csv
    note: worker attempt counts before and after duplicate triggers
  - ref: EB-B1:operator-runbook
    kind: control_plane_artifact
    source_role: control_plane_artifact
    locator: ops/retry_runbook.md#duplicate-start
    note: operator guidance says duplicate retry after terminal success should not restart work
  - ref: EB-B1:stale-notebook-mislabels-terminal-state
    kind: negative_evidence
    source_role: derived_analysis
    locator: notebooks/duplicate-trigger-exploration.ipynb
    note: older notebook treated done as restartable; superseded by authoritative state projection
chains:
  - chain_ref: EB-B1:chain:duplicate-start-after-terminal-success
    clause_focus: duplicate start after terminal success remains a duplicate no-op without re-entry into execution
    boundary_stimulus_or_precondition: duplicate start replay through queue adapter v2 and public start API after authoritative succeeded state
    implicated_artifact_roles_or_source_classes:
      - fixed_boundary_artifact
      - runtime_ledger
      - control_plane_artifact
    structural_or_documentary_corroboration_refs:
      - EB-B1:operator-runbook
    execution_conditions:
      - isolated queue clone restored before each case
      - worker pool pinned to one logical execution slot
      - ledger snapshot fixed to revision 2026-02-11
    runtime_witness_refs:
      - EB-B1:status-snapshot
      - EB-B1:duplicate-trigger-trace
      - EB-B1:worker-attempt-counter
    observable_effect: duplicate no-op event is recorded, status remains succeeded, and worker attempts do not increase
    external_dependence_refs: []
    current_disposition: needs-refinement
    assertion_basis: direct-observation
environment_notes:
  - isolated queue clone restored before each case
  - worker pool pinned to one logical execution slot
  - ledger snapshot fixed to revision 2026-02-11
```

### Validation Report

```yaml
slice_id: duplicate-start-after-terminal-success
contract_ref: SC-B1
witness_refs:
  - EB-B1:status-snapshot
  - EB-B1:duplicate-trigger-trace
  - EB-B1:worker-attempt-counter
criterion_results:
  - criterion_id: K1
    witness_ref: EB-B1:status-snapshot
    outcome: pass
    observed_divergence: none
    corrective_next_action: none
  - criterion_id: K2
    witness_ref: EB-B1:duplicate-trigger-trace
    outcome: pass
    observed_divergence: none
    corrective_next_action: none
  - criterion_id: F1
    witness_ref: EB-B1:duplicate-trigger-trace
    outcome: pass
    observed_divergence: none
    corrective_next_action: none
  - criterion_id: F2
    witness_ref: EB-B1:duplicate-trigger-trace
    outcome: pass
    observed_divergence: none
    corrective_next_action: none
  - criterion_id: F3
    witness_ref: EB-B1:duplicate-trigger-trace
    witness_refs:
      - EB-B1:duplicate-trigger-trace
      - EB-B1:worker-attempt-counter
    outcome: pass
    observed_divergence: queue and API triggers showed the same duplicate-no-op boundary effects
    corrective_next_action: none
  - criterion_id: L1
    witness_ref: EB-B1:duplicate-trigger-trace
    outcome: pass
    observed_divergence: none
    corrective_next_action: none
  - criterion_id: L2
    witness_ref: EB-B1:duplicate-trigger-trace
    outcome: pass
    observed_divergence: duplicate no-op event recorded as expected
    corrective_next_action: none
  - criterion_id: L3
    witness_ref: EB-B1:worker-attempt-counter
    outcome: pass
    observed_divergence: none
    corrective_next_action: none
interpretation:
  action_reference: SC-B1.executable_action
  expected_outcome: duplicate start after terminal success is a no-op across admissible instances
  observed_outcome: duplicate no-op event recorded, status remained succeeded, and worker attempts did not increase
  expectation_delta: none
  contractual_significance: duplicate start after terminal success is a load-bearing lifecycle rule that warrants inclusion in the recovered contract
  next_move: update support to runtime-validated and request authority disposition for terminal-succeeded scope
active_overlays:
  - concept_overlay
  - abstract_witness_overlay
  - lifecycle_overlay
cross_overlay_findings:
  - admissibility depended on lifecycle state derivation and same-job_id matching
  - concept interpretation remained stable across admissible queue and API instances
  - preservation under concretization held for both trigger sources in the declared scope
concept_overlay:
  concept_criteria_outcomes:
    - concept_id: idempotent_terminal_handling
      outcome: pass
    - concept_id: authoritative_status_projection
      outcome: pass
  concept_coverage_findings: terminal succeeded only; terminal failed still out of scope
abstract_witness_overlay:
  family_validity: pass
  concrete_admissibility: pass
  preservation_under_concretization: pass
lifecycle_overlay:
  witness_classification:
    - witness_ref: EB-B1:status-snapshot
      kind: state_snapshot
      supports:
        - succeeded
    - witness_ref: EB-B1:duplicate-trigger-trace
      kind: transition_trace
      supports:
        - succeeded -> succeeded on duplicate_start
    - witness_ref: EB-B1:worker-attempt-counter
      kind: derived_summary
      supports:
        - no second execution attempt
```

Non-normative note: Criterion `F3` carries both `EB-B1:duplicate-trigger-trace` and `EB-B1:worker-attempt-counter` as decisive witnesses. In this appendix, both witnesses align to the runtime-and-boundary source class under Source hierarchy, so the primary `witness_ref` is selected by the lexicographic tiebreak and resolves to `EB-B1:duplicate-trigger-trace`.

### Authority Review Packet

```yaml
packet_ref: ARP-B1
scope: slice:duplicate-start-after-terminal-success
recovery_objective: backward-compatible reimplementation
compatibility_target: preserve current workflow engine behavior for queue and API callers
review_mode: per-slice
authority_independence: independent-reviewer
questions_for_authority:
  - Should duplicate start after terminal success be preserved as a load-bearing lifecycle rule for queue adapter v2 and public start API?
candidate_decisions:
  - approve the clause for the declared trigger sources and terminal-succeeded scope
  - bound the clause to one trigger source or defer it pending additional lifecycle evidence
basis_refs:
  - SC-B1
  - EB-B1:chain:duplicate-start-after-terminal-success
  - EB-B1:worker-attempt-counter
  - EB-B1:operator-runbook
open_objections: []
compatibility_and_security_impact: approving hidden re-execution would normalize double-billing risk; the first-pass packet uses witness refs and redacted summaries only
recommended_disposition: approved
follow_up_if_rejected: split the clause by trigger source or leave it bounded at Level 2 until narrower lifecycle evidence closes the gap
```

### Authority Disposition

```yaml
artifact_ref: slice:duplicate-start-after-terminal-success
packet_ref: ARP-B1
decision: approved
review_mode: per-slice
scope: include duplicate-start-after-terminal-success as a load-bearing lifecycle clause for queue adapter v2 and public start API
basis_refs:
  - SC-B1
  - EB-B1:chain:duplicate-start-after-terminal-success
  - EB-B1:worker-attempt-counter
  - EB-B1:operator-runbook
normative_effect: promote
rationale: lifecycle witnesses, concept checks, family admissibility, and preservation checks all passed within the declared scope
remaining_exceptions:
  - terminal failed duplicate handling remains a separate slice
  - trigger sources outside queue adapter v2 and public start API remain out of scope
follow_up_actions:
  - open a separate slice for terminal failed duplicate handling
ratifier: workflow-platform owner
```


## Appendix C

This appendix is a non-normative example of a recovery state that remains useful even though promotion has not yet occurred. The example assumes one high-risk slice is closed, one slice is bounded, and authority review is not yet available for the remaining normative question.

```yaml
snapshot_ref: RS-C1
scope: data-export-service contract recovery
recovery_objective: backward-compatible reimplementation
compatibility_target: preserve scheduler, storage, dashboard, and CLI integrations
target_anchor: revision=repo@1f2e3d4 + deployment-family=export-service-2026-02
workflow_spec_ref: SPEC-SHR-002@0.13.0
promotion_readiness: slice-backed
recon_refs:
  - recon:system-sketch
  - recon:artifact-roles
plan_ref: plan:recovery/plan.yaml
closed_slice_refs:
  - slice:api-job-lifecycle
bounded_slice_refs:
  - slice:cli-summary-line:failure-path-output-field
open_slice_refs:
  - slice:storage-output-format
  - slice:status-db-contract
authority_gaps:
  - no release-engineering authority reviewer available for the remaining CLI carve-out question
resume_order:
  - slice:storage-output-format
  - slice:status-db-contract
  - slice:cli-summary-line:failure-path-output-field
memory_refs:
  - memory:strategy:replay-consumer-parser-before-cleanup
  - memory:technical-witness:fixture-corpus-v7
stop_reason: budget pause before promotion; unresolved authority review remains
```
