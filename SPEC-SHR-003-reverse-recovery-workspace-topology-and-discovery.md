---
doc_id: SPEC-SHR-003
title: Reverse-Recovery Workspace Topology and Discovery
status: active
version: 0.7.0
related_docs:
  - SPEC-SHR-000
  - SPEC-SHR-002
  - SPEC-SHR-004
  - SPEC-SHR-005
  - SPEC-SHR-006
  - APP-SHR-004
  - APP-SHR-005
---

# Reverse-Recovery Workspace Topology and Discovery

## Purpose and boundary

This specification defines the local structural surface by which a repository or adjacent governed workspace declares that it is conducting reverse recovery under `SPEC-SHR-002` and, when applicable, which governed-artifact schema companion is adopted for future work.

It standardizes five related surfaces:

- the local reverse-recovery adoption and discovery file
- effective artifact-schema-companion resolution for that discovery file
- the corpus-default minimum topology rooted at `recovery_root`
- the derived workspace structural posture model
- the transition rules by which structural posture changes

This specification therefore lets a reader discover, without guesswork:

- the adopted reverse-recovery workflow version
- the effective governed-artifact schema companion for future work
- the root of the reverse-recovery working tree
- the governing Specification Plan locator
- the designated recovery-memory locator
- the latest Recovery Snapshot locator when one is designated
- the authoritative local override site when one is designated
- the corpus-default carriers for the minimum recovery control surfaces
- the derived structural posture of the workspace from present repository surfaces rather than from a writable summary field
- any machine-discoverable workflow override index when active local overrides against `SPEC-SHR-002` exist

This specification does not redefine `SPEC-SHR-002` workflow-state semantics, support-level semantics, Promotion Gate logic, evidence hierarchy, or consuming-domain override semantics. It does not redefine `SPEC-SHR-004` artifact schemas, field minima, or overlay matrix rules. It does not redefine `SPEC-SHR-000` bootstrap, front-matter, lifecycle, or local binding rules. It does not create target-system behavioral authority, and it does not govern target-system structure recovery such as build graphs, deployment topology, configuration planes, generated artifacts, extension surfaces, data stores, or service seams. Those target-system structure questions remain outside this specification's boundary.

The discovery file adopts `SPEC-SHR-002` for reverse-recovery workflow use within the local workspace and, when resolved explicitly or by default, the governing `SPEC-SHR-004` schema companion for future work in that workspace. It does not, by itself, make any recovered target-system clause locally binding. Local binding of promoted governed documents continues to follow `SPEC-SHR-000` lifecycle and adoption rules unless a stricter authoritative local rule says otherwise.

For a short normative cold-start assembly, use `SPEC-SHR-005` for in-repository entry and `SPEC-SHR-006` for adjacent-workspace entry. For stable repository-shaped operating guidance after the first loop, use `APP-SHR-004`. For stable authority closure and promotion handoff, use `APP-SHR-005`. Broader and more revisable operating practice remains in `APP-SHR-001`.

## Conformance language

The key words `MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, and `MAY` indicate requirement strength.

- `MUST` and `MUST NOT` indicate absolute requirements for conformance.
- `SHOULD` and `SHOULD NOT` indicate strong recommendations. A workspace may depart from them only with a deliberate reason.
- `MAY` indicates an allowed option.

## Versioning

This specification uses semantic versioning in `MAJOR.MINOR.PATCH` form. The `version` field in this document's front matter, the version portion of `recovery_workflow_spec_ref`, and the version portion of `recovery_artifact_schema_spec_ref` all use this form.

- A `PATCH` revision clarifies or corrects the specification without changing its requirements.
- A `MINOR` revision adds backward-compatible surface area.
- A `MAJOR` revision changes or removes existing rules.

A workspace conforms to the version it adopts.

In the discovery file, `recovery_workflow_spec_ref` always names the currently governing `SPEC-SHR-002` version for future work in the workspace. `recovery_artifact_schema_spec_ref`, when present, always names the currently governing `SPEC-SHR-004` schema companion for future work in the workspace.

Changing either field is not historical commentary. The same change set MUST also carry either the required recovery-artifact migrations or an explicit authoritative compatibility note naming which retained artifacts remain valid unchanged under the new adopted version line.

A schema-only revision may require artifact migration or a compatibility note even when `recovery_workflow_spec_ref` does not change. When `local_override_locator` is present, `reverse_recovery_overrides.workflow_version_compatibility` and `reverse_recovery_overrides.artifact_schema_version_compatibility` MAY serve as the machine-discoverable homes of those compatibility notes.

Pre-split workflow versions remain authoritative for both workflow and inline schema ownership unless the effective artifact-schema resolution rules below resolve them to an external companion explicitly.

## Repository entry point

A repository or governed workspace that uses this specification MUST expose a discovery file at the repository-root-relative path `/.nlspec/reverse-recovery.yaml`.

This file MAY coexist with `/.nlspec/authoritative-sources.yaml`. The two files govern different surfaces. `SPEC-SHR-000` continues to govern bootstrap discovery, document classes, front matter, lifecycle, and governed-document authority. This specification governs reverse-recovery local adoption, topology, structural posture, and discovery.

This specification uses repository-root-relative POSIX paths that begin with `/`. The leading `/` is repository-root-relative, not a host-filesystem absolute path.


### Discovery file fields

The discovery file has two required fields and five optional fields.

| Field | Type | Presence | Meaning |
|---|---|---|---|
| `recovery_workflow_spec_ref` | string | REQUIRED | Adopted `SPEC-SHR-002` version, in the form `SPEC-SHR-002@<MAJOR.MINOR.PATCH>`. It names the currently governing workflow version for future work in the workspace. |
| `recovery_root` | string | REQUIRED | Repository-root-relative path to the reverse-recovery working tree |
| `recovery_artifact_schema_spec_ref` | string | OPTIONAL | Adopted `SPEC-SHR-004` schema companion, in the form `SPEC-SHR-004@<MAJOR.MINOR.PATCH>`. It names the currently governing governed-artifact schema companion for future work in the workspace. |
| `plan_locator` | string | OPTIONAL | Repository-root-relative path to the governing Specification Plan |
| `memory_locator` | string | OPTIONAL | Repository-root-relative path to the designated recovery-memory root |
| `latest_snapshot_locator` | string | OPTIONAL | Repository-root-relative path to the currently designated Recovery Snapshot |
| `local_override_locator` | string | OPTIONAL | Repository-root-relative path to the single authoritative local active `SPEC` document whose front matter carries the `reverse_recovery_overrides` block defined by this specification |

The default rules are:

- if `recovery_artifact_schema_spec_ref` is omitted, resolve the effective schema companion through `Effective artifact schema companion resolution`
- if `plan_locator` is omitted, its value is `<recovery_root>/plan.yaml`
- if `memory_locator` is omitted, its value is `<recovery_root>/memory/`
- if `latest_snapshot_locator` is omitted, no current Recovery Snapshot is designated
- if `local_override_locator` is omitted, no machine-discoverable local override site is declared
- if `local_override_locator` is present, `SPEC-SHR-000` bootstrap MUST also be present in the same repository or governed workspace and the target MUST resolve to a scanned local active `SPEC`; otherwise the discovery file is invalid

Unknown fields are allowed. Tooling that implements only this specification MUST ignore fields that are not defined by this section.

This schema is intentionally small. A consuming domain MUST NOT add new discovery-file fields merely to restate corpus-default topology carriers that can already be derived from `recovery_root` or declared once at `local_override_locator`.

A minimal discovery file that uses the paired companion default is:

```yaml
recovery_workflow_spec_ref: SPEC-SHR-002@0.13.0
recovery_root: /recovery
```

A fuller discovery file that adopts the schema companion explicitly is:

```yaml
recovery_workflow_spec_ref: SPEC-SHR-002@0.13.0
recovery_artifact_schema_spec_ref: SPEC-SHR-004@0.13.0
recovery_root: /recovery
plan_locator: /recovery/plan.yaml
memory_locator: /recovery/memory/
latest_snapshot_locator: /recovery/snapshots/RS-0007.yaml
local_override_locator: /specs/SPEC-004-reverse-recovery-local-rules.md
```

### Effective artifact schema companion resolution

A reader MUST resolve the effective artifact schema companion as follows.

1. If `recovery_artifact_schema_spec_ref` is present, use it.
2. Else, if `recovery_workflow_spec_ref` matches a row in the following default-companion table, use the mapped companion.
3. Else, if the adopted workflow version predates the split release and no companion row exists, treat artifact schema authority as inline in that adopted `SPEC-SHR-002` version.
4. Else, the discovery file is invalid because the schema owner cannot be resolved uniquely.

The default-companion table is:

| `recovery_workflow_spec_ref` | Default `effective_recovery_artifact_schema_spec_ref` | Note |
|---|---|---|
| `SPEC-SHR-002@0.11.0` | `SPEC-SHR-004@0.11.0` | first split release and paired companion default |
| `SPEC-SHR-002@0.12.0` | `SPEC-SHR-004@0.12.0` | previous paired companion default |
| `SPEC-SHR-002@0.13.0` | `SPEC-SHR-004@0.13.0` | current paired companion default |

String substitution by filename or version number alone is not the resolution rule. The explicit table above is authoritative.

When the effective schema owner is inline in a pre-split workflow version, later schema-only version movement is not available until the workspace adopts a split-release workflow line and a resolvable schema companion.

### Locator constraints

All locator fields in this specification MUST satisfy the following constraints.

- The value MUST be a repository-root-relative POSIX path.
- The value MUST be stable and reviewable within the local workspace.
- The value MUST NOT contain a credential, secret-bearing token, temporary search cursor, editor-local handle, or host-filesystem absolute path.
- `recovery_root` MUST identify the root of the reverse-recovery working tree. If `SPEC-SHR-000` bootstrap is present in the same repository, `recovery_root` MUST live outside `document_roots` or be excluded from local governed-document discovery.
- `plan_locator`, when present, MUST locate a file at or below `recovery_root`.
- `memory_locator`, when present, MUST locate a directory or designated future directory at or below `recovery_root`.
- `latest_snapshot_locator`, when present, MUST locate a file at or below `recovery_root`.
- `local_override_locator`, when present, MAY point outside `recovery_root`, but it MUST still remain inside the local repository or governed workspace, `SPEC-SHR-000` bootstrap MUST also be present in that same repository or governed workspace, and the target MUST be one scanned local active `SPEC` whose front matter carries the `reverse_recovery_overrides` block defined by this specification.

A discovery file MAY be created before every referenced recovery artifact exists. In that case, the locator names the designated future location. The locator does not prove that the target file already exists.

The same path constraints apply to any non-default topology carrier, legacy designation note, workflow-version compatibility note, schema-version compatibility note, or workflow-override `local_rule_ref` declared inside the `reverse_recovery_overrides` block.

## Workspace structural posture and adjacent records

Workspace structural posture is the repository- or workspace-level structural condition of a reverse-recovery effort. It answers a different question than slice workflow, snapshot maturity, or local governed-document lifecycle.

Structural posture is derived from observable repository surfaces. It is not a writable field, not a front-matter status, and not a replacement for `SPEC-SHR-002` workflow records.

A repository or workspace MAY satisfy more than one posture predicate at once. Readers MAY refer to the highest satisfied predicate as a diagnostic summary, but that summary is informative only. It does not create a new authority surface.

The distinction table below is authoritative for corpus-level record separation.

| Record family | Governing document | Canonical schema form | What it answers | What it does not answer |
|---|---|---|---|---|
| workspace structural posture | `SPEC-SHR-003` | derived only; no writable field | whether bootstrap, reverse-recovery adoption, materialized recovery topology, and recovery-derived governed outputs are structurally present | whether a slice is open, whether evidence is strong, whether promotion is ready, or whether a local `SPEC` is good |
| workflow state | `SPEC-SHR-002` | `workflow_state` | where one slice sits in the recovery process | whether the workspace has a bootstrap, a materialized topology, or an active promoted `SPEC` |
| promotion readiness | `SPEC-SHR-002` | `promotion_readiness` | how mature a Recovery Snapshot is for later promotion review | whether the workspace is bootstrap-governed, recovery-materialized, or locally active |
| governed-document lifecycle status | `SPEC-SHR-000` | `status` | whether one governed document is `draft`, `active`, `superseded`, or another class-valid lifecycle state | whether the recovery workspace itself is adopted or materialized |

### Derived-not-declared rule

A conforming workspace MUST NOT introduce a writable `structural_posture` field, a front-matter alias for posture, or an equivalent machine summary that claims independent authority. Structural posture is derived from present surfaces under this specification. Manual declaration by prose implication, by status imitation, or by an added metadata field is non-conforming.

## Corpus-default topology

This section defines the corpus-default minimum topology rooted at `recovery_root`. It defines the default carriers for the minimum reverse-recovery control surfaces. It does not standardize every optional subtree that a larger engagement may later add.

Unless an authoritative local site named by `local_override_locator` declares a different carrier once, the corpus-default topology is:

| Structural surface | Corpus-default carrier or root | Required for posture | Absence meaning under the default topology |
|---|---|---|---|
| recovery working-tree root | `<recovery_root>/` | `recovery-adopted` and later | the root is only designated, not yet materialized |
| governing Specification Plan | resolved `plan_locator`, default `<recovery_root>/plan.yaml` | `recovery-materialized` and later | the workspace is not yet materially usable for governed recovery work |
| artifact-role inventory carrier | `<recovery_root>/recon/artifact-roles.yaml` | `recovery-materialized` and later | the workspace is not yet materially usable for governed recovery work |
| slice-working root | `<recovery_root>/slices/` | `recovery-materialized` and later | no materialized slice-working surface exists yet |
| recovery-memory root | resolved `memory_locator`, default `<recovery_root>/memory/` | `recovery-materialized` and later | no materialized recovery-memory root exists yet |
| Recovery Snapshot subtree | `<recovery_root>/snapshots/` | optional before any snapshot exists | no default snapshot subtree is materialized yet |
| current Recovery Snapshot designation | resolved `latest_snapshot_locator` when present | never required for adoption or materialization by itself | no current Recovery Snapshot is designated |
| local override site | resolved `local_override_locator` when present | optional at every posture | no machine-discoverable local override site is declared |
| governed output roots | `SPEC-SHR-000` `document_roots`, or the `SPEC-SHR-000` omission default when bootstrap is present | `promoted-spec-set-present` and `active-spec-set-present` | no scanned governed output roots are available for promoted local `SPEC` documents |

The corpus-default minimum topology intentionally separates two families of surfaces:

- the recovery working tree under `recovery_root`
- the governed document roots discovered through `SPEC-SHR-000`

When bootstrap is present, the separation invariant is mandatory: `recovery_root` and any non-governed input corpus MUST live outside `document_roots` or be excluded explicitly. Reverse-recovery working artifacts and promoted governed outputs are different artifact families and MUST NOT be interleaved.


### Non-default topology declarations

A consuming domain MAY use non-default carriers for the artifact-role inventory, the slice-working root, the recovery-memory root, the Recovery Snapshot subtree, or equivalent minimum topology surfaces. It MUST declare those deviations once in the `reverse_recovery_overrides.topology_overrides` array at the authoritative site named by `local_override_locator`.

A valid `topology_overrides` entry MUST name:

- `structural_surface`
- `replacement_path`
- `override_kind`
- `compatibility_note`

Silence, path conventions in examples, or ad hoc relocation of one artifact instance do not override the corpus-default topology.

## Derived structural posture model

This section defines the derived posture predicates used by this specification. These predicates are cumulative in the ordinary progressive route, but they are not mutually exclusive lifecycle states.

The ordinary progressive route is:

`bootstrap-governed` -> `recovery-adopted` -> `recovery-materialized` -> `promoted-spec-set-present` -> `active-spec-set-present`

A discovery-only workspace may satisfy `recovery-adopted` before it is bootstrap-governed. Such a workspace remains outside the governed-output part of the posture ladder until bootstrap is added. Discovery-only adoption therefore remains structurally valid for adjacent-workspace or reconnaissance-first situations. When the adjacent workspace is intended to be the governed cold-start workspace rather than discovery-only, use `SPEC-SHR-006`. When the target repository itself is the governed cold-start workspace, use `SPEC-SHR-005`.

For in-repository reverse recovery that intends to create local governed outputs in the same repository, bootstrap should exist before the first Slice Contract and must exist before the first local governed output is created or counted. `Bootstrap-governed` remains required before promoted or active governed outputs can count under local posture. This clarification changes no posture predicate. It makes the intended cold-start path explicit.

### Local-override validity matrix

A machine-discoverable local override site is valid only when bootstrap-governed local authority already exists. The matrix below is authoritative for the interaction between bootstrap presence and `local_override_locator`.

| Bootstrap presence in the same repository or governed workspace | `local_override_locator` state | Structural validity | Meaning |
|---|---|---|---|
| absent | omitted | valid | discovery-only adoption or adjacent-workspace adoption uses corpus-default topology and posture rules only |
| absent | present | invalid | machine-discoverable local overrides require a bootstrap-governed local active `SPEC` carrier |
| present | omitted | valid | bootstrap-governed recovery uses corpus-default topology and posture rules only |
| present | present | valid only when the target is a scanned local active `SPEC` carrying `reverse_recovery_overrides` | machine-discoverable local overrides are available through the single authoritative local site |

| Posture predicate | Required present surfaces | Allowed absent surfaces | Meaning | What it does not imply |
|---|---|---|---|---|
| `ungoverned-target` | no `SPEC-SHR-000` bootstrap file and no `/.nlspec/reverse-recovery.yaml` discovery file | all governed and recovery surfaces may be absent | the repository exposes no local NLSpec bootstrap and no reverse-recovery adoption surface | it does not imply that the repository has no documentation, no local conventions, or no future recovery effort |
| `bootstrap-governed` | conforming `/.nlspec/authoritative-sources.yaml` under `SPEC-SHR-000` | discovery file and all recovery surfaces may be absent | the repository has governed-document scan roots, document classes, front matter, lifecycle, and local authority semantics | it does not imply that reverse recovery is adopted, that any recovery tree exists, or that any promoted document is active |
| `recovery-adopted` | conforming `/.nlspec/reverse-recovery.yaml` discovery file | plan, artifact-role inventory carrier, slice-working root, memory root, and any snapshot may still be absent | the repository or governed workspace has adopted the reverse-recovery workflow and declared its discovery coordinates | it does not imply that the recovery tree is materially usable, that bootstrap is present, or that any slice has been opened |
| `recovery-materialized` | `recovery-adopted`, a materialized `recovery_root`, the resolved governing Specification Plan, the resolved recovery-memory root, the default or overridden artifact-role inventory carrier, and the default or overridden slice-working root | a current Recovery Snapshot may still be absent; promoted local `SPEC` documents may still be absent | the workspace has the minimum materialized topology needed for governed reverse-recovery work | it does not imply that any slice is closed, that promotion is ready, or that any local `SPEC` has been promoted or activated |
| `promoted-spec-set-present` | `bootstrap-governed` and at least one scanned local `SPEC` that qualifies as a recovery-derived governed output under this specification | an active local `SPEC` may still be absent; open recovery work may remain | one or more recovery-derived governed `SPEC` outputs are present under the governed document roots | it does not imply that those `SPEC` documents are locally binding, that all recovered scope has been promoted, or that recovery work is finished |
| `active-spec-set-present` | `promoted-spec-set-present` and at least one qualifying recovery-derived local `SPEC` with `status: active` | other recovery-derived `SPEC` documents may still be `draft`; open slices may remain | at least one recovery-derived local `SPEC` is locally active under `SPEC-SHR-000` lifecycle rules | it does not imply that every promoted output is active, that no additional slices remain open, or that ratification and activation were coupled |

A repository or workspace that wants a stricter posture ladder MAY declare stricter local posture rules at `local_override_locator`. A stricter rule may add requirements. It MUST NOT silently relax the required present surfaces above.

## Structural transition rules

Structural posture changes only when repository or workspace surfaces change in a way that satisfies a new posture predicate. Slice-local workflow events do not advance posture by themselves.

The transition table below is authoritative for the ordinary posture-changing edits.

| Repository or workspace change | Newly satisfied predicate | Required follow-on obligations | What the change does not imply |
|---|---|---|---|
| add a conforming `SPEC-SHR-000` bootstrap file | `bootstrap-governed` | honor `SPEC-SHR-000` scan roots, authority rules, and lifecycle semantics | reverse recovery is not yet adopted merely because bootstrap exists |
| add a conforming `/.nlspec/reverse-recovery.yaml` discovery file | `recovery-adopted` | resolve topology defaults from `recovery_root` and honor any declared local override site | the recovery tree is not yet materialized merely because discovery exists |
| materialize the minimum recovery-control surfaces required by `recovery-materialized` | `recovery-materialized` | keep the recovery tree outside bootstrap scan roots when bootstrap is present; for in-repository recovery that intends local governed outputs, ensure bootstrap already exists before first governed slice work continues | no slice is closed and no promoted local `SPEC` exists merely because the topology is now usable |
| add one or more recovery-derived local `SPEC` documents under the governed document roots | `promoted-spec-set-present` | preserve recovery-origin lineage through `derived_from` or an explicit local legacy designation | local activation is not implied merely because a promoted `SPEC` exists |
| move at least one qualifying recovery-derived local `SPEC` to `status: active` | `active-spec-set-present` | honor `SPEC-SHR-000` lifecycle semantics and any stricter local activation rule | open recovery work, bounded slices, and non-active promoted outputs may still exist |

The following operations do **not** advance workspace structural posture by themselves:

- opening a slice
- changing `workflow_state`, `support_level`, `disposition`, `ambiguity_status`, or `promotion_readiness`
- emitting a Validation Report
- creating or refreshing a Recovery Snapshot
- emitting an Authority Review Packet or Authority Disposition
- adding an informative `APP`, `ADR`, `RFC`, or `RES`
- adding shared informative inputs outside the governed document roots

The following shortcuts are invalid:

- a discovery file alone MUST NOT be treated as proof of `recovery-materialized`
- a discovery file with `local_override_locator` but no bootstrap MUST NOT be treated as valid discovery-only adoption
- a designated `latest_snapshot_locator` alone MUST NOT be treated as proof of `promoted-spec-set-present`
- a ratified packet or disposition alone MUST NOT be treated as proof of `active-spec-set-present`
- a `local_override_locator` that points at a `draft` local `SPEC`, an informative document, or an arbitrary Markdown note MUST NOT be treated as a conforming authoritative local override site
- a local `SPEC` lacking normalized recovery-origin lineage MUST NOT be counted as a recovery-derived governed output unless the authoritative local override site explicitly designates it

These shortcut invalidations do not remove discovery-only adoption. They make clear that discovery alone is not enough for materialized or governed-output posture and that in-repository cold-start work should add bootstrap before first governed slice work when local outputs are intended.

## Recovery-derived governed outputs and lineage

This specification reuses the existing `derived_from` front-matter field defined by `SPEC-SHR-000`. It does not create a new lineage field, a promotion index file, or a second registry.

For reverse-recovery-derived governed outputs, the corpus-default lineage form carried in `derived_from` is:

```text
recovery:plan=<plan-ref>;slices=<slice-ref>[,<slice-ref>...][;snapshot=<snapshot-ref>][;packet=<packet-ref>]
```

The segment rules are:

| Segment | Include when | Meaning |
|---|---|---|
| `plan` | always for slice-backed recovery-derived governed outputs | stable reference to the governing Specification Plan |
| `slices` | always for slice-backed recovery-derived governed outputs | contributing slice refs |
| `snapshot` | only when a paused or resumed Recovery Snapshot materially shaped the result | stable reference to the controlling Recovery Snapshot |
| `packet` | only when a bounded Authority Review Packet materially closed the promoted scope | stable reference to the controlling authority packet |

The minimum conforming recovery-origin lineage is `plan` plus `slices`. Do not insert placeholders for `snapshot` or `packet` when they were not materially controlling.

This structured lineage form is governed by this specification only. `SPEC-SHR-000` continues to treat `derived_from` as a string field. Tooling that implements only `SPEC-SHR-000` MUST treat the value as opaque.

A local governed `SPEC` qualifies as a recovery-derived governed output for posture purposes when all of the following are true:

1. the document is discovered under the local governed document roots governed by `SPEC-SHR-000`
2. the document is a local `SPEC`
3. `derived_from` uses the corpus-default reverse-recovery lineage form above, or the authoritative local override site explicitly designates the document as a legacy recovery-derived output
4. the referenced recovery plan and slice refs are stable and reviewable within the local recovery corpus


### Legacy recovery-derived output designation

Older local `SPEC` documents may predate the normalized lineage form. They MUST NOT silently count as recovery-derived outputs for posture purposes.

A legacy designation is sufficient only when one entry in `reverse_recovery_overrides.legacy_recovery_output_designations` names:

- the local governed document by stable `doc_id`
- the controlling `plan_ref`
- the contributing `slice_refs`
- the controlling `snapshot_ref` or `packet_ref` when one materially shaped the output

A legacy designation is transitional. New recovery-derived governed outputs SHOULD use normalized `derived_from` rather than relying on local designation.

## Authoritative local override site

When `local_override_locator` is present, it identifies the single authoritative local site for reverse-recovery topology deviations, stricter posture rules, workflow-version compatibility notes, legacy recovery-derived-output designation, and machine-discoverable indexing of active local overrides against `SPEC-SHR-002`.

That site is valid only when `SPEC-SHR-000` bootstrap is present in the same repository or governed workspace. The target MUST be a Markdown document that is discovered as a local `SPEC` under that bootstrap and that currently carries `status: active`. A `draft` local `SPEC`, an `APP`, an `RFC`, a `RES`, or an arbitrary Markdown note cannot satisfy this section. Body prose may explain or justify the declarations. The front matter block is the authoritative machine-readable source for override discoverability and topology declarations. It is not a second workflow or schema owner.

### `reverse_recovery_overrides` front-matter block

The front-matter block has four required top-level arrays, one conditional top-level array, and one optional top-level array.

| Top-level array | Presence | Purpose | Default if the array is empty or omitted |
|---|---|---|---|
| `topology_overrides` | REQUIRED with empty allowed | names non-default carriers for corpus-default topology surfaces | corpus-default topology governs |
| `stricter_posture_rules` | REQUIRED with empty allowed | adds stricter local posture predicates or stricter required surfaces | posture predicates are derived exactly as defined in this specification |
| `workflow_version_compatibility` | REQUIRED with empty allowed | records authoritative compatibility notes for workflow-version changes and retained artifacts | no workflow-version compatibility note is declared beyond same-change-set migration work |
| `legacy_recovery_output_designations` | REQUIRED with empty allowed | designates legacy local `SPEC` documents that still count as recovery-derived outputs for posture purposes | a legacy local `SPEC` does not count as recovery-derived for posture purposes |
| `workflow_overrides` | CONDITIONAL | indexes active local overrides against `SPEC-SHR-002` that govern future work in the workspace | omission means no machine-discoverable workflow override is declared; omission is invalid when such active overrides exist |
| `artifact_schema_version_compatibility` | OPTIONAL | records authoritative compatibility notes for schema-companion changes and retained artifacts | omission means no schema-version compatibility note is declared |

Minimum subrecord requirements are:

| Subrecord family | Minimum required fields |
|---|---|
| `topology_overrides` | `structural_surface`, `replacement_path`, `override_kind`, `compatibility_note` |
| `stricter_posture_rules` | `posture_predicate`, `added_required_surfaces`, `stricter_condition` |
| `workflow_version_compatibility` | `from_workflow_spec_ref`, `to_workflow_spec_ref`, `retained_artifact_refs`, `compatibility_note` |
| `legacy_recovery_output_designations` | `doc_id`, `plan_ref`, `slice_refs`, and any materially controlling `snapshot_ref` or `packet_ref` |
| `workflow_overrides` | `affected_site_ref`, `override_kind`, `local_rule_ref`, `compatibility_note` |
| `artifact_schema_version_compatibility` | `from_artifact_schema_spec_ref`, `to_artifact_schema_spec_ref`, `retained_artifact_refs`, `compatibility_note` |

The block rules are:

- if `local_override_locator` is omitted, no machine-discoverable override site exists
- if `local_override_locator` is present, prose-only override semantics are invalid
- empty required arrays mean no override of that class is declared
- omission of `workflow_overrides` means no machine-discoverable workflow override is declared
- explicit `workflow_overrides: []` means the local override site exists and currently declares no workflow override entries
- omission of `artifact_schema_version_compatibility` means no schema-version compatibility note is declared
- the body may explain the declarations, but it MUST NOT silently broaden, narrow, or contradict the front-matter block
- the override block MAY be stricter than the corpus default; it MUST NOT silently relax locator constraints, the recovery-root separation invariant, the derived-not-declared posture rule, or effective schema-companion resolution

Migration note. A workspace that historically pointed `local_override_locator` at a `draft` local `SPEC` or at a non-governed Markdown note must either: (1) add or confirm `SPEC-SHR-000` bootstrap, reissue the carrier as a scanned local active `SPEC`, and preserve any needed compatibility note in that carrier; or (2) remove `local_override_locator` and fall back to the corpus-default topology until a conforming carrier exists. Historical tolerance does not make the older carrier authoritative for future advancing work.

`Workflow_overrides` is an index only. It does not replace the authoritative local rule that actually makes the override binding. Workflow meaning and override validity remain owned by `SPEC-SHR-002`.

This specification does not define same-operator local-authority designation. That question remains governed by `SPEC-SHR-000` together with the relevant `SPEC-SHR-002` authority-review rules.

## Reader interpretation

A reader that encounters `/.nlspec/reverse-recovery.yaml` MUST interpret it as follows.

1. `recovery_workflow_spec_ref` declares the currently governing reverse-recovery workflow version for future work in the workspace.
2. `recovery_artifact_schema_spec_ref`, when present, declares the currently governing governed-artifact schema companion for future work in the workspace.
3. When `recovery_artifact_schema_spec_ref` is omitted, resolve the effective schema companion through `Effective artifact schema companion resolution`.
4. `recovery_root` declares where the reverse-recovery working tree lives.
5. `plan_locator` identifies the governing Specification Plan. When `plan_locator` is omitted, use `<recovery_root>/plan.yaml`.
6. `memory_locator` identifies the designated recovery-memory root. When `memory_locator` is omitted, use `<recovery_root>/memory/`.
7. `latest_snapshot_locator`, when present, identifies the Recovery Snapshot that should be treated as the current resumable handoff surface. When omitted, no current snapshot is designated.
8. `local_override_locator`, when present, identifies the authoritative local active `SPEC` whose front matter carries `reverse_recovery_overrides`. This route is valid only when `SPEC-SHR-000` bootstrap is present in the same repository or governed workspace. When omitted, the corpus-default topology and posture rules apply without a machine-discoverable local override site.
9. Unless the authoritative override block says otherwise, resolve the corpus-default artifact-role inventory carrier to `<recovery_root>/recon/artifact-roles.yaml`, the corpus-default slice-working root to `<recovery_root>/slices/`, and the corpus-default Recovery Snapshot subtree to `<recovery_root>/snapshots/`.
10. Read any override semantics from the `reverse_recovery_overrides` front matter first. Body prose may explain those declarations, but it does not replace them.
11. When `workflow_overrides` is present, treat it as the machine-discoverable index of active local overrides against `SPEC-SHR-002` for future work in the workspace. Follow the cited `local_rule_ref` records for the actual binding local rule.
12. Derive workspace structural posture from the present repository or workspace surfaces under this specification. Do not look for a writable `structural_posture` field, and do not infer posture from `workflow_state`, `promotion_readiness`, or local governed-document `status`.
13. Discovery-only `recovery-adopted` without bootstrap is structurally possible for discovery-only or adjacent-workspace situations, but discovery-only adoption must omit `local_override_locator` because machine-discoverable local overrides require a bootstrap-governed local active `SPEC` carrier. Use `SPEC-SHR-006` when the adjacent workspace is the governed cold-start workspace and the target repository should remain external during cold start. For in-repository reverse recovery that intends local governed outputs, use `SPEC-SHR-005`; bootstrap should already exist before the first Slice Contract and must exist before the first local governed output is created or counted.
14. This file adopts the reverse-recovery workflow and its structural discovery points, resolves the governing schema companion for future artifact validation, and points to the single local override site when one exists. It does not, by itself, promote, ratify, activate, or bind any target-system behavioral clause.

## Interaction with SPEC-SHR-000, SPEC-SHR-002, and SPEC-SHR-004

`SPEC-SHR-000` allows a shared `SPEC` to define its own local adoption surface. This specification is that adoption surface for `SPEC-SHR-002` and, when resolved explicitly or by default, for `SPEC-SHR-004`. `SPEC-SHR-000` continues to govern bootstrap, governed-document classes, front matter, lifecycle, and local binding. When this specification names `local_override_locator`, it relies on `SPEC-SHR-000` discovery and lifecycle semantics for local active `SPEC` documents and does not create a locator-only authority exception. This specification does not restate those rules.

`SPEC-SHR-000` also defines `derived_from` as an optional string field. This specification defines the corpus-default reverse-recovery lineage subform used inside that field for recovery-derived governed outputs.

`SPEC-SHR-002` continues to govern reverse-recovery workflow semantics, evidence handling, authority review, Recovery Snapshot meaning, Promotion Gate logic, and ratification requirements. `SPEC-SHR-004` governs governed-artifact field minima, omission semantics, overlay field minima, and worked schema examples. This specification names the local discovery points for those adopted surfaces, defines the corpus-default minimum topology in which those artifacts ordinarily live, and defines the derived structural posture model for the workspace. It does not restate or replace `SPEC-SHR-002` or `SPEC-SHR-004` requirements.

Target-system structure recovery remains outside this specification. Stable baseline guidance for repository-shaped target-system structure recovery now lives in `APP-SHR-004`. Stable authority-closure and promotion-handoff guidance now lives in `APP-SHR-005`. Broader and more revisable operating practice remains in `APP-SHR-001`.

## Definition of Done

A repository or governed workspace conforms to this specification when all of the following are true.

1. `/.nlspec/reverse-recovery.yaml` exists.
2. `recovery_workflow_spec_ref` is present and names the adopted `SPEC-SHR-002` version in the form `SPEC-SHR-002@<MAJOR.MINOR.PATCH>`.
3. `recovery_artifact_schema_spec_ref`, when present, names the adopted `SPEC-SHR-004` version in the form `SPEC-SHR-004@<MAJOR.MINOR.PATCH>`.
4. `recovery_root` is present and uses a valid repository-root-relative POSIX path.
5. `plan_locator`, `memory_locator`, `latest_snapshot_locator`, and `local_override_locator` either satisfy the locator constraints above or are omitted with the default meanings defined by this specification.
6. The effective governed-artifact schema companion resolves uniquely through `Effective artifact schema companion resolution`.
7. The corpus-default minimum topology is resolved from `recovery_root` and the existing locator fields unless an authoritative local override site explicitly names a non-default carrier in `reverse_recovery_overrides.topology_overrides`.
8. Workspace structural posture is derived from present repository or workspace surfaces under this specification and is not represented as a writable field.
9. If `SPEC-SHR-000` bootstrap is present in the same repository, `recovery_root` lives outside `document_roots` or is excluded from local governed-document discovery.
10. A local governed `SPEC` counts as a recovery-derived governed output for posture purposes only when its lineage is represented by the corpus-default `derived_from` form or by an explicit entry in `reverse_recovery_overrides.legacy_recovery_output_designations`.
11. When `local_override_locator` is present, `SPEC-SHR-000` bootstrap is present in the same repository or governed workspace, the target document is Markdown, the target is a scanned local active `SPEC`, its front matter includes the `reverse_recovery_overrides` block, all four required top-level arrays are present, `workflow_overrides` is present whenever active local overrides against `SPEC-SHR-002` govern future work in the workspace, and no prose-only override semantics are treated as normative.
12. When `recovery_workflow_spec_ref` changes, the same change set also records the required artifact migrations or an authoritative compatibility note for unchanged artifacts in `reverse_recovery_overrides.workflow_version_compatibility`.
13. When the effective artifact schema companion changes, the same change set also records the required artifact migrations or an authoritative compatibility note for unchanged artifacts in `reverse_recovery_overrides.artifact_schema_version_compatibility`, when that array is present.
14. Discovery-only adoption and in-repository cold-start are interpreted using the clarification in `Derived structural posture model` and `Reader interpretation`; discovery alone is not misread as proof of materialized or governed-output posture, and discovery-only adoption with `local_override_locator` is not treated as valid.
15. The discovery file and the derived structural posture model are used only for reverse-recovery adoption, topology, and discovery. They are not misread as sources of target-system behavioral authority.

Nothing else is required by this specification.
