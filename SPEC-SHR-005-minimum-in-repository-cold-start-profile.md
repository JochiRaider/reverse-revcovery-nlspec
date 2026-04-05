---
doc_id: SPEC-SHR-005
title: Minimum In-Repository Cold-Start Profile for Reverse Recovery
status: active
version: 0.2.0
related_docs:
  - SPEC-SHR-000
  - SPEC-SHR-001
  - SPEC-SHR-002
  - SPEC-SHR-003
  - SPEC-SHR-004
  - SPEC-SHR-006
  - APP-SHR-002
  - APP-SHR-003
  - APP-SHR-004
  - APP-SHR-005
---

# Minimum In-Repository Cold-Start Profile for Reverse Recovery

## Purpose and boundary

This specification defines one short normative profile for the minimum in-repository reverse-recovery start state used when a repository is naive to the shared document taxonomy and the recovery effort intends to create local governed outputs in that same repository.

The profile is intentionally narrow. It assembles, in one place, the minimum authoritative stack already owned by `SPEC-SHR-000`, `SPEC-SHR-002`, `SPEC-SHR-003`, and `SPEC-SHR-004`. It does not create a second bootstrap surface, a second workflow specification, a second topology specification, a second schema specification, or a second Promotion Gate.

Read owner documents for owner meaning. Read this document for the minimum assembly rule only. If recovery is being run in a separate governed workspace and the target repository should remain external during cold start, do not stretch this profile by implication. Use `SPEC-SHR-006` instead.

## Conformance language

The key words `MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, and `MAY` indicate requirement strength.

- `MUST` and `MUST NOT` indicate absolute requirements for conformance.
- `SHOULD` and `SHOULD NOT` indicate strong recommendations. A repository may depart from them only with a deliberate reason.
- `MAY` indicates an allowed option.

## Applicability profile

This profile applies when all of the following are true.

1. reverse recovery is being run inside the target repository rather than only in an adjacent workspace
2. the target repository is naive to the shared taxonomy at the point recovery begins
3. the recovery effort intends to create local governed outputs in the same repository if promotion later succeeds

This profile does not invalidate adjacent-workspace recovery, discovery-only adoption, or other conforming local topologies allowed by `SPEC-SHR-003`. It defines the minimum in-repository route only. The parallel adjacent-workspace route is `SPEC-SHR-006`.

## Owner map for the minimum authoritative stack

The table below is authoritative for profile assembly. It does not transfer owner meaning away from the owner document.

| Cold-start obligation | What must exist or resolve explicitly | Authoritative owner | First point required |
|---|---|---|---|
| local bootstrap | `/.nlspec/authoritative-sources.yaml` with explicit `document_roots` such as `/specs` | `SPEC-SHR-000` | before the first governed in-repository recovery loop |
| reverse-recovery adoption | `/.nlspec/reverse-recovery.yaml` | `SPEC-SHR-003` | before the first Slice Contract |
| effective schema companion | explicit `recovery_artifact_schema_spec_ref` or unique resolution through the paired-companion rule | `SPEC-SHR-003` | before governed artifacts are validated or promoted under the adopted split-release line |
| system sketch or equivalent scope record | a current declared scope record that makes system identity, declared scope cut, primary boundary surfaces or repository-surface families in view, and principal external actors or adjacent systems explicit | `SPEC-SHR-002` | before the first Slice Contract |
| recovery objective | explicit `recovery_objective` | `SPEC-SHR-002` | before the first Slice Contract |
| compatibility target | explicit `compatibility_target` | `SPEC-SHR-002` | before the first Slice Contract |
| target anchor | normalized `target_anchor` | `SPEC-SHR-002` | before the first Slice Contract |
| draft artifact-role inventory | stable `inventory_item_ref` values plus the minimum inventory row shape | workflow meaning in `SPEC-SHR-002`; field minima in `SPEC-SHR-004` | before the first Slice Contract |
| repository-surface-family record when the target is repository-shaped | row-complete `repository_surface_families` record | workflow meaning in `SPEC-SHR-002`; field minima in `SPEC-SHR-004` | before the first Slice Contract |
| first slice seed | stable seed label, targeted boundary surface, provisional question or clause, and explicit selection reason; when the seed already lives inside the first plan entry, that entry satisfies this obligation directly | `SPEC-SHR-002` | before the first Slice Contract |
| first Specification Plan entry | one slice-backed plan entry | workflow meaning in `SPEC-SHR-002`; field minima in `SPEC-SHR-004` | before the first Slice Contract |
| first Slice Contract | one contract for one discriminating question | workflow meaning in `SPEC-SHR-002`; field minima in `SPEC-SHR-004` | before nontrivial critique, validation, or non-executable closure |
| first Validation Report | one report tied to the first contract | workflow meaning in `SPEC-SHR-002`; field minima in `SPEC-SHR-004` | immediately after executable validation or decisive non-executable closure |

## Corpus-default carrier and resolution map

Unless an authoritative local override site names a different carrier once, the corpus-default in-repository cold-start carriers are:

| Surface | Corpus-default carrier or resolution | Default meaning |
|---|---|---|
| governed document roots | `/specs` through explicit `document_roots` | promoted governed outputs live under the bootstrap scan roots rather than in the recovery tree |
| reverse-recovery working-tree root | `/recovery` through `recovery_root` | the recovery tree is outside `document_roots` |
| governing Specification Plan | `/recovery/plan.yaml` when `plan_locator` is omitted | the default plan carrier |
| artifact-role inventory carrier | `/recovery/recon/artifact-roles.yaml` | the default inventory carrier |
| recovery-memory root | `/recovery/memory/` when `memory_locator` is omitted | the default recovery-memory root |
| slice-working root | `/recovery/slices/` | the default slice-working root |
| effective governed-artifact schema companion | paired default or explicit `recovery_artifact_schema_spec_ref` | omission is acceptable only when one unique effective companion resolves |

This profile does not add a second discovery-file field for any of these surfaces. Use `SPEC-SHR-003` plus the single `local_override_locator` site when a non-default carrier is needed. When no locally declared recovery-memory profile exists and the target is repository-shaped, the shared default operational profile at the resolved memory root is `APP-SHR-001 / Recovery memory in practice`; conformance remains owned by `SPEC-SHR-002`, and a different local mechanism remains valid only when it preserves that document's minimum queryable interface and preserved distinctions.

## Minimum materialization order

The order below is the default and binding minimum order for this profile.

| Stage | What must be materialized or resolved | Default posture consequence | Stop condition before the next stage |
|---|---|---|---|
| Stage 1 | bootstrap file and reverse-recovery discovery file | `bootstrap-governed` plus `recovery-adopted` when both files conform | the repository can be entered without guessing and the recovery tree is declared |
| Stage 2 | system sketch or equivalent scope record with explicit system identity, declared scope cut, primary boundary surfaces or repository-surface families in view, and principal external actors or adjacent systems; explicit `recovery_objective`; explicit `compatibility_target`; normalized `target_anchor`; draft artifact-role inventory; row-complete `repository_surface_families` when repository-shaped; and at least one first slice seed with stable label, targeted boundary surface, provisional question or clause, and selection reason | entry minima are explicit but the first slice may still be unopened | an independent reader can explain the declared scope, the first targeted boundary surface, the provisional question or clause, and why that slice was chosen first |
| Stage 3 | materialized `recovery_root`, governing plan carrier, inventory carrier, recovery-memory root, and slice-working root | `recovery-materialized` under `SPEC-SHR-003` | the workspace is materially usable for governed reverse recovery |
| Stage 4 | one Specification Plan entry and one Slice Contract | active first-loop governed work exists | the first discriminating question is explicit and reviewable |
| Stage 5 | one Validation Report tied to the first contract | first governed loop has closed or bounded one slice | the engagement is governable even though it is not yet complete |

A workspace with both `/.nlspec/` files but without the Stage 3 materialized recovery-control surfaces is `recovery-adopted`, not `recovery-materialized`.

## Default first-loop rules

This profile imports the following defaults from owner documents and makes them explicit as the profile baseline.

1. The first slice SHOULD be boundary-observable, narrow, and chosen for closure speed rather than for architectural prestige.
2. `active_overlays` defaults to `[]` for the first slice unless the slice genuinely needs an overlay.
3. `validation_mode` defaults to `executable` when a safe low-floor executable probe can materially answer the discriminating question.
4. Omission of `recovery_artifact_schema_spec_ref` is acceptable only when `SPEC-SHR-003` resolves one unique effective schema companion.
5. Omission of `closure_basis_refs` on a repository-surface-family row means promotion-bearing family closure has not yet been linked reviewably. It does not mean the family is absent, out of scope, or already closed.
6. Discovery-only adoption does not, by itself, satisfy materialized topology requirements.

Operational mirrors of this profile live in `APP-SHR-002`. Stable authority-closure and promotion-handoff guidance lives in `APP-SHR-005`. Corpus routing for first-time readers lives in `APP-SHR-003`.

## Invalid shortcuts

The following shortcuts are invalid under this profile.

- Treating discovery-file presence as proof of `recovery-materialized`
- Omitting a currently known boundary-bearing surface from the artifact-role inventory
- Omitting a required repository-surface-family row for a repository-shaped target
- Silently changing default carriers instead of using the single local override site defined by `SPEC-SHR-003`
- Treating a discovery-only or bootstrap-only repository as if the first governed recovery loop had already been opened
- Rephrasing workflow, topology, or schema owner rules in this document as if this profile were a second source of truth
- Treating this in-repository profile as if it also governed adjacent-workspace cold start

## Definition of Done

A repository conforms to this profile when all of the following are true.

1. The repository satisfies the in-repository applicability profile above.
2. `/.nlspec/authoritative-sources.yaml` exists and makes the governed output roots explicit.
3. `/.nlspec/reverse-recovery.yaml` exists and resolves the governing workflow version plus the effective governed-artifact schema companion uniquely.
4. The recovery tree is structurally separated from the governed document roots.
5. Before the first Slice Contract, the working record makes a system sketch or equivalent scope record explicit with system identity, declared scope cut, primary boundary surfaces or repository-surface families in view, and principal external actors or adjacent systems, together with `recovery_objective`, `compatibility_target`, normalized `target_anchor`, a draft artifact-role inventory, a row-complete `repository_surface_families` record when the target is repository-shaped, and at least one slice seed that states a stable label, targeted boundary surface, provisional question or clause, and selection reason.
6. The minimum materialized recovery-control surfaces required for `recovery-materialized` are present before the first governed loop is treated as materially usable.
7. One Specification Plan entry, one Slice Contract, and one Validation Report can be located reviewably through the adopted topology and schema companion.
8. No sentence in this document is being used to override owner meaning housed in `SPEC-SHR-000`, `SPEC-SHR-002`, `SPEC-SHR-003`, or `SPEC-SHR-004`.

Nothing else is required by this specification.
