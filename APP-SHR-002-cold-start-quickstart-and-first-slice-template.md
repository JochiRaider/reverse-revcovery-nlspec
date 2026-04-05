---
doc_id: APP-SHR-002
title: Cold-Start Quickstart and First-Slice Template for Reverse Recovery
status: active
version: 0.10.0
related_docs:
  - SPEC-SHR-000
  - SPEC-SHR-001
  - SPEC-SHR-002
  - SPEC-SHR-003
  - SPEC-SHR-004
  - SPEC-SHR-005
  - SPEC-SHR-006
  - APP-SHR-001
  - APP-SHR-003
  - APP-SHR-004
  - APP-SHR-005
  - RES-SHR-001
---

# Cold-Start Quickstart and First-Slice Template for Reverse Recovery

## Purpose and boundary

This document is a non-normative companion to `SPEC-SHR-000`, `SPEC-SHR-001`, `SPEC-SHR-002`, `SPEC-SHR-003`, `SPEC-SHR-004`, `SPEC-SHR-005`, `SPEC-SHR-006`, `APP-SHR-003`, `APP-SHR-004`, and `APP-SHR-005`. It reduces time-to-first-governed-artifact for a cold-start reverse-recovery engagement.

Its job is narrow and practical. It operationalizes `SPEC-SHR-005` for the first governed loop when the governed workspace is the target repository, and it operationalizes `SPEC-SHR-006` for the first governed loop when the governed workspace is adjacent and the target repository should remain external during cold start. It gives a first practitioner or coding agent the smallest useful pack needed to begin governed recovery work without synthesizing the whole shared corpus first:

- a minimal governed-workspace bootstrap plus reverse-recovery discovery file
- a small placement-mode chooser
- a small first-slice selection rule
- an annotated Specification Plan entry
- an annotated Slice Contract
- a minimal Validation Report
- a simple chooser for `executable` versus `non-executable` validation mode

This document does not add a new specification surface. It does not change `SPEC-SHR-002` workflow semantics, `SPEC-SHR-004` artifact schemas, `SPEC-SHR-000` taxonomy or authority rules, or `SPEC-SHR-001` completeness standards. It applies them at the smallest useful scale. `APP-SHR-003` remains the routing surface for what to read first. `SPEC-SHR-003` owns the normative minimum workspace topology and workspace structural posture model. `SPEC-SHR-005` and `SPEC-SHR-006` own the short normative cold-start profiles that this document mirrors operationally for the first loop. `APP-SHR-004` owns the stable repository-shaped operating baseline after the first loop. `APP-SHR-005` owns the stable authority-closure and promotion-handoff baseline. This document remains the APP-layer home for first-loop templates, placement choice, first-loop correction, and copy-adapt examples only.

If this document conflicts with a governing specification, the governing specification controls.

## When to use this document

Use this document when the governed workspace is a true cold start: no governed recovery artifacts exist yet, the boundary survey is only partly formed, and the immediate goal is to open the first slice cleanly.

Use `SPEC-SHR-005` when the governed workspace is the target repository and local governed outputs are intended there during cold start. Use `SPEC-SHR-006` when the governed workspace is adjacent and the target repository should remain external during cold start.

Use `APP-SHR-004` when the engagement moves beyond the first slice and needs stable repository-shaped guidance for reconnaissance order, executable-feasibility judgment, target-anchor axis selection, or repository-surface-family operating patterns.

Use `APP-SHR-005` when the next seam is authority review, same-operator disclosure, or promotion-bearing handoff. Use `APP-SHR-001` only when the engagement also needs broader draft guidance for recovery memory, resumable handoff, multi-slice composition, or other wider operating seams.

If the shared corpus itself is unfamiliar, read `APP-SHR-003` first. This document assumes the reader already knows where normative authority lives and it does not restate a second reading-order router.

## Placement-mode chooser

Choose the short cold-start profile before writing the first governed bootstrap and discovery files.

| Cold-start condition | Use this short profile | Where bootstrap and discovery live | Where governed outputs live during cold start | What stays external | Invalid shortcut to avoid |
|---|---|---|---|---|---|
| The governed workspace is the target repository | `SPEC-SHR-005` | the target repository | the target repository `document_roots` | nothing about target governance is external by default | treating adjacent-workspace guidance as if the target repository were already the governed workspace |
| The governed workspace is adjacent to the target repository | `SPEC-SHR-006` | the adjacent governed workspace | the adjacent workspace `document_roots` | the target repository remains external until a separate explicit local action says otherwise | treating adjacency alone as proof that the target repository has adopted shared governance |

Discovery-only adoption without bootstrap is a different case. It is structurally allowed by `SPEC-SHR-003`, but it is not the first-loop governed default for either profile above.

## What must exist before the first Slice Contract

This document stays small, but it does not waive the entry record required by `SPEC-SHR-002`. Before the first Slice Contract is opened, the working record should already make the following facts explicit.

| Required record | Typical cold-start location | Minimum content |
|---|---|---|
| System sketch | `/recovery/recon/system-sketch.md` | System identity; current scope cut; primary boundary surfaces or repository-surface families now in view; principal external actors, adjacent systems, or operator classes. |
| Recovery objective and compatibility target | first plan entry or a short recon note | Enough specificity to explain what kind of preservation, cleanup, migration, or description is being attempted. |
| `target_anchor` | first plan entry and first contract | Normalized `axis=value` anchor using only materially relevant axes for the declared slice and scope. |
| Draft artifact-role inventory | `/recovery/recon/artifact-roles.yaml` or equivalent | At least the currently known boundary surfaces, each with stable `inventory_item_ref`, provisional role, explicit scope classification, mutation authority, mutation conditions, and violation effect. |
| `repository_surface_families` when the target is repository-shaped | `/recovery/recon/artifact-roles.yaml` or equivalent | A row-complete family record using the canonical `surface_family` and `assessment` vocabularies, with `inspection-pending` used only where stronger bounding is not yet honest. |
| First slice seed | candidate slice list or first Specification Plan entry | Stable seed label; targeted boundary surface; provisional question or provisional clause; explicit selection reason. |

These records may remain provisional. Their existence is the entry condition. For the shortest normative statement of this minimum stack, use `SPEC-SHR-005` for in-repository work and `SPEC-SHR-006` for adjacent-workspace work. For the full workflow-owned entry rule, use `SPEC-SHR-002`. For canonical artifact-role inventory and repository-surface-family minima, use `SPEC-SHR-004`. For broader draft reconnaissance method, use `APP-SHR-001`. For the stable repository-shaped reconnaissance baseline after the first loop, use `APP-SHR-004`.

In practice, the lightly structured entry artifacts should already answer two small review questions. The system sketch should tell a new reader what system or subsystem is in scope, where the current scope cut sits, which boundary surfaces or repository-surface families define the immediate frontier, and who or what materially interacts with that scope. The first slice seed should tell the same reader what stable label identifies the proposed slice, which boundary surface it targets, what provisional question or clause it is expected to test, and why it was chosen first.

A minimal draft inventory snippet that already satisfies the shared corpus default shape is:

```yaml
items:
  - inventory_item_ref: ARI-0001
    surface: terminal stdout summary line
    role: machine_parsed_operational_output
    scope_classification: in-scope
    mutation_authority: producer-controlled surface
    mutation_conditions: authority-reviewed interface change only
    violation_effect: downstream parser failure
repository_surface_families:
  - surface_family: build_graph_and_packaging_boundary
    assessment: inspection-pending
    assessment_note: packaging boundary not yet bounded
  - surface_family: deployment_unit_and_release_topology
    assessment: not-present
  - surface_family: configuration_plane_and_default_source
    assessment: inspection-pending
    assessment_note: default-source semantics not yet bounded
  - surface_family: generated_artifact_or_codegen_boundary
    assessment: not-present
  - surface_family: extension_plugin_or_hook_surface
    assessment: not-present
  - surface_family: data_store_schema_and_migration_seam
    assessment: not-present
  - surface_family: service_topology_and_cross_service_coordination_seam
    assessment: not-present
```


Draft means the item may still be revised. It does not mean the identity, scope classification, mutation record, or repository-surface-family row completeness may be omitted.

A minimal candidate-slice seed carried outside the plan may look like:

```yaml
candidate_slices:
  - seed_label: seed:cli-summary-line
    targeted_boundary_surface: terminal stdout summary line
    provisional_question: Is the success summary line a stable machine-parsed interface surface?
    selection_reason: small boundary-visible surface with clear parser consequence and low-cost replay
```

If the current working record already carries the active `target_anchor`, omission of a seed-local anchor here means inheritance. Once the seed becomes a Specification Plan entry, state the plan-local `target_anchor` explicitly.

Omission of `closure_basis_refs` on a draft repository-surface-family row means promotion-bearing family closure has not yet been linked reviewably. Do not read `assessment` alone as completed family closure.


## The smallest viable workspace

The first governed recovery loop needs only a bootstrap, a reverse-recovery discovery file, a governed-document root for future promoted outputs, and a recovery tree that sits outside the bootstrap scan. `SPEC-SHR-003` owns that minimum layout normatively. This section is an operational mirror of the current corpus-default topology so a first practitioner can instantiate it without re-deriving the tree from prose.

A minimal in-repository layout is:

```text
/
  .nlspec/
    authoritative-sources.yaml
    reverse-recovery.yaml
  specs/
  recovery/
    recon/
      system-sketch.md
      artifact-roles.yaml
    memory/                     # may start empty; default future location for recovery memory
    plan.yaml
    slices/
```

The corresponding minimal local governance and recovery-discovery files are:

```yaml
# /.nlspec/authoritative-sources.yaml
governance_spec_ref: SPEC-SHR-000@0.5.4
document_roots:
  - /specs
```

```yaml
# /.nlspec/reverse-recovery.yaml
recovery_workflow_spec_ref: SPEC-SHR-002@0.13.0
recovery_root: /recovery
```

A minimal adjacent-workspace layout is the same governed shape, but it lives in the adjacent workspace rather than in the target repository:

```text
<adjacent-governed-workspace>/
  .nlspec/
    authoritative-sources.yaml
    reverse-recovery.yaml
  specs/
  recovery/
    recon/
      system-sketch.md
      artifact-roles.yaml
    memory/
    plan.yaml
    slices/
```

In that adjacent-workspace case, the bootstrap and discovery files still live at the workspace root shown above. Do not place them in the target repository during cold start. Keep the target repository external and identify it reviewably through the system sketch, normalized `target_anchor`, and stable witness locators.

Under `SPEC-SHR-003`, omission of `recovery_artifact_schema_spec_ref` means the effective schema companion is resolved by the paired default or inline fallback rule, omission of `plan_locator` means the governing Specification Plan lives at `/recovery/plan.yaml`, omission of `memory_locator` means the designated recovery-memory root lives at `/recovery/memory/`, the corpus-default artifact-role inventory carrier resolves to `/recovery/recon/artifact-roles.yaml`, and the corpus-default slice-working root resolves to `/recovery/slices/`. This keeps promoted governed documents under `/specs/` and keeps recovery working state outside `document_roots`.

Discovery-only without bootstrap can still be structurally valid in limited adjacent-workspace or reconnaissance-first cases. The recommended governed cold-start paths, however, are `SPEC-SHR-005` for in-repository entry and `SPEC-SHR-006` for adjacent-workspace entry. The minimal trees shown here are operational examples of the current `SPEC-SHR-003` default, not second topology authorities.

For reverse-recovery adoption, explicit `document_roots` such as `/specs` are the recommended default under `SPEC-SHR-000`. Full-repository scan remains legal when `document_roots` is omitted, but it is usually the wrong default for large or mixed-governance target repositories.

If the recovery workspace lives in a separate repository, use the adjacent-workspace shape above unless an authoritative local override site names a different carrier. The important separation is logical rather than physical: governed outputs live under the bootstrap scan, `SPEC-SHR-002` working artifacts do not, and the target repository remains external until a separate explicit local action changes that status.

A workspace that has both `/.nlspec/` files but none of the materialized recovery surfaces yet is ordinarily only `recovery-adopted` under `SPEC-SHR-003`. When the schema companion field is omitted, the paired default is acceptable only when the reader can resolve one unique effective schema owner. It becomes `recovery-materialized` when the plan, artifact-role inventory carrier, slice-working root, and recovery-memory root are materially present.

### Copy-adapt template: local override for non-default topology

Use a local override only when the corpus-default topology is wrong for the repository. Do not silently change paths in plan entries or local notes and then expect readers to infer the new carrier.

A minimal copy-adapt pair is:

```yaml
# /.nlspec/reverse-recovery.yaml
recovery_workflow_spec_ref: SPEC-SHR-002@0.13.0
recovery_artifact_schema_spec_ref: SPEC-SHR-004@0.13.0
recovery_root: /recovery
local_override_locator: /specs/SPEC-004-reverse-recovery-local-rules.md
```

```markdown
# /specs/SPEC-004-reverse-recovery-local-rules.md
---
doc_id: SPEC-004
title: Reverse-recovery local rules
status: active
related_docs:
  - SPEC-SHR-003
reverse_recovery_overrides:
  topology_overrides:
    - structural_surface: artifact-role inventory carrier
      replacement_path: /recovery/control/artifact-roles.yaml
      override_kind: different
      compatibility_note: older inventories remain readable until refreshed into the new carrier
    - structural_surface: slice-working root
      replacement_path: /recovery/work-items/
      override_kind: different
      compatibility_note: existing slice refs continue to resolve through stable artifact refs
  stricter_posture_rules: []
  workflow_version_compatibility: []
  legacy_recovery_output_designations: []
  workflow_overrides: []
  artifact_schema_version_compatibility: []
---

## Local reverse-recovery note

Body prose may explain why the override exists. The front-matter block above remains the authoritative machine-readable source.
```

This template is illustrative only. Under the repaired `SPEC-SHR-003` rule, `local_override_locator` is valid only when bootstrap is present and the carrier is a scanned local active `SPEC`. Discovery-only adoption without bootstrap must omit `local_override_locator`. The authoritative rule is still `SPEC-SHR-003`: use the single override site, keep the four required top-level arrays present, treat `workflow_overrides` as the conditional machine-discoverable index for active local overrides against `SPEC-SHR-002`, add `artifact_schema_version_compatibility` when schema-version compatibility notes are needed, and record topology deviations in `reverse_recovery_overrides` rather than in prose-only notes.

## Choose the first slice by closure speed, not by architectural importance

The first slice should be small enough to close in one recovery loop and important enough that the result matters if you are right or wrong.

A good first slice usually has four traits:

- it is boundary-observable
- it has clear caller or operator consequence
- it can be tested or bounded by one discriminating question
- it does not require overlays to become legible

Good first-slice candidates include one machine-parsed CLI line, one versioned API field, one file-format rule, one status-surface rule, or one small terminal-state behavior.

Bad first-slice candidates include an entire subsystem, an end-to-end lifecycle with many branches, or a broad claim such as "error handling" or "scheduling behavior."

## Cold-start sequence after reconnaissance

These steps are the mechanical setup sequence after reconnaissance, scope declaration, and first-slice judgment already exist. A first-time practitioner should not read the sequence as a promise that the judgment work itself fits into a fixed time budget.

1. Create `/.nlspec/authoritative-sources.yaml` and `/.nlspec/reverse-recovery.yaml` in the chosen governed workspace. Use `SPEC-SHR-005` when the governed workspace is the target repository. Use `SPEC-SHR-006` when the governed workspace is adjacent and the target repository should remain external during cold start.
2. Write a system sketch or equivalent scope record in `/recovery/recon/system-sketch.md` that makes system identity, the current scope cut, the boundary surfaces or repository-surface families now in view, and the principal external actors or adjacent systems explicit. In adjacent-workspace mode, make the target repository identity and external review coordinates explicit there as well.
3. Record `recovery_objective`, `compatibility_target`, `target_anchor` in normalized axis form, a one-screen draft artifact-role inventory with stable `inventory_item_ref` values, the row-complete `repository_surface_families` record when the target is repository-shaped, and at least one slice seed with stable label, targeted boundary surface, provisional question or clause, and selection reason under `/recovery/recon/`.
4. Open `/recovery/plan.yaml` and convert exactly one seed into one Specification Plan entry.
5. Draft the Slice Contract under `/recovery/slices/<slice-id>/contract.yaml`.
6. Choose `validation_mode` using the chooser below.
7. Execute or close non-executably, then emit the first Validation Report.
8. Update the plan entry before opening a second slice.

This sequence is intentionally small. It is better to close one narrow slice than to open many broad ones with weak support.

The first step is not optional for governed cold-start work. File presence alone does not adopt `SPEC-SHR-002`. The local discovery file does.

## Minimal chooser for `executable` versus `non-executable`

Start with `executable` unless a narrower non-executable closure is clearly justified.

Use `executable` when any of the following is true:

- the clause depends on runtime behavior, sequencing, retries, concurrency, configuration, machine parsing, or environment
- a small replay or probe is safe and realistically obtainable
- the clause would otherwise remain only statically plausible
- parser replay, schema diff, format-invariant replay, configuration-precedence replay, or an equivalent low-floor probe can answer the discriminating question materially

Use `non-executable` only when one of the following is already true:

- execution is unavailable, unsafe, or disproportionate for the current engagement even after considering low-floor probes
- the clause is a narrow interface or boundary fact that already has a decisive existing basis
- the slice is being closed by an explicit authority-bounded rule rather than by fresh execution

A simple decision path is:

```text
Does the clause depend on runtime, state, sequencing, or environment?
  yes -> executable
  no  -> Is a safe low-floor probe realistically obtainable?
           yes -> executable
           no  -> Do stronger existing witnesses or authority closure already decide the narrowed clause?
                    yes -> non-executable
                    no  -> narrow the slice or gather better evidence before closing it
```

Do not choose `non-executable` merely because execution is inconvenient. If executable witnesses are obtainable, runtime-sensitive claims should not be advanced on static evidence alone. For the stable repository-shaped executable-feasibility baseline, use `APP-SHR-004 / Executable validation feasibility floor`. For broader draft operating context, use `APP-SHR-001`.

## Copy-adapt template: first Specification Plan entry

The following entry is non-normative. It is intentionally small and uses a machine-parsed CLI summary line because that shape is boundary-visible, easy to reason about, and easy to adapt.

```yaml
slice_id: cli-summary-line                         # short stable id used in paths and refs
slice_summary: terminal stdout summary line on successful export
recovery_objective: backward-compatible reimplementation
compatibility_target: preserve current release automation contract
target_anchor: revision=repo@<revision> + consumer-version=parser@<release> # use normalized axis=value form; include only materially relevant axes
completeness_dimensions:
  - interface                                      # start with the smallest relevant completeness set
  - boundary
artifact_roles:
  - inventory_item_ref: ARI-0001                # must resolve to the draft artifact-role inventory
    role: machine_parsed_operational_output
    surface: terminal stdout summary line
    mutation_authority: producer-controlled surface
    mutation_conditions: authority-reviewed interface change only
    violation_effect: downstream parser failure
candidate_clause: >
  On successful completion, the CLI emits exactly one terminal summary line
  with a stable prefix and stable key names.
evidence_chain_refs: []                            # explicit [] means no chain-capable evidence refs are attached yet
support_level: observed                            # grounding level, not importance
workflow_state: planned                            # process state, not confidence
disposition: needs-refinement
ambiguity_status: known-open
ambiguity_note: key order and failure-path behavior are not yet recovered
risk_impact: release automation may fail if the line changes
regime_or_environment_status: capture-pending
regime_or_environment_note: need isolated replay with pinned parser revision
negative_evidence_status: not-searched
next_corrective_action: draft the first Slice Contract and capture a canonical success witness
stop_condition: either the canonical line is validated or the clause is narrowed to a smaller stable surface
authority_disposition_needed: false
active_overlays: []                                # most first slices should start without overlays
current_step_id: step:capture-canonical-success-output
step_list:
  - step_id: step:capture-canonical-success-output
    description: capture canonical success output
    proposed_action: run exporter against a known-good fixture corpus
    status: planned
    basis_refs: []
    result_note: null
```

### How to adapt the plan entry

Keep `candidate_clause` narrow and boundary-centered. A first clause should say what must hold at the boundary, not how the current code happens to achieve it.

Use `support_level` for evidentiary grounding and `workflow_state` for process position. They are different records and should not be collapsed into a single confidence judgment.

Use `target_anchor` in the normalized `axis=value` form from SPEC-SHR-002. Omitted axes mean not material for the declared slice. If an axis is material but unresolved, record `axis=unknown` or `axis=capture-pending` rather than omitting it. Do not use temporary paths, local branch names, or secrets.

Each plan-local `artifact_roles` entry should cite the corresponding `inventory_item_ref` from the draft artifact-role inventory. The plan may narrow the surface for the slice, but it should not silently contradict the inventory item it cites. `SPEC-SHR-002` owns that workflow rule. `SPEC-SHR-004` owns the canonical field minima for the plan entry and its linked artifact-role rows.

Use explicit empty lists where the schema gives them meaning. In cold-start work, `evidence_chain_refs: []`, `active_overlays: []`, and a step-local `basis_refs: []` are often the right starting values. On an initial Evidence Bundle, `chains: []` is likewise a useful explicit starting value when no plan-level chain refs have been attached yet.

Leave `authority_disposition_needed` at `false` unless you already know that the first slice turns on a normative decision that only an authority reviewer can make. Use `current_step_id` plus the active step record to make the next governed move explicit rather than infer it from surrounding prose.

## Copy-adapt template: first Slice Contract

The first Slice Contract should ask one discriminating question and define the smallest executable or non-executable action that can answer it.

```yaml
slice_id: cli-summary-line
contract_id: SC-0001                               # stable contract identity, not a temp filename
locator: /recovery/slices/cli-summary-line/contract.yaml
recovery_objective: backward-compatible reimplementation
compatibility_target: preserve current release automation contract
target_anchor: revision=repo@<revision> + consumer-version=parser@<release> # must match the active plan entry or an explicitly revised normalized anchor
candidate_clause: >
  On successful completion, the CLI emits exactly one terminal summary line
  with a stable prefix and stable key names.
discriminating_question: >
  Is the success summary line a stable machine-parsed interface surface for downstream release automation?
validation_mode: executable
required_criteria:
  - criterion_id: C1
    description: one canonical success line is emitted exactly once
    observable_or_witness_class: runtime_witness
    success_threshold: one terminal summary line with the expected prefix appears on success
    refuting_condition: missing line, duplicate line, or wrong prefix
  - criterion_id: C2
    description: downstream parser accepts the captured canonical line
    observable_or_witness_class: compatibility_check
    success_threshold: parser extracts the expected keys without error
    refuting_condition: parser rejects the line or parses different keys
execution_preconditions:
  - known-good fixture corpus available
  - current release parser available
environment_assumptions:
  - exporter run occurs in an isolated non-production workspace
  - parser revision is pinned for the replay
memory_refs_consulted: []                           # explicit [] means consultation happened and nothing applied
mutation_policy: non-mutating replay only
reset_method: restore the fixture snapshot before rerun
expected_observables:
  - one success summary line in stdout
  - parser success on the captured line
witness_capture_path:
  - store stdout transcript under a stable witness ref
  - store parser replay output under a stable witness ref
corrective_next_action: >
  If the parser does not depend on the line, narrow the clause to the smaller boundary surface that is actually stable.
active_overlays: []
executable_action:
  action_kind: controlled_runtime_plus_parser_replay
  execution_harness: shell
  entrypoint: bash
  argument_vector:
    - -lc
  working_directory: /workspace
  input_refs: []
  runnable_payload: >
    Run the exporter against a known-good fixture, capture stdout, and replay the current release parser against the captured line.
  intent: determine whether the success summary line is part of the current automation contract
  expected_observable: canonical output is emitted once and the parser accepts it
  declared_assumptions:
    - the current parser is representative of the in-scope downstream consumer
```

### How to adapt the Slice Contract

Keep the `discriminating_question` singular. If the question contains "and" twice, split the slice or split the criteria.

Write criteria that can fail independently. For a first slice, it is usually enough to separate boundary emission from downstream dependence, as in `C1` and `C2` above.

Use `witness_capture_path` to say where the decisive witnesses will be recoverable after the action runs. The path form is local practice. The important requirement is stable, reviewable witness reference. `contract_ref` always targets `contract_id`, not `locator`, so moving the contract file later does not create a new contract artifact.

Do not mix witnesses from materially different or non-comparable anchors under one contract. If target-anchor comparison changes materially or becomes non-comparable, issue a new contract or explicitly revalidate the current one under the revised anchor. `SPEC-SHR-002` owns the workflow rule. `SPEC-SHR-004` owns the canonical field minima for the contract and any `framing_ablation` subrecord.

For executable work, keep `execution_harness`, `entrypoint`, `argument_vector`, `working_directory`, and `input_refs` machine-actionable enough that a runtime validator does not have to infer the harness shape from prose. Capability class is the requirement. Tool brand is not.

Keep `corrective_next_action` concrete. "Investigate further" is too weak. Name the exact narrowing, replay, or split you will do if the current contract fails.

### If the first Slice Contract is `non-executable`

When `validation_mode` is `non-executable`, keep the base contract shape and replace only the executable-specific fields.

Use this replacement block:

```yaml
validation_mode: non-executable
environment_assumptions: []                        # still required
witness_capture_path:
  - reuse existing decisive witnesses already cited for the slice
corrective_next_action: none; contract closes on current decisive basis
active_overlays: []
non_executable_resolution:
  reason: safe execution is unavailable in the current engagement
  decisive_basis: current boundary artifacts and stronger existing witnesses are sufficient for the narrowed clause
  closure_rule: close only the narrowed clause that the existing basis actually decides
```

When `validation_mode` is `non-executable`, omit `execution_preconditions`, `reset_method`, `expected_observables`, and `executable_action`.

## Copy-adapt template: first Validation Report

Emit the first Validation Report immediately after execution or decisive non-executable closure. Do not wait for a second slice.

```yaml
slice_id: cli-summary-line
contract_ref: SC-0001
witness_refs:
  - EB-0001:success-transcript
  - EB-0001:parser-replay
criterion_results:
  - criterion_id: C1
    witness_ref: EB-0001:success-transcript
    outcome: pass
    observed_divergence: none
    corrective_next_action: none
  - criterion_id: C2
    witness_ref: EB-0001:parser-replay
    outcome: pass
    observed_divergence: none
    corrective_next_action: none
interpretation:
  action_reference: SC-0001.executable_action
  expected_outcome: one canonical success line is emitted and the downstream parser accepts it
  observed_outcome: one success line was emitted and the parser accepted it
  expectation_delta: none
  contractual_significance: the success summary line is part of the current machine-parsed automation surface
  next_move: update the plan entry, raise support as warranted by the witnesses, and decide whether authority review is needed
active_overlays: []
```

### If the first Validation Report is `non-executable`

Use the same outer structure, but set the interpretation to match non-executable closure:

```yaml
interpretation:
  action_reference: none; non-executable closure
  expected_outcome: existing decisive witnesses are sufficient to close the narrowed clause
  observed_outcome: existing witnesses converged without fresh execution
  expectation_delta: none
  contractual_significance: bounded non-executable closure only; runtime-sensitive claims remain open unless separately validated
  next_move: none; slice closed
```

If a validator was used, add the required `validator_record`. If more than one decisive witness materially supports one criterion, include `witness_refs` at that criterion.

The Validation Report inherits its anchor from `contract_ref`. If the anchor changes materially, issue a new Slice Contract or explicitly revalidate the current one under the new anchor. Do not add an ad hoc top-level `target_anchor` field to the report. `SPEC-SHR-004` owns the canonical report minima, including validator and framing-ablation result records.

## If the first slice does not close cleanly

A first slice that fails to close cleanly is still useful if it reduces the next question and updates the governed record immediately. Unless scope or authority requires another route, the safe default is to keep the slice open by setting `workflow_state` to `in-progress`, `disposition` to `needs-refinement`, and `next_corrective_action` to the smallest corrective move.

| Failure mode | Recognition signal | Immediate corrective move | Specification Plan updates | Slice Contract / Validation Report updates |
|---|---|---|---|---|
| Too broad | The slice requires more than one independent decision, more than one runtime condition, or more than one downstream consequence to close. | Split by boundary surface or by one decision. | Narrow `slice_summary`; rewrite `candidate_clause`; set `workflow_state` to `in-progress`; set `disposition` to `needs-refinement`; replace `step_list`; set `current_step_id` to the successor step; update `next_corrective_action`. | Issue a new `discriminating_question`; shrink `required_criteria`; assign a new `contract_id` if the split creates a new governed contract surface. |
| Wrong question | Gathered evidence is real but does not discriminate between live alternatives. | Rewrite the question before gathering more evidence. | Update `ambiguity_note`; keep `workflow_state` at `in-progress`; update `next_corrective_action`; replace or supersede the affected step; move `current_step_id` to the corrective step; attach any new `basis_refs`. | Replace `discriminating_question`; revise `required_criteria`; change `executable_action` or `non_executable_resolution` so the contract closes the right uncertainty rather than a nearby one. |
| Anchor unclear or mixed | The same clause appears to disagree only after normalized `target_anchor` comparison returns `materially-different` or `non-comparable` across revisions, deployments, or environments. | Narrow to one anchor or split the slice by anchor. | Set or refine `target_anchor`; replace mixed-anchor evidence with per-anchor follow-up steps; keep `workflow_state` at `in-progress` unless the anchor change blocks execution; move `current_step_id` to the anchor-specific corrective step. | Do not compare mixed-anchor witnesses under one contract; issue a new contract when target-anchor comparison changes materially or becomes non-comparable; in the report, record anchor-bounded findings rather than fake contradiction. |
| Execution unavailable | Safe execution is unavailable, unsafe, or disproportionate for the current engagement. | Narrow the clause to the decisive existing basis. Switch to `validation_mode: non-executable` only if the narrowed clause is already decided. Otherwise keep the slice open or bound it. | Keep the slice unclosed; update `regime_or_environment_status` and `regime_or_environment_note`; set `authority_disposition_needed` only if closure becomes authority-owned; keep or set `workflow_state` to `in-progress` unless explicit scope or authority routing changes it. | Remove executable-only fields when they no longer apply; add `non_executable_resolution` only for the narrowed clause; in the report, record bounded closure rather than runtime confirmation. |
| Weak evidence | Execution ran, but the criteria fail, block, or remain weaker than the clause. | Record the negative result and open the smallest corrective follow-up. | Set `negative_evidence_status` to `found`; attach `negative_evidence_refs`; adjust or hold `support_level`; set `workflow_state` to `in-progress` or `blocked` according to the failure; update `next_corrective_action`; add a corrective step with explicit `basis_refs`. | Record criterion-level `observed_divergence`; set `criterion_results.corrective_next_action`; revise `required_criteria` or `executable_action` only after the failed basis has been preserved. |

A failed first slice is still governed progress if the plan, contract, report, and negative evidence are updated immediately. For the later-stage analogues of these failure modes, use `APP-SHR-001 / Later-stage failure and correction patterns` and the hard-path composition example in that document.

## What should exist after the first successful loop

At the end of the first loop, a cold-start recovery should have at least:

- one governed-workspace bootstrap file
- one reverse-recovery discovery file
- one system sketch
- one declared `target_anchor`
- one draft artifact-role inventory
- one row-complete `repository_surface_families` record when the target is repository-shaped
- one Specification Plan entry
- one Slice Contract
- one Validation Report
- stable witness refs or a lightweight Evidence Bundle conforming to the effective schema companion, including chain records when `evidence_chain_refs` have been attached, sufficient for review

That is enough to make the engagement governable. It is not enough to make it complete.

At this stage, a repository-shaped draft inventory may still omit `closure_basis_refs` on a `present-in-scope` family row. The omission means promotion-bearing family closure has not yet been linked reviewably. It does not mean the family is already closed.

Under `SPEC-SHR-003`, a first successful loop ordinarily leaves the workspace in `recovery-materialized` posture. It does not imply `promoted-spec-set-present` or `active-spec-set-present`. In adjacent-workspace mode, it also does not imply that the target repository has adopted shared governance or now contains the governed outputs.

## Common first-slice mistakes

The most common cold-start failures are practical rather than theoretical.

First, the slice is too broad. If one contract requires three different runtime conditions to answer, it is probably not a first slice.

Second, the clause is mechanistic. "The exporter calls helper X before helper Y" is almost never a useful first recovered contract.

Third, `non-executable` is used as an escape hatch. If execution is feasible and would materially answer the question, prefer `executable`.

Fourth, the plan is treated as narrative rather than as a queue. `next_corrective_action`, `current_step_id`, and step-local `basis_refs` should drive the next move.

Fifth, the report is delayed until several slices exist. That weakens continuity and makes the first slice hard to review.

## What to do immediately after the first slice closes

After the first slice closes, move first to `APP-SHR-004` for stable repository-shaped practice, then to `APP-SHR-005` when the next seam is authority review or promotion-bearing handoff, and use `APP-SHR-001` only when broader draft guidance is needed. Continue to use `SPEC-SHR-002` and `SPEC-SHR-004` for the governing workflow and schema surfaces.

- Use `APP-SHR-004 / Repository reconnaissance` to refresh the system sketch, artifact-role inventory, and repository-shaped reconnaissance order.
- Use `APP-SHR-001 / Recovery memory in practice` when a tactic or witness is reusable.
- Use `APP-SHR-005 / Packet-first authority intake` when the next question becomes authority-owned.
- Use `APP-SHR-005 / Copy-adapt template: explicit local same-operator authority designation` when a small team needs the same-operator local-authority path.
- Use `APP-SHR-005 / Boundary-clean promotion transformation` and `APP-SHR-005 / Promotion-trace assembly and review checks` when promotion-bearing handoff begins.
- Use `APP-SHR-001 / Partial and incremental recovery` when work pauses before promotion.
- Use `APP-SHR-001 / Expanded workspace topology in practice` when the workspace must grow beyond the minimal cold-start layout.
- Use `APP-SHR-004 / Repository-shaped structure recovery patterns` when repository-shaped structure surfaces begin to drive slice choice and closure.

The first slice should reduce ambiguity, not create a second informal process beside the governed one.
