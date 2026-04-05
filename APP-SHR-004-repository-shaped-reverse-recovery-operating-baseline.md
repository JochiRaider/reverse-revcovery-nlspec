---
doc_id: APP-SHR-004
title: Repository-Shaped Reverse-Recovery Operating Baseline
status: active
version: 0.2.0
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
  - APP-SHR-005
---

# Repository-Shaped Reverse-Recovery Operating Baseline

## Purpose and boundary

This document is an informative operational baseline for repository-shaped reverse recovery after the selected short cold-start profile is already understood.

It stabilizes four repository-shaped operating surfaces that were previously available only inside the broader `draft` companion:

- repository reconnaissance outputs and ordered phases
- executable-validation feasibility floor for repository-shaped slices
- repository-shaped `target_anchor` axis selection
- repository-surface-family operating patterns for build, deployment, configuration, generated artifacts, extension surfaces, data stores, and service topology

This APP does not create new workflow, topology, schema, adoption, or promotion rules. `SPEC-SHR-005` and `SPEC-SHR-006` remain the short normative cold-start profiles for in-repository and adjacent-workspace entry respectively. `APP-SHR-002` remains the first-loop operational mirror. `APP-SHR-005` remains the stable authority-closure and promotion-handoff baseline. `APP-SHR-001` remains the broader draft companion for wider operating practice, composition examples, and revisable guidance that is intentionally outside this stabilized baseline.

## When to use this document

Use this document when the first-loop entry pack is no longer enough and the engagement needs stable repository-shaped operating guidance without relying immediately on the broader `draft` APP.

Use `APP-SHR-005` when the next seam is authority review, same-operator disclosure, or promotion-bearing handoff rather than repository-shaped reconnaissance or structure recovery.

Use `APP-SHR-001` when the engagement also needs broader draft material such as recovery-memory layout conventions, partial-recovery composition examples, later-stage failure patterns, or other wider operating seams.

## Stable-baseline extraction crosswalk

The table below records the stable subset extracted from `APP-SHR-001`.

| Historical `APP-SHR-001` section | Stable baseline now lives here |
|---|---|
| `APP-SHR-001 / Repository reconnaissance` | `APP-SHR-004 / Repository reconnaissance` |
| `APP-SHR-001 / Executable validation feasibility floor` | `APP-SHR-004 / Executable validation feasibility floor` |
| `APP-SHR-001 / Scope declaration and compatibility target patterns / Repository-shaped target-anchor axis mapping` | `APP-SHR-004 / Repository-shaped target-anchor axis mapping` |
| `APP-SHR-001 / Appendix D: Target-System Structure Recovery Patterns` | `APP-SHR-004 / Repository-shaped structure recovery patterns` |

## Repository reconnaissance


### What reconnaissance produces

Reconnaissance produces six outputs that together satisfy `SPEC-SHR-002`'s entry minima before the first Slice Contract is opened:

1. A **system sketch with current declared scope**. This is a short prose description of what the system is, what it does, where its boundaries are, who or what interacts with it, and which current scope cut the recovery effort is actually pursuing. At minimum it should already make four facts explicit: system identity, declared scope cut, the primary boundary surfaces or repository-surface families currently in view, and the principal external actors, adjacent systems, or operator classes that materially interact with the declared scope. It is not a recovered specification. It is the initial hypothesis that scopes everything downstream.

2. A **declared recovery objective and compatibility target**. This is the explicit statement of whether the effort is pursuing backward-compatible reimplementation, normative cleanup, migration support, documentation-only description, cleanroom rebuild, or another declared objective, together with the compatibility boundary that makes the work consequential.

3. A **technology and deployment surface inventory plus a provisional `target_anchor`**. This includes languages, frameworks, build systems, runtime targets, deployment shape, external dependencies, and the initial normalized revision, deployment-family, environment-regime, consumer-version, data-snapshot, or composite anchor against which the first slice will be asserted. This inventory constrains what kinds of witnesses are obtainable and what `executable` validation means for this system.

4. A **draft artifact-role inventory**. This is the `SPEC-SHR-002` artifact-role inventory in its initial form, populated from reconnaissance rather than from slice-level analysis. Each currently known surface should already have stable `inventory_item_ref`, explicit `scope_classification`, role, mutation authority, mutation conditions, and violation effect. This is the seed that slice work refines.

5. A **row-complete `repository_surface_families` record** when the target is repository-shaped. This record explicitly classifies or bounds the major repository surface families already examined so build, deployment, configuration, generated-artifact, extension, data-store, and service-topology surfaces do not disappear by silence. The record may still contain `inspection-pending` rows at reconnaissance time.

6. A **candidate slice list with at least one first-slice seed**. This is the ordered set of surfaces that appear worth recovering, ranked by estimated load-bearing risk, with at least one proposed first slice concrete enough to become a Specification Plan entry. For the first proposed slice, the working record should already make a stable seed label, targeted boundary surface, provisional question or provisional clause, and explicit selection reason visible.

Items 1 and 6 are the APP-layer mirror of `SPEC-SHR-002 / Workflow Overview / Entry minima before first Slice Contract / Minimum content floor for the system sketch and slice seed`. They keep the entry surface small and do not create a new governed artifact family.

Reconnaissance is ready to yield when scope, `recovery_objective`, `compatibility_target`, `target_anchor`, a draft artifact-role inventory, the repository-surface-family record when applicable, and at least one candidate slice with an explicit first-slice seed are all explicit in the working record. These records may remain provisional. The entry condition is explicitness, not polish.

### Reconnaissance order

The following order reflects dependency: each phase uses information from the phases before it. Phases may overlap, but the outputs of earlier phases constrain the later ones.

**Phase 1: Boundary identification.** Before looking at internals, identify what the system presents to the outside world. The relevant surfaces include public APIs, CLIs, wire protocols, file formats, schemas, UI surfaces, published events or messages, and machine-parsed operational outputs. These are the surfaces where Hyrum's Law operates most intensely and where caller dependence is most likely. Start here because boundary surfaces constrain what counts as "behaviorally complete" for any subsequent recovery.

Artifacts to examine: API definitions (OpenAPI, protobuf, GraphQL, WSDL), CLI help text and argument parsers, schema registries, published SDKs or client libraries, documented webhook or event contracts, and any surface described in public-facing documentation.

**Phase 2: Coordination seam survey.** Identify where the system coordinates with other systems or teams. SPEC-SHR-002's "coordination-boundary alignment" classification dimension asks whether a boundary shows durable coordination-seam hallmarks. Reconnaissance is the right time to locate these seams because they often carry contractual weight disproportionate to their code surface.

Signals: versioned compatibility machinery (API versions, schema evolution, migration shims), defensive parsing or deserialization, asynchronous handoffs (queues, event buses, webhooks), separate deployment or release cycles for producer and consumer, backward-compatibility test suites, and contract tests.

**Phase 3: Build, test, and deployment shape.** Understand how the system is built, tested, and deployed. This constrains the feasibility of executable validation and determines what kinds of runtime witnesses are obtainable. This phase usually yields the first provisional `target_anchor`, because it is where revision, deployment family, feature-flag regime, and environment-sensitive divergence first become concrete.

Artifacts to examine: build configuration, CI/CD pipelines, test suites and their structure (unit, integration, contract, end-to-end), deployment manifests, environment configurations, feature flag systems, and release processes.

The test suite deserves special attention. A well-structured test suite is among the strongest available evidence about intended contract surface. Tests that assert on boundary behavior are candidate witnesses for contract clauses. Tests that assert on internal structure are evidence about current decomposition, not about the contract. The distinction matters for SPEC-SHR-002's classification work downstream.

**Phase 4: Documentation and decision-record landscape.** Survey existing documentation, not to take it at face value, but to understand what the system's authors believed was worth documenting and what they left implicit.

Artifacts to examine: READMEs, architecture documents, design documents, ADRs, runbooks, operator manuals, onboarding guides, API documentation, changelogs, and any existing specifications or RFCs.

Documentation is framing metadata under SPEC-SHR-002's source hierarchy until corroborated by stronger witnesses. But it is valuable framing metadata. It suggests where the authors believed the contract boundaries were, which is a useful starting hypothesis even when it turns out to be incomplete or stale.

**Phase 5: Control-plane prose survey.** If the system is prose-orchestrated or agent-consumed - if prompts, runbooks, evaluator instructions, operator policies, or similar prose artifacts govern mutation, acceptance, escalation, or state transitions - identify those surfaces early. SPEC-SHR-002 defines control-plane prose as a first-class recovery target. Missing it during reconnaissance means missing some of the most consequential contractual surfaces.

Artifacts to examine: system prompts, agent instructions, evaluation rubrics, operator playbooks, escalation policies, approval workflows, and any prose that governs what the system does rather than merely describing it.

**Phase 6: Initial artifact-role inventory.** Synthesize the preceding phases into a draft artifact-role inventory using the SPEC-SHR-002 role vocabulary. For each inventoried surface, record stable `inventory_item_ref`, `scope_classification`, role, mutation authority, mutation conditions, and violation effect. If the surface is already known to be protected, record that explicitly through protected-surface treatment rather than by prose implication alone. This is the draft; slice-level work will refine it.

### Reconnaissance discipline


### Translating reconnaissance output into the canonical artifact-role inventory

A reconnaissance note becomes recovery control state only when it is translated into the canonical inventory fields expected by `SPEC-SHR-002`. The table below is a working aid for that translation.

| Reconnaissance finding | Inventory fields to populate immediately | Default handling note |
|---|---|---|
| boundary or coordination seam clearly in scope | `inventory_item_ref`, `surface`, `role`, `scope_classification: in-scope`, mutation fields, and violation effect | use the narrowest stable boundary name that the next slice can still cite without guesswork |
| currently known surface intentionally outside the declared objective | `inventory_item_ref`, `surface`, `role`, `scope_classification: out-of-scope`, mutation fields, and violation effect | do not omit the surface merely because it is excluded |
| currently known surface judged non-contract-bearing for the declared objective | `inventory_item_ref`, `surface`, `role`, `scope_classification: not-boundary-bearing`, mutation fields, and violation effect | use this only when the judgment is explicit enough that later slices will not rediscover the surface as a silent omission |
| surface whose stability itself is load-bearing | all ordinary fields plus protected-surface treatment | state what must not change and what breaks if the protection is violated |

Draft status means the item may still be revised. It does not waive stable identity or scope classification. Once a plan entry cites the surface, the inventory item should already exist and the cited `inventory_item_ref` should remain stable across later refinements.

### Populating `repository_surface_families`

For repository-shaped targets, reconnaissance should also populate the row-complete `repository_surface_families` record required by `SPEC-SHR-002`. The table below is a working aid for first-pass classification.

| `surface_family` | Typical first-pass evidence | When `inventory_item_refs` should already exist | When `inspection-pending` is still acceptable |
|---|---|---|---|
| `build_graph_and_packaging_boundary` | build manifests, lockfiles, release jobs, published package metadata, packaging tests | when callers consume a package, image, build artifact, or versioned release artifact | when build evidence exists but the caller-visible package boundary is not yet bounded |
| `deployment_unit_and_release_topology` | deployment manifests, release pipelines, runtime topology ledgers, rollback playbooks | when deployment shape changes caller-visible behavior, operator-visible semantics, or release compatibility | when deployment evidence exists but variant-specific behavior is not yet separated from infrastructure mechanism |
| `configuration_plane_and_default_source` | config schemas, shipped defaults, startup traces, operator runbooks, precedence tests | when omitted-value behavior, precedence, or default source affects the boundary or operators | when configuration inputs are visible but default-source or precedence rules are not yet bounded |
| `generated_artifact_or_codegen_boundary` | generated outputs, generator inputs, golden tests, regeneration scripts | when downstream consumers parse, compile, or otherwise depend on generated outputs | when generated artifacts are present but regeneration invariants or caller-visible generated-surface semantics remain unclear |
| `extension_plugin_or_hook_surface` | plugin registries, hook schemas, extension tests, compatibility shims | when third parties or adjacent components register, invoke, or depend on extension behavior | when extensibility is suspected but registration, ordering, or failure semantics are not yet bounded |
| `data_store_schema_and_migration_seam` | schema snapshots, migration scripts, backup or restore runbooks, compatibility tests | when schema shape, migration invariants, or durable storage semantics affect callers or operators | when the data store is clearly present but contract-bearing schema or migration semantics remain unbounded |
| `service_topology_and_cross_service_coordination_seam` | API contracts, event schemas, queue traces, retry rules, incident history | when more than one service coordinates across a seam with observable translation, retry, deduplication, or status effects | when coordination exists but the seam-specific contract has not yet been isolated from the current call graph |

Use `present-in-scope`, `present-out-of-scope`, `present-not-boundary-bearing`, or `not-present` as soon as the stronger judgment is reviewable. Leave `inspection-pending` only where the family has not yet been bounded enough to make one of those stronger classifications honestly.

Reconnaissance is discovery, not recovery. The system sketch and candidate slice list are hypotheses, not claims. They carry no support level, no evidence chains, and no contractual weight. Their value is in directing subsequent recovery effort toward the surfaces most likely to be load-bearing. Shared informative resources may sharpen those hypotheses, but they do not convert them into target-specific claims.

The principal risk during reconnaissance is premature commitment: deciding too early what the system "really does" and then confirming that hypothesis through selective slice work. The corrective is to treat reconnaissance outputs as revisable and to let early slice results update the system sketch and candidate slice list.

A secondary risk is reconnaissance that never converges. If the system is large or unfamiliar, reconnaissance can expand indefinitely. A reasonable heuristic: reconnaissance is ready to yield when the boundary identification phase has stabilized - when examining additional code or documentation is refining the boundary inventory rather than discovering new boundary surfaces. Interior structure can be explored as slice work demands it.

## Executable validation feasibility floor

Early repository recovery often turns on text-, file-, or schema-shaped boundary surfaces rather than on a full live service. The practical question is not "can the whole system be started?" It is "can a safe, target-anchored executable probe answer the discriminating question?" This section gives an APP-layer capability floor for that judgment.

### Minimum capability floor

Before a repository-shaped slice can honestly claim that executable validation is unavailable or disproportionate, the working environment should have bounded answers to at least the following questions.

| Capability | Why it matters | Default APP-layer expectation |
|---|---|---|
| stable read access to the target revision or extracted input set | the probe must run against reviewable target material rather than against recollection or ad hoc manual transcription | prefer revision-pinned or snapshot-pinned reads |
| a non-persistent write boundary or explicit reset path | `mutation_policy: non-mutating` and `reset_method` should be structurally true, not merely promised | prefer discardable writes, clone reset, snapshot restore, or an equivalent reset boundary |
| a stable invocation surface | the probe must be rerunnable and reviewable by another operator | prefer one named harness path, entrypoint, or replay command rather than an ad hoc notebook sequence |
| stable witness-capture coordinates | executable work that cannot be reviewed later is only a transient observation | capture stdout, traces, diffs, parser results, and derived witness artifacts at stable refs |
| explicit mutation policy and secret-handling rules | early low-floor probes often fail because the environment boundary was never stated | default to non-mutating probes, least-secret capture, and explicit redaction notes |
| default network posture | low-floor probes drift quickly when they silently reach out to mutable external systems | default to closed-by-default network posture unless the slice explicitly requires networked behavior |

### What counts as a low-floor executable probe

Low-floor executable probes include parser replay, schema diff, format-invariant replay, configuration-precedence replay, golden-input replay against a machine consumer, and other bounded text-, file-, or schema-shaped checks that still answer the discriminating question materially. These probes do not require the real system to be fully live. They do require a real executable action, a reviewable harness, and a safe environment boundary.

### Default APP-layer choice rule

When a safe low-floor executable probe can answer the discriminating question materially, prefer `validation_mode: executable`. Full service startup is not the threshold for executable confirmation. The threshold is whether the probe is safe, target-anchored, discriminating, and reviewable.

### Non-normative implementation note

A named implementation can lower the floor without becoming the rule. A Bash-first harness such as `just-bash` can be enough for parser replay, schema diff, and other text-shaped probes when the question is boundary-visible and file-driven. OverlayFS or an equivalent discard-reset filesystem layer can likewise make `mutation_policy: non-mutating` and `reset_method` structurally true for repository-local probes by giving the probe read-from-real-repo and write-to-discarded-layer behavior. These are informative examples only. `SPEC-SHR-002` binds the contract shape, not the tool brand.

## Repository-shaped target-anchor axis mapping

Repository recovery often begins with `revision` alone and then discovers that one more axis is contract-bearing. The table below is the default translation aid for that moment.

| When revision alone is insufficient | Add this `target_anchor` axis | Keep the fact in `target_anchor` or only in environment capture | Default corrective move when discovered late |
|---|---|---|---|
| downstream parser, adapter, or client version changes boundary interpretation | `consumer-version` | keep it in `target_anchor` when the consumer version changes the clause or compatibility judgment | mark the affected slice `revalidation_required`, normalize the anchor, and do not compare mixed-anchor witnesses under the old contract |
| the same code ships in materially different deployed variants or productized topologies | `deployment-family` | keep it in `target_anchor` when caller-visible behavior or lifecycle semantics differ by variant | split the slice or the witness basis by deployment family before drawing a contract conclusion |
| release-branch or compatibility family matters more than one repository revision | `release-family` | keep it in `target_anchor` when branch or family identity constrains what counts as compatible | normalize the active plan and packet basis before further advancement |
| dataset identity or fixture corpus changes comparability or output shape | `data-snapshot` | keep it in `target_anchor` when the data identity is itself part of the contractual comparison surface; otherwise capture it in environment notes only | if later evidence shows the data identity was material, add the axis and reopen any slice that relied on cross-snapshot comparison |
| feature flags or policy bundles materially change boundary behavior | `feature-regime` | keep it in `target_anchor` when the flag or policy bundle is part of the claim rather than mere runtime setup | split the evidence by regime and avoid silent equality across flag sets |
| warm-start state, cache state, or hardware class changes reproducibility but not the contractual surface | none by default; keep in environment capture | keep it out of `target_anchor` unless the regime actually changes the clause itself | refresh environment capture and validator assumptions without automatically widening the anchor |

The safe default is smallest sufficient anchor, not smallest convenient anchor. Add an axis only when omitting it would make the clause, comparison, or revalidation judgment unstable.


A scope declaration and compatibility target are stated at the beginning of recovery and revised as recovery proceeds. Early slices often reveal that the initial scope was too broad, too narrow, or misaligned with the actual system boundaries. The corrective is revision, not rigidity.

Under `SPEC-SHR-002`, when the scope changes, the Specification Plan must reflect the change. Slices that were in scope may move to `out-of-scope`. Slices that were out of scope may be opened. The recovery objective and compatibility target in the Specification Plan entries must remain consistent with the current scope declaration.

## Structure-closure reminder by family

The table below is informative, but it is aligned intentionally to the minimum captured-fact closure expected by `SPEC-SHR-004 / Repository-surface-family record for repository-shaped targets`. Use it as the APP-layer reminder of what must eventually be made explicit when the family is `present-in-scope`.

| `repository_surface_family` | Minimum closure facts to verify | Strong evidence families | Common false positives or mechanistic traps | Recommended final placement |
|---|---|---|---|---|
| `build_graph_and_packaging_boundary` | authoritative build inputs, caller-visible package or release artifact identity, versioning or packaging boundary, and any regeneration or reproducibility invariant that callers or operators depend on | build manifests, lockfiles, release jobs, packaging tests, published package metadata | treating one local build script as the package contract; promoting internal task decomposition | promote only the caller-visible package or release contract; keep internal build mechanics in recovery notes or `APP` |
| `deployment_unit_and_release_topology` | deployed unit or release-family boundary, materially caller-visible topology split, rollout or rollback invariant when load-bearing, and any caller-visible divergence between variants | deployment manifests, release pipelines, runtime topology ledgers, rollback playbooks, incident history | mistaking current infrastructure vendor choice for required contract; over-promoting temporary topology quirks | promote only topology that callers, operators, or adjacent services depend on; keep vendor-specific mechanism in recovery record or `APP` |
| `configuration_plane_and_default_source` | authoritative configuration inputs, precedence order, default source, and omitted-value behavior | config schemas, defaults in shipped artifacts, operator runbooks, startup traces, precedence tests | treating every internal knob as contract; leaving omitted-value behavior implicit | promote defaults, precedence, and boundary-visible configuration semantics into `SPEC`; keep operational rationale in `APP` |
| `generated_artifact_or_codegen_boundary` | authoritative generator input surface, generated output contract, regeneration invariant, and downstream dependency shape | generated files consumed downstream, generator inputs, golden tests, regeneration scripts, replay harnesses | promoting generator internals instead of generated-surface contract; confusing cache artifacts with source-of-truth artifacts | promote the generated-surface contract and regeneration invariants; keep generator decomposition in recovery notes or `APP` |
| `extension_plugin_or_hook_surface` | registration surface, invocation or ordering rule, failure or isolation rule, and compatibility boundary for supported extensions | plugin registries, hook schemas, extension tests, compatibility shims, error contracts | mistaking one built-in extension for the general extension contract; promoting incidental load order | promote registration, invocation, ordering, and failure semantics only when boundary-visible; keep local plugin implementation details out of the final contract |
| `data_store_schema_and_migration_seam` | authoritative schema surface, migration direction or invariant, compatibility or rollback rule, and operator-visible durable-state consequence | migration scripts, schema snapshots, compatibility tests, backup or restore runbooks, incident history | promoting storage-engine internals; confusing one migration path with the schema contract itself | promote boundary-visible schema and migration invariants that callers or operators depend on; keep storage mechanism in recovery notes or `APP` |
| `service_topology_and_cross_service_coordination_seam` | participating seam, retry rule, deduplication or idempotence rule, status or error translation rule, and any operator-visible coordination invariant | API contracts, event schemas, queue traces, retry and timeout rules, incident history | treating current call graph as contract; under-specifying retries, deduplication, or status translation | promote coordination semantics, translation tables, retries, and lifecycle seams into `SPEC`; keep current decomposition and local routing mechanics in recovery notes or `APP` |


## Repository-shaped structure recovery patterns

This section is non-normative. It gives a default pattern language for repository-shaped reverse recovery when the difficult question is not only boundary behavior, but also how build, deployment, configuration, generated artifacts, extension points, data stores, and service seams shape the effective target-system contract.

This section is about target-system structure, not about recovery-workspace topology. Use it to recognize likely slice families, evidence families, and final placement choices once the workspace topology itself is already governed elsewhere by `SPEC-SHR-003`. It does not create a second artifact-role vocabulary. Use the `SPEC-SHR-002` completeness dimensions, artifact-role inventory, and `repository_surface_families` record when writing the governed record.

### Pattern table

| `repository_surface_family` | Likely artifact roles | Strong evidence families | Common false positives or mechanistic traps | Recommended final placement |
|---|---|---|---|---|
| `build_graph_and_packaging_boundary` | `fixed_boundary_artifact`, `derived_analysis`, `framing_metadata` | build manifests, lockfiles, release jobs, packaging tests, published package metadata | treating internal task decomposition as contract; confusing one local build script with the canonical package boundary | promote only the caller-visible build or package contract; keep internal build mechanics in recovery notes or `APP` |
| `deployment_unit_and_release_topology` | `fixed_boundary_artifact`, `runtime_ledger`, `control_plane_artifact` | deployment manifests, release pipelines, runtime topology ledgers, rollback playbooks, incident history | mistaking current infrastructure vendor choice for required contract; over-promoting temporary topology quirks | promote only topology that callers, operators, or adjacent services depend on; keep vendor-specific mechanism in recovery record or `APP` |
| `configuration_plane_and_default_source` | `fixed_boundary_artifact`, `mutable_operator_surface`, `control_plane_artifact` | config schemas, defaults in shipped artifacts, operator runbooks, startup traces, compatibility tests | treating every internal knob as contract; leaving omitted-value behavior implicit | promote defaults, precedence, and boundary-visible configuration semantics into `SPEC`; keep operational rationale in `APP` |
| `generated_artifact_or_codegen_boundary` | `fixed_boundary_artifact`, `derived_analysis`, `runtime_ledger` | generated files consumed downstream, generator inputs, golden tests, replay harnesses | promoting generator internals instead of generated-surface contract; confusing cache artifacts with source-of-truth artifacts | promote the generated-surface contract and regeneration invariants; keep generator decomposition in recovery notes or `APP` |
| `extension_plugin_or_hook_surface` | `fixed_boundary_artifact`, `control_plane_artifact`, `machine_parsed_operational_output` | plugin registries, hook schemas, extension tests, compatibility shims, error contracts | mistaking one built-in extension for the general extension contract; promoting load order that is merely incidental | promote registration, invocation, ordering, and failure semantics only when boundary-visible; keep local plugin implementation details out of the final contract |
| `data_store_schema_and_migration_seam` | `fixed_boundary_artifact`, `runtime_ledger`, `mutable_operator_surface` | migration scripts, schema snapshots, compatibility tests, backup or restore runbooks, incident history | promoting storage-engine internals; confusing one migration path with the schema contract itself | promote boundary-visible schema and migration invariants that callers or operators depend on; keep storage mechanism in recovery notes or `APP` |
| `service_topology_and_cross_service_coordination_seam` | `fixed_boundary_artifact`, `runtime_ledger`, `control_plane_artifact` | API contracts, event schemas, queue traces, retry and timeout rules, incident history | treating current call graph as contract; under-specifying retries, deduplication, or status translation | promote coordination semantics, translation tables, retries, and lifecycle seams into `SPEC`; keep current decomposition and local routing mechanics in recovery notes or `APP` |

### Reading the pattern table

Use the table in the following order:

1. identify the structure surface type that is most likely load-bearing
2. choose the strongest obtainable evidence family before drafting the clause
3. test whether the suspected structure is boundary-visible or only mechanistic
4. place the recovered result in `SPEC`, `APP`, or recovery memory according to whether the result is contractual, explanatory, or provisional

### Placement defaults

Use these defaults unless a stronger local rule says otherwise.

- Promote structure findings into a `SPEC` only when the finding affects caller-visible behavior, operator-visible invariants, defaults, interoperability, or acceptance criteria.
- Place explanatory repository topology, rationale, and operating guidance in `APP`.
- Keep provisional or weakly supported structure findings in the recovery tree until target-specific evidence makes their contractual force explicit.
- When a structure surface translates among non-identical vocabularies or statuses, use the row-complete mapping form from `SPEC-SHR-002 / Translation-surface completeness and mapping form` and the canonical field carriers defined in `SPEC-SHR-004` when the translation surface is represented inside a governed artifact.

## Relation to `APP-SHR-001`

`APP-SHR-001` remains the broader `draft` operational companion. It keeps recovery-memory practice, partial-recovery handling, composition examples, later-stage failure patterns, and other wider or more revisable operating material. `APP-SHR-005` now owns the stable authority-closure and promotion-handoff baseline. This document is the stable repository-shaped baseline, not the full operating corpus.
