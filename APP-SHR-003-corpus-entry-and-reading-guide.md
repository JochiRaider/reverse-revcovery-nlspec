---
doc_id: APP-SHR-003
title: Corpus Entry and Reading Guide for the Shared Reverse-Recovery Corpus
status: active
version: 0.8.0
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
  - APP-SHR-004
  - APP-SHR-005
  - RES-SHR-001
---

# Corpus Entry and Reading Guide for the Shared Reverse-Recovery Corpus

## Purpose and boundary

This document is an informative entry guide for the shared reverse-recovery corpus. Its job is to reduce entry cost for readers and repositories that are naive to the taxonomy by routing them to the authoritative documents and sections they actually need.

It does not create a new authority surface, a new schema, a new adoption mechanism, or a new promotion rule. `SPEC-SHR-000` remains the authoritative document for bootstrap, document classes, front matter, lifecycle, and local adoption semantics. `SPEC-SHR-003` remains the authoritative document for reverse-recovery workspace topology, structural posture, discovery, and local adoption of the workflow spec plus, when applicable, its schema companion. `SPEC-SHR-002` remains the authoritative document for reverse-recovery workflow, authority review, evidence discipline, promoted-output requirements, and Promotion Gate logic. `SPEC-SHR-004` remains the authoritative document for governed-artifact schema minima, overlay matrix rules, and worked artifact examples. `SPEC-SHR-005` and `SPEC-SHR-006` remain the short normative cold-start profiles for in-repository and adjacent-workspace entry respectively. `SPEC-SHR-001` remains the grounding document for NLSpec quality.

Document order in this guide is a reading convenience only. It does not create binding force by itself. This guide owns routing only. `APP-SHR-002` owns the APP-layer quickstart, first-loop templates, and first-loop correction. `APP-SHR-004` owns the stable repository-shaped operating baseline after the first loop. `APP-SHR-005` owns the stable authority-closure and promotion-handoff baseline. `APP-SHR-001` remains the broader draft companion for revisable operating material outside those stabilized baselines, and `RES-SHR-001` remains the auxiliary compact reading aid.

## Conformance-boundary note between `SPEC-SHR-001` and `SPEC-SHR-002`

Read the boundary between these two documents as follows.

- `SPEC-SHR-001 / Conformance Scope and Interpretive Boundary` is the interpretive entry point for deciding what carries conformance weight inside the grounding document.
- `SPEC-SHR-001` remains the quality target for what a good NLSpec is.
- Appendix D remains advisory within `SPEC-SHR-001`.
- `SPEC-SHR-002 / Imported NLSpec tests and recovery-specific restatements` is the authoritative site where recovery-specific binding imports and restatements are made explicit.
- `SPEC-SHR-004` does not change that import boundary. It carries schema minima after `SPEC-SHR-002` has already made the workflow-owned binding imports explicit.
- `SPEC-SHR-005` and `SPEC-SHR-006` assemble the minimum cold-start stack only. They do not redefine the import boundary.
- APP documents may explain how to apply that boundary. They do not change it.

## Reading rule

Use this guide in the following order:

1. Identify the role you are performing now.
2. Read only the minimum authoritative set for that role first.
3. Add APP and RES documents only when they reduce ambiguity or operating cost.
4. When in doubt, normative `SPEC` documents control, `SPEC-SHR-001` defines the NLSpec quality target, and APP/RES documents support but do not bind.
5. Do not collapse `SPEC-SHR-003` workspace structural posture into `SPEC-SHR-002` `workflow_state`, `promotion_readiness`, or `SPEC-SHR-000` document `status`. Those are different records with different owners.

## Foundation packet classification

This guide uses three reading tiers for repositories that are naive to the taxonomy. The tiers are routing only. They do not create a new authority class.

| Classification | Documents | Use |
|---|---|---|
| Initial foundation packet | `SPEC-SHR-000` through `SPEC-SHR-004`, exactly one short cold-start profile (`SPEC-SHR-005` for in-repository work or `SPEC-SHR-006` for adjacent-workspace work), `APP-SHR-002`, `APP-SHR-003` | minimum first-time packet for opening the first conforming loop |
| Stabilized baselines | `APP-SHR-004`, `APP-SHR-005` | stable operating baselines after the first-loop foundation packet is understood |
| Auxiliary seam-triggered companions | `APP-SHR-001`, `RES-SHR-001` | read only when a named wider seam or recall need actually triggers them |

## Corpus role map

| Document | Authority class | Primary use | Read when |
|---|---|---|---|
| `SPEC-SHR-000` | normative bootstrap specification | local repository bootstrap, governed-document classes, front matter, lifecycle, and adoption semantics | you need to know how governed documents are discovered, named, activated, or made locally authoritative |
| `SPEC-SHR-001` | grounding quality authority | NLSpec quality target, completeness, recreatability, conceptual fidelity, economy, and recovered-spec evaluation | you need to judge whether a recovered or promoted spec is actually good enough |
| `SPEC-SHR-002` | normative reverse-recovery workflow specification | slice workflow, workflow-state transitions, revalidation recomputation, evidence, authority review, promoted-output requirements, and Promotion Gate | you are opening, reviewing, resuming, or promoting reverse-recovery work |
| `SPEC-SHR-003` | normative reverse-recovery topology specification | local adoption of `SPEC-SHR-002` and, when applicable, `SPEC-SHR-004`; reverse-recovery workspace topology; structural posture; recovery-tree discovery; and machine-readable local override discovery | reverse recovery is being run inside a local repository or adjacent governed workspace and you need to know what structural state the workspace is in |
| `SPEC-SHR-004` | normative reverse-recovery schema specification | governed-artifact schema minima, overlay matrix rules, omission semantics, continuity keys, and worked artifact examples | you need canonical field minima, omission semantics, or example shapes for recovery artifacts |
| `SPEC-SHR-005` | normative cold-start profile specification | shortest authoritative statement of the minimum in-repository reverse-recovery start state | the governed workspace is the target repository and you need the minimum normative stack without reconstructing it from several owner documents |
| `SPEC-SHR-006` | normative cold-start profile specification | shortest authoritative statement of the minimum adjacent-workspace reverse-recovery start state | the governed workspace is adjacent to the target repository and the target repository should remain external during cold start |
| `APP-SHR-002` | informative cold-start companion in the initial foundation packet | corpus-default quickstart, first-slice templates, placement chooser, machine-readable local override example, and first-loop failure handling | you need to open the first conforming slice quickly after the normative stack is clear |
| `APP-SHR-004` | informative stabilized repository-shaped baseline | stable repository-shaped reconnaissance order, executable-feasibility floor, target-anchor axis mapping, and repository-surface-family operating patterns | you have passed the first-loop entry pack and now need stable repository-shaped operating guidance |
| `APP-SHR-005` | informative stabilized authority and promotion baseline | stable packet-first authority intake, same-operator disclosure exemplar, minimum viable disposition practice, and promotion-handoff assembly | the next seam is authority review, same-operator disclosure, or promotion-bearing handoff |
| `APP-SHR-001` | informative auxiliary draft operational companion | broader repository-shaped examples, the shared default recovery-memory profile, partial recovery, multi-slice composition, and other revisable operating material outside the stabilized baselines | you already know where the stable baselines live and now need a wider draft seam |
| `RES-SHR-001` | informative auxiliary reading aid | compact route-family index for workflow-state reading and disposition/support interaction | you need fast orientation after the authoritative transition and recomputation rules in `SPEC-SHR-002` are already understood |

## Default reading path for a first-time cold-start practitioner

The default first-time path consumes the initial foundation packet only. It does not require `APP-SHR-001` or `RES-SHR-001` before the first conforming loop.

1. Read `SPEC-SHR-000 / Purpose and boundary`, `SPEC-SHR-000 / Repository entry point`, `SPEC-SHR-000 / Authority rules`, `SPEC-SHR-000 / Front matter`, and `SPEC-SHR-000 / Status semantics`.
2. Choose the placement mode for the governed workspace. Read `SPEC-SHR-005` in full when the governed workspace is the target repository. Read `SPEC-SHR-006` in full when the governed workspace is adjacent and the target repository should remain external during cold start.
3. Read `SPEC-SHR-003 / Purpose and boundary`, `SPEC-SHR-003 / Discovery file fields`, `SPEC-SHR-003 / Effective artifact schema companion resolution`, `SPEC-SHR-003 / Corpus-default topology`, `SPEC-SHR-003 / Workspace structural posture and adjacent records`, `SPEC-SHR-003 / Derived structural posture model`, `SPEC-SHR-003 / Local-override validity matrix`, `SPEC-SHR-003 / Structural transition rules`, `SPEC-SHR-003 / Authoritative local override site`, and `SPEC-SHR-003 / Reader interpretation`.
4. Read `SPEC-SHR-001 / Conformance Scope and Interpretive Boundary` so the NLSpec-quality boundary is explicit before recovery-specific imports are read.
5. Read `SPEC-SHR-002 / Purpose, Artifact Class, Audience, and Authority Boundary`, `SPEC-SHR-002 / Imported NLSpec tests and recovery-specific restatements`, `SPEC-SHR-002 / Glossary and Normalization`, `SPEC-SHR-002 / Recovery Inputs and Artifact Roles / Control-plane prose and artifact-role inventory`, `SPEC-SHR-002 / Workflow Overview`, `SPEC-SHR-002 / Workflow Overview / Entry minima before first Slice Contract`, `SPEC-SHR-002 / Workflow Overview / Minimum content floor for the system sketch and slice seed`, `SPEC-SHR-002 / Workflow Overview / Target-anchor normalized form and comparison rules`, `SPEC-SHR-002 / Default workflow-state transitions`, `SPEC-SHR-002 / Revalidation-triggered workflow-state recomputation`, `SPEC-SHR-002 / Authority review operating model`, and `SPEC-SHR-002 / Governed Working Artifacts`.
6. Read `SPEC-SHR-004 / Artifact-role inventory minimum shape and linkage`, `SPEC-SHR-004 / Repository-surface-family record for repository-shaped targets`, `SPEC-SHR-004 / Default slice artifact continuity and assembly`, `SPEC-SHR-004 / Specification Plan`, `SPEC-SHR-004 / Slice Contract`, `SPEC-SHR-004 / Validation Report`, `SPEC-SHR-004 / Other governed artifacts / Evidence Bundle`, `SPEC-SHR-004 / Other governed artifacts / Authority Review Packet`, `SPEC-SHR-004 / Other governed artifacts / Authority Disposition`, `SPEC-SHR-004 / Recovery snapshots and incremental resumption`, and `SPEC-SHR-004 / Overlays`.
7. Read `APP-SHR-002 / When to use this document`, `APP-SHR-002 / Placement-mode chooser`, `APP-SHR-002 / What must exist before the first Slice Contract`, `APP-SHR-002 / The smallest viable workspace`, `APP-SHR-002 / Copy-adapt template: local override for non-default topology`, `APP-SHR-002 / Copy-adapt template: first Specification Plan entry`, `APP-SHR-002 / Copy-adapt template: first Slice Contract`, `APP-SHR-002 / Copy-adapt template: first Validation Report`, and `APP-SHR-002 / If the first slice does not close cleanly`.
8. Read `APP-SHR-004 / Repository reconnaissance`, `APP-SHR-004 / Executable validation feasibility floor`, `APP-SHR-004 / Repository-shaped target-anchor axis mapping`, and `APP-SHR-004 / Repository-shaped structure recovery patterns` when the first loop has closed or when stable repository-shaped practice is now needed.
9. Read `APP-SHR-005 / Packet-first authority intake`, `APP-SHR-005 / Review-mode choice in practice`, `APP-SHR-005 / Copy-adapt template: explicit local same-operator authority designation`, `APP-SHR-005 / Minimum viable Authority Disposition`, and `APP-SHR-005 / Promotion-trace assembly and review checks` when the next seam is authority review or promotion-bearing handoff.
10. Use `APP-SHR-001` only when the engagement now needs broader draft guidance, revisable composition examples, later-stage failure patterns, recovery-memory profile detail, or another wider operational seam.
11. Use `RES-SHR-001` only as an auxiliary compact aid after the authoritative transition and recomputation rules in `SPEC-SHR-002` are already understood.

## Entry-scenario chooser

This chooser begins after the first-time cold-start route above. It does not restate a second first-time route.

| Reader role | Ordered reading set | Stop when |
|---|---|---|
| Authority reviewer | `SPEC-SHR-002 / Authority review operating model`; `SPEC-SHR-004 / Other governed artifacts / Authority Review Packet`; `SPEC-SHR-004 / Other governed artifacts / Authority Disposition`; `SPEC-SHR-002 / Governed Working Artifacts / Witness locator stability and decisive-witness selection`; `SPEC-SHR-002 / Evidence and Black-Box Discipline / Evidence chains`; `SPEC-SHR-002 / Promoted-output completeness and representation`; `SPEC-SHR-002 / Promotion Gate`; `APP-SHR-005 / Packet-first authority intake`; `APP-SHR-005 / Minimum viable Authority Disposition`; `APP-SHR-005 / Promotion-trace assembly and review checks` | you can review a packet or a promotion handoff without reading the whole recovery tree |
| Small repository owner or single operator with explicit local authority designation | `SPEC-SHR-000 / Authority rules`; `SPEC-SHR-002 / Authority review operating model`; `SPEC-SHR-004 / Other governed artifacts / Authority Review Packet`; `SPEC-SHR-002 / Promotion Gate`; `APP-SHR-005 / Copy-adapt template: explicit local same-operator authority designation`; `APP-SHR-005 / Copy-adapt template: same-operator packet and disposition disclosure` | you can determine whether same-operator ratification is actually authorized and how it must be disclosed |
| Reader joining an existing recovery effort | `SPEC-SHR-003 / Workspace structural posture and adjacent records`; `SPEC-SHR-003 / Derived structural posture model`; `SPEC-SHR-003 / Reader interpretation`; `SPEC-SHR-002 / Recovery snapshots and incremental resumption`; `SPEC-SHR-004 / Recovery snapshots and incremental resumption`; `SPEC-SHR-002 / Recovery Memory and Negative Evidence`; `SPEC-SHR-002 / Slice advancement and revalidation`; `SPEC-SHR-002 / Revalidation-triggered workflow-state recomputation`; `APP-SHR-001 / Partial and incremental recovery`; `APP-SHR-001 / Recovery memory in practice`; `RES-SHR-001` | you can load the latest Recovery Snapshot, understand the current state, and name the next governed action |
| Tooling implementer or harness author | `SPEC-SHR-000 / Repository entry point`; `SPEC-SHR-000 / Front matter`; `SPEC-SHR-003 / Discovery file fields`; `SPEC-SHR-003 / Effective artifact schema companion resolution`; `SPEC-SHR-003 / Corpus-default topology`; `SPEC-SHR-003 / Workspace structural posture and adjacent records`; `SPEC-SHR-003 / Derived structural posture model`; `SPEC-SHR-003 / Locator constraints`; `SPEC-SHR-003 / Authoritative local override site`; `SPEC-SHR-002 / Recovery Inputs and Artifact Roles / Control-plane prose and artifact-role inventory`; `SPEC-SHR-004 / Artifact-role inventory minimum shape and linkage`; `SPEC-SHR-004 / Repository-surface-family record for repository-shaped targets`; `SPEC-SHR-004 / Default slice artifact continuity and assembly`; `SPEC-SHR-004 / Other governed artifacts / Evidence Bundle`; `SPEC-SHR-004 / Overlays / Overlay field minima matrix`; `SPEC-SHR-004 / Recovery snapshots and incremental resumption`; `SPEC-SHR-002 / Recovery Definition of Done`; exactly one of `SPEC-SHR-005` or `SPEC-SHR-006`; `APP-SHR-002 / The smallest viable workspace`; `APP-SHR-005 / Packet-first authority intake` | you can implement artifact handling and recovery discovery without inventing unstated fields, override parsing rules, chain-carrier rules, anchor comparison behavior, or authority/promotion handoff steps |

## Minimum viable reading sets

| Need | Read this set | Why this is enough |
|---|---|---|
| Open the first slice correctly | `SPEC-SHR-000 / Repository entry point`, `Authority rules`, and `Front matter`; exactly one of `SPEC-SHR-005` or `SPEC-SHR-006`; `SPEC-SHR-003 / Discovery file fields`; `SPEC-SHR-003 / Effective artifact schema companion resolution`; `SPEC-SHR-003 / Corpus-default topology`; `SPEC-SHR-003 / Workspace structural posture and adjacent records`; `SPEC-SHR-003 / Local-override validity matrix`; `SPEC-SHR-003 / Authoritative local override site`; `SPEC-SHR-003 / Reader interpretation`; `SPEC-SHR-001 / Conformance Scope and Interpretive Boundary`; `SPEC-SHR-002 / Glossary and Normalization`; `SPEC-SHR-002 / Recovery Inputs and Artifact Roles / Control-plane prose and artifact-role inventory`; `SPEC-SHR-002 / Workflow Overview`; `SPEC-SHR-002 / Workflow Overview / Entry minima before first Slice Contract`; `SPEC-SHR-002 / Workflow Overview / Minimum content floor for the system sketch and slice seed`; `SPEC-SHR-002 / Default workflow-state transitions`; `SPEC-SHR-004 / Artifact-role inventory minimum shape and linkage`; `SPEC-SHR-004 / Repository-surface-family record for repository-shaped targets`; `SPEC-SHR-004 / Default slice artifact continuity and assembly`; `SPEC-SHR-004 / Specification Plan`; `SPEC-SHR-004 / Other governed artifacts / Evidence Bundle`; `SPEC-SHR-004 / Slice Contract`; `SPEC-SHR-004 / Validation Report`; `APP-SHR-002 / Placement-mode chooser`; `APP-SHR-002 / What must exist before the first Slice Contract`; `APP-SHR-002 / The smallest viable workspace`; and the three first-loop templates | this set covers bootstrap, the condensed normative cold-start profile for the chosen placement mode, local workflow discovery, schema-companion resolution, machine-readable override discovery, workspace topology, structural posture distinctions, the NLSpec-quality boundary, the clarified entry-content floor, repository-family coverage, anchor normalization, transition ownership, chain-carrier rules, base artifact schemas, and the smallest operational loop without requiring the auxiliary `APP-SHR-001` or `RES-SHR-001` surfaces first |
| Review an authority packet | `SPEC-SHR-002 / Authority review operating model`; `SPEC-SHR-004 / Other governed artifacts / Authority Review Packet`; `SPEC-SHR-004 / Other governed artifacts / Authority Disposition`; `SPEC-SHR-002 / Governed Working Artifacts / Witness locator stability and decisive-witness selection`; `SPEC-SHR-002 / Promotion Gate`; `APP-SHR-005 / Packet-first authority intake`; `APP-SHR-005 / Minimum viable Authority Disposition` | this set defines packet semantics, disposition semantics, same-operator disclosure, decisive-basis selection, and promotion-bearing authority closure |
| Resume a paused recovery | `SPEC-SHR-003 / Workspace structural posture and adjacent records`; `SPEC-SHR-003 / Derived structural posture model`; `SPEC-SHR-003 / Reader interpretation`; `SPEC-SHR-002 / Recovery snapshots and incremental resumption`; `SPEC-SHR-004 / Recovery snapshots and incremental resumption`; `SPEC-SHR-002 / Recovery Memory and Negative Evidence`; `SPEC-SHR-002 / Memory consultation protocol`; `SPEC-SHR-002 / Continuity conflicts`; `SPEC-SHR-002 / Slice advancement and revalidation`; `SPEC-SHR-002 / Revalidation-triggered workflow-state recomputation`; `APP-SHR-001 / Partial and incremental recovery`; `RES-SHR-001` | this set defines what a Recovery Snapshot means, what fields it carries, how workspace posture differs from workflow state, where to find the current plan, when revalidation is required, and how workflow state is recomputed |
| Judge whether a promoted artifact is actually NLSpec-quality | `SPEC-SHR-001 / Conformance Scope and Interpretive Boundary`; `SPEC-SHR-001 / Why NLSpecs Work When They Work`; `SPEC-SHR-001 / What Completeness Means`; `SPEC-SHR-001 / Conceptual Fidelity`; `SPEC-SHR-001 / Spec Economy`; `SPEC-SHR-001 / Intentional vs. Accidental Ambiguity`; `SPEC-SHR-001 / Appendix D: When Evaluating a Recovered NLSpec`; `SPEC-SHR-002 / Imported NLSpec tests and recovery-specific restatements`; `SPEC-SHR-002 / Promoted-output completeness and representation`; `SPEC-SHR-002 / Promotion Gate` | this set joins the grounding quality target to the reverse-recovery-specific import and closure rules |
| Move from first-loop operation to stable repository-shaped or authority-bearing practice | `APP-SHR-002 / What to do immediately after the first slice closes`; `APP-SHR-004 / Repository reconnaissance`; `APP-SHR-004 / Executable validation feasibility floor`; `APP-SHR-004 / Repository-shaped target-anchor axis mapping`; `APP-SHR-004 / Repository-shaped structure recovery patterns`; `APP-SHR-005 / Packet-first authority intake`; `APP-SHR-005 / Boundary-clean promotion transformation` | this set hands the reader from the first-loop entry pack into the stable repository-shaped and authority-bearing baselines without forcing reliance on the broader draft APP first |

## Glossary pointers

| Term family | Go here first | Why |
|---|---|---|
| `workflow_state`, `support_level`, `disposition`, `review_mode`, `target_anchor`, `promotion_readiness`, `authority_independence`, `promotion_trace_matrix` | `SPEC-SHR-002 / Glossary and Normalization`; `SPEC-SHR-002 / Vocabulary Declarations`; `SPEC-SHR-002 / Workflow Overview / Entry minima before first Slice Contract`; `SPEC-SHR-002 / Default workflow-state transitions`; `SPEC-SHR-002 / Revalidation-triggered workflow-state recomputation`; `SPEC-SHR-002 / Recovery snapshots and incremental resumption` | these sections define the workflow-owned governed terms, their transition ownership, their anchor-comparison rules, and their literal vocabularies |
| governed-artifact minima, omission semantics, overlay fields, plan or contract field meaning | `SPEC-SHR-004 / Canonical schema status legend`; `SPEC-SHR-004 / Specification Plan`; `SPEC-SHR-004 / Slice Contract`; `SPEC-SHR-004 / Validation Report`; `SPEC-SHR-004 / Overlays` | these sections define the canonical field-minimum tables and field-local omission semantics |
| `governance_spec_ref`, `derived_from`, `related_docs`, local versus shared authority, ratification versus activation | `SPEC-SHR-000 / Bootstrap file fields`; `SPEC-SHR-000 / Authority rules`; `SPEC-SHR-000 / Status semantics`; `SPEC-SHR-000 / Front matter`; `SPEC-SHR-003 / Recovery-derived governed outputs and lineage` | these sections define local governance semantics, the difference between reading a shared document and making it locally binding, and the corpus-default reverse-recovery lineage subform carried in `derived_from` |
| workspace structural posture, corpus-default topology, reverse-recovery discovery, `recovery_root`, `recovery_artifact_schema_spec_ref`, `plan_locator`, `memory_locator`, `latest_snapshot_locator`, `local_override_locator`, `reverse_recovery_overrides` | `SPEC-SHR-003 / Discovery file fields`; `SPEC-SHR-003 / Effective artifact schema companion resolution`; `SPEC-SHR-003 / Corpus-default topology`; `SPEC-SHR-003 / Workspace structural posture and adjacent records`; `SPEC-SHR-003 / Derived structural posture model`; `SPEC-SHR-003 / Authoritative local override site`; `SPEC-SHR-003 / Reader interpretation` | these sections define the machine-discoverable local workflow-adoption surface, the default topology, the schema-companion resolution rule, the machine-readable override block, and the difference between posture and workflow state |
| minimum cold-start stack by placement mode | `SPEC-SHR-005`; `SPEC-SHR-006`; `APP-SHR-002 / Placement-mode chooser`; `APP-SHR-002 / What must exist before the first Slice Contract`; `APP-SHR-002 / The smallest viable workspace` | these sections give the short normative stack first and then the operational first-loop mirror for each placement mode |
| stable repository-shaped operating baseline | `APP-SHR-004 / Repository reconnaissance`; `APP-SHR-004 / Executable validation feasibility floor`; `APP-SHR-004 / Repository-shaped target-anchor axis mapping`; `APP-SHR-004 / Repository-shaped structure recovery patterns` | these sections collect the stabilized repository-shaped operating guidance that no longer needs to live only in the broader draft APP |
| Authority Review Packet, Authority Disposition, promotion handoff | `SPEC-SHR-002 / Authority review operating model`; `SPEC-SHR-004 / Other governed artifacts / Authority Review Packet`; `SPEC-SHR-004 / Other governed artifacts / Authority Disposition`; `APP-SHR-005 / Packet-first authority intake`; `APP-SHR-005 / Recovered-behavior representation mapping`; `APP-SHR-005 / Promotion-trace assembly and review checks` | these sections split workflow meaning from canonical minima and then provide the stable operational authority and promotion baseline |

## Split-release section relocation crosswalk

This table is non-normative. It exists so readers can relocate historical `SPEC-SHR-002` section names after the schema split.

| Historical `SPEC-SHR-002` section | New authoritative site |
|---|---|
| `Recovery Inputs and Artifact Roles / Artifact-role inventory minimum shape and linkage` | `SPEC-SHR-004 / Artifact-role inventory minimum shape and linkage` |
| `Recovery Inputs and Artifact Roles / Repository-surface-family record for repository-shaped targets` | `SPEC-SHR-004 / Repository-surface-family record for repository-shaped targets` |
| `Governed Working Artifacts / Default slice artifact continuity and assembly` | `SPEC-SHR-004 / Default slice artifact continuity and assembly` |
| `Governed Working Artifacts / Governed artifact identity, provenance, and attempt metadata` | `SPEC-SHR-004 / Governed artifact identity, provenance, and attempt metadata` |
| `Governed Working Artifacts / Canonical schema status legend` | `SPEC-SHR-004 / Canonical schema status legend` |
| `Governed Working Artifacts / Specification Plan` | `SPEC-SHR-004 / Specification Plan` |
| `Governed Working Artifacts / Slice Contract` | `SPEC-SHR-004 / Slice Contract` |
| `Governed Working Artifacts / Validation Report` | `SPEC-SHR-004 / Validation Report` |
| `Governed Working Artifacts / Other governed artifacts / Critique Report` | `SPEC-SHR-004 / Other governed artifacts / Critique Report` |
| `Governed Working Artifacts / Other governed artifacts / Evidence Bundle` | `SPEC-SHR-004 / Other governed artifacts / Evidence Bundle` |
| `Governed Working Artifacts / Other governed artifacts / Authority Review Packet` | `SPEC-SHR-004 / Other governed artifacts / Authority Review Packet` |
| `Governed Working Artifacts / Other governed artifacts / Authority Disposition` | `SPEC-SHR-004 / Other governed artifacts / Authority Disposition` |
| `Overlays` | `SPEC-SHR-004 / Overlays` |
| `Overlays / Overlay field minima matrix` | `SPEC-SHR-004 / Overlays / Overlay field minima matrix` |
| `Appendix A`, `Appendix B`, and `Appendix C` | `SPEC-SHR-004 / Appendix A`, `Appendix B`, and `Appendix C` |

## Maintenance note

Update this guide whenever a shared foundation document is added, removed, renamed, or materially changes its role in the reading paths above. The addition of `SPEC-SHR-006` and `APP-SHR-005` is therefore a router update and not merely an editorial note. Any future section relocation between `SPEC-SHR-002` and `SPEC-SHR-004`, any future cold-start profile change, and any future stabilization or extraction of APP-layer guidance are also router-update triggers. The guide should continue to point to authoritative sites by document name and exact section name, and it should preserve the routing split in this document: the initial foundation packet is `SPEC-SHR-000` through `SPEC-SHR-004` plus exactly one short cold-start profile together with `APP-SHR-002` and `APP-SHR-003`; `APP-SHR-004` and `APP-SHR-005` carry the stabilized baselines after the first loop; and `APP-SHR-001` together with `RES-SHR-001` remains auxiliary and seam-triggered. It should not become a hidden authority surface.
