---
doc_id: SPEC-SHR-002
title: Reverse-Engineering Specs from Existing Systems
status: active
version: 0.13.0
related_docs:
  - SPEC-SHR-000
  - SPEC-SHR-001
  - SPEC-SHR-003
  - SPEC-SHR-004
  - SPEC-SHR-005
  - SPEC-SHR-006
  - APP-SHR-001
  - APP-SHR-002
  - APP-SHR-003
  - APP-SHR-004
  - APP-SHR-005
  - RES-SHR-001
---

# Reverse-Engineering Specs from Existing Systems

## Purpose, Artifact Class, Audience, and Authority Boundary

This document is a normative reverse-recovery workflow specification. It governs how recovery authors, critics, validators, and authority reviewers recover Level 1 and Level 2 artifacts from existing systems and prepare a Level 3 handoff that can be ratified into an NLSpec. This document uses `reverse recovery` as the canonical term.

Its authority boundary is procedural and workflow-owned. It governs workflow states, transitions, revalidation, evidence discipline, authority review operating model, validation semantics, recovery memory, Recovery Snapshot meaning, promoted-output representation, and Promotion Gate behavior. Governed-artifact field-minimum tables live in `SPEC-SHR-004`, the governed-artifact schema companion.

This document defines the repair path used when the forward `Intent -> NLSpec -> Implementation` chain was skipped, only partially followed, or has become historically unavailable. After reading it, a practitioner can inventory artifact roles, choose discriminating slices, coordinate critique and validation, route authority-owned questions, preserve resumable state, and prepare a ratifiable Level 3 handoff.

This workflow is software-repository-primary. It may also be applied to other recoverable system types when the target exposes stable boundary artifacts, reviewable witnesses, and a recoverable compatibility target. Repository-local revision anchors, filesystem locators, build surfaces, and deployment-family examples in this document are repository-shaped defaults rather than exclusive applicability conditions.

This document follows the define-once principle. Each major concept has one authoritative definition site. Workflow meaning lives here. Canonical governed-artifact minima live in `SPEC-SHR-004`. Later sections refer back to the authoritative site by name instead of restating the same requirement in multiple forms.

### Relation to SPEC-SHR-001

This document applies NLSpec discipline to a procedural workflow rather than directly specifying the construction of a target software system. It governs the recovery workflow and the workflow-owned meaning of the governed artifacts that lead to a ratifiable Level 3 handoff. It is therefore adjacent to, but not identical with, the artifact class defined in `SPEC-SHR-001`.

Only a recovered Level 3 artifact that has passed the Promotion Gate and has been ratified by a domain authority may be submitted as the candidate target-system NLSpec. Whether it becomes locally binding then depends on the consuming domain's lifecycle and adoption rules. Until that point, this document governs procedure, evidence handling, review, and promotion logic rather than the target system's runtime behavior.

When reverse recovery runs inside a governed repository or adjacent governed workspace, `SPEC-SHR-003` defines the local adoption, workspace topology, structural posture, and discovery surface for this workflow and, when applicable, for its governed-artifact schema companion. This document does not restate that discovery-file schema or that posture model.

For a short normative assembly of the minimum cold-start stack, use `SPEC-SHR-005` for the in-repository case and `SPEC-SHR-006` for the adjacent-workspace case. Stable repository-shaped operating guidance after the first loop lives in `APP-SHR-004`. Stable authority-closure and promotion-handoff guidance lives in `APP-SHR-005`. Broader and more revisable operating practice remains in `APP-SHR-001`.

A ratified Level 3 artifact becomes locally binding only through the consuming domain's local lifecycle and adoption rules. Under `SPEC-SHR-000`, a `draft` `SPEC` remains non-binding until it becomes `active` unless an explicit authoritative local rule couples ratification and activation.

Explanatory discussion of why reverse recovery exists, what it does not resolve, and how it repairs rather than replaces the forward model lives in `APP-SHR-001 / Why reverse recovery exists and what it does not fix` and `SPEC-SHR-001 / Appendix D: When Evaluating a Recovered NLSpec`. Those sites are advisory. They do not change this workflow.

### Imported NLSpec tests and recovery-specific restatements

`SPEC-SHR-001` remains conformance-bearing on its own terms. The table below records which NLSpec tests this workflow consumes directly and which recovery-specific questions are made binding here by explicit restatement. Recovery-specific imports from `SPEC-SHR-001` Appendix D are binding in this document because they are restated here. Appendix D remains advisory within `SPEC-SHR-001`.

| Imported or restated NLSpec test | `SPEC-SHR-001` source | Binding force in this workflow | Closure site in this document |
|---|---|---|---|
| behavioral completeness, unambiguous interfaces, explicit defaults and boundaries, mapping-table discipline, and testable acceptance criteria | `Why NLSpecs Work When They Work` | binding for any Level 3 artifact that claims NLSpec status | `Recovered completeness`, `Translation-surface completeness and mapping form`, `Promoted-output completeness and representation`, `Promotion Gate` |
| recreatability test | `What Completeness Means` | binding for the declared Level 3 scope | `Recovered completeness`, `Promoted-output completeness and representation`, `Promotion Gate / PG-14` |
| conceptual fidelity and economy discipline | `Conceptual Fidelity`; `Spec Economy` | binding for final promoted prose and for recovery records that would otherwise duplicate or distort the recovered contract | `Black-Box Constraint`, `Promoted-output completeness and representation`, `Promotion Gate / PG-15` |
| intentional versus accidental ambiguity | `Intentional vs. Accidental Ambiguity` | binding for clause classification and for final representation of preserved, excluded, or bounded accidental behavior | `The Core Difficulty: Hyrum's Law and Classification`, `Promoted-output completeness and representation`, `Promotion Gate / PG-2` |
| clause provenance must remain reviewable without raw conversational history | `Appendix D: When Evaluating a Recovered NLSpec` | restated here as binding for promotion-bearing recovery | `Evidence chains`, `Authority review operating model`, `Promoted-output completeness and representation`, `Promotion Gate / PG-7` |
| revision, release-family, deployment-family, or environment anchoring must be explicit | `Appendix D: When Evaluating a Recovered NLSpec` | restated here as binding for promotion-bearing recovery | `Entry minima before first Slice Contract`, `Recovery snapshots and incremental resumption`, `Promoted-output completeness and representation` |
| repository-shaped recovery must classify or bound the major repository surface families explicitly rather than leaving them to silence | `Appendix D: When Evaluating a Recovered NLSpec` | restated here as binding for repository-shaped targets | `Recovery Inputs and Artifact Roles / Repository-surface-family record for repository-shaped targets`, `Workflow Overview / Entry minima before first Slice Contract`, `Promoted-output completeness and representation` |
| promotion-batch handoff must preserve clause-level provenance through an explicit promotion-trace surface | `Appendix D: When Evaluating a Recovered NLSpec` | restated here as binding for `review_mode = promotion-batch` | `Authority review operating model`, `Other governed artifacts / Authority Review Packet`, `Promoted-output completeness and representation`, `Promotion Gate / PG-7` |
| accidental contracts must be explicitly preserved, carved out, excluded, or bounded rather than silently normalized | `Appendix D: When Evaluating a Recovered NLSpec` | restated here as binding for promotion-bearing recovery | `Promoted-output completeness and representation`, `Promotion Gate / PG-2` |
| final prose must be black-box clean | `Appendix D: When Evaluating a Recovered NLSpec` | restated here as binding for Level 3 contract prose | `Black-Box Constraint`, `Promoted-output completeness and representation`, `Promotion Gate / PG-15` |


### Normative language

In this document, `must` introduces a conformance requirement. `may` introduces a permitted option or an explicit implementation freedom. `should` is advisory only and does not determine workflow validity, slice advancement, authority routing, promotion, or ratification.

The workflow prose, workflow-owned tables, and workflow-owned checklists in this document are authoritative for process semantics. Canonical field-minimum tables for the artifact-role inventory, repository-surface-family record, plan, contract, validation report, evidence bundle, critique report, authority packet, authority disposition, Recovery Snapshot, and overlay matrix are authoritative in `SPEC-SHR-004`.

When this document says that a rule is `default and binding`, `default` identifies the baseline that governs unless a consuming domain overrides it under `Consuming-domain overrides`. It does not make the rule advisory.

### Consuming-domain overrides

Unless a local section says otherwise, a consuming domain may override a specific workflow requirement, protocol step, transition rule, checklist item, or workflow-owned artifact obligation without replacing the whole document.

A valid override must be declared once at an authoritative site in the consuming domain and must name the governing document id plus the affected section, rule, table, row, field, or checklist item together with the replacement rule, stricter rule, or narrowed scope.

All `SPEC-SHR-002` requirements not explicitly named by that override remain binding.

A workflow override against `SPEC-SHR-002` does not, by implication, override schema minima housed in `SPEC-SHR-004`. Schema changes must name `SPEC-SHR-004` explicitly.

Silence, implication, examples, local convenience templates, or looser prose do not override normative requirements.

A consuming-domain override must not silently displace governed-artifact continuity, authority-owned normative closure, or the Promotion Gate. Any intended displacement of one of those core invariants must be named explicitly at the override site.

When a workspace adopts `SPEC-SHR-003` and declares `local_override_locator`, every active local override against `SPEC-SHR-002` that governs future work in that workspace must also be indexed once in `reverse_recovery_overrides.workflow_overrides`. The index is discoverability only. It does not replace the authoritative local rule.

The minimum discoverability payload for that index is:

| Override kind | Required machine-discoverable payload | Discoverability meaning |
|---|---|---|
| `replacement-rule` | `affected_site_ref`, `override_kind`, `local_rule_ref`, `compatibility_note` | the cited local rule replaces the named shared workflow rule for the declared local scope |
| `stricter-rule` | `affected_site_ref`, `override_kind`, `local_rule_ref`, `compatibility_note` | the cited local rule adds stricter local conditions without replacing the shared owner surface wholesale |
| `narrowed-scope` | `affected_site_ref`, `override_kind`, `local_rule_ref`, `compatibility_note` | the cited local rule bounds where the shared workflow rule applies locally without silently changing the rule elsewhere |

`Affected_site_ref` must identify the affected section, rule, table, row, field, or checklist item reviewably enough that a reader can find the governing shared site without prose-only hunting. `Local_rule_ref` must identify the authoritative local rule that actually makes the override binding. `Compatibility_note` records any retained-artifact, migration, or mixed-corpus handling note needed to keep future work reviewable.

## Glossary and Normalization

This section is a governed glossary index. Authoritative definitions live only at the section-local sites named below. The glossary normalizes prose forms and schema forms without creating a second normative source of truth.

### Normalization rule

1. Use the canonical prose form when a governed term appears in narrative prose.
2. Use the canonical schema form when the same concept is represented as a governed field name, field value, or witness-classification literal.
3. Use an allowed alias only as a cross-reference or historical note. An allowed alias is non-governing unless the authoritative site restates it as canonical.
4. Treat any undeclared synonym or paraphrase as non-governing. It may aid reading, but it must not add or change normative meaning.
5. When the same literal can appear in more than one field, the field-local vocabulary declaration governs its meaning. Meaning does not transfer across fields by spelling alone.

### Glossary index

This glossary is exhaustive for governed concept terms. Closed and extensible literal sets are declared separately in Vocabulary Declarations.

| Governed term | Canonical prose form | Canonical schema form | Allowed aliases | Authoritative site |
|---|---|---|---|---|
| Recovery level | recovery level | n/a | none | Three Levels of Recovery |
| Workflow state | workflow state | `workflow_state` | none | Working vocabulary and Vocabulary Declarations |
| Support level | support level | `support_level` | none | Working vocabulary and Vocabulary Declarations |
| Disposition | disposition | `disposition` | none | Working vocabulary and Vocabulary Declarations |
| Ambiguity status | ambiguity status | `ambiguity_status` | none | Working vocabulary and Vocabulary Declarations |
| Negative evidence status | negative evidence status | `negative_evidence_status` | none | Working vocabulary and Vocabulary Declarations |
| Regime or environment status | regime or environment status | `regime_or_environment_status` | none | Working vocabulary and Vocabulary Declarations |
| Validation mode | validation mode | `validation_mode` | none | Working vocabulary and Vocabulary Declarations |
| Completeness dimension | completeness dimension | `completeness_dimensions` | completeness axis | Completeness and Criteria and Vocabulary Declarations |
| Active overlay | active overlay | `active_overlays` | overlay | Overlays and Vocabulary Declarations |
| Authority review mode | authority review mode | `review_mode` | none | Authority review operating model and Vocabulary Declarations |
| Normative effect | normative effect | `normative_effect` | none | Authority review operating model and Vocabulary Declarations |
| Memory layer | memory layer | `layer` when represented explicitly in recovery memory | none | Recovery Memory and Negative Evidence / Memory item minima and Vocabulary Declarations |
| Recovery Snapshot | Recovery Snapshot | `snapshot_ref` when represented as a governed artifact identity | none | Recovery snapshots and incremental resumption |
| Target anchor | target anchor | `target_anchor` | none | Workflow Overview / Entry minima before first Slice Contract |
| Repository surface family | repository surface family | `repository_surface_families` when represented as the repository-shaped entry-minimum record | none | `SPEC-SHR-004 / Repository-surface-family record for repository-shaped targets` |
| Execution harness | execution harness | `execution_harness` | none | Slice Contract |
| Promotion readiness | promotion readiness | `promotion_readiness` | none | Recovery snapshots and incremental resumption and Vocabulary Declarations |
| Authority independence | authority independence | `authority_independence` | none | Authority review operating model and Other governed artifacts / Authority Review Packet |
| Promotion trace matrix | promotion trace matrix | `promotion_trace_matrix` | none | Authority review operating model and Other governed artifacts / Authority Review Packet |
| Control-plane prose | control-plane prose | `control_plane_artifact` when represented as an artifact role | none | Control-plane prose and artifact-role inventory |
| Framing metadata | framing metadata | `framing_metadata` when represented as an artifact role | none | Control-plane prose and artifact-role inventory |
| Protected surface | protected surface | n/a | `negative contract`, `immutability boundary` | Control-plane prose and artifact-role inventory |
| Discriminating question | discriminating question | `discriminating_question` | none | Slice Contract |
| Discriminating evidence | discriminating evidence | n/a | none | Slice Contract |
| Decisive witness | decisive witness | n/a | none | Slice Contract |
| Recovered concept | recovered concept | n/a | none | `SPEC-SHR-004 / Overlays / Concept Recovery and Semantic Bases` |
| Concept-bearing slice | concept-bearing slice | n/a | none | `SPEC-SHR-004 / Overlays / Concept Recovery and Semantic Bases` |
| Abstract witness family | abstract witness family | `abstract_witness_form` | none | `SPEC-SHR-004 / Overlays / Abstract Witness Families and Instantiation Preservation` |
| Promotion-bearing lifecycle model | promotion-bearing lifecycle model | `promotion-bearing` in `model_mode` | none | `SPEC-SHR-004 / Overlays / Recovered Lifecycle Semantics` |
| State snapshot | state snapshot | `state_snapshot` | none | `SPEC-SHR-004 / Overlays / Recovered Lifecycle Semantics` |
| Transition trace | transition trace | `transition_trace` | none | `SPEC-SHR-004 / Overlays / Recovered Lifecycle Semantics` |
| Derived summary | derived summary | `derived_summary` | none | `SPEC-SHR-004 / Overlays / Recovered Lifecycle Semantics` |
| Semantics-first | semantics-first | n/a | none | Validation / Framing-ablation |
| Framing-ablation | framing-ablation | `framing_ablation` and `framing_ablation_result` | none | Validation / Framing-ablation |
| Framing-sensitive | framing-sensitive | n/a | none | Validation / Framing-ablation |
| Governed artifact identity | governed artifact identity | field-local: `contract_id`, `contract_ref`, `artifact_ref`, `packet_ref`, or `snapshot_ref` | stable identity | `SPEC-SHR-004 / Governed artifact identity, provenance, and attempt metadata` |
| Provenance coordinate | provenance coordinate | field-local: `locator` | provenance | `SPEC-SHR-004 / Governed artifact identity, provenance, and attempt metadata` |
| Attempt metadata | attempt metadata | n/a | volatile attempt data | `SPEC-SHR-004 / Governed artifact identity, provenance, and attempt metadata` |
| Source class | source class | n/a | none | Evidence and Black-Box Discipline / Source hierarchy |
| Witness locator | witness locator | `locator` | none | Other governed artifacts / Witness locator stability and decisive-witness selection |
| Continuity conflict | continuity conflict | n/a | none | Recovery Memory and Negative Evidence / Continuity conflicts |

## The Problem

The `Intent -> NLSpec -> Implementation` chain assumes forward construction: intent exists first, a spec is authored, implementation follows. Many real systems were not built that way. They were built incrementally, organically, under time pressure, with intent distributed across commit messages, issue threads, tests, documentation, tribal knowledge, prompts, runbooks, and the code itself. No forward-authored spec exists. The implementation is often the strongest surviving evidence of current behavior, but it is not yet a prescriptive NLSpec.

Reverse-engineering a spec means recovering, from an implementation and related evidence, a document that makes the system legible and potentially recreatable. This inverts the usual direction. Instead of `NLSpec -> Implementation`, the workflow attempts `Implementation -> Recovered Artifact`. That inversion is asymmetric. In forward authoring, intent constrains implementation. In reverse recovery, implementation constrains inference. The code and surrounding artifacts tell us a great deal about what the current system does. They tell us much less about which behaviors were intended, which are accidental, which are load-bearing, and which should bind future implementations.

## Three Levels of Recovery

What can be recovered from an existing system falls into three artifact classes.

**Level 1: Behavioral description.** This records what the system observably does. It may be rich and even exhaustive, but it remains descriptive. It says what the current system does, not yet what future implementations must do.

**Level 2: Provisional behavioral contract.** This classifies observed behavior by apparent contractual weight. Some clauses look intentional, boundary-central, externally depended on, or otherwise load-bearing. Others remain ambiguous or look incidental. Level 2 is useful because it makes both support and uncertainty explicit. It is not yet an NLSpec.

**Level 3: Ratified prescriptive specification.** A domain authority reviews the recovered material, decides what is normative, and ratifies a document that governs future implementations. Only this level may inherit the term NLSpec, and only if it also satisfies NLSpec completeness for the declared scope.

Level 2 is therefore a discipline of controlled provisionality. Recovery quality is not clause volume. A small set of witness-backed clauses with explicit support state and honest bounds is stronger than a large field of plausible but weakly supported prose.

## The Core Difficulty: Hyrum's Law and Classification

In mature systems, observable behavior and intended behavior drift together because callers adapt to what exists. A behavior that began as an accident may become load-bearing simply because enough downstream systems, operators, or agents rely on it.

For any recovered behavior, four states are possible:

|                              | Intended by author | Not intended by author |
|------------------------------|--------------------|------------------------|
| **Depended on by callers**   | Unambiguous requirement | Accidental contract |
| **Not depended on by callers** | Latent requirement | Implementation artifact |

Reverse recovery directly observes implemented behavior. Caller dependence and authorial intent are not directly visible. They are inferred from secondary evidence such as tests, telemetry, consumer code, support history, change history, control-plane artifacts, and runtime witnesses. The core difficulty is therefore classification, not observation.

An agent-consumed contract introduces a distinctive Hyrum risk. An agent may depend not only on formal fields or schemas but also on incidental phrasing, instruction order, example choice, formatting, or output layout. That does not make stochastic agent behavior equivalent to a formal guarantee. It does make agent traces, compliance failures, cross-model differences, and downstream tooling relevant evidence about the effective binding force of a surface.

The matrix above names the four intent and dependence states that recovery tries to classify. The classification dimensions below are the local signals used to place a candidate clause into one of those states, so the matrix and dimensions are intended to be read together for local classification work.

### Classification dimensions

Each nontrivial candidate clause must be triaged along at least the following dimensions.

| Dimension | Question | Effect on classification |
|-----------|----------|--------------------------|
| **Boundary observability** | Can a caller, adjacent system, agent, or operator observe divergence at the abstraction boundary? | Strong yes favors promotion. |
| **Compatibility impact** | Would divergence break callers, interoperability, or operational expectations? | Strong yes outweighs local plausibility or code neatness. |
| **Interface centrality** | Is the clause part of a public API, schema, protocol, default, state transition, error contract, or machine-parsed operational output? | Strong yes favors promotion. |
| **Coordination-boundary alignment** | Does the boundary show durable coordination-seam hallmarks such as versioned contracts, backward-compatibility shims, defensive parsing, asynchronous handoff, or visible independent upgrade boundaries? | Strong yes makes the surface a candidate protected surface and raises its likely contractual durability beyond what local code complexity alone would predict. |
| **Artifact role** | Is the clause carried by a fixed boundary artifact, control-plane prose, runtime ledger, derived analysis, proposal, or framing metadata? | Strong support from boundary artifacts, control-plane prose, or ledgers favors promotion. Proposal-only or framing-only support blocks it. |
| **Framing sensitivity** | Does the clause, critique outcome, or validator outcome change materially when non-binding framing cues are ablated while stronger witnesses stay fixed? | Strong yes marks the slice `framing-sensitive` and caps support until stronger evidence dominates. |
| **Runtime sensitivity** | Does the clause depend on timing, sequencing, concurrency, configuration, recovery logic, or environment? | Strong yes requires executable confirmation when obtainable. |
| **Environment dependence** | Does the clause depend on caches, generated state, warm starts, hardware, feature flags, or fixed budget conventions? | Strong yes requires explicit environment capture before promotion. |
| **Metric centrality** | Is the clause part of a metric or a decision gate, and is the measurement regime fully recovered? | Strong yes requires metric-contract reconstruction before promotion. |
| **External dependence** | Is there affirmative evidence that consumers, operators, or adjacent systems rely on the behavior? | Strong yes may elevate an accidental contract. |
| **Criteria legibility** | If the clause is qualitative, are the governing criteria explicit, observable, and prioritized? | Strong no blocks promotion. |
| **Mechanistic residue** | Is the clause mostly about current decomposition, library choice, file layout, or incidental control flow? | Strong yes argues against promotion into the recovered contract. |

`Coordination-boundary alignment` is a software-observable heuristic recovered from seam signals such as interface-formality gradients, versioned compatibility machinery, defensive parsers, asynchronous handoff surfaces, and visible upgrade boundaries. It is a classification aid, not proof of actual team structure, staffing decisions, or project chronology.

Framing sensitivity is orthogonal to artifact role and runtime sensitivity. A clause can be carried by a strong boundary artifact and still be `framing-sensitive` if its proposal, critique, or validation outcome flips when non-binding contextual cues are removed or contradicted.

### Working vocabulary

A useful Level 2 vocabulary distinguishes workflow state, support level, disposition, ambiguity status, negative evidence status, regime or environment status, validation mode, risk or compatibility impact, completeness dimensions, and active overlays. These are different records and must not be collapsed into one generic confidence score. They are also distinct from `SPEC-SHR-003` workspace structural posture and from `SPEC-SHR-000` governed-document lifecycle `status`. Shared spellings or adjacent lifecycle prose do not collapse those records into one another. The literal sets used by governed artifacts are declared in Vocabulary Declarations.

**Workflow state** tracks where a slice sits in the process. The closed `workflow_state` vocabulary is declared in Vocabulary Declarations. A slice may also carry a `revalidation_required` flag when prior witnesses are affected by a validator defect, known unsoundness window, continuity conflict, or another material basis impairment. `revalidation_required` is a separate flag or note, not a workflow-state member. When `revalidation_required` becomes true, the recovery coordinator must recompute `workflow_state` under Validation / Slice advancement and revalidation rather than inferring that the prior state remains valid.

**Support level** tracks how strongly a clause is grounded. The closed `support_level` vocabulary is declared in Vocabulary Declarations. A support record may also carry a `framing-sensitive` note, as defined in Validation / Framing-ablation. The note is not a separate support level.

**Disposition** tracks the current review judgment. The closed `disposition` vocabulary is declared in Vocabulary Declarations.

**Ambiguity status** tracks whether material ambiguity is still open, has been bounded by scope, or is believed absent. It governs how `ambiguity_note` is read on a Specification Plan entry.

**Negative evidence status** tracks whether negative evidence has not yet been searched for, has been searched for and not found, or has been found and linked. It governs how `negative_evidence_refs` is read.

**Regime or environment status** tracks whether regime or environment conditions are materially load-bearing and, if so, whether they have been captured, remain pending, or remain unknown. It governs how `regime_or_environment_note` is read.

**Validation mode** tracks whether a Slice Contract closes through executable confirmation or an explicit non-executable resolution. It governs whether `executable_action` or `non_executable_resolution` is required.

**Risk or compatibility impact** records what is at stake if the clause is wrong. The field is intentionally extensible because the correct impact categories are domain-specific, but the working record must make the impact explicit.

### Vocabulary Declarations

The following declarations are authoritative for governed artifact literals and for the appendix examples in this document.

| Vocabulary | Governing field or site | Declaration | Members or minimum required members | Notes |
|---|---|---|---|---|
| Recovery levels | Three Levels of Recovery | closed | `Level 1`, `Level 2`, `Level 3` | Artifact classes only. |
| Workflow state | `workflow_state` | closed | `planned`, `in-progress`, `validation-pending`, `blocked`, `rejected`, `out-of-scope`, `authority-required`, `closed` | `revalidation_required` remains separate from the state vocabulary. |
| Support level | `support_level` | closed | `observed`, `corroborated`, `runtime-validated`, `externally-depended-on`, `authority-ratified` | A `framing-sensitive` note does not create another support level. |
| Disposition | `disposition` | closed | `approved`, `rejected`, `needs-refinement`, `authority-required` | Governs review judgment. |
| Ambiguity status | `ambiguity_status` | closed | `none-known`, `known-open`, `bounded-by-scope`, `unreviewed` | Governs interpretation of `ambiguity_note`. |
| Negative evidence status | `negative_evidence_status` | closed | `not-searched`, `searched-none-found`, `found` | Governs interpretation of `negative_evidence_refs`. |
| Regime or environment status | `regime_or_environment_status` | closed | `not-material`, `captured`, `capture-pending`, `unknown` | Governs interpretation of `regime_or_environment_note`. |
| Validation mode | `validation_mode` | closed | `executable`, `non-executable` | Governs whether `executable_action` or `non_executable_resolution` is required. |
| Execution harness | `executable_action.execution_harness` | extensible | must support at least `shell`, `python`, `container`, and `external_runner` | Capability-class field only. Branded tools remain non-governing. |
| Completeness dimensions | `completeness_dimensions` | closed | `behavioral`, `interface`, `boundary`, `metric`, `environment` | No additional completeness axis is introduced by overlays. |
| Active overlays | `active_overlays` | closed | `concept_overlay`, `abstract_witness_overlay`, `lifecycle_overlay` | Overlays extend the base artifacts rather than creating parallel artifact families. |
| Memory layer | recovery-memory item layer when represented explicitly | extensible | must support at least `contract-pattern`, `strategy`, and `technical-witness` | A local recovery domain may add more layers only by an explicit local rule or consuming-domain override. |
| Authority review mode | `Authority Review Packet.review_mode` | closed | `per-slice`, `bounded-batch`, `promotion-batch` | Omission semantics live in Authority review operating model. |
| Authority independence | `Authority Review Packet.authority_independence` | closed | `independent-reviewer`, `same-operator-explicit-local-authority` | Omission semantics live in Authority review operating model. |
| Promotion trace representation class | `Authority Review Packet.promotion_trace_matrix[*].representation_class` | closed | `main_contract_clause`, `compatibility_clause_or_appendix`, `explicit_exclusion_or_carve_out`, `explicit_out_of_scope_note` | Used only when `review_mode = promotion-batch`. |
| Promotion readiness | `Recovery Snapshot.promotion_readiness` | closed | `recon-only`, `slice-backed`, `gate-candidate` | Omission semantics live in Recovery snapshots and incremental resumption. |
| Step status | `step_list.status` | closed | `planned`, `in-progress`, `done`, `blocked`, `discarded`, `superseded` | Governs iterative slice steps only. |
| Criterion outcome | `criterion_results.outcome` | closed | `pass`, `fail`, `blocked`, `untested` | Governs criterion-local execution results. |
| Validator outcome | `validator_record.validator_outcome` | closed | `proven`, `disproven`, `unsupported`, `heuristic-only` | `unsupported` is not evidence of falsehood by itself. |
| Lifecycle model mode | `lifecycle_overlay.model_mode` | closed | `descriptive`, `promotion-bearing` | The conceptual definitions live in Recovered Lifecycle Semantics. |
| Lifecycle witness classification | `lifecycle_overlay.witness_classification.kind` | closed | `state_snapshot`, `transition_trace`, `derived_summary` | Distinct from the more general extensible `witness.kind` field. |
| Artifact role | `artifact_roles.role` | extensible | must support at least `fixed_boundary_artifact`, `mutable_operator_surface`, `control_plane_artifact`, `framing_metadata`, `runtime_ledger`, `machine_parsed_operational_output`, and `derived_analysis` | Additional artifact roles are allowed when the local recovery domain needs them. |
| Witness kind | `witness.kind` | extensible | must support at least `runtime_witness`, `static_corroborant`, `compatibility_check`, `support_history`, `state_snapshot`, `transition_trace`, `derived_summary`, `control_plane_artifact`, and `negative_evidence` | Additional witness kinds are allowed when named explicitly. |
| Source role | `witness.source_role` | extensible | must support at least `runtime_ledger`, `fixed_boundary_artifact`, `derived_analysis`, and `control_plane_artifact`; any other named `source_role` may be added explicitly | Adding a `source_role` literal does not create a new source class. Each added `source_role` must align with an inventoried artifact role or be mapped explicitly to a source class under Evidence and Black-Box Discipline / Source hierarchy. |
| Observable or witness class | `required_criteria.observable_or_witness_class` | extensible | field-local labels are allowed; examples in this document normalize them as snake_case schema literals | The class must remain explicit and reviewable. |
| Action kind | `executable_action.action_kind` | extensible | field-local labels are allowed; examples in this document normalize them as snake_case schema literals | The action kind must remain explicit and reviewable. |
| Risk or compatibility impact | `risk_impact` | extensible | field-local explicit impact statements are allowed; examples in this document use direct prose statements | The field must still make impact explicit. |
| Artifact inventory scope classification | artifact-role inventory item `scope_classification` | closed | `in-scope`, `out-of-scope`, `not-boundary-bearing` | Governs explicit treatment of currently known surfaces. |
| Repository surface family | `repository_surface_families[*].surface_family` | closed | `build_graph_and_packaging_boundary`, `deployment_unit_and_release_topology`, `configuration_plane_and_default_source`, `generated_artifact_or_codegen_boundary`, `extension_plugin_or_hook_surface`, `data_store_schema_and_migration_seam`, `service_topology_and_cross_service_coordination_seam` | Repository-shaped targets only. |
| Repository surface family assessment | `repository_surface_families[*].assessment` | closed | `present-in-scope`, `present-out-of-scope`, `present-not-boundary-bearing`, `not-present`, `inspection-pending` | Repository-shaped targets only. |
| Assertion basis | `Evidence Bundle.chains.assertion_basis` or equivalent local chain-capable carrier | extensible | must support at least `direct-observation`, `inference`, and `interpretation` | Distinguishes how a synthesized chain claim was formed. |
| Authority decision | `Authority Disposition.decision` | extensible | must support at least `approved`, `approved-with-exceptions`, `rejected`, `deferred`, and `scope-bounded` | Domains may add narrower decision literals when authority review requires them. |
| Authority normative effect | `Authority Disposition.normative_effect` | extensible | must support at least `promote`, `bound`, `exclude`, `defer`, and `carve-out` | Required when an authority decision is promotion-bearing or scope-altering. |

## Recovery Inputs and Artifact Roles

### Control-plane prose and artifact-role inventory

This section is the authoritative definition site for `control-plane prose`, `framing metadata`, and `protected surface`.

Control-plane prose includes prompts, runbooks, operator manuals, policies, evaluator instructions, migration playbooks, or similar prose artifacts that govern mutation boundaries, acceptance logic, state transitions, escalation paths, or roles. When a governed artifact inventories such a surface as an artifact role, use the schema literal `control_plane_artifact`.

Framing metadata is non-binding contextual metadata that may steer interpretation without itself being part of the contract unless the recovery objective independently establishes that the surface is contract-bearing. When a governed artifact inventories such a surface as an artifact role, use the schema literal `framing_metadata`.

A protected surface is a boundary artifact whose stability is itself load-bearing because changing it would break comparability, safety, interoperability, or operational meaning. Allowed aliases are `negative contract` and `immutability boundary`. The working record must state what must not change and what breaks if the protection is violated.

The effective contract of a real system is often distributed across more than executable code. Reverse recovery must therefore treat binding prose and artifact roles as first-class targets from the beginning.

A useful subtype is the **prose-orchestrated system**: a system in which prose does not merely constrain executable orchestration but replaces some or all of it. In such systems, the control loop may live primarily in prompts, runbooks, evaluator instructions, or operator policy. That does not make every prose artifact binding. It means recovery should ask whether adjacent tools, agents, or humans actually treat the prose as the operating contract.

A first pass over a new target should usually produce an artifact-role inventory before broad clause extraction begins. At minimum, the working record should distinguish:

- `fixed_boundary_artifact`
- `mutable_operator_surface`
- `control_plane_artifact`
- `framing_metadata`
- `runtime_ledger`
- `machine_parsed_operational_output`
- `derived_analysis`

Each inventoried artifact must also record mutation authority: who or what may change it, under what conditions, and what contract breaks if that boundary is crossed.

A surface may be fixed not merely because it exists, but because its stability preserves comparability, safety, or operational meaning. When so, recovery must name it explicitly as a protected surface instead of leaving the prohibition implicit. The working record must say what must not change and what breaks if the protection is violated. Protected surfaces are the strongest case of mutation authority, not the only case. Boundaries with coordination-seam hallmarks deserve early scrutiny during inventory because they often carry contractual weight disproportionate to their local technical surface and may require protected-surface treatment.

Framing metadata may generate slices, suggest likely intent, or narrow historical hypotheses. It does not materially raise support for a disputed load-bearing clause without independent corroboration from stronger witnesses, and it must not be conflated with control-plane prose.


#### Artifact-role inventory minimum shape and linkage

The workflow requires a reviewable artifact-role inventory before the first advancing Slice Contract and whenever a new boundary-bearing surface is discovered.

This document continues to govern the workflow meaning of inventory-first recovery, scope classification as an explicit workflow decision, and the rule that slice-local `artifact_roles` entries may narrow but must not silently contradict the authoritative inventory item they cite.

The canonical minimum row shape, field-status legend, omission semantics, and linkage rules for the artifact-role inventory now live in `SPEC-SHR-004 / Artifact-role inventory minimum shape and linkage`.

Use `SPEC-SHR-004` for field minima. Use this document for the workflow rule that a currently known boundary-bearing surface must not disappear by silence.

#### Repository-surface-family record for repository-shaped targets

The workflow requires a row-complete repository-surface-family record for repository-shaped targets so major surface families do not disappear by silence.

This document continues to govern the workflow meaning of explicit family classification, entry-time row completeness, and promotion-time closure expectations.

The canonical row shape, field-status legend, omission semantics, linkage rules, and family-specific closure-fact minima for `repository_surface_families` now live in `SPEC-SHR-004 / Repository-surface-family record for repository-shaped targets`.

`Inspection-pending` remains an allowed temporary state at entry. It is not an acceptable permanent substitute for classification on a family that materially intersects promoted scope. Likewise, a `present-in-scope` family that materially intersects promoted scope is not promotion-ready until its row carries reviewable `closure_basis_refs` whose linked records collectively close the family-specific minimum facts owned by `SPEC-SHR-004`.

### Recovery inputs

Legitimate recovery inputs include code, tests, configurations, schemas, migration shims, prompts, runbooks, logs, traces, runtime ledgers, consumer code, telemetry, incident history, support history, and public documentation. These inputs are not equal in evidentiary weight, but they are all valid discovery surfaces when attached to specific slices.

## Workflow Overview

Reverse recovery must not proceed as one giant summarization pass. It must proceed in short loops that isolate one unresolved slice at a time, recover it, validate it or bound it, update the working artifacts, and either close it or spawn the next corrective step.

A useful default loop is:

1. Declare the recovery objective and compatibility target.
2. Build or refresh the artifact-role inventory.
3. Open or refresh the Specification Plan.
4. Choose one unresolved slice by load-bearing risk rather than source-tree order.
5. Create or update the Slice Contract for that slice.
6. Gather evidence and, when appropriate, perform executable or otherwise checkable validation.
7. Emit a Validation Report or an explicit non-executable resolution.
8. Update the plan, evidence bundle, negative evidence, and authority needs, then either repeat or stop.


### Entry minima before first Slice Contract

A first Slice Contract is valid only when the recovery record already makes the entry minima below explicit. These minima may remain provisional, but they must be reviewable enough that an independent reader can explain why the first slice was opened, what boundary it is intended to recover, and which current system shape the work is anchored to.

For a short normative assembly of this minimum stack, use `SPEC-SHR-005` in the specific in-repository cold-start case and `SPEC-SHR-006` in the specific adjacent-workspace cold-start case. Those documents assemble the owner surfaces. They do not replace them.

| Entry minimum | What must be explicit | Notes |
|---|---|---|
| Scope record | current declared recovery scope | The scope record may live in a system sketch or an equivalent working artifact. |
| Recovery-objective record | `recovery_objective` | One explicit statement may govern several unopened candidate slices. |
| Compatibility-target record | `compatibility_target` | It must be concrete enough to explain what callers, behaviors, or migration boundaries matter. |
| Target-anchor record | `target_anchor` | Uses the normalized form and comparison rules immediately below. |
| Artifact-role inventory | at least a draft artifact-role inventory covering currently known boundary surfaces | Draft status is allowed if every currently known item still carries stable identity, explicit scope classification, mutation authority, mutation conditions, and violation effect. Silence is not. |
| Repository-surface-family record | a row-complete `repository_surface_families` record when the target is repository-shaped | `inspection-pending` is allowed at entry, but any family materially intersecting promoted scope must not remain `inspection-pending` at promotion time. A `present-in-scope` family that materially intersects promoted scope must also carry reviewable `closure_basis_refs` before promotion-bearing review. |
| Slice seed | at least one proposed recovery slice represented as a candidate-slice record or as a first Specification Plan entry | The first Slice Contract may not be the first place where the slice exists. |

#### Minimum content floor for the system sketch and slice seed

The `Scope record` and `Slice seed` rows above intentionally allow small working artifacts rather than forcing one new governed artifact family. That flexibility does not waive minimum content.

When the current declared scope is carried by a system sketch or an equivalent scope record, that record must make at least the following facts explicit:

| Required fact | Meaning |
|---|---|
| system identity | stable designation of the target system or bounded subsystem under recovery |
| declared scope cut | the current scope boundary for the recovery effort |
| primary boundary surfaces in view | the boundary surfaces or repository-surface families that currently define the immediate recovery frontier |
| principal external actors or adjacent systems | the callers, operators, adjacent systems, or operator classes that materially interact with the declared scope |

When the first slice exists as a candidate-slice record outside the Specification Plan, that record must make at least the following facts explicit:

| Required fact | Meaning |
|---|---|
| stable seed label | stable local handle for the proposed slice |
| targeted boundary surface | the boundary surface or repository-surface family the first slice is expected to recover |
| provisional question or provisional clause | the smallest reviewable question or clause the first contract is expected to test |
| selection reason | why this slice is first, such as closure speed, caller consequence, or risk concentration |

A candidate-slice record carried outside the Specification Plan may omit a seed-local `target_anchor` only when the current working record already carries one explicit active `target_anchor`. In that case omission means inheritance from the current working record. Once the slice seed becomes a Specification Plan entry, the plan-local `target_anchor` requirement applies and inheritance by silence is invalid.

`Target_anchor` identifies the revision, release family, deployment family, environment regime, consumer version, data snapshot, feature regime, or explicit composite anchor for which the current slice claim is asserted. When more than one dimension materially affects behavior, the record must use an explicit composite anchor. A bare revision identifier is sufficient only when that revision fully determines the in-scope behavior.

#### Target-anchor normalized form and comparison rules

Unless a consuming domain defines a stricter local rule, `target_anchor` uses the following corpus-default normalized form:

- single-axis anchor: `<axis>=<stable-reviewable-id>`
- composite anchor: `<axis>=<stable-reviewable-id> + <axis>=<stable-reviewable-id> + ...`

Within a composite anchor, do not repeat an axis name. Use the core axis order below when a listed axis is present. Any local extension axis must follow the core axes and must then sort lexicographically by axis name.

| Core axis | Use when the axis materially affects the in-scope behavior | Example |
|---|---|---|
| `revision` | the repository revision or equivalent build identity fully determines the in-scope behavior | `revision=repo@8f31c2` |
| `release-family` | release-branch or compatibility family matters independently of one repository revision | `release-family=release@2026-02` |
| `deployment-family` | the deployed topology or productized variant changes boundary behavior | `deployment-family=workflow-engine-2026-02` |
| `environment-regime` | environment class, execution regime, or policy regime materially changes the claim | `environment-regime=isolated-replay-v1` |
| `consumer-version` | a downstream parser, adapter, client, or caller version materially constrains the boundary claim | `consumer-version=parser@release-tool-2026-02` |
| `data-snapshot` | dataset identity or fixture identity materially changes comparability or boundary behavior | `data-snapshot=fixture-corpus-v7` |
| `feature-regime` | feature flags or policy bundles materially change the boundary contract | `feature-regime=flags@default-export-path` |

Axis state rules are:

- omitting an axis means the axis is not material for the declared slice and scope
- if an axis is material but the exact value is not yet known, record `axis=unknown`
- if an axis is material and capture is still pending, record `axis=capture-pending`
- omission must not be used to mean `unknown` or `capture-pending`

Comparison is deterministic and uses only the normalized axis form.

| Comparison outcome | Rule | Required workflow consequence |
|---|---|---|
| `equal` | the two anchors contain the same axis set in canonical order and every axis has the same concrete value | no anchor-driven revalidation is required |
| `materially-different` | any axis has a different concrete value, or an axis appears in one anchor and not the other | treat the active claim as anchor-changed; do not reuse the old contract or closure basis by implication |
| `non-comparable` | either anchor contains `unknown` or `capture-pending` for a material axis, or a local authoritative rule explicitly marks the comparison indeterminate | treat the active claim as materially unstable until the indeterminacy is resolved |

A material anchor change exists whenever comparison returns `materially-different` or `non-comparable`. The same rule governs plan refresh, contract reuse, Validation Report interpretation, revalidation, and Recovery Snapshot resume. Unknown local extension axes must not silently compare equal.

`Target_anchor` values must use stable reviewable identifiers. They must not encode secrets, ephemeral credentials, temporary filesystem coordinates, or session-local handles.

This section governs required entry records only. It does not standardize reconnaissance method, storage topology, or local file placement. When a governed workspace adopts `SPEC-SHR-003`, that specification defines the corpus-default minimum topology and any non-default topology declaration.

### Hypothesis recovery and execution-grounded confirmation

Reverse recovery performs two different kinds of work. First, it generates candidate clauses from static analysis, tests, documentation, traces, logs, and configuration. Second, it confirms or rejects those clauses against runtime behavior or other decisive checks. Discovery and confirmation must be recorded as different operations. Static evidence is often enough to recover candidate clauses and many stable interface facts. It is not, on its own, enough for claims whose truth depends on timing, state, sequencing, retries, concurrency, configuration, environment, or distributed interaction. When executable witnesses are obtainable, those claims must not advance beyond `corroborated` on static evidence alone.

### Coordinating and review functions

A reverse-recovery workflow must distinguish at least six functions:

- the **recovery coordinator**, which validates inputs, chooses discovery versus targeted confirmation, maintains the plan, and enforces stop conditions
- the **clause proposer**, which drafts candidate clauses
- the **clause critic**, which challenges whether the evidence really supports the clause
- the **runtime validator**, which checks whether a proposed probe or executable action is sound before it runs
- the **evidence interpreter**, which translates low-level traces or failures into contractual significance
- the **authority reviewer**, which decides what becomes normative

These are roles, not necessarily different people or different agents. One actor may perform all of them, but not in one unexamined pass. In agentic settings, the coordinating role also scopes tool access and stabilizes the artifact formats passed between functions.

Self-evaluation, exposed validators, and heuristic judges may still help triage. They are weak support for disputed, qualitative, or runtime-sensitive clauses until an independent check or explicit authority disposition closes the gap.

### Default interaction protocol

Unless a consuming domain defines an alternative protocol under Normative language / Consuming-domain overrides, the following interaction protocol governs slice progression and conflict handling.

1. The recovery coordinator opens or refreshes the slice, updates the Specification Plan entry, records the current `workflow_state` and `disposition`, and assigns the next governed action.
2. The clause proposer drafts or revises the candidate clause and the Slice Contract. A proposal is not eligible for execution, advancement, or promotion until it has passed through critique.
3. The clause critic either clears the draft or emits a Critique Report naming objections, missing criteria, and requested revisions.
4. If critic objections remain unresolved, the recovery coordinator must return the slice to active work by setting `disposition` to `needs-refinement` and `workflow_state` to `in-progress`, or by marking the slice `authority-required` when only a normative decision can close the dispute.
5. When the next step is executable, the runtime validator must approve or block the `executable_action` before execution. A blocked probe is negative evidence against the probe design, fixture design, or stated assumptions, not automatically against the candidate clause itself.
6. After execution or explicit non-executable closure, the evidence interpreter writes the Validation Report, states the contractual significance of the witnesses, and recommends the next move.
7. When authority review is required, the recovery coordinator must open or refresh an Authority Review Packet and route the question through Authority review operating model. Only the authority reviewer may close promotion-bearing normative questions, approve scope carve-outs that alter contractual force, or ratify a Level 3 artifact, and such closure must be recorded in an Authority Disposition.
8. The recovery coordinator then updates the plan entry, records any revalidation requirement, and either closes the slice or schedules the next corrective action.

A consuming domain may override one of the following conflict rules without replacing the whole protocol, but the override must satisfy Normative language / Consuming-domain overrides.

The following conflict rules are default and binding under Normative language / Consuming-domain overrides:

- unresolved critic rejection blocks slice advancement until revision or authority disposition
- runtime-validator rejection blocks execution until probe repair or explicit authority disposition, but it does not by itself reject the clause
- the recovery coordinator owns workflow-state transitions and governed-artifact continuity
- the authority reviewer owns normative closure and ratification
- silence, objection fatigue, or a missing critic pass does not count as approval


### Default workflow-state transitions

This subsection is the sole normative owner of the default `workflow_state` transition matrix. `workflow_state`, `disposition`, and `support_level` remain distinct from one another and from `SPEC-SHR-003` workspace structural posture. A transition listed here is allowed by default. A transition omitted here is unspecified unless a consuming domain names it explicitly under Consuming-domain overrides.

| Family | `from_state` | `to_state` | Governing trigger | Minimum required condition | Required artifact updates | Owning role | Notes |
|---|---|---|---|---|---|---|---|
| opening and active-work | `planned` | `in-progress` | The recovery coordinator opens a selected slice for active work. | The slice is selected as the next unresolved question and the next governed action is assigned. | Update `workflow_state`, refresh `disposition`, and record `next_corrective_action`, `current_step_id`, and `step_list`. | recovery coordinator | Opening a slice does not by itself raise support. |
| scope-bounding | `planned` | `out-of-scope` | The declared recovery objective or compatibility target excludes the slice before active work begins. | The exclusion is explicit in current scope rather than implied by omission. | Update `workflow_state`, bound or remove the plan entry, and record any replacement follow-up slice. | recovery coordinator | Use explicit scope, not silent abandonment. |
| opening and active-work | `in-progress` | `validation-pending` | Drafting and critique clear the slice for validation. | The current Slice Contract is ready and unresolved critic rejection no longer blocks advancement. | Update `workflow_state`; preserve the current Slice Contract and any consulted memory refs. | recovery coordinator | This is the ordinary validation-entry route. |
| authority-routing | `in-progress` | `authority-required` | Only a normative judgment can close the live dispute. | Critic objections or live alternatives remain and technical work alone cannot close them. | Set `workflow_state` to `authority-required`; open or refresh an Authority Review Packet; keep basis and objections explicit. | recovery coordinator | Do not infer approval from silence or objection fatigue. |
| scope-bounding | `in-progress` | `out-of-scope` | Scope narrows or compatibility target changes while work is active. | The new scope intentionally excludes the slice. | Update `workflow_state`; revise `candidate_clause`, `stop_condition`, and `next_corrective_action` to match the narrowed scope. | recovery coordinator | Scope change must remain explicit. |
| validation-entry and validation-exit | `validation-pending` | `in-progress` | Critique, failed criteria, or probe design issues require revision. | The current contract no longer answers the live discriminating question cleanly. | Set `workflow_state` to `in-progress`; set `disposition` to `needs-refinement`; update `next_corrective_action`, `current_step_id`, and the affected step records. | recovery coordinator | This is the ordinary refine-and-retry route. |
| blocker and unblocker | `validation-pending` | `blocked` | The runtime validator blocks execution or a material technical blocker appears. | The executable or non-executable path cannot proceed until probe, environment, or witness issues are repaired. | Set `workflow_state` to `blocked`; record the blocker basis and an immediate corrective task. | runtime validator recommends; recovery coordinator records | A blocked probe is negative evidence against probe design, not automatically against the clause. |
| authority-routing | `validation-pending` | `authority-required` | Validation closes the technical question but leaves a normative decision open. | `authority_disposition_needed = true`, a proposed carve-out alters contractual force, or another authority-owned question remains. | Set `workflow_state` to `authority-required`; open or refresh an Authority Review Packet; preserve decisive basis and open objections. | recovery coordinator | This is the common post-validation authority route. |
| validation-entry and validation-exit | `validation-pending` | `rejected` | Decisive evidence refutes the current candidate clause as written. | The current clause is no longer defensible and the workflow records a rejected clause rather than mere incompleteness. | Set `workflow_state` to `rejected`; preserve negative evidence, rejected-clause memory, and any replacement corrective action. | recovery coordinator | Rejection attaches to the current clause, not automatically to every narrower neighboring clause. |
| closure transitions | `validation-pending` | `closed` | All required criteria pass, or explicit non-executable closure resolves the active question. | Every required criterion has a passing witness or an explicit scope or authority disposition. | Set `workflow_state` to `closed`; preserve the Validation Report and decisive witness refs; update support and disposition as warranted. | recovery coordinator | The default close path passes through `validation-pending`. |
| blocker and unblocker | `blocked` | `in-progress` | The blocker requires contract redesign or broader technical clarification. | The live question or method must change before another validation attempt. | Set `workflow_state` to `in-progress`; revise `next_corrective_action`, `current_step_id`, and any affected step records or contract fields. | recovery coordinator | Use this route when repair changes the question or method. |
| blocker and unblocker | `blocked` | `validation-pending` | The blocker is repaired without changing the live question. | The same contract remains valid and the validation path is ready again. | Set `workflow_state` to `validation-pending`; refresh execution-specific assumptions or preconditions when needed. | recovery coordinator | Use this route when the repair is local to probe or environment. |
| authority-routing | `blocked` | `authority-required` | Technical repair is exhausted and only a normative decision can close the slice. | The blocker cannot be cleared technically, but contractual force still depends on the answer. | Set `workflow_state` to `authority-required`; packet the bounded question together with the blocker basis. | recovery coordinator | Common when safe execution is unavailable but the clause still matters. |
| scope-bounding | `blocked` | `out-of-scope` | The blocked surface is explicitly removed from the declared scope. | The exclusion is recorded as a scope decision rather than a silent stop. | Set `workflow_state` to `out-of-scope`; preserve the scope boundary and any replacement corrective action. | recovery coordinator | Use a bounded exclusion when the work stops for scope reasons rather than for evidence reasons. |
| authority-routing | `authority-required` | `in-progress` | Authority defers, requests more evidence, or requests reframing. | The authority reviewer does not close the question and the next move is technical work. | Set `workflow_state` to `in-progress`; update `next_corrective_action`, packet follow-up, and any open objections. | recovery coordinator following authority disposition | Authority review may reopen active work. |
| closure transitions | `authority-required` | `rejected` | Authority rejects the candidate clause or question as framed. | An Authority Disposition records rejection or an equivalent exclusion of the current clause. | Set `workflow_state` to `rejected`; preserve `packet_ref`, `basis_refs`, rationale, and follow-up action. | authority reviewer decides; recovery coordinator records | This is normative rejection of the reviewed question. |
| scope-bounding | `authority-required` | `out-of-scope` | Authority bounds scope or excludes the questioned surface from contractual force. | An Authority Disposition records `scope-bounded`, `bound`, `exclude`, or `carve-out` effect. | Set `workflow_state` to `out-of-scope`; preserve the authority boundary and remaining exceptions. | authority reviewer decides; recovery coordinator records | Bounded exclusion is not silent omission. |
| closure transitions | `authority-required` | `closed` | Authority approves or ratifies the remaining normative question. | The Authority Disposition closes the open normative issue and no further corrective move remains for the slice. | Set `workflow_state` to `closed`; preserve packet and disposition refs; raise support when explicit authority ratification applies. | authority reviewer decides; recovery coordinator records | Use this path for promotion-bearing or scope-altering closure. |
| closure and revalidation | `closed` | `blocked` | Later evidence materially impairs the technical basis of the current closure. | `revalidation_required = true` and the impaired witness was material to the slice's current closure or promotion basis. | Set `workflow_state` to `blocked`; enumerate impacted witnesses or artifacts; insert an immediate corrective task. | recovery coordinator | Closed slices are not irrevocable when their basis is impaired. |
| closure and revalidation | `closed` | `authority-required` | Later evidence materially impairs the closure basis and normative judgment is now needed. | `revalidation_required = true` and technical repair alone can no longer sustain current contractual force. | Set `workflow_state` to `authority-required`; enumerate the impaired basis; open or refresh an Authority Review Packet. | recovery coordinator | Use this path when prior closure cannot be preserved by technical repair alone. |

By default, `rejected`, `out-of-scope`, and `closed` are non-active end states for the current clause or scope cut. `Closed` may reopen under revalidation when its basis is materially impaired.

### Unspecified workflow-state transitions

The transitions below are not given default semantics by this specification. A consuming domain that wants one of them must name it explicitly at an authoritative local site and keep the core invariants intact.

| Transition | Why the base workflow leaves it unspecified | A valid local override would have to name | Core invariants that still remain binding |
|---|---|---|---|
| `planned` -> `rejected` | The base workflow defines planning, drafting, critique, validation, and authority review, but it does not define rejecting a slice before active work or before a clause has been materially tested. | The affected `workflow_state` route, the condition that makes pre-work rejection legitimate, and whether the rejected object is the slice, the current clause, or both. | Governed-artifact continuity, explicit negative-evidence recording, and no silent bypass of later authority review for promotion-bearing questions. |
| `planned` -> `authority-required` | Authority routing is opened when a normative question is live, but the base protocol does not require direct routing from untouched planning state. | The trigger for direct authority escalation, the packet-minimum context, and what evidence is sufficient before authority sees an unopened slice. | Packet-first authority review, least-context review discipline, and no silent displacement of the Promotion Gate. |
| `in-progress` -> `rejected` | Active drafting can surface weak ideas, but the base protocol does not define a direct rejection cutover from drafting alone. It routes unresolved or revised work through critique, validation, or authority review. | Whether drafting-stage rejection is allowed, what minimum critique or evidentiary threshold it requires, and how rejected-clause memory is preserved. | Negative-evidence preservation, explicit `disposition` recording, and no silent loss of reviewable basis. |
| `in-progress` -> `closed` without `validation-pending` | The default close path in this specification runs through executed or explicit non-executable closure recorded by a Validation Report. | The exact shortcut condition, the required replacement for the missing validation-entry record, and the closure evidence needed to preserve reviewability. | Validation Report continuity, explicit decisive basis, and no bypass of `PG-9`, `PG-10`, `PG-13`, or `PG-14` when they apply. |
| `rejected` -> `in-progress` | This workflow records rejected clauses and dead-end hypotheses, but it does not define whether reopening is the same slice, a new clause revision, or a new slice identity. | The identity rule for reopening, the supersession or replacement rule, and the treatment of earlier negative evidence. | Governed-artifact identity, supersession basis, and continuity-conflict handling. |

### Revalidation-triggered workflow-state recomputation

When `revalidation_required` becomes true, the recovery coordinator must recompute `workflow_state` immediately. The rule is not "preserve the old state until someone has time to look." The rule is "move to `blocked`, move to `authority-required`, or stay where you are only when unaffected evidence still satisfies the active contract."

When the triggering impairment is an anchor change rather than a validator defect, use the normalized `target_anchor` comparison result from Workflow Overview / Entry minima before first Slice Contract / Target-anchor normalized form and comparison rules rather than ad hoc textual judgment. `Equal` does not trigger anchor-driven recomputation by itself. `Materially-different` and `non-comparable` are the comparison outcomes that require the state-selection logic below.

| Prior `workflow_state` | Impairment materiality | Output state | Required recomputation note |
|---|---|---|---|
| `planned` or `in-progress` | Not material to the live question or next governed action | unchanged | Keep the current state, but record the bounded basis and the corrected `next_corrective_action` explicitly. |
| `planned` or `in-progress` | Material technical impairment | `blocked` | The next action no longer has a reliable technical basis. Record the impaired witnesses or artifacts and repair path before resuming active work. |
| `validation-pending` | Material technical impairment | `blocked` | Suspend execution or non-executable close work until probe, environment, or witness integrity is repaired. |
| `closed` | Material technical impairment | `blocked` | Reopen the slice for technical revalidation because prior closure or promotion relied materially on the impaired basis. |
| `planned`, `in-progress`, `validation-pending`, or `closed` | Material normative impairment | `authority-required` | Technical evidence alone no longer decides contractual force. Refresh the packet basis before further advancement. |
| `authority-required` | Packet basis impaired but the question remains authority-owned | `authority-required` | Stay authority-routed, but refresh the packet, basis refs, and remaining objections before review continues. |
| `blocked` | New impairment is still technical and does not change ownership of the question | `blocked` | Stay blocked, but replace the blocker note with the materially current blocker basis. |
| `rejected` or `out-of-scope` | Later evidence affects historical basis but the slice has not been explicitly reopened | unchanged | Record the note, preserve the impaired basis, and reopen only under an explicit local rule or as a new slice. |


## Authority review operating model

This section is the authoritative definition site for `authority review mode`, `normative effect`, `authority independence`, and `promotion trace matrix`.

The authority reviewer reviews a bounded packet, not the whole recovery corpus. When `authority_disposition_needed = true`, when a slice is in `workflow_state = authority-required`, when a proposed carve-out would alter contractual force, or when a Level 3 handoff is submitted for final ratification, the recovery coordinator must open or refresh an Authority Review Packet before authority review begins.

The packet must be sufficient for first-pass review. It must not require the authority reviewer to absorb raw code, the whole recovery tree, or ambient conversational history unless those materials are themselves cited as decisive basis.

The minimum context that an authority reviewer must absorb is:

1. the declared scope and compatibility target
2. the candidate clause or clause set under decision
3. the live alternatives or ambiguities being resolved
4. the decisive witnesses and any unresolved critic objections
5. the compatibility, safety, and operational consequences of each live decision
6. the immediate next move if the question is approved, bounded, deferred, or rejected

Authority review mode uses the closed `review_mode` vocabulary declared in Vocabulary Declarations.

| Review mode | Typical use | Required entry conditions | Required result |
|---|---|---|---|
| `per-slice` | Default review path for new, contentious, or repository-local normative questions | Packet readiness only | One packet and one Authority Disposition for the reviewed slice or question |
| `bounded-batch` | Multiple slices or clauses that share one normative question | Same recovery objective, same compatibility target, same intended authority owner or designated reviewer, same confidentiality or sensitivity class, same policy question shape, no unresolved critic conflict, and no unnormalized decisive evidence | One batch packet plus either one Authority Disposition per slice or one disposition with per-slice annexes |
| `promotion-batch` | Final ratification of a coherent Level 3 handoff | All non-authority Promotion Gate items are already closed for the submitted scope | One promotion packet and one promotion-bearing Authority Disposition |

`Per-slice` is the default and binding mode. If a packet representation omits `review_mode`, the omission means `per-slice`.

A `bounded-batch` review must not mix clauses with materially different recovery objectives, compatibility targets, intended authority owners, confidentiality classes, or normative question shapes. It must not be used when any participating slice still depends on unresolved critic objections or on decisive evidence that has not yet been normalized into a reviewable basis.

A repository or consuming domain that does not yet have at least one accepted local Authority Review Packet exemplar or an equivalent local vocabulary note must remain in `per-slice` mode. The purpose of that default is legibility, not throughput.

Authority independence uses the closed `authority_independence` vocabulary declared in Vocabulary Declarations. Omission means `independent-reviewer`. `Same-operator-explicit-local-authority` is allowed only when an authoritative local rule or an adopted workflow-specific procedure explicitly designates that authority path. This specification does not define that local designation surface; `SPEC-SHR-000` governs the local-authority question. Packet-first review, Authority Disposition, and the Promotion Gate remain mandatory even when the same operator is the designated local authority owner. Silence, convenience, reviewer absence, or informal project habit must not be read as same-operator authority.

When `review_mode = promotion-batch`, the Authority Review Packet must also carry `promotion_trace_matrix`. The matrix is the clause-level promotion handoff surface. It must preserve stable reviewable provenance for each promoted clause, compatibility clause, explicit exclusion or carve-out, and explicit out-of-scope note. `Derived_from` remains document-level lineage only. It does not replace the clause-level matrix.

A normative effect records what the authority decision does to the contract surface. At minimum, a conforming domain must support the normative-effect outcomes declared in Vocabulary Declarations.

Authority review is a least-context function. First-pass packets must prefer witness references, redacted summaries, and boundary-centered descriptions over raw secrets, raw credentials, unredacted production payloads, or unnecessary personally identifying information. Batch review must not cross sensitivity classes.

If no authority reviewer exists, or no designated domain-authorized agent is available for the question at hand, recovery may continue at Level 1 or Level 2. It must not claim Level 3 completion, and any pause, handoff, or stop with unresolved authority-owned questions must emit a Recovery Snapshot that records the authority gap.

## Governed Working Artifacts

The recovery record must be schema-shaped once work spans more than one slice or one operator. Free-form notes are fine for scratch work. They are inadequate as the durable memory of a governed recovery effort.

This document now owns workflow meaning, participation obligations, and requirement triggers for the governed artifact families. `SPEC-SHR-004` owns the canonical field-minimum tables, field-local omission semantics, continuity-key minima, and overlay matrix.

### Default slice artifact continuity and assembly

The workflow uses one governed continuity chain rather than ad hoc per-slice notes. The ordinary artifact chain is:

- Specification Plan entry before the first Slice Contract for a slice
- Slice Contract before nontrivial critique, validation, or explicit non-executable closure
- Validation Report after executable validation or explicit non-executable closure
- Critique Report when objections remain
- Authority Review Packet before authority review begins
- Authority Disposition when an authority-owned question is closed
- Recovery Snapshot when work pauses, is handed off, or loses authority availability before promotion

`SPEC-SHR-004 / Default slice artifact continuity and assembly` owns the canonical continuity keys, default assembly rules, and route-family minima. This document owns the workflow rule that promotion reuses the governed chain rather than inventing an ad hoc handoff bundle.

### Governed artifact identity, provenance, and attempt metadata

Canonical identity, provenance-coordinate, and attempt-metadata rules now live in `SPEC-SHR-004 / Governed artifact identity, provenance, and attempt metadata`.

This workflow specification continues to require that `contract_ref`, `artifact_ref`, `packet_ref`, and `snapshot_ref` operate on stable governed identity rather than on paths, temporary handles, or session-local coordinates.

### Canonical schema status legend

The canonical schema status legend now lives in `SPEC-SHR-004 / Canonical schema status legend`.

Read `SPEC-SHR-004` for `required`, `required-with-empty-allowed`, `conditional`, and `optional-with-default` semantics on governed artifact fields.

### Compatibility and migration note for workflow versions 0.11.0 through 0.13.0

Workflow version `0.11.0` introduced the structural split between workflow semantics and governed-artifact schema ownership. Workflow version `0.12.0` preserved that owner split and added three coordinated surfaces:

- `SPEC-SHR-005` as the short normative in-repository cold-start profile
- machine-discoverable workflow-override indexing through `SPEC-SHR-003`
- promotion-bearing repository-surface-family closure linkage through `SPEC-SHR-004`

Workflow version `0.13.0` preserves those surfaces and adds two clarifications that materially improve cold-start reviewability:

- a named minimum content floor for the system sketch or equivalent scope record and for any slice seed carried outside the Specification Plan
- an explicit shared default operational profile for recovery memory in repository-shaped work while keeping conformance mechanism-open

This revision does not change the closed `workflow_state` vocabulary, the default transition matrix, the revalidation-triggered state-selection logic, the evidence-weighting hierarchy, the authority-review mode model, or the Promotion Gate semantics relative to `0.12.0`. It changes cold-start entry explicitness and the default operational profile for recovery-memory materialization.

A workspace that adopts `SPEC-SHR-002@0.13.0` SHOULD also adopt `SPEC-SHR-003@0.7.0` or a later compatible topology specification so that the effective schema companion resolves explicitly or by the paired companion default and so that workflow overrides, when present, are machine-discoverable at the single local override site.

Workflow advancement under `0.13.0` must use the schema companion rather than historical inline tables as the authoritative owner of field minima. Repository-shaped work under `0.13.0` must also satisfy the minimum content floor for the system sketch and any out-of-plan slice seed and must treat the shared default recovery-memory profile as operationally current unless a local alternate profile is declared explicitly.

### Specification Plan

The Specification Plan is the queue and state record of unresolved slices. It is required before the first Slice Contract for a slice and remains the continuity spine throughout active recovery.

This document owns the workflow meaning of `candidate_clause`, `workflow_state`, `disposition`, `next_corrective_action`, `authority_disposition_needed`, `current_step_id`, and step supersession. `SPEC-SHR-004 / Specification Plan` owns the canonical field-minimum table, step-list record minima, field-local omission semantics, and `inventory_item_ref` linkage rules.

### Slice Contract

The Slice Contract is the local validation contract and the local Definition of Done for the next recovery action on a slice. It bridges a high-level objective and a checkable action.

This document owns the workflow meaning of the discriminating question, decisive witnesses, validation mode choice, and contract-local closure. `SPEC-SHR-004 / Slice Contract` owns the canonical field-minimum table, executable-action minima, non-executable-resolution minima, framing-ablation subrecord minima, and field-local omission semantics.

### Validation Report

Every executed Slice Contract must emit a Validation Report. A Slice Contract that closes through `validation_mode = non-executable` must also emit a Validation Report recording the decisive non-executable basis.

This document owns criterion-relative closure meaning, validator interpretation, and revalidation consequences. `SPEC-SHR-004 / Validation Report` owns the canonical field-minimum table, criterion-result minima, interpretation minima, validator-record minima, and framing-ablation-result minima.

### Other governed artifacts

The lighter governed artifacts remain workflow-significant even though their canonical field tables moved to `SPEC-SHR-004`.

#### Critique Report

A Critique Report records structured objections against a proposed clause or Slice Contract. It blocks advancement until revision or authority disposition closes the objection path. `SPEC-SHR-004 / Other governed artifacts / Critique Report` owns the canonical field-minimum table.

#### Evidence Bundle

An Evidence Bundle carries witnesses and chain-capable evidence records at bundle scope. It is the default chain-capable carrier for stable `chain_ref` values used by the workflow. `SPEC-SHR-004 / Other governed artifacts / Evidence Bundle` owns the canonical field-minimum table.

#### Authority Review Packet

An Authority Review Packet records the bounded decision surface presented to the authority reviewer. Packet-first review, review-mode semantics, authority-independence semantics, and least-context intake remain owned by `Authority review operating model`. `SPEC-SHR-004 / Other governed artifacts / Authority Review Packet` owns the canonical field-minimum table and promotion-trace row minima.

#### Authority Disposition

An Authority Disposition records the explicit normative decision that closes an authority-owned question. Decision meaning and normative effect remain governed by the workflow and the authority model. `SPEC-SHR-004 / Other governed artifacts / Authority Disposition` owns the canonical field-minimum table.

#### Witness locator stability and decisive-witness selection

This subsection is the authoritative definition site for witness locator stability.

A witness `locator` must be stable enough that an independent reviewer can recover the same evidentiary object within the declared recovery corpus, retained witness set, or revision scope. Session-local notebook positions, temporary filesystem paths, transient search-result offsets, and purely conversational descriptions are insufficient as sole locators.

When a derived analysis stands in for stronger witnesses, the working record must preserve provenance coordinates to the stronger witnesses or exact artifact coordinates that the analysis summarizes.

`criterion_results.witness_ref` names the primary decisive witness for the criterion. When a criterion closes on a decisive witness set rather than one decisive witness, `criterion_results.witness_refs` must enumerate the full set.

Here, `source class` means the ranked evidentiary tier defined in Evidence and Black-Box Discipline / Source hierarchy rather than the literal `witness.source_role`.

Unless a higher-authority consuming domain defines a stricter rule once at its own authoritative site under Normative language / Consuming-domain overrides, the primary decisive witness must be selected deterministically as follows:

1. prefer the strongest available source class under Evidence and Black-Box Discipline / Source hierarchy
2. if more than one decisive witness remains within the strongest applicable source class, choose the lexicographically smallest stable `ref`

Discovery order must not control primary decisive-witness selection.

These artifacts are the durable continuity of the recovery effort. The mechanism by which continuity is preserved is intentionally unspecified. Compaction, reset-and-handoff, checkpoint files, database-backed queues, or equivalent mechanisms are all acceptable. The normative requirement is durable continuity of the governed artifacts, not a particular harness topology.

## Overlays

Overlay activation is a workflow choice that adds closure burden to the slice.

Canonical overlay-local meaning, overlay field minima, worked examples, and the overlay matrix now live in `SPEC-SHR-004 / Overlays`.

This document continues to govern the workflow rule that overlays are activated only when the slice genuinely needs the added burden, that base criteria and active overlay criteria are conjunctive unless authority explicitly bounds scope, and that cross-overlay dependencies must be explicit when more than one overlay is active.

## Completeness and Criteria

### Qualitative clauses and criteria engineering

Some of the hardest recovery targets are not binary interface facts but holistic labels such as `usable`, `production-ready`, `feature-complete`, `safe`, or `polished`. Left unanalyzed, such labels compress multiple distinct claims into one vague clause.

A candidate clause that uses a holistic label must therefore be decomposed into named criteria, each with an observable or witness class and, where meaningful, a threshold or priority ordering. If tradeoffs exist, the working record must state which criteria dominate and which are tie-breakers. Hidden criteria embedded only in prompts, runbooks, evaluator instructions, skills, policy documents, or reviewer folklore are still load-bearing if they steer acceptance or rejection. They must be surfaced into the Slice Contract or other governed artifacts as reviewable inputs.

Recovery must also reconstruct decision policy, not just named criteria. If acceptance depends on a primary metric plus soft constraints, lexicographic tie-breaks, simplicity preferences, or weighted judgment, the ordering or weighting must be explicit.

### Recovered completeness

SPEC-SHR-001 identifies behavioral, interface, and boundary completeness for forward-authored NLSpecs. This document keeps those three completeness dimensions intact and extends the reverse-recovery checklist with two additional recurring checks, metric completeness and environment completeness, because reverse recovery frequently fails on hidden measurement regimes and hidden environment state.

**Behavioral completeness** means every externally observable, load-bearing behavior in scope is specified.

**Interface completeness** means every boundary where components meet is defined precisely enough for independent implementations to connect correctly. This includes function signatures, record shapes, wire formats, error contracts, machine-parsed operational outputs, and, when relevant, lifecycle vocabularies and state-derivation rules.

**Boundary completeness** means defaults, limits, timeouts, edge cases, illegal triggers, duplicate delivery behavior, restart interpretation, refusal behavior, and other limit conditions are specified for the declared scope.

**Metric completeness** means that when a system is optimized, gated, ranked, or compared by a metric, the recovered document reconstructs the metric contract: formula, normalization, included and excluded cases, evaluation slice or corpus, measurement regime, comparability invariant set, and invalid comparison regimes.

A comparability invariant set is the conjunction of conditions that must remain fixed for two measurements to retain contractual meaning. Invalid comparison regimes are its complement, not its substitute. Some members of the comparability invariant set may live in environment capture rather than in the metric formula itself.

**Environment completeness** means the recovery record surfaces caches, generated artifacts, warm-start state, feature flags, hardware assumptions, time budgets, snapshot or clone provenance, cache-coherence boundaries, and other environment-bound state when they affect observable behavior, reproducibility, or comparability.

Concept-bearing slices and abstract witness families do not create new completeness axes. If their semantic basis, admissibility rules, or preservation conditions remain implicit, the defect lives inside behavioral, interface, boundary, metric, or environment completeness.

A completeness judgment is always relative to the declared recovery objective. A document may be complete for a normative cleanup or versioned replacement while being incomplete for backward-compatible reimplementation if it deliberately excludes an accidental contract that current callers still rely on. That exclusion must be explicit.

The practical proof remains the recreatability test: if the implementation were destroyed and only the recovered document remained, could a competent implementer recreate an interchangeable system for the stated scope without guesswork? If not, the artifact is not yet complete enough to serve as a Level 3 NLSpec.

### Translation-surface completeness and mapping form

This section applies to any boundary-visible translation surface, not only to lifecycle-shaped vocabulary mappings. Error-code mappings, wire-literal translations, field-name translations, status translations, and equivalent cross-surface normalization rules are all in scope when divergence would be caller-visible.

When a recovered clause translates among non-identical external vocabularies, status surfaces, wire literals, error codes, field-name systems, or equivalent boundary-visible representations, prose alone is usually under-specified. Unless the translation is one-to-one, total, and explicitly verified as trivial, the recovered artifact must use a row-complete mapping table.

| Required row class | Meaning |
|---|---|
| Direct mapping row | One observed or external value maps directly to one canonical or target value. |
| Derived or inferred mapping row | The target value is obtained through an explicit derivation rule rather than by direct literal equality. |
| Fallback row | A less-specific mapping applies when no more-specific row matches. |
| No-direct-literal row | The target concept has no dedicated external literal and must be inferred from another signal or signal combination. |
| Out-of-scope or intentionally-unmapped row | An external value or state exists but is explicitly outside the declared scope or intentionally not translated. |

When such a translation surface exists, omission of one of the applicable row classes means the mapping is incomplete.

This rule generalizes the mapping-table discipline already used by `lifecycle_overlay.observed_to_canonical_mapping` and `lifecycle_overlay.external_status_mapping`. When a lifecycle-bearing slice already carries those fields, satisfy this rule there rather than by creating a parallel mapping artifact.



### Promoted-output completeness and representation

Any Level 3 artifact that claims NLSpec status must satisfy the ordinary completeness duties above and the promoted-output duties below. These duties are binding for promotion-bearing recovery. They are not APP-layer advice.

A promotion-bearing artifact must, in its final promoted form:

1. declare the promoted scope, `recovery_objective`, `compatibility_target`, and the target anchor or an explicit timelessness rule
2. include document-level acceptance criteria or an equivalent document-level Definition of Done
3. make defaults, omitted-value behavior, and boundary exclusions explicit for every in-scope configurable or optional surface
4. use row-complete mapping tables when any in-scope translation surface exists
5. when `review_mode = promotion-batch`, preserve clause-level provenance through `promotion_trace_matrix` so that every promoted unit is reviewable from slice, witness, and authority records without requiring raw conversational history
6. keep each promoted clause reviewable from slice, witness, and authority records even when the final output is assembled from more than one slice
7. keep the final prose black-box clean and free of non-load-bearing implementation residue

Silent omission is not an acceptable representation for externally depended-on accidental behavior, explicit scope bounds, still-unresolved behavior, or repository surface families that materially intersect promoted scope. Use the representation rules below instead.

| `representation_class` | Recovered behavior class | Required Level 3 representation |
|---|---|---|
| `main_contract_clause` | intended and load-bearing behavior | main contract clause |
| `compatibility_clause_or_appendix` | externally depended-on accidental behavior that will be preserved | explicit compatibility clause or compatibility appendix |
| `explicit_exclusion_or_carve_out` | externally depended-on behavior that will not be preserved | explicit exclusion, carve-out, deprecation, or sunset note tied to authority review |
| `explicit_out_of_scope_note` | behavior outside the declared promoted scope | explicit out-of-scope note or bounded exclusion; do not rely on silent omission |

Behavior that is still unresolved or still authority-pending is not eligible for promotion and must remain Level 2 with explicit bounded status in the working record.

For repository-shaped targets, promoted completeness also requires that any repository surface family materially intersecting promoted scope be represented, explicitly excluded, or explicitly assessed as `not-present`, `present-out-of-scope`, or `present-not-boundary-bearing`. It must not disappear by silence. When such a family is `present-in-scope`, the governing row must also carry non-empty reviewable `closure_basis_refs` whose linked records collectively close the family-specific minimum facts defined in `SPEC-SHR-004 / Repository-surface-family record for repository-shaped targets`. A `present-in-scope` family with omitted or underclosed closure linkage is not promotion-ready. `Derived_from` remains document-level lineage only; `promotion_trace_matrix` is the clause-level handoff surface.

A promoted artifact that cannot yet satisfy this section is not ready to claim NLSpec status even if an authority reviewer has provisionally approved the direction of travel.

## Evidence and Black-Box Discipline

### Evidence chains

Every nontrivial recovered clause must carry an evidence chain, not merely a citation list. At minimum, the chain must identify:

- the boundary stimulus or precondition
- the implicated artifact role or source class
- the structural or documentary corroboration
- the execution conditions when execution occurred
- the runtime witness when one exists
- the observable effect
- the external dependence evidence when relevant
- the current disposition

When an evidence chain synthesizes more than one witness or serves as a handoff artifact, each nontrivial synthesized claim must carry an assertion-basis label distinguishing direct observation, inference, and interpretation, or an equivalent local vocabulary. Assertion basis is orthogonal to support level: it records how the claim was formed, not how strongly it is supported.

`Specification Plan.evidence_chain_refs` must resolve to stable `chain_ref` values carried by a chain-capable carrier. Under the default and binding carrier rule in this document, `Evidence Bundle.chains` is the only chain-capable carrier standardized here. A consuming domain may declare another chain-capable carrier only at an authoritative local site and only if it preserves the same minimum chain fields, stable reference behavior, and reviewability.

Multiple `evidence_chain_refs` on one plan entry are conjunctive and ordered as listed. If a clause depends on an alternative relation rather than conjunction, represent that relation inside a higher-order chain record or through an explicit consuming-domain override rather than by adjacency alone.

Witness locator stability and decisive-witness selection are governed by Other governed artifacts / Witness locator stability and decisive-witness selection and are not restated here.

When a slice is concept-bearing, the evidence chain must also identify the semantic basis, including any recovered concepts or structured absences that make the clause reviewable.

When a slice is lifecycle-bearing, the evidence chain must preserve whether each witness is a `state_snapshot`, `transition_trace`, or `derived_summary` so that summary surfaces are not misread as proof of every intermediate transition.

When a clause is materially code-derived, the working record must also preserve the stronger code-centric evidence form:

- entry surface or triggering source
- relevant propagation or control path when material
- sink or boundary effect
- protection gap, failing guard, or missing condition
- exact artifact coordinates
- expected externally visible impact

This stronger form belongs in the working record, not the final Level 3 contract prose.

### Source hierarchy

This subsection is the authoritative definition site for `source class`.

Unless a consuming domain defines a stricter hierarchy or a local source-class mapping under Normative language / Consuming-domain overrides, the following source hierarchy is the default binding ranking for evidence weighting and decisive-witness selection.

A source class is the ranked evidentiary tier used for weighting and decisive-witness selection. It is not the same thing as the literal `witness.source_role`. `witness.source_role` names the local source surface. The source class is the tier to which that surface belongs under this subsection or an explicit consuming-domain override. Adding a new `witness.source_role` literal does not create a new source class and does not insert a new tier into the hierarchy.

1. **Runtime-and-boundary source class.** This class includes runtime witnesses, runtime ledgers, runtime counters, current contract artifacts, stable schemas, migration shims, machine-parsed outputs, fixed boundary artifacts, and binding control-plane prose tied to the target revision or deployment shape.
2. **Target-matched corroborant source class.** This class includes static corroborants, consumer code, support history, incident records, external standards, public documentation, protocol references, migration notes, and derived analyses when they are attached to the specific slice and matched to the relevant target revision or deployment shape.
3. **Framing-and-commentary source class.** This class includes framing metadata, proposals, issue threads, roadmap notes, historical hypotheses, and other forward-looking or interpretive commentary.

A consuming domain that introduces an additional `witness.source_role` must map it explicitly to one of these source classes unless it defines a stricter hierarchy under Normative language / Consuming-domain overrides. The mapping must be declared once at an authoritative local site. Silent inference from spelling, example placement, or current tooling is not sufficient.

Derived analyses may summarize stronger sources, but they do not outrank them. Framing metadata may generate slices or historical hypotheses, but it does not materially raise support for a disputed load-bearing clause on its own unless the surface is independently shown to be contract-bearing.

Shared `APP-SHR`, `RFC-SHR`, and `RES-SHR` documents are legitimate discovery aids, vocabulary sources, and slice-seeding materials. They are informative inputs. They do not, by themselves, materially raise support for a disputed target-specific clause and they do not satisfy promotion on their own. Shared `ADR-SHR` documents may explain decisions made by the shared corpus that maintains them, but they do not decide target-system normativity absent explicit adoption by the consuming domain.

External standards, public documentation, protocol references, and migration notes are legitimate corroborants when they are attached to a specific slice and matched to the relevant target revision or deployment shape. Generic patterns and stale documents remain guidance until the target system itself supplies corroborating evidence.

### Black-Box Constraint

The recovery target is a black-box specification and contract. The job is to recover what must be true at the abstraction boundary, not to memorialize the current decomposition of the codebase.

Human-readable outputs that are routinely machine-parsed or operator-parsed belong to that boundary when downstream behavior depends on them. Interlocking lifecycle slices obey the same discipline. A parent lifecycle slice composes with a child slice only through the child's boundary artifacts, declared interfaces, or terminal outcomes unless the child's internals are themselves separately recovered as contractual surfaces.

Recovered concepts do not weaken this discipline. A concept may justify a clause in the working record even when the concept was reconstructed from code paths or traces. That does not entitle the final Level 3 contract to speak in mechanistic terms unless the mechanism is itself boundary-observable or otherwise load-bearing.

Structural evidence in the working record does not license structural language in the final spec body.

## Validation

Validation supplies the back pressure that keeps recovery from turning into polished conjecture. It may use traces, fixtures, black-box executions, differential tests, compatibility checks, builds, typechecks, log evidence, consumer dependency analysis, configuration proofs, or explicit authority review. Whatever the method, the point is the same: constrain what may responsibly advance.

### Pre-execution validation

For executable probes, validation is a hard gate before execution. A proposed probe, fixture, reproducer, or black-box test must be checked against the active `executable_action` record for at least four things:

- action-type consistency
- payload shape or syntax
- intent alignment with the discriminating question
- readiness against observed state and declared assumptions

A failed pre-execution validation is negative evidence against the probe design, not against the clause.

### Low-floor executable confirmation

Executable confirmation includes safe, bounded, target-anchored probes whose execution still answers the discriminating question even when the full target system is not started. Typical examples include parser replay, schema diff, format-invariant replay, configuration-precedence replay, and other bounded text-, file-, or schema-shaped probes.

`Validation_mode = non-executable` is invalid when a safe low-floor executable probe exists, is proportionate for the current engagement, and would materially decide the live question. Full service startup is not the threshold for executable validation. The threshold is whether the executed probe is real, target-anchored, discriminating, and reviewable.

### Runtime and environment capture

When a clause is runtime-sensitive, metric-bearing, or environment-bound, the working record must capture the measurement regime and environment boundary explicitly. Relevant fields commonly include isolation method, snapshot or clone provenance, mutation policy, cache state, generated artifacts, data snapshot, feature flags, warm-start state, hardware class, time budget, reset conditions, and known divergence from the live target.

A witness without its regime is weak support for a claim about comparability or environment-sensitive behavior.

### Framing-ablation

This section is the authoritative definition site for `semantics-first`, `framing-ablation`, and `framing-sensitive`.

A semantics-first review mode privileges boundary-observable semantics and stronger witnesses over non-binding framing cues, naming cues, or narrative reconstruction.

Framing-ablation is rerunning proposal, critique, or validation with stronger witnesses held fixed while non-binding framing cues such as comments, titles, names, or advisory wording are removed, normalized, neutralized, or contradicted.

When a slice is proposed, critiqued, or validated using mixed code-plus-language context and non-binding framing metadata is present, the Slice Contract must require a semantics-first framing-ablation pass.

The canonical subrecord minima for `Slice Contract.framing_ablation` and `Validation Report.framing_ablation_result` now live in `SPEC-SHR-004 / Validation and framing-ablation subrecord minima`.

Actual removal or neutralization of non-binding cues is stronger than merely instructing a reviewer to ignore them. If the clause, critique outcome, or validator outcome changes materially under framing-ablation or another safe non-binding perturbation, the slice is `framing-sensitive`. A `framing-sensitive` slice must not advance beyond `corroborated` until stronger independent witnesses or authority judgment close the gap.

Lower objection volume, cleaner narratives, or higher reviewer agreement under favorable framing do not by themselves indicate stronger recovery. Where framing or heuristic screening could suppress objections, the working record must note missed-issue risk, counterexample sensitivity, or comparable coverage limits.

### Criterion status and validator semantics

Criterion status and validator semantics are different records.

Criterion status uses the closed `criterion_results.outcome` vocabulary declared in Vocabulary Declarations.

When a validator is used, the Validation Report must preserve the full `validator_record` defined in `SPEC-SHR-004 / Validation Report` rather than collapsing validator behavior into the criterion outcome alone.

The closed `validator_outcome` vocabulary is declared in Vocabulary Declarations. `Unsupported` means the validator could not decide under its own completeness limits. It is not, by itself, evidence that the clause is false. `Heuristic-only` means the signal may rank, cluster, or corroborate cases but is not itself decisive proof.

Concept-bearing slices should prefer concept-local validation as well as clause-local validation when a clean separation is feasible. Slices that rely on an abstract witness family must treat family validity, concrete admissibility, and preservation under concretization as separate criteria. Promotion-bearing lifecycle slices must validate claimed states and transitions locally. The existence of a current or terminal status surface does not, by itself, prove every intermediate transition.

When relevant cases are obtainable and in scope, a promotion-bearing lifecycle slice must define a minimal witness suite that covers the canonical happy path, each claimed terminal failure or refusal state, at least one illegal or out-of-order trigger, and at least one replay, duplicate, restart, or interrupted-transition case when the lifecycle crosses execution boundaries.

Calibration runs for critics or validators are workflow aids. They may justify confidence in the tool or reviewer. They do not count as evidence for a recovered clause.

### Heuristic screening and exposed validators

When the target event is sparse and false positives are expensive to triage, probabilistic judges and heuristic screens are screening aids rather than promotion gates. They may nominate or rank cases. They must not be the sole basis for slice advancement.

In agentic settings, inspectable critique prompts, public review configurations, and visible adjudication rubrics are attack surfaces. Promotion-critical slices should therefore prefer conservative validators, blinded or independently parameterized checks when feasible, or explicit human or authority adjudication when an exposed validator could be directly optimized against.

Passes from exposed validators, proposer self-checks, or calibration artifacts do not materially raise the support level of a disputed, qualitative, or runtime-sensitive clause until an independent check or explicit authority disposition closes the gap.

### Slice advancement and revalidation

Slice advancement is not aggregate. A slice does not advance because the overall run felt persuasive or because most things worked. It advances only when every required criterion in the active Slice Contract has a passing witness or has been explicitly dispositioned by scope or authority.

A failed executed criterion in a validated probe is negative evidence against the current clause, the current thresholds, or the current environment understanding, and it must be recorded as such. A visible control, passing screenshot, declared endpoint, or nominal toggle is not by itself evidence of semantic completeness. Interactive, stateful, or sequence-dependent clauses should prefer live or replayable witnesses over static artifacts when feasible.

If later evidence reveals a validator defect, a known unsoundness window, or a material limitation affecting prior witnesses, the affected slice records must be marked `revalidation_required`, the impacted witnesses must be enumerated, and the plan must spawn an immediate corrective task.

When `revalidation_required` becomes true, the recovery coordinator must recompute `workflow_state` immediately for each affected slice. If the impaired witnesses were material to the slice's current advancement, support claim, closure basis, or promotion basis, the slice must move to `blocked` or `authority-required` according to whether technical clarification or normative judgment is needed. If unaffected evidence still satisfies the active contract and no existing judgment materially relied on the impaired witness, the slice may remain in its prior `workflow_state`, but the bounded basis and corrective action must be recorded explicitly.

Until revalidation or explicit authority disposition occurs, the impaired witnesses provide only bounded support.

## Recovery Memory and Negative Evidence

Recovery must preserve not only successful clauses but also the memory of how the recovery effort learned what it learned, including the routes that failed.

Recovery memory is a governed continuity surface, but it does not receive a fourth canonical schema table in this document. Its mechanism, storage topology, and record shape remain intentionally open. A companion operational document may choose concrete paths and field names. For repository-shaped work with no locally declared alternate profile, `APP-SHR-001 / Recovery memory in practice` is the shared default operational profile for materializing this section's requirements. That profile is operational rather than schema-authoritative. A local profile remains valid only when it preserves the minimum queryable interface, the preserved distinctions, the consultation triggers, and the supersession handling defined here. This document governs the minimum preserved distinctions, the minimum queryable interface, and the points where recovery memory must be consulted.

Memory obligation map:

- use `Minimum queryable memory interface` for the capability floor that makes consultation reproducible
- use `Memory item minima` for the preserved-fact set, layer distinctions, write triggers, negative-evidence linkage, and supersession basis
- use `Memory consultation protocol` for the required consultation points and the `memory_refs_consulted` recording trigger
- use `Continuity conflicts` for conflict detection, preserved divergence basis, and revalidation trigger handling
- use `Validation / Slice advancement and revalidation` when a memory-linked defect or continuity conflict impairs prior support

A conforming recovery-memory mechanism must preserve durably and reviewably, when material, at least the following distinctions:

- memory layer
- write trigger
- slice, witness, artifact, or authority linkage
- negative evidence, rejected clauses, and dead-end hypotheses
- validator provenance and defect history
- evidence-loss mechanisms
- supersession basis

These distinctions may be recorded in one store or many. They need not appear as one canonical record shape, but they must remain explicit enough for independent review and stable handoff.

### Minimum queryable memory interface

The memory consultation protocol is normatively meaningful only if the memory mechanism is queryable. A conforming recovery-memory mechanism may be filesystem-backed, database-backed, notebook-backed with stable exports, or equivalent. It must nonetheless support the following minimum operations:

1. resolve a stable memory identity to the current item that carries that identity
2. enumerate current items by memory layer
3. filter items by applicability rule or an equivalent declared match condition
4. return the source linkage and negative-evidence linkage attached to an item
5. expose supersession or current-replacement relations rather than silently overwriting prior items
6. return stable reviewable references suitable for `memory_refs_consulted`

A query that returns no relevant item is a successful consultation with an empty result, not a broken memory mechanism. Consultation does not raise support by itself. Digest summaries or indexes may aid reading, but they do not displace the authoritative operational records they summarize.

When the workspace uses `SPEC-SHR-003`, `memory_locator` identifies the designated discovery coordinate for this mechanism. The locator may point to a future location before the first memory item exists.

### Memory item minima

Whether or not a local mechanism uses these exact field names, each material recovery-memory item must preserve at least the following facts.

| Preserved fact | Meaning |
|---|---|
| Stable memory identity | Continuity key for supersession, review, and citation |
| Memory layer | `contract-pattern`, `strategy`, `technical-witness`, or an explicit local extension |
| Summary and applicability rule | What the item says and when it should be consulted |
| Write trigger | Why the item was recorded |
| Linkage refs | Which slices, witnesses, packets, reports, or authority actions support it |
| Negative-evidence linkage | Failed probes, rejected clauses, misleading artifacts, or dead-end hypotheses tied to the item |
| Supersession basis | What displaced the item or what it displaced |
| Security or handling note | Redaction, secrecy, or execution-handling constraint when material |
| Review marker | Last review, confirmation, or reuse point |

A local mechanism may add more fields or different names. It must not collapse these preserved facts into conversational residue or leave them implicit.

A useful recovery memory has at least three layers:

- **contract-pattern memory**, which records recurring behaviors, invariants, accidental-contract shapes, and background semantic patterns that repeatedly turn out to be load-bearing
- **strategy memory**, which records the investigative moves that resolved similar ambiguities, including informative failures
- **technical witness memory**, which records concrete reusable artifacts such as scripts, fixtures, traces, corpora, generator settings, compatibility checks, and environment corrections

These layers must also have write triggers. Write to contract-pattern memory when a recurring clause shape or accidental-contract pattern has been validated strongly enough to guide future recovery. Write to strategy memory when a specific move decisively resolves ambiguity, including when it fails in an informative way. Write to technical witness memory when a concrete artifact becomes reusable.

Negative evidence belongs in all three layers. Failed interpretations, rejected clauses, blocked probes, misleading artifacts, and dead-end hypotheses are load-bearing for the recovery process because they prevent the same implementation detail from being rediscovered and re-promoted as apparent contract.

Seeded domain memory is acceptable when it accelerates search. Standards, prior incidents, protocol patterns, and curated warm-start examples can all suggest where to look. They are guidance, not support. No memory item upgrades a clause's support state until it is tied to target-specific evidence.

Improvement in long-running recovery is not guaranteed to be monotonic, so supersession must preserve prior witness references, prior support levels, and the reason a newer synthesis displaced an older one.

A recovery-memory mechanism must not silently rewrite or destructively replace a material memory item without preserving supersession basis. When a secret-bearing artifact must be remembered, the memory item should prefer a locator plus handling rule over a copied secret payload whenever the lighter form remains reviewable.

### Memory consultation protocol

This subsection is the authoritative definition site for the memory consultation protocol.

Recovery memory is guidance and continuity. It is not clause support. A memory item may seed a slice, suggest a probe, or warn against a dead end. It does not raise support until target-specific evidence independently closes the clause.

The recovery coordinator, clause proposer, or runtime validator must consult recovery memory at the following default points:

1. before choosing the next slice, consult contract-pattern and strategy memory for applicable prior patterns, failures, and scope traps
2. before drafting or materially revising a nontrivial Slice Contract, consult strategy memory and technical witness memory
3. before executable validation, check whether an existing technical witness can be reused and whether a known failure mode already blocks the probe
4. on slice closure, informative failure, supersession, or authority-bounded carve-out, evaluate whether a new memory item must be written or an older one superseded

The Slice Contract must record `memory_refs_consulted` when the contract is executable, when it could reuse prior technical witnesses or prior failed strategies, or when the clause materially affects support, scope, or authority review. Explicit `[]` means consultation occurred and no relevant recovery-memory item applied.

### Continuity conflicts

This subsection is the authoritative definition site for `continuity conflict`.

A continuity conflict exists when two non-identical governed artifacts claim the same governed artifact identity within the same recovery corpus and no explicit supersession rule, scope split, or declared revision rule explains the difference. A new provenance coordinate or new attempt metadata for otherwise equivalent content is not, by itself, a continuity conflict.

The workflow must treat a continuity conflict as first-class. It must not silently overwrite, silently merge, or silently replace one claimant with another.

When a continuity conflict is detected, the workflow must preserve both artifacts, record the divergence basis after excluding declared attempt metadata, and stop any slice advancement, promotion, or ratification that materially relies on the conflicted basis until explicit supersession, scope split, revalidation, or authority disposition resolves the conflict.

If prior support, critique, or validation relied materially on the conflicted artifact, the affected slice records must be marked `revalidation_required = true`, the impacted witnesses or artifact references must be enumerated, and the plan must insert an immediate corrective action. Pending resolution, each affected slice must apply the general revalidation-state rule in Validation / Slice advancement and revalidation and therefore move to `blocked` or `authority-required` according to whether technical clarification or normative judgment is needed.

## Recovery snapshots and incremental resumption

Reverse recovery does not fail merely because it stops before promotion. Partial recovery may still produce closed slices, bounded slices, system sketches, artifact-role inventories, or durable memory that later work can resume from.

This section is the authoritative definition site for `Recovery Snapshot` and `promotion readiness`.

A Recovery Snapshot is a governed working artifact that records resumable recovery state. It is required whenever an engagement pauses, is handed off, loses priority, loses authority availability, or otherwise stops before promotion and the working record would otherwise lose resumable state.

A Recovery Snapshot is informative working state. It does not satisfy the Promotion Gate and it does not elevate any Level 1 or Level 2 artifact to Level 3 by itself.

`Promotion_readiness` records the maturity of resumable recovery state for later promotion review. It is not a workspace-structural posture label under `SPEC-SHR-003`, and it must not be used to infer bootstrap, topology materialization, or local governed-document activation.

The canonical Recovery Snapshot field-minimum table and field-local omission semantics now live in `SPEC-SHR-004 / Recovery snapshots and incremental resumption`.

On resume, the recovery coordinator must:

1. load the latest Recovery Snapshot for the declared scope
2. compare `workflow_spec_ref`, when present, or the legacy-unknown omission state, against the current `SPEC-SHR-003` discovery-file `recovery_workflow_spec_ref`
3. compare the snapshot `target_anchor` to the current revision, deployment family, or environment regime using the normalized target-anchor comparison rule
4. mark affected slices `revalidation_required = true` when target-anchor comparison returns `materially-different` or `non-comparable`, or when workflow-version compatibility is materially changed or still unknown after review
5. refresh the Specification Plan in the order indicated by `resume_order` or an explicitly justified replacement order
6. resume from the highest-risk unresolved slice rather than from filesystem order or note order

If `workflow_spec_ref` is missing on an older snapshot, the current discovery-file version may be used as a provisional compatibility clue only. It is not proof that the snapshot was created under that same workflow version.

A partial recovery output remains at its current level until the ordinary workflow advances it. A closed Level 2 slice carried through a Recovery Snapshot remains Level 2. A system sketch remains informative. An APP or RES extracted from partial recovery remains informative unless some other governing document makes it authoritative.

## Promotion Gate

Use the following gate as a closure checklist. Each item resolves to earlier authoritative sections and introduces no new normative requirement.

A Level 2 artifact becomes a Level 3 prescriptive specification only when all of the following are true:

1. **PG-1** Scope is explicit.
2. **PG-2** Load-bearing ambiguities are dispositioned.
3. **PG-3** Completeness satisfies Completeness and Criteria for the declared objective.
4. **PG-4** Artifact roles, mutation authority, protected surfaces, and repository surface families where applicable are explicit and closure-linked where promotion-bearing repository-family completion is claimed.
5. **PG-5** Qualitative clauses are criteria-engineered where required.
6. **PG-6** Decision policies are explicit where they are load-bearing.
7. **PG-7** Clauses carry evidence chains and, when `review_mode = promotion-batch`, a promotion trace matrix sufficient for independent review.
8. **PG-8** Any active abstract witness family is closed under admissibility and preservation.
9. **PG-9** Runtime-sensitive clauses are confirmed or explicitly bounded, including safe low-floor text-, file-, and schema-shaped probes when obtainable, and including framing-ablation and concept-local validation where required.
10. **PG-10** Validator semantics, provenance, and defect status are explicit, and no in-scope witness remains under an unresolved unsoundness or revalidation flag.
11. **PG-11** Sparse-positive heuristic screening and exposed validators are independently adjudicated or explicitly authority-dispositioned.
12. **PG-12** Any promotion-bearing lifecycle slice satisfies Recovered Lifecycle Semantics and the relevant lifecycle validation duties.
13. **PG-13** Interactive, stateful, and machine-parsed interface claims are validated for semantic completeness, not merely surface presence.
14. **PG-14** The recreatability test passes for the declared scope.
15. **PG-15** Non-load-bearing implementation detail has been removed from the final contract prose.
16. **PG-16** A domain authority, or an explicit same-operator local authority path disclosed and locally authorized under the authority review operating model, has ratified the normative judgments through the authority review operating model.

Ratification through the authority review operating model is necessary. It is not sufficient. Authority over an incomplete document does not create a complete NLSpec.


## Recovery Definition of Done

A reverse-recovery effort conforms to this specification when all of the following are true.

1. Before the first Slice Contract, the recovery record makes scope, the minimum content floor for any system sketch or equivalent scope record, `recovery_objective`, `compatibility_target`, a normalized `target_anchor`, a draft artifact-role inventory with explicit item refs and scope classifications for currently known surfaces, a row-complete `repository_surface_families` record for repository-shaped targets, and at least one slice seed explicit, with the minimum content floor required when that seed is carried outside the Specification Plan.
2. Every governed working artifact used by the effort satisfies the workflow-owned requirements in this document and the canonical schema tables, lighter-artifact tables, framing-ablation subrecord minima, Recovery Snapshot minima, and overlay matrix rows owned by `SPEC-SHR-004`.
3. Any local extension fields preserve the meaning and omission semantics of the defined fields.
4. Every nontrivial candidate clause carries a reviewable evidence chain resolved through a chain-capable carrier, stable witness locators, and decisive-witness selection consistent with the source hierarchy.
5. Runtime-sensitive, environment-sensitive, metric-bearing, framing-sensitive, and text-, file-, or schema-shaped claims are executable-confirmed when safe low-floor executable probes are obtainable or are explicitly bounded by non-executable closure, scope, or authority.
6. The default interaction protocol, the default workflow-state transitions, the revalidation-triggered workflow-state recomputation rule, or an explicit consuming-domain override of those surfaces, is used to coordinate critique, runtime validation, evidence interpretation, authority review, and workflow-state updates.
7. Recovery memory, negative evidence, supersession basis, continuity conflicts, and required Recovery Snapshots are preserved as durable governed state rather than as conversational residue.
8. Any promotion-bearing output satisfies `Promoted-output completeness and representation`, the `Authority review operating model`, `SPEC-SHR-004`'s packet and promotion-trace minima when those surfaces are in use, any required repository-family `closure_basis_refs` and family-specific closure-fact minima for `present-in-scope` families materially intersecting promoted scope, and the `Promotion Gate`.
9. Same-operator Level 3 ratification is used only when an explicit local authority basis exists and is disclosed through `authority_independence` and `local_authority_basis_ref`.
10. A ratified Level 3 handoff is not represented as locally binding unless the consuming domain's lifecycle and adoption rules make it locally binding explicitly.

Nothing else is required by this specification.

## Minimal Protocol for Practice

Use the following checklist as a pointer protocol. Each item has a stable ID, each item refers back to earlier authoritative sections rather than restating them, and the traceability matrix below records those resolutions.

1. **MP-1** Interpret this document under Relation to SPEC-SHR-001 and Normative language before applying any local workflow override.
2. **MP-2** State the artifact class, declared scope, recovery objective, compatibility target, and `target_anchor` in normalized axis form.
3. **MP-3** Build an artifact-role inventory with stable item refs and explicit scope classification, add a row-complete `repository_surface_families` record when the target is repository-shaped, and satisfy Entry minima before first Slice Contract, including the minimum content floor for the system sketch or equivalent scope record and any slice seed carried outside the plan, before broad clause extraction.
4. **MP-4** Maintain a Specification Plan using the canonical schema in `SPEC-SHR-004`, including normalized `target_anchor`, `inventory_item_ref` linkage inside `artifact_roles`, `current_step_id`, and absent-field semantics for ambiguity, negative evidence, regime or environment capture, and active overlays.
5. **MP-5** Follow the Default interaction protocol and update `workflow_state` and `disposition` only through the recovery coordinator.
6. **MP-6** Work one unresolved slice at a time.
7. **MP-7** Create or update a Slice Contract with explicit `validation_mode`, any required `memory_refs_consulted` record, and machine-actionable executable fields or non-executable resolution before nontrivial critique or validation, using `SPEC-SHR-004` as the canonical schema owner of those fields.
8. **MP-8** Attach `concept_overlay`, `abstract_witness_overlay`, and `lifecycle_overlay` only when the slice truly needs them, and satisfy the overlay field minima matrix in `SPEC-SHR-004` when they are active.
9. **MP-9** Carry evidence chains through chain-capable carriers, preserve clause-level promotion trace when `review_mode = promotion-batch`, and respect the source hierarchy.
10. **MP-10** Preserve black-box discipline in both the working record and the final contract prose.
11. **MP-11** Validate executable work before it runs, treat safe low-floor executable probes as executable when they materially answer the live question, and capture regime and environment details when they matter.
12. **MP-12** Require semantics-first framing-ablation when the framing trigger is present.
13. **MP-13** Emit a Validation Report according to the canonical schema in `SPEC-SHR-004`, including validator and framing-ablation records when required by the contract and the validation path.
14. **MP-14** Preserve recovery memory, negative evidence, and any required Recovery Snapshot as durable artifacts, not conversational residue, using `SPEC-SHR-004` for snapshot minima and this document for resume semantics.
15. **MP-15** Advance support only through the working vocabulary and only when all required criteria have been satisfied or explicitly dispositioned.
16. **MP-16** Promote to Level 3 only through the Promotion Gate, the authority review operating model, clause-level promotion trace when required, and authority ratification.

## Checklist Traceability Matrix

The following matrix is authoritative for the checklist traceability audit. Every item resolves to earlier authoritative sections and introduces no unique normative requirement.

| Item | Earlier authoritative section(s) | Pointer-only closure note |
|---|---|---|
| PG-1 | Purpose, Artifact Class, Audience, and Authority Boundary; Relation to SPEC-SHR-001; Workflow Overview; Governed Working Artifacts / Slice Contract | Restates the existing duty to make scope, artifact class, and target-NLSpec boundary explicit. |
| PG-2 | Governed Working Artifacts / Specification Plan; Default interaction protocol; Authority review operating model; Validation / Slice advancement and revalidation; Completeness and Criteria / Promoted-output completeness and representation | Restates the existing duty to surface and disposition unresolved ambiguity before advancement and before final representation. |
| PG-3 | Completeness and Criteria; Completeness and Criteria / Promoted-output completeness and representation | Points to the declared completeness standard and the final promoted-output closure rule only. |
| PG-4 | Control-plane prose and artifact-role inventory; `SPEC-SHR-004 / Repository-surface-family record for repository-shaped targets`; `SPEC-SHR-004 / Governed artifact identity, provenance, and attempt metadata` | Points to the existing artifact-role, repository-family, mutation-authority, closure-linkage, and stable artifact-reference duties only. |
| PG-5 | Completeness and Criteria / Qualitative clauses and criteria engineering | Restates the existing criteria-engineering requirement only. |
| PG-6 | Completeness and Criteria / Qualitative clauses and criteria engineering | Restates the existing duty to surface decision policy when it is load-bearing. |
| PG-7 | Evidence and Black-Box Discipline / Evidence chains; Governed Working Artifacts / Witness locator stability and decisive-witness selection; Authority review operating model; `SPEC-SHR-004 / Other governed artifacts / Authority Review Packet`; Completeness and Criteria / Promoted-output completeness and representation | Restates the evidence-chain, chain-carrier, locator-stability, decisive-witness, promotion-trace, and final clause-reviewability duties only. |
| PG-8 | `SPEC-SHR-004 / Overlays / Overlay field minima matrix`; `SPEC-SHR-004 / Overlays / Abstract Witness Families and Instantiation Preservation`; Validation / Criterion status and validator semantics | Points to the existing family-validity, admissibility, and preservation duties only. |
| PG-9 | Hypothesis recovery and execution-grounded confirmation; `SPEC-SHR-004 / Overlays / Concept Recovery and Semantic Bases`; Validation / Low-floor executable confirmation; Validation / Runtime and environment capture; Validation / Framing-ablation | Restates the existing executable-confirmation and bounding duties only. |
| PG-10 | `SPEC-SHR-004 / Governed artifact identity, provenance, and attempt metadata`; `SPEC-SHR-004 / Validation Report`; Validation / Criterion status and validator semantics; Validation / Slice advancement and revalidation; Recovery Memory and Negative Evidence; Recovery Memory and Negative Evidence / Continuity conflicts | Points to the existing validator-provenance, stable-reference, and continuity-conflict duties only. |
| PG-11 | Validation / Heuristic screening and exposed validators | Restates the existing independent-adjudication duty only. |
| PG-12 | `SPEC-SHR-004 / Overlays / Recovered Lifecycle Semantics`; `SPEC-SHR-004 / Overlays / Overlay field minima matrix`; Validation / Criterion status and validator semantics | Points to the existing lifecycle-model and lifecycle-validation duties only. |
| PG-13 | Evidence and Black-Box Discipline / Black-Box Constraint; Validation / Slice advancement and revalidation; Completeness and Criteria / Promoted-output completeness and representation | Restates the semantic-completeness, explicit-default, and boundary-discipline requirements only. |
| PG-14 | Completeness and Criteria / Recovered completeness; Completeness and Criteria / Promoted-output completeness and representation | Points to the recreatability test and the document-level promoted-output closure rule already stated there. |
| PG-15 | Evidence and Black-Box Discipline / Black-Box Constraint; Completeness and Criteria / Promoted-output completeness and representation | Restates the existing duty to remove non-load-bearing implementation detail from final contract prose. |
| PG-16 | Authority review operating model; Three Levels of Recovery; Governed Working Artifacts / Other governed artifacts | Points to the existing ratification requirement, including the explicit same-operator local-authority disclosure path when locally authorized. |
| MP-1 | Relation to SPEC-SHR-001; Normative language | Restates the binding interpretive frame for the workflow only. |
| MP-2 | Purpose, Artifact Class, Audience, and Authority Boundary; Workflow Overview; Workflow Overview / Entry minima before first Slice Contract | Restates the existing requirement to declare artifact class, scope, objective, compatibility target, and normalized `target_anchor`. |
| MP-3 | Control-plane prose and artifact-role inventory; `SPEC-SHR-004 / Repository-surface-family record for repository-shaped targets`; Workflow Overview / Entry minima before first Slice Contract | Restates the existing inventory-first, repository-family, stable-item-ref, and entry-minima requirements only. |
| MP-4 | `SPEC-SHR-004 / Governed artifact identity, provenance, and attempt metadata`; `SPEC-SHR-004 / Canonical schema status legend`; `SPEC-SHR-004 / Specification Plan`; Workflow Overview / Entry minima before first Slice Contract; Working vocabulary | Points to the existing plan-schema, stable-identity, normalized `target_anchor`, inventory linkage, `current_step_id`, and absent-field requirements only. |
| MP-5 | Default interaction protocol; Working vocabulary | Restates the existing coordination and state-transition rules only. |
| MP-6 | Workflow Overview | Restates the one-slice loop already defined there. |
| MP-7 | `SPEC-SHR-004 / Slice Contract`; Recovery Memory and Negative Evidence / Memory consultation protocol | Points to the existing contract-before-validation, machine-actionable executable-field, and consultation-record duties only. |
| MP-8 | `SPEC-SHR-004 / Overlays`; `SPEC-SHR-004 / Overlays / Overlay field minima matrix` | Restates the existing overlay-attachment and overlay-field duties only. |
| MP-9 | Evidence and Black-Box Discipline / Evidence chains; Evidence and Black-Box Discipline / Source hierarchy; Governed Working Artifacts / Witness locator stability and decisive-witness selection; `SPEC-SHR-004 / Other governed artifacts / Authority Review Packet` | Points to the existing evidence-handling, chain-carrier, locator-stability, decisive-witness, and promotion-trace duties only. |
| MP-10 | Evidence and Black-Box Discipline / Black-Box Constraint | Restates the existing boundary-discipline requirement only. |
| MP-11 | Validation / Pre-execution validation; Validation / Low-floor executable confirmation; Validation / Runtime and environment capture | Points to the existing execution-readiness, low-floor executable-confirmation, and environment-capture duties only. |
| MP-12 | Validation / Framing-ablation | Restates the existing semantics-first framing-ablation requirement only. |
| MP-13 | `SPEC-SHR-004 / Validation Report`; Validation / Criterion status and validator semantics | Points to the existing report-schema and validator-record duties only. |
| MP-14 | Recovery Memory and Negative Evidence; Recovery snapshots and incremental resumption; `SPEC-SHR-004 / Recovery snapshots and incremental resumption` | Restates the existing durable-memory and resumable-handoff duties only. |
| MP-15 | Working vocabulary; Validation / Slice advancement and revalidation | Points to the existing advancement rule and support vocabulary only. |
| MP-16 | Authority review operating model; Promotion Gate; Three Levels of Recovery | Restates the existing promotion, promotion-trace, and ratification gate only. |

## Open Questions

This section is non-binding research commentary. The questions below are non-blocking for use of this document as a workflow specification unless a consuming domain declares otherwise. Closure of any question that would change conformance, schema minima, workflow-state handling, or Promotion Gate behavior must occur through ordinary specification revision rather than through an update to this section alone.

Each entry in this section must represent a real unresolved design question with plausible future impact on the corpus. Resolved items must be removed from this section or migrated to revision rationale or an ADR-style record. Updating this section alone does not change conformance.

- **Can test suites substitute for intent?** A comprehensive test suite is strong evidence about contract surface, but it does not reliably distinguish original intent from compatibility-preserving accretion.
- **How well can commit history, documentation, and telemetry recover caller dependence?** Sometimes well enough to classify accidental contracts, but often noisily and incompletely.
- **How should stop conditions and bounded iteration be tuned across domains?** Distributed systems, embedded systems, UI surfaces, and pure libraries impose different costs and risks on runtime confirmation.
- **How much of the Level 2 vocabulary and artifact schema can be standardized across domains?** The base workflow generalizes well, but semantic families, admissibility rules, and evidentiary thresholds vary by domain.
- **What substitutes are sufficient when execution is impossible or unsafe?** Some domains may require simulation, formal models, or secondary operational evidence.
- **How should accidental contracts be represented in ratified Level 3 specs?** They may need explicit marking, compatibility appendices, or sunset provisions instead of silent promotion.
- **What role can LLMs play as recovery assistants and as contract consumers inside target systems?** They are useful for extraction, comparison, summarization, hypothesis generation, critique, and trace interpretation. They are not ground truth, and their outputs are not authoritative by themselves.

## Provenance and modification notice

Derived from `unreviewed appendix-d reverse engineering specs.md` in `TG-Techie/NLSpec-Spec`, licensed under Apache-2.0.

This file has been modified and extended.