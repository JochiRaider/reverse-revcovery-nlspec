---
doc_id: SPEC-SHR-006
title: Minimum Adjacent-Workspace Cold-Start Profile for Reverse Recovery
status: active
version: 0.1.0
related_docs:
  - SPEC-SHR-000
  - SPEC-SHR-001
  - SPEC-SHR-002
  - SPEC-SHR-003
  - SPEC-SHR-004
  - SPEC-SHR-005
  - APP-SHR-002
  - APP-SHR-003
  - APP-SHR-004
  - APP-SHR-005
---

# Minimum Adjacent-Workspace Cold-Start Profile for Reverse Recovery

## Purpose and boundary

This specification defines one short normative profile for the minimum adjacent-workspace reverse-recovery start state used when a target repository is naive to the shared document taxonomy and the recovery effort should not yet materialize governance or promoted outputs inside that target repository.

The profile is intentionally narrow. It assembles, in one place, the minimum authoritative stack already owned by `SPEC-SHR-000`, `SPEC-SHR-002`, `SPEC-SHR-003`, and `SPEC-SHR-004` for the adjacent-workspace case. It does not create a second bootstrap surface, a second workflow specification, a second topology specification, a second schema specification, or a second Promotion Gate.

Read owner documents for owner meaning. Read this document for the minimum adjacent-workspace assembly rule only. If reverse recovery is being run inside the target repository itself, use `SPEC-SHR-005` instead.

## Conformance language

The key words `MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, and `MAY` indicate requirement strength.

- `MUST` and `MUST NOT` indicate absolute requirements for conformance.
- `SHOULD` and `SHOULD NOT` indicate strong recommendations. A governed workspace may depart from them only with a deliberate reason.
- `MAY` indicates an allowed option.

## Applicability profile

This profile applies when all of the following are true.

1. reverse recovery is being run in a governed workspace that is separate from the target repository
2. the target repository is naive to the shared taxonomy at the point recovery begins
3. the cold-start governed outputs are intended to live in the adjacent workspace rather than in the target repository
4. any later transfer of promoted outputs into the target repository, if it happens at all, will be a separate explicit local adoption or materialization event

This profile does not invalidate discovery-only adoption or other conforming local topologies allowed by `SPEC-SHR-003`. It defines the minimum adjacent-workspace governed route only.

## Conformance subject and external-target rule

The table below is authoritative for how this profile interprets the governed workspace and the target repository during cold start.

| Interface | Required interpretation |
|---|---|
| conformance subject | the adjacent governed workspace, not the unmodified target repository |
| local bootstrap and reverse-recovery discovery | live in the adjacent workspace only |
| default governed output home during cold start | the adjacent workspace `document_roots` |
| reverse-recovery working tree during cold start | the adjacent workspace `recovery_root` |
| target repository during cold start | an external recovery target identified reviewably through the working record rather than through local bootstrap presence |
| later transfer of promoted outputs into the target repository | a separate explicit event, not an implied consequence of adjacent-workspace recovery |
| `local_override_locator` | a locator inside the adjacent workspace that resolves to one scanned local active `SPEC`; it does not point into the target repository by proximity alone |

This profile does not create a second discovery-file field for target coordinates. Reviewable target identity and replayability remain assembled from the scope record, normalized `target_anchor`, stable witness locators, and other owner-defined working records.

## Owner map for the minimum authoritative stack

The table below is authoritative for profile assembly. It does not transfer owner meaning away from the owner document.

| Cold-start obligation | What must exist or resolve explicitly | Authoritative owner | First point required |
|---|---|---|---|
| local bootstrap in the adjacent workspace | `/.nlspec/authoritative-sources.yaml` with explicit `document_roots` such as `/specs` | `SPEC-SHR-000` | before the first governed adjacent-workspace recovery loop |
| reverse-recovery adoption in the adjacent workspace | `/.nlspec/reverse-recovery.yaml` | `SPEC-SHR-003` | before the first Slice Contract |
| effective schema companion | explicit `recovery_artifact_schema_spec_ref` or unique resolution through the paired-companion rule | `SPEC-SHR-003` | before governed artifacts are validated or promoted under the adopted split-release line |
| system sketch or equivalent scope record | a current declared scope record that makes system identity, declared scope cut, primary boundary surfaces or repository-surface families in view, and principal external actors or adjacent systems explicit | `SPEC-SHR-002` | before the first Slice Contract |
| recovery objective | explicit `recovery_objective` | `SPEC-SHR-002` | before the first Slice Contract |
| compatibility target | explicit `compatibility_target` | `SPEC-SHR-002` | before the first Slice Contract |
| target anchor | normalized `target_anchor` | `SPEC-SHR-002` | before the first Slice Contract |
| external target reference basis | a reviewable target identity basis assembled from the scope record, normalized `target_anchor`, and stable decisive-witness locators | `SPEC-SHR-002` workflow entry and witness-reviewability rules | before the first Slice Contract |
| draft artifact-role inventory | stable `inventory_item_ref` values plus the minimum inventory row shape | workflow meaning in `SPEC-SHR-002`; field minima in `SPEC-SHR-004` | before the first Slice Contract |
| repository-surface-family record when the target is repository-shaped | row-complete `repository_surface_families` record | workflow meaning in `SPEC-SHR-002`; field minima in `SPEC-SHR-004` | before the first Slice Contract |
| first slice seed | stable seed label, targeted boundary surface, provisional question or clause, and explicit selection reason; when the seed already lives inside the first plan entry, that entry satisfies this obligation directly | `SPEC-SHR-002` | before the first Slice Contract |
| first Specification Plan entry | one slice-backed plan entry | workflow meaning in `SPEC-SHR-002`; field minima in `SPEC-SHR-004` | before the first Slice Contract |
| first Slice Contract | one contract for one discriminating question | workflow meaning in `SPEC-SHR-002`; field minima in `SPEC-SHR-004` | before nontrivial critique, validation, or non-executable closure |
| first Validation Report | one report tied to the first contract | workflow meaning in `SPEC-SHR-002`; field minima in `SPEC-SHR-004` | immediately after executable validation or decisive non-executable closure |

## Corpus-default carrier and resolution map

Unless an authoritative local override site names a different carrier once, the corpus-default adjacent-workspace cold-start carriers are:

| Surface | Corpus-default carrier or resolution | Default meaning |
|---|---|---|
| governed document roots | `/specs` through explicit `document_roots` in the adjacent workspace | promoted governed outputs live under the adjacent bootstrap scan roots rather than in the recovery tree |
| reverse-recovery working-tree root | `/recovery` through `recovery_root` in the adjacent workspace | the recovery tree is outside adjacent `document_roots` |
| governing Specification Plan | `/recovery/plan.yaml` when `plan_locator` is omitted | the default plan carrier in the adjacent workspace |
| artifact-role inventory carrier | `/recovery/recon/artifact-roles.yaml` | the default inventory carrier in the adjacent workspace |
| recovery-memory root | `/recovery/memory/` when `memory_locator` is omitted | the default recovery-memory root in the adjacent workspace |
| slice-working root | `/recovery/slices/` | the default slice-working root in the adjacent workspace |
| effective governed-artifact schema companion | paired default or explicit `recovery_artifact_schema_spec_ref` | omission is acceptable only when one unique effective companion resolves |
| external target reference basis | current scope record, normalized `target_anchor`, and stable witness locators | the target repository remains external and reviewable without a second discovery-file field |

This profile does not add a second discovery-file field for target coordinates or for target bootstrap state. Use `SPEC-SHR-003` plus the single `local_override_locator` site when a non-default carrier is needed inside the adjacent workspace. When no locally declared recovery-memory profile exists and the target is repository-shaped, the shared default operational profile at the resolved memory root is `APP-SHR-001 / Recovery memory in practice`; conformance remains owned by `SPEC-SHR-002`, and a different local mechanism remains valid only when it preserves that document's minimum queryable interface and preserved distinctions.

## Minimum materialization order

The order below is the default and binding minimum order for this profile.

| Stage | What must be materialized or resolved | Default posture consequence | Stop condition before the next stage |
|---|---|---|---|
| Stage 1 | bootstrap file and reverse-recovery discovery file in the adjacent workspace | `bootstrap-governed` plus `recovery-adopted` when both files conform in that workspace | the adjacent workspace can be entered without guessing and the recovery tree is declared without modifying the target repository |
| Stage 2 | system sketch or equivalent scope record with explicit system identity, declared scope cut, primary boundary surfaces or repository-surface families in view, and principal external actors or adjacent systems; explicit `recovery_objective`; explicit `compatibility_target`; normalized `target_anchor`; reviewable external target reference basis; draft artifact-role inventory; row-complete `repository_surface_families` when repository-shaped; and at least one first slice seed with stable label, targeted boundary surface, provisional question or clause, and selection reason | entry minima are explicit but the first slice may still be unopened | an independent reader can explain the declared scope, the external target being examined, the first targeted boundary surface, the provisional question or clause, and why that slice was chosen first |
| Stage 3 | materialized adjacent `recovery_root`, governing plan carrier, inventory carrier, recovery-memory root, and slice-working root | `recovery-materialized` under `SPEC-SHR-003` | the adjacent workspace is materially usable for governed reverse recovery |
| Stage 4 | one Specification Plan entry and one Slice Contract | active first-loop governed work exists in the adjacent workspace | the first discriminating question is explicit and reviewable |
| Stage 5 | one Validation Report tied to the first contract | first governed loop has closed or bounded one slice | the engagement is governable even though it is not yet complete |

A workspace with both `/.nlspec/` files but without the Stage 3 materialized recovery-control surfaces is `recovery-adopted`, not `recovery-materialized`.

## Default first-loop rules

This profile imports the following defaults from owner documents and makes them explicit as the profile baseline.

1. The first slice SHOULD be boundary-observable, narrow, and chosen for closure speed rather than for architectural prestige.
2. `active_overlays` defaults to `[]` for the first slice unless the slice genuinely needs an overlay.
3. `validation_mode` defaults to `executable` when a safe low-floor executable probe can materially answer the discriminating question.
4. Omission of `recovery_artifact_schema_spec_ref` is acceptable only when `SPEC-SHR-003` resolves one unique effective schema companion.
5. Omission of `closure_basis_refs` on a repository-surface-family row means promotion-bearing family closure has not yet been linked reviewably. It does not mean the family is absent, out of scope, or already closed.
6. `local_override_locator`, when present, resolves only inside the adjacent workspace and only to one scanned local active `SPEC`.
7. A workspace-local copy of target material is an evidence aid by default. It does not become the target of record unless the working record makes its relation to the external target reviewable enough for replay and handoff.
8. Transfer of promoted outputs into the target repository is not implied by adjacent-workspace success. It requires a separate explicit local action.

Operational mirrors of this profile live in `APP-SHR-002`. Stable authority-closure and promotion-handoff guidance lives in `APP-SHR-005`. Corpus routing for first-time readers lives in `APP-SHR-003`.

## Invalid shortcuts

The following shortcuts are invalid under this profile.

- Treating adjacent-workspace bootstrap as if it were target-repository bootstrap
- Treating adjacency alone as proof that the target repository has adopted shared governance or reverse recovery
- Pointing `local_override_locator` into the target repository or into any workspace that is not bootstrap-governed locally
- Omitting a currently known boundary-bearing surface from the artifact-role inventory
- Omitting a required repository-surface-family row for a repository-shaped target
- Silently changing default carriers instead of using the single local override site defined by `SPEC-SHR-003`
- Letting temporary host-local paths, editor cursors, credentials, or secret-bearing locators stand in for external target identity
- Treating a workspace-local mirror or copied file as the sole decisive basis when materially reviewable external target coordinates exist
- Treating discovery-file presence as proof of `recovery-materialized`
- Rephrasing workflow, topology, or schema owner rules in this document as if this profile were a second source of truth

## Definition of Done

A governed workspace conforms to this profile when all of the following are true.

1. The workspace satisfies the adjacent-workspace applicability profile above.
2. `/.nlspec/authoritative-sources.yaml` exists in the adjacent workspace and makes the governed output roots explicit.
3. `/.nlspec/reverse-recovery.yaml` exists in the adjacent workspace and resolves the governing workflow version plus the effective governed-artifact schema companion uniquely.
4. The adjacent recovery tree is structurally separated from the adjacent governed document roots.
5. The target repository remains external during cold start and is identified reviewably enough through the scope record, normalized `target_anchor`, and stable decisive-witness locators that replay and handoff do not depend on conversational memory.
6. Before the first Slice Contract, the working record makes a system sketch or equivalent scope record explicit with system identity, declared scope cut, primary boundary surfaces or repository-surface families in view, and principal external actors or adjacent systems, together with `recovery_objective`, `compatibility_target`, normalized `target_anchor`, a draft artifact-role inventory, a row-complete `repository_surface_families` record when the target is repository-shaped, and at least one slice seed that states a stable label, targeted boundary surface, provisional question or clause, and selection reason.
7. The minimum materialized recovery-control surfaces required for `recovery-materialized` are present in the adjacent workspace before the first governed loop is treated as materially usable.
8. One Specification Plan entry, one Slice Contract, and one Validation Report can be located reviewably through the adopted topology and schema companion in the adjacent workspace.
9. No sentence in this document is being used to override owner meaning housed in `SPEC-SHR-000`, `SPEC-SHR-002`, `SPEC-SHR-003`, or `SPEC-SHR-004`.

Nothing else is required by this specification.
