---
doc_id: SPEC-SHR-000
title: Local Repository Bootstrap for NLSpec Projects
status: active
version: 0.5.4
---

# Local Repository Bootstrap for NLSpec Projects

## Purpose and boundary

This specification defines the minimum conventions that let a coding agent enter an NLSpec project repository, find the governing documents, distinguish binding specifications from supporting documents, and create or update governed files without guessing.

It standardizes two related surfaces:

- the local bootstrap surface
- the identifier forms and default authority semantics for shared governed documents that a local repository references explicitly

The local bootstrap surface includes:

- the bootstrap file location
- local document classes
- local scan behavior
- filename and identifier conventions
- document front matter and its minimal schema
- basic lifecycle and supersession rules
- coding-agent authoring defaults
- handling of non-conforming governed documents
- a repository-facing Definition of Done

For shared governed documents, this specification standardizes:

- the shared filename families
- the shared `doc_id` rules
- shared class inheritance from local counterparts unless this specification says otherwise
- the default authority semantics for explicitly referenced shared documents

This specification does not standardize validator protocols, diagnostic schemas, external registries, recursive shared-governed-document discovery, generic multi-document shared-spec version coordination beyond `governance_spec_ref`, generic machine-typed reference models, or external location and distribution mechanisms for shared documents. A separate shared specification may define a spec-specific local adoption, discovery, or topology surface for itself, including derived-posture or workflow-lineage rules when the workflow needs them. Repositories that need those surfaces should use or define them in separate specifications.

This document is intentionally small. If a repository choice does not affect local document discovery, local document authority, local authoring correctness, or the correct interpretation of explicitly referenced shared governed documents, this specification leaves that choice open.

## Conformance language

The key words `MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, and `MAY` indicate requirement strength.

- `MUST` and `MUST NOT` indicate absolute requirements for conformance.
- `SHOULD` and `SHOULD NOT` indicate strong recommendations. A repository may depart from them only with a deliberate reason.
- `MAY` indicates an allowed option.

## Versioning

This specification uses semantic versioning in `MAJOR.MINOR.PATCH` form. The `version` field in this document's front matter and the version portion of `governance_spec_ref` both use this form.

- A `PATCH` revision clarifies or corrects the specification without changing its requirements.
- A `MINOR` revision adds backward-compatible surface area.
- A `MAJOR` revision changes or removes existing rules.

This section defines versioning for this specification only. Governed documents MAY carry a `version` field, but this specification does not assign semantic-versioning requirements to those documents.

A repository conforms to the version it adopts.

## Repository entry point

A repository that uses this bootstrap MUST expose a bootstrap file at the repository-root-relative path `/.nlspec/authoritative-sources.yaml`.

The `/.nlspec/` directory is reserved for bootstrap artifacts. Governed documents MUST NOT be placed under `/.nlspec/`.

This specification uses repository-root-relative POSIX paths that begin with `/`.

### Bootstrap file fields

The bootstrap file has one required field and two optional fields.

- `governance_spec_ref` is REQUIRED. It is a string that names the adopted version of this specification. Its value MUST use the form `SPEC-SHR-000@<MAJOR.MINOR.PATCH>`. The version portion identifies the adopted `version` of this specification. This specification does not define how a repository reader locates the referenced shared specification artifact outside the repository.
- `document_roots` is OPTIONAL. It is a YAML list of repository-root-relative POSIX paths to scan for local governed documents.
- `excluded_roots` is OPTIONAL. It is a YAML list of additional repository-root-relative POSIX paths to ignore during scanning.

The omission default for `document_roots` remains full-repository scan. Repositories that are large, mixed-governance, or adopting reverse-recovery workflows SHOULD declare explicit `document_roots` rather than rely on the omission default.

A minimal bootstrap file is:

```yaml
governance_spec_ref: SPEC-SHR-000@0.5.4
```

A repository MAY add scan hints when they reduce unnecessary traversal. For example:

```yaml
governance_spec_ref: SPEC-SHR-000@0.5.4
document_roots:
  - /specs
  - /docs/governance
excluded_roots:
  - /specs/archive
  - /docs/governance/generated
```

### Scan behavior

A coding agent or other repository reader MUST interpret the bootstrap file as follows.

- If `document_roots` is present, scan only those roots.
- If `document_roots` is absent, scan the repository recursively from `/`.
- Always ignore `/.git`, `/.hg`, `/.svn`, and `/.nlspec`.
- Also ignore every path named in `excluded_roots`.
- `excluded_roots` MAY be nested within paths from `document_roots`. If a path matches both lists, exclusion wins.

The omission default in the second bullet is exact. It means full recursive scan, not an inferred scan of only likely governance directories. That omission default is conforming, but repositories that are large, mixed-governance, or colocating reverse-recovery working trees should usually declare explicit `document_roots` and keep non-governed working trees or input corpora outside those roots or excluded explicitly. When reverse recovery is adopted locally, `SPEC-SHR-003` defines the separation invariant between `document_roots` and `recovery_root`. This specification does not restate that invariant in full.

This specification defines no external intrinsic registry and no alternate metadata carrier. Document-local YAML front matter is the only authoritative intrinsic metadata surface in scope.

A repository MAY contain arbitrary non-governed files. This specification applies to local governed documents discovered by the local scan and to shared governed documents referenced explicitly from that local repository.

Shared governed documents are referenced explicitly rather than discovered by the local scan. A local scan MUST NOT treat the mere presence of a `*-SHR-*` file under a scanned path or in an adjacent workspace as making that file part of the local governed corpus.

## Governed documents

This specification defines two governed-document scopes.

A local governed document is a UTF-8 Markdown file in the repository that:

- uses one of the local filename forms defined by this specification
- has the `.md` extension
- begins on line 1 with a YAML front matter block delimited by `---`
- carries the required front matter fields for its class
- is discovered by the local scan

A shared governed document uses one of the shared filename forms defined by this specification, has the same Markdown and front matter structure, and is referenced explicitly rather than discovered by the local scan.

Unless this specification says otherwise, a shared governed-document class inherits the semantic role, front matter semantics, lifecycle semantics, and identifier conventions of its local counterpart.

Only active `SPEC` documents define binding implementation behavior.

If a requirement affects externally observable behavior, interoperability, defaults, error behavior, or acceptance criteria, that requirement MUST appear in one or more `SPEC` documents.

`APP`, `ADR`, `RFC`, and `RES` documents may explain, propose, or record decisions, but they do not substitute for a `SPEC`.

If a non-`SPEC` document includes behavior-affecting consequences or supporting material about a binding rule, it MUST identify the controlling active `SPEC` in prose or in `related_docs`. It MUST NOT be the sole source of that rule.

### Authority rules

The repository applies authority by class.

1. Active `SPEC` documents are the only binding behavioral source.
2. Accepted `ADR` documents are authoritative as decision records. They do not substitute for a `SPEC`.
3. `APP`, `RFC`, and `RES` documents are informative.

Explicit reference and explicit adoption are different. Reference makes a shared document part of the reading set. Adoption makes it locally authoritative.

A shared `SPEC-SHR` document inherits `SPEC` semantics, but it is locally binding only when the consuming repository adopts it explicitly. `governance_spec_ref` is one such adoption surface for this specification. A shared `SPEC` may also define its own spec-specific local adoption surface. `SPEC-SHR-003` defines `/.nlspec/reverse-recovery.yaml` as the local adoption, workspace topology, and discovery surface for `SPEC-SHR-002` and, when resolved explicitly or by default, its governed-artifact schema companion. When an adopted shared specification names a repository-local carrier for workflow, topology, or other locally binding reverse-recovery rules, that carrier is locally authoritative only when it is a local active `SPEC` discovered under this specification's scan and lifecycle rules unless the adopted shared specification explicitly states a different carrier rule. Naming such a carrier does not make a `draft` `SPEC`, an `APP`, an `RFC`, a `RES`, or an arbitrary Markdown note authoritative by locator alone. Beyond those spec-specific surfaces, this specification defines no new machine-typed adoption field. A repository MAY adopt a shared `SPEC` only through authoritative local prose, an authoritative local governed document that states applicability, or a spec-specific adoption surface defined by the adopted shared `SPEC` itself.

A repository MAY designate a same-operator local authority owner only through an authoritative local rule or an adopted spec-specific procedure. Such a designation does not waive the adopted workflow specification's own packet, disposition, promotion, or ratification requirements. Silence, shared vocabulary, or reviewer absence do not create same-operator authority by implication.

Shared `APP-SHR`, `RFC-SHR`, and `RES-SHR` documents are informative and additive by default.

A shared `ADR-SHR` document is authoritative as the decision record for the shared corpus that maintains it. It does not, by itself, define target-repository behavior. If a consuming repository wants that decision to control local behavior, the repository MUST adopt it explicitly and the resulting behavior-affecting rules MUST also appear in a locally binding `SPEC`.

A shared document does not become locally binding merely because it appears in `related_docs`, is linked from prose, or is present in the filesystem.

If an accepted `ADR` or `ADR-SHR` has behavior-affecting consequences for a local repository, those consequences MUST also appear in a locally binding `SPEC`.

If two active `SPEC` documents describe incompatible behavior for the same case, the repository is malformed and MUST repair the conflict. There is no implicit precedence by numeric order, repository path, recency, or local-versus-shared status.

### SPEC documents

`SPEC-###-<slug>.md` defines binding local repository behavior.

Shared `SPEC` documents use the shared forms defined in "Naming and identifiers" and are referenced explicitly rather than discovered by the local scan.

Create or edit a `SPEC` when the repository needs to define required behavior, interface shape, defaults, acceptance criteria, or other implementation-affecting rules.

### APP documents

`APP-###-<slug>.md` is a non-binding companion document.

Shared `APP` documents use the shared forms defined in "Naming and identifiers" and inherit `APP` semantics unless this specification says otherwise.

Create an `APP` when the repository needs durable explanation, examples, diagrams, migration notes, or other supporting material that should remain outside the binding specification surface.

An `APP` MUST NOT be the sole home of a binding requirement.

### ADR documents

`ADR-###-<slug>.md` records an adopted or proposed architectural decision.

Shared `ADR` documents use the shared forms defined in "Naming and identifiers" and inherit `ADR` semantics unless this specification says otherwise.

Create an `ADR` when the repository needs a durable record of an architecturally significant decision, the alternatives considered, and the consequences of that decision.

An `ADR` does not define implementation behavior by itself. If the decision affects required behavior, the controlling rule MUST live in a `SPEC`.

### RFC documents

`RFC-###-<slug>.md` and `RFC-<slug>.md` record proposals and convergence work.

Shared `RFC` documents use the shared forms defined in "Naming and identifiers" and inherit `RFC` semantics unless this specification says otherwise.

Create an `RFC` when the repository needs a proposal artifact for a new capability, a major change, or an unresolved design question.

The local and shared unnumbered working forms are allowed while the proposal is still fluid. When the repository or shared corpus wants a stable durable reference, it SHOULD assign a numbered identifier and rename the file to the numbered form.

An accepted `RFC` remains informative. It does not become binding merely because it was accepted.

### RES documents

`RES-###-<slug>.md` captures research and implementation-supporting notes.

Shared `RES` documents use the shared forms defined in "Naming and identifiers" and inherit `RES` semantics unless this specification says otherwise.

Create a `RES` when the repository needs durable research on a domain question, library, protocol, dependency, integration, or other implementation-supporting topic.

A `RES` is informative only.

## Naming and identifiers

Local governed documents MUST use one of the following filename forms.

```text
SPEC-###-<slug>.md
APP-###-<slug>.md
ADR-###-<slug>.md
RFC-###-<slug>.md
RFC-<slug>.md
RES-###-<slug>.md
```

### Shared governed documents

Shared governed documents defined by this specification MUST use one of the following filename forms. The canonical shared filename family is:

```text
SPEC-SHR-###-<slug>.md
APP-SHR-###-<slug>.md
ADR-SHR-###-<slug>.md
RFC-SHR-###-<slug>.md
RFC-SHR-<slug>.md
RES-SHR-###-<slug>.md
```

Shared class series are independent of the corresponding local class series and of one another. A repository may therefore refer to both `SPEC-001` and `SPEC-SHR-000`, or both `RFC-003` and `RFC-SHR-003`, without conflict.

### Identifier rules

`###` means exactly three decimal digits. Numeric identifiers MUST be zero-padded.

Local numeric identifiers are unique within their local class series. Shared numeric identifiers are unique within their shared class series. `SPEC-001`, `SPEC-SHR-001`, `APP-001`, and `APP-SHR-001` may all coexist.

Within any local or shared numeric class series, identifiers:

- MUST be assigned monotonically
- MUST NOT be reused after archival, rejection, withdrawal, or supersession
- MUST remain stable for the life of the document

The working RFC forms, local and shared, are working identifiers rather than numbered stable identifiers. Once an RFC is assigned a number, the repository or shared corpus SHOULD update references that still use the working form. The old working identifier MUST NOT be reused for a different RFC.

### Slug rules

The `<slug>` portion of a local or shared governed filename:

- MUST use lowercase ASCII letters, digits, and hyphens only
- SHOULD describe the subject of the document rather than restating its class
- SHOULD be concise and stable

A slug MAY be refined for clarity, but the identifier portion of the document name MUST remain stable.

### Filename and `doc_id` agreement

Every local or shared governed document MUST declare a `doc_id` in front matter.

For local numbered forms, `doc_id` MUST match the class-plus-number portion of the filename.

Examples:

```text
SPEC-003-provider-routing.md      -> doc_id: SPEC-003
APP-002-authoring-examples.md     -> doc_id: APP-002
ADR-012-routing-boundary.md       -> doc_id: ADR-012
RFC-004-batch-execution.md        -> doc_id: RFC-004
RES-005-postgres-rls-notes.md     -> doc_id: RES-005
```

For the local working RFC form, `doc_id` MUST match the full filename stem.

Example:

```text
RFC-adaptive-harness-governance.md -> doc_id: RFC-adaptive-harness-governance
```

For numbered shared forms, `doc_id` MUST match the class-plus-`SHR`-plus-number portion of the filename.

Examples:

```text
SPEC-SHR-000-local-repository-bootstrap.md -> doc_id: SPEC-SHR-000
APP-SHR-002-shared-authoring-patterns.md   -> doc_id: APP-SHR-002
ADR-SHR-003-shared-taxonomy-source.md      -> doc_id: ADR-SHR-003
RFC-SHR-004-shared-adoption-rules.md       -> doc_id: RFC-SHR-004
RES-SHR-005-shared-namespace-notes.md      -> doc_id: RES-SHR-005
```

For the working shared RFC form, `doc_id` MUST match the full filename stem.

Example:

```text
RFC-SHR-shared-adoption-rules.md -> doc_id: RFC-SHR-shared-adoption-rules
```

## Front matter

Every governed document MUST begin on line 1 with a YAML front matter block delimited by `---`.

Shared governed documents use the same intrinsic field set and field meanings as their local counterparts unless this specification says otherwise.

The intrinsic fields defined by this specification are:

| Field | Type | Presence | Notes |
|---|---|---|---|
| `doc_id` | string | REQUIRED | Must match the filename rules in this specification |
| `title` | string | REQUIRED | Human-readable title |
| `status` | string | REQUIRED | Must be in the allowed set for the document's class |
| `version` | string | OPTIONAL | Allowed on any governed document. For this specification, the value uses `MAJOR.MINOR.PATCH`. |
| `supersedes` | string or list of strings | OPTIONAL | Records the document or documents replaced by the current document |
| `superseded_by` | string or list of strings | CONDITIONAL | REQUIRED when `status` is `superseded` |
| `derived_from` | string | OPTIONAL | Records the source document or external source from which the current document was derived |
| `source_version` | string | CONDITIONAL | Allowed only when `derived_from` is present |
| `related_docs` | list of strings | OPTIONAL | Records lightweight cross-links to local or shared governed documents. It does not create binding force by itself. |

Unknown front matter fields are allowed. Tooling that implements only this specification MUST ignore fields that are not defined by this section.

A spec-specific front-matter extension block defined by another adopted shared specification remains valid as an unknown field at this layer. Tooling that implements only this bootstrap MUST treat such a block as opaque after it validates the fields defined here. For example, `SPEC-SHR-003` may require a `reverse_recovery_overrides` block on a document named by `local_override_locator`; bootstrap-only tooling validates the intrinsic fields defined here and does not attempt to parse that block as a generic bootstrap surface.

This specification does not define `controlling_specs`, `structured_references`, `canonical_source`, `coordination_protocol_ref`, `authoritative_source_discovery_locator`, or any other machine-typed reference or adoption surface. Use prose and, when helpful, `related_docs` for lightweight cross-document links.

`Derived_from` remains a string field at this specification's level. A shared specification may define a corpus-default structured lineage subform for its own workflow-derived outputs while still using this field. `SPEC-SHR-003` does so for reverse-recovery-derived governed outputs. Tooling that implements only this specification MUST treat the field value as opaque and MUST NOT require knowledge of that subform.

If `source_version` is present, `derived_from` MUST also be present.

If `status: superseded` is used, `superseded_by` MUST be present.

When `supersedes`, `superseded_by`, or `derived_from` names a governed document defined by this specification, whether local or shared, the value MUST equal that document's `doc_id`. For `supersedes` and `superseded_by`, this rule applies to every value when the field is a list.

### Examples

A minimal local `SPEC` document:

```markdown
---
doc_id: SPEC-003
title: Provider routing
status: draft
related_docs:
  - ADR-012
---
```

A shared governance specification front matter block:

```markdown
---
doc_id: SPEC-SHR-000
title: Local Repository Bootstrap for NLSpec Projects
status: active
version: 0.5.4
---
```

A shared `APP` document:

```markdown
---
doc_id: APP-SHR-002
title: Shared authoring patterns
status: active
related_docs:
  - SPEC-SHR-000
---
```

A shared `ADR` document:

```markdown
---
doc_id: ADR-SHR-003
title: Shared taxonomy source
status: accepted
related_docs:
  - SPEC-SHR-000
---
```

A numbered shared `RFC` document:

```markdown
---
doc_id: RFC-SHR-004
title: Shared adoption rules
status: draft
---
```

A working shared `RFC` document:

```markdown
---
doc_id: RFC-SHR-shared-adoption-rules
title: Shared adoption rules
status: draft
---
```

A shared `RES` document:

```markdown
---
doc_id: RES-SHR-005
title: Shared namespace notes
status: active
---
```

A working local `RFC` document:

```markdown
---
doc_id: RFC-adaptive-harness-governance
title: Adaptive harness governance
status: draft
---
```

A recovery-derived local `SPEC` document:

```markdown
---
doc_id: SPEC-004
title: Export service contract
status: draft
derived_from: recovery:plan=plan:recovery/plan.yaml;slices=slice:api-job-lifecycle,slice:storage-output-format;packet=ARP-0001
---
```

A superseded `SPEC` document:

```markdown
---
doc_id: SPEC-002
title: Legacy routing rules
status: superseded
superseded_by: SPEC-003
---
```

## Non-conforming governed documents

A repository or adjacent shared workspace MAY contain files that match a local or shared governed filename form but do not satisfy this specification. A coding agent or other repository reader MUST handle any in-scope such file as a non-conforming governed document rather than silently normalizing it.

Governed-document conformance checks apply only to in-scope UTF-8 Markdown `.md` files. A file that is outside scan scope or lacks the `.md` extension is not a governed-document candidate merely because its basename resembles a governed filename form.

- If an in-scope `.md` file matches a local or shared governed filename form but its YAML front matter is missing, unparseable, or missing required fields, the reader MUST NOT treat it as conforming and MUST NOT silently treat it as non-governed. The reader SHOULD warn and MAY continue only if the defect is irrelevant to the current task.
- If an in-scope `.md` file matches a local or shared governed filename form because it predates governance or was authored outside the current governed process, the reader MUST still treat it as a non-conforming governed document until it is repaired. Filename resemblance alone MUST NOT silently adopt it as governed, and MUST NOT silently erase the defect by pretending the file was never a candidate.
- If a file's filename and `doc_id` disagree, the reader MUST treat the file as non-conforming, SHOULD warn, and MAY continue only if the defect is irrelevant to the current task.
- If a file uses a `status` that is invalid for its class, the reader MUST treat the file as non-conforming, SHOULD warn, and MAY continue only if the defect is irrelevant to the current task.
- If a reader detects conflicting active `SPEC` documents for the same case, including a conflict between local and explicitly adopted shared `SPEC` documents, it MUST warn and MUST NOT silently resolve the conflict by choosing one document over another. The reader MAY continue only if the conflict is irrelevant to the current task.

This section defines reader behavior only. It does not define a validator protocol or diagnostic format.

## Lifecycle and supersession

Lifecycle is class-scoped. A status that is valid for one class may be invalid for another. Shared forms use the same lifecycle vocabulary and meanings as their local counterparts unless this specification says otherwise.

This specification defines allowed statuses and their meanings. It does not define an exhaustive transition graph. Repositories that need stricter lifecycle transition rules SHOULD define them in a local `SPEC`.

This specification defines no separate `adopted` status for shared documents. Adoption, when it occurs, is an authority question rather than a lifecycle state.

### Allowed statuses by class

- `SPEC` and `SPEC-SHR`: `draft`, `active`, `superseded`, `archived`
- `APP` and `APP-SHR`: `draft`, `active`, `archived`
- `ADR` and `ADR-SHR`: `proposed`, `accepted`, `superseded`, `archived`
- `RFC` and `RFC-SHR`, including their working forms: `draft`, `active`, `accepted`, `rejected`, `withdrawn`, `archived`
- `RES` and `RES-SHR`: `draft`, `active`, `withdrawn`, `archived`

A repository MUST NOT assign a status outside the allowed set for a document's class.

### Status semantics

`draft` means the document is under active authoring. A `draft` document is never binding.

`active` means the document is current for its class when that class uses `active` as its current state.

`proposed` applies only to `ADR` and `ADR-SHR` documents. It means the architectural direction is being considered but has not yet been adopted by the owning project or shared corpus.

`accepted` applies to `ADR`, `ADR-SHR`, `RFC`, and `RFC-SHR` documents.

- For `ADR`, `accepted` means the project has adopted the architectural direction.
- For `ADR-SHR`, `accepted` means the shared corpus that owns the document has adopted the architectural direction. It does not, by itself, create target-repository behavioral authority.
- For `RFC` and `RFC-SHR`, `accepted` means the proposal outcome was accepted, but the RFC remains informative.

`superseded` means the document was replaced by one or more successor documents.

`archived` means the document is retained for reference but is no longer maintained.

`withdrawn` means the proposal or research note was retracted without adoption.

`rejected` applies only to `RFC` and `RFC-SHR` documents and means the proposal was considered and not adopted.

Only active `SPEC` documents can be binding implementation authority. For shared `SPEC-SHR`, local binding depends on explicit adoption under "Authority rules."

Ratification and local lifecycle activation are different by default. A `draft` `SPEC` that has been reviewed or ratified under another workflow remains non-binding until the local repository lifecycle moves it to `active`. A repository MAY couple ratification and activation only by an explicit authoritative local rule.

Accepted `ADR` documents are authoritative as decision records, but they do not override a `SPEC`.

When a document is replaced, the new document SHOULD record `supersedes`, and the replaced document MUST record `superseded_by` if it uses `status: superseded`.

## Coding-agent authoring defaults

A coding agent working in a local repository that uses this bootstrap SHOULD proceed in the following order.

1. Read `/.nlspec/authoritative-sources.yaml`.
2. Determine the document roots.
3. If you are establishing or revising bootstrap for a large, mixed-governance, or reverse-recovery workspace, prefer explicit `document_roots` and keep non-governed working trees or input corpora outside them or excluded explicitly.
4. Read the active local `SPEC` documents relevant to the task.
5. Read any explicitly referenced shared governed documents relevant to the task.
6. Choose the smallest document class that fits the change.
7. Create or update the governed file with a conforming filename and front matter.
8. Keep binding behavior in `SPEC` documents.

Use these defaults when creating new governed documents unless the repository has a stronger local rule.

- New `SPEC`, `APP`, `RFC`, and `RES` documents start as `draft`.
- New `ADR` documents start as `proposed`.
- The same starting-status defaults apply to the corresponding shared forms.
- If you are changing binding behavior, create or edit a `SPEC`.
- If you are recording an adopted architectural decision, create or update an `ADR`.
- If you are proposing or converging on a change, create or update an `RFC`.
- If you need supporting explanation or examples, create or update an `APP`.
- If you are capturing research or implementation notes, create or update a `RES`.

If the task requires creating or updating a shared governed document, use the corresponding `*-SHR-*` filename form and matching `doc_id` defined in "Naming and identifiers."

If a non-`SPEC` document starts carrying binding rules, move those rules into a `SPEC` and leave only supporting material in the original document.

If a local repository needs a binding rule that is presently stated only in an informative shared document, restate or adopt that rule in a locally binding active `SPEC`. Do not treat an informative shared document as the sole source of a binding local rule.

When keeping behavior-affecting supporting material in a non-`SPEC` document, point readers to the controlling active `SPEC` in prose or in `related_docs`. A mere `related_docs` mention is insufficient to make a shared document locally binding.

Do not infer shared adoption from link structure, directory layout, filename pattern, or mere file presence.

If you encounter a non-conforming governed document, follow the handling rules in the preceding section instead of silently repairing the defect by assumption.

## Definition of Done

A repository conforms to this bootstrap when all of the following are true.

1. `/.nlspec/authoritative-sources.yaml` exists and `governance_spec_ref` names the adopted version of this specification in the form `SPEC-SHR-000@<MAJOR.MINOR.PATCH>`.
2. Governed documents live outside `/.nlspec/`. Local governed documents use only the allowed local classes and filename forms. Any referenced or materialized shared governed documents use only the allowed shared classes and filename forms. Any bootstrap scan hints use the path forms and overlap semantics defined by this specification.
3. The local scan obeys this specification: it discovers only local governed documents, applies full-repository scan only when `document_roots` is omitted, ignores the required excluded paths, considers only in-scope UTF-8 Markdown `.md` files as governed-document candidates, and does not treat shared governed documents as scan-discovered merely because `*-SHR-*` files exist under a scanned path or in an adjacent workspace.
4. Every local or shared governed document in scope begins with a YAML front matter block on line 1, and each filename agrees with its `doc_id`.
5. Every governed document in scope conforms to the front matter field schema defined by this specification: required fields are present, conditional fields satisfy their conditions, and every field defined by this specification uses its declared type.
6. Binding behavior is defined only by active `SPEC` documents. Informative local or shared documents are not the sole source of a binding rule. If a non-`SPEC` document includes behavior-affecting consequences, it identifies the controlling active `SPEC` in prose or in `related_docs`. A mere mention of a shared document in `related_docs` or by prose link does not create local binding force.
7. Status, supersession, and lineage fields satisfy all applicable invariants: each `status` is valid for its class, `superseded_by` is present when `status` is `superseded`, `source_version` appears only when `derived_from` is present, each local governed document named by `supersedes`, `superseded_by`, or `derived_from` exists and is referenced by its `doc_id`, and any in-scope shared governed document named by those fields is referenced by its `doc_id`.

Nothing else is required by this specification.
