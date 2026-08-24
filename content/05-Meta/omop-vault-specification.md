---
title: "EHR → OMOP Transformation Vault Specification"
aliases:
  - "EHR to OMOP Specification"
status: draft
type: specification
visibility: public
---

# EHR → OMOP Transformation Vault Specification

## 1. Purpose

This specification defines an **Obsidian-friendly Markdown vault** that documents the transformation of an **imaginary Electronic Health Record (EHR) dataset** into an **OMOP Common Data Model (CDM) release**.

The vault is designed to serve simultaneously as:

1. an **engineering reference** for building a reproducible raw-EHR-to-OMOP pipeline; and
2. a **problem-solving reference** for resolving common OMOP transformation challenges.

The vault will be maintained locally in Obsidian and published as a public website from GitHub.

The content must therefore be:

- readable as plain Markdown;
- navigable in Obsidian;
- renderable on GitHub;
- suitable for static-site publication;
- independent of proprietary EHR data;
- reproducible from an entirely synthetic or imaginary source dataset.

---

## 2. Core Principle

Every substantive transformation page MUST connect **source semantics** to **OMOP semantics**.

Each page MUST contain:

1. a minimal raw EHR example;
2. the corresponding OMOP representation;
3. transformation logic;
4. assumptions;
5. validation checks;
6. known failure modes;
7. links to related engineering stages and challenge pages.

A page is incomplete if it explains a transformation only conceptually without showing both sides of the transformation.

---

## 3. Vault Dimensions

The vault has two primary dimensions.

### 3.1 Dimension A — Engineering Lifecycle

This dimension explains how raw EHR data becomes a releasable OMOP database.

Recommended lifecycle:

```text
Imaginary EHR source
        ↓
Source profiling
        ↓
Raw ingestion
        ↓
Staging
        ↓
Semantic normalization
        ↓
Vocabulary mapping
        ↓
OMOP domain transformation
        ↓
Referential integrity checks
        ↓
Data quality validation
        ↓
Clinical validation
        ↓
Release packaging
        ↓
OMOP release
```

Engineering pages SHOULD focus on implementation, reproducibility, lineage, testing, and release management.

### 3.2 Dimension B — Transformation Challenges

This dimension explains how specific semantic or technical problems are resolved.

Examples include:

- patient identity resolution;
- observation-period construction;
- visit reconstruction;
- visit-detail construction;
- diagnosis semantics;
- problem-list versus encounter diagnosis;
- medication order versus administration;
- drug exposure dates;
- laboratory concept mapping;
- laboratory unit normalization;
- categorical measurement results;
- duplicate event handling;
- local-code mapping;
- `concept_id = 0`;
- source provenance;
- timestamp selection;
- source-to-standard concept mapping;
- domain assignment;
- late-arriving data;
- deleted or corrected source records;
- vocabulary upgrades;
- incremental loading;
- clinical plausibility validation.

Challenge pages SHOULD focus on decision rules, trade-offs, examples, failure modes, and verification.

---

## 4. Information Architecture

Recommended vault structure:

```text
/
├── index.md
│
├── 01-source-ehr/
│   ├── source-model-overview.md
│   ├── patients.md
│   ├── encounters.md
│   ├── diagnoses.md
│   ├── procedures.md
│   ├── medications.md
│   ├── medication-administrations.md
│   ├── laboratory-results.md
│   ├── observations.md
│   └── code-systems.md
│
├── 02-engineering/
│   ├── engineering-overview.md
│   ├── 01-source-profiling.md
│   ├── 02-raw-ingestion.md
│   ├── 03-staging-model.md
│   ├── 04-normalization.md
│   ├── 05-vocabulary-mapping.md
│   ├── 06-omop-transformation.md
│   ├── 07-data-quality.md
│   ├── 08-clinical-validation.md
│   ├── 09-incremental-loads.md
│   └── 10-release-process.md
│
├── 03-omop-domains/
│   ├── person.md
│   ├── observation-period.md
│   ├── visit-occurrence.md
│   ├── visit-detail.md
│   ├── condition-occurrence.md
│   ├── procedure-occurrence.md
│   ├── drug-exposure.md
│   ├── measurement.md
│   ├── observation.md
│   ├── device-exposure.md
│   ├── death.md
│   ├── note.md
│   └── specimen.md
│
├── 03-Challenges/
│   ├── challenge-index.md
│   ├── patient-identity-resolution.md
│   ├── visit-reconstruction.md
│   ├── local-code-mapping.md
│   ├── domain-assignment.md
│   ├── medication-order-vs-administration.md
│   ├── lab-unit-normalization.md
│   ├── timestamp-selection.md
│   ├── duplicate-events.md
│   ├── unmapped-concepts.md
│   ├── source-provenance.md
│   └── late-arriving-data.md
│
├── 05-examples/
│   ├── example-patient-001.md
│   ├── example-outpatient-diabetes.md
│   ├── example-emergency-to-inpatient.md
│   └── example-medication-administration.md
│
├── 04-Quality/
│   ├── quality-framework.md
│   ├── structural-checks.md
│   ├── conformance-checks.md
│   ├── plausibility-checks.md
│   └── source-vs-omop-reconciliation.md
│
└── 05-Meta/
    ├── README.md
    ├── specification.md
    ├── omop-vault-specification.md
    ├── vault-conventions.md
    ├── glossary.md
    ├── synthetic-data-principles.md
    ├── omop-version-policy.md
    ├── release-history.md
    └── assets/
        ├── diagrams/
        └── data/
```

The exact folder names MAY change, but the separation between source, engineering, OMOP domains, challenges, worked examples, and quality SHOULD remain.

---

## 5. Navigation Model

Every page SHOULD participate in both dimensions through links.

Example:

```markdown
Engineering stage: [[05-vocabulary-mapping]]
OMOP domain: [[measurement]]
Challenge: [[local-code-mapping]]
Related example: [[example-outpatient-diabetes]]
```

A challenge page SHOULD link back to the engineering stage where the problem is handled.

An engineering page SHOULD link to the challenge pages that commonly occur during that stage.

An OMOP domain page SHOULD link to:

- relevant source tables;
- relevant engineering stages;
- common transformation challenges;
- worked examples.

This produces a graph that can be navigated by **pipeline stage**, **OMOP table**, or **problem type**.

---

## 6. Page Types

The vault defines six primary page types.

| Page type | Purpose |
|---|---|
| Source page | Explain one imaginary EHR entity or table |
| Engineering page | Explain one pipeline stage |
| OMOP domain page | Explain transformation into one OMOP domain |
| Challenge page | Resolve one recurring transformation problem |
| Worked example | Trace one synthetic clinical story end-to-end |
| Quality page | Define validation and reconciliation rules |

---

## 7. Required Frontmatter

Every substantive page MUST contain YAML frontmatter.

Minimum:

```yaml
---
title: "Visit Reconstruction"
type: challenge
status: draft
tags:
  - omop
  - ehr
  - transformation
---
```

Recommended extended metadata:

```yaml
---
title: "Visit Reconstruction"
type: challenge
status: stable
omop_domains:
  - VISIT_OCCURRENCE
  - VISIT_DETAIL
source_entities:
  - encounter
  - admission
engineering_stages:
  - normalization
  - omop-transformation
challenges:
  - nested-encounters
  - duplicate-encounters
tags:
  - omop
  - ehr
  - visit
  - etl
---
```

Allowed `type` values SHOULD be:

```text
meta
source
engineering
omop-domain
challenge
example
quality
```

---

## 8. Required Content for Transformation Pages

Every OMOP-domain page and challenge page MUST contain the following sections.

```markdown
# Title

## Problem

## Source Semantics

## Minimal Raw EHR Example

## Target OMOP Representation

## Transformation Rules

## Mapping Decisions

## Validation

## Failure Modes

## Related Pages
```

Additional sections MAY be added where useful.

---

## 9. Minimal Raw EHR Example Standard

Raw examples MUST be:

- synthetic;
- small enough to understand visually;
- representative of a real transformation issue;
- stable across documentation versions where practical;
- expressed in Markdown tables, CSV, SQL, or JSON;
- explicit about null values and code systems when relevant.

Preferred default format: Markdown table.

Example:

```markdown
### Raw EHR: diagnosis

| patient_id | encounter_id | dx_code | code_system | dx_type | recorded_at |
|---|---:|---|---|---|---|
| P001 | E1001 | E11.9 | ICD10CM | encounter | 2026-01-14 09:42 |
```

For more complex records, use fenced CSV or JSON.

Do not use screenshots as the primary representation of data.

---

## 10. OMOP Example Standard

Corresponding OMOP examples MUST identify the destination table and show the important fields.

Example:

```markdown
### OMOP: CONDITION_OCCURRENCE

| condition_occurrence_id | person_id | condition_concept_id | condition_start_date | condition_type_concept_id | visit_occurrence_id | condition_source_value |
|---:|---:|---:|---|---:|---:|---|
| 90001 | 1 | `<standard concept>` | 2026-01-14 | `<type concept>` | 50001 | E11.9 |
```

Documentation MUST NOT invent authoritative OMOP concept IDs unless they have been verified.

When the exact concept ID is not important to the example, use semantic placeholders:

```text
<standard SNOMED concept for type 2 diabetes mellitus>
<source ICD-10-CM concept for E11.9>
<standard visit concept>
```

This prevents documentation examples from silently becoming incorrect when vocabularies change.

---

## 11. Raw-to-OMOP Field Mapping

Where field-level transformation is central, include a mapping table.

Example:

| Source field | OMOP field | Rule |
|---|---|---|
| `patient_id` | `person_id` | Resolve through patient identity map |
| `dx_code` | `condition_source_value` | Preserve original value |
| `dx_code` + `code_system` | `condition_source_concept_id` | Resolve source concept |
| mapped standard concept | `condition_concept_id` | Standard vocabulary mapping |
| `recorded_at` | `condition_start_date` | Use clinical diagnosis timestamp according to source rule |
| `encounter_id` | `visit_occurrence_id` | Resolve through visit crosswalk |

Mapping rules MUST distinguish:

- direct copy;
- lookup;
- semantic mapping;
- calculated value;
- derived relationship;
- default;
- intentionally null;
- unsupported/unmapped.

---

## 12. Transformation Rule Style

Rules SHOULD be written as deterministic statements.

Good:

```text
If `dx_type = 'encounter'`, create a CONDITION_OCCURRENCE row.
Preserve `dx_code` in `condition_source_value`.
Resolve the code to a source concept using the declared source vocabulary.
Map the source concept to a standard concept.
Link the row to the reconstructed visit through the encounter crosswalk.
```

Avoid vague rules such as:

```text
Map diagnoses appropriately.
```

Where ambiguity exists, document the decision explicitly.

---

## 13. Challenge Page Pattern

Each challenge page SHOULD answer five questions:

1. **What makes the source data ambiguous or difficult?**
2. **What incorrect OMOP representation is tempting?**
3. **What rule does this vault adopt?**
4. **What information is preserved for provenance?**
5. **How is the result validated?**

Recommended structure:

```markdown
# Medication Order vs Administration

## Problem

## Why It Matters

## Source Semantics

## Minimal Raw EHR Example

## Incorrect Transformation

## Adopted Rule

## Target OMOP Representation

## Edge Cases

## Validation

## Failure Modes

## Related Pages
```

---

## 14. Worked Example Pattern

Worked examples trace a synthetic patient journey across multiple source and OMOP tables.

Example story:

```text
P001
→ outpatient visit
→ diabetes diagnosis
→ HbA1c measurement
→ metformin prescription
```

A worked example SHOULD include:

1. source records;
2. transformation sequence;
3. generated OMOP records;
4. crosswalk identifiers;
5. vocabulary decisions;
6. validation results;
7. links to each relevant challenge.

Worked examples SHOULD use stable synthetic identifiers so readers can follow the same patient across pages.

---

## 15. Synthetic Dataset Conventions

The source dataset is imaginary.

No example may contain real patient data.

Recommended identifier conventions:

```text
Patients:      P001, P002, ...
Encounters:    E1001, E1002, ...
Orders:        O2001, O2002, ...
Administrations: A3001, A3002, ...
Specimens:     S4001, S4002, ...
Source rows:   SRC-...
```

OMOP surrogate identifiers MAY use integers:

```text
person_id = 1
visit_occurrence_id = 50001
condition_occurrence_id = 90001
```

The vault SHOULD maintain a small, coherent imaginary source dataset rather than creating unrelated records on every page.

---

## 16. Canonical Synthetic Clinical Scenarios

The vault SHOULD define a small set of reusable scenarios.

Suggested scenarios:

### Scenario A — Outpatient diabetes

Includes:

- outpatient encounter;
- diagnosis;
- HbA1c;
- metformin prescription.

Covers:

- visit occurrence;
- condition occurrence;
- measurement;
- drug exposure;
- vocabulary mapping.

### Scenario B — Emergency to inpatient admission

Includes:

- ED encounter;
- inpatient admission;
- ICU segment;
- discharge.

Covers:

- visit reconstruction;
- visit detail;
- nested encounters;
- timestamps.

### Scenario C — Inpatient medication

Includes:

- medication order;
- scheduled dose;
- administration;
- held dose;
- discontinued order.

Covers:

- medication semantics;
- order versus administration;
- exposure dates;
- duplicate prevention.

### Scenario D — Local laboratory code

Includes:

- local lab identifier;
- numeric result;
- local unit;
- reference range.

Covers:

- local-code mapping;
- measurement concepts;
- unit mapping;
- provenance.

### Scenario E — Duplicate diagnosis

Includes:

- clinical diagnosis;
- billing diagnosis;
- problem-list record.

Covers:

- duplicate handling;
- source semantics;
- provenance;
- condition type.

---

## 17. OMOP Version Policy

The vault MUST declare the target OMOP CDM version in one canonical location:

```text
05-Meta/omop-version-policy.md
```

The README SHOULD link to it.

The vault MUST also record:

- OMOP CDM version;
- vocabulary release or snapshot;
- transformation release;
- synthetic source-data version.

Concept IDs SHOULD be treated as vocabulary-version-dependent unless intentionally fixed and verified.

---

## 18. Vocabulary Documentation

Vocabulary mapping pages SHOULD distinguish:

```text
source value
→ source vocabulary concept
→ standard concept
→ OMOP domain
```

Example:

```text
"E11.9"
→ ICD-10-CM source concept
→ standard SNOMED concept
→ Condition domain
```

Each mapping decision SHOULD state:

- source code system;
- source code;
- source concept resolution;
- standard concept;
- target domain;
- mapping relationship;
- handling when no mapping exists.

---

## 19. Provenance Requirements

Transformation examples MUST preserve enough information to trace an OMOP row back to its source representation.

Where applicable, show:

- source value;
- source concept;
- source table;
- source record identifier;
- source timestamp;
- transformation rule;
- OMOP row identifier.

The documentation SHOULD distinguish between:

```text
clinical provenance
technical lineage
vocabulary provenance
```

---

## 20. Data Quality Model

Every transformation SHOULD be testable at four levels.

### 20.1 Structural

Examples:

- required identifiers are present;
- foreign keys resolve;
- dates have valid types;
- expected rows were generated.

### 20.2 Conformance

Examples:

- concept belongs to the expected domain;
- standard concepts are used where required;
- unit concepts are compatible with measurements.

### 20.3 Plausibility

Examples:

- event occurs after birth;
- visit end is not before visit start;
- laboratory values are clinically plausible;
- drug administration falls within a plausible encounter period.

### 20.4 Reconciliation

Examples:

```text
source diagnosis count
vs
OMOP condition count
```

```text
source admissions
vs
OMOP inpatient visits
```

```text
source lab distribution
vs
OMOP measurement distribution
```

Each page SHOULD include at least one page-specific validation rule.

---

## 21. Failure-Mode Documentation

Failure modes are first-class content.

Each challenge SHOULD document:

| Failure mode | Example |
|---|---|
| Data loss | Source events disappear during transformation |
| Duplication | One clinical event becomes multiple OMOP rows |
| Misclassification | A measurement becomes an observation |
| Semantic inflation | An order is interpreted as an administration |
| Temporal distortion | Result time is used instead of collection time |
| Vocabulary failure | Local concept maps to `concept_id = 0` |
| Broken lineage | OMOP row cannot be traced to source |
| Invalid aggregation | Multiple encounters collapse into one visit |

Where useful, include an explicit **incorrect transformation** before showing the adopted solution.

---

## 22. Engineering Documentation Requirements

Each engineering page SHOULD document:

```text
Inputs
Outputs
Transformation responsibility
Dependencies
Persistent artifacts
Idempotency expectations
Incremental-load behavior
Error handling
Quality gates
Release implications
```

Example:

```markdown
## Inputs

- normalized encounter rows
- patient identity crosswalk

## Outputs

- VISIT_OCCURRENCE staging rows
- encounter-to-visit crosswalk

## Quality Gates

- every visit has a valid person_id
- visit_end_datetime >= visit_start_datetime
- source encounter reconciliation within documented tolerance
```

---

## 23. Crosswalks

The engineering model SHOULD explicitly document crosswalk tables.

Examples:

```text
source_patient_id → person_id
source_encounter_id → visit_occurrence_id
source_medication_id → drug_concept_id
source_lab_code → measurement_concept_id
```

Crosswalks SHOULD be deterministic, versioned, and auditable.

Pages SHOULD state whether identifiers are:

- source-derived;
- hash-derived;
- sequence-generated;
- persistent across releases;
- regenerated per release.

---

## 24. Incremental Load Policy

The vault SHOULD document how the imaginary pipeline handles:

- newly inserted records;
- updated source records;
- deleted records;
- late-arriving records;
- patient merges;
- encounter corrections;
- vocabulary changes;
- reprocessing.

Every incremental-load example SHOULD explain whether OMOP identifiers remain stable.

---

## 25. Release Definition

An OMOP release is more than a completed ETL run.

A release SHOULD include:

```text
OMOP database snapshot
ETL version
source cutoff timestamp
source-data version
OMOP CDM version
vocabulary version
mapping version
quality report
known issues
release notes
```

The release page SHOULD make it possible to reproduce the transformation.

---

## 26. Obsidian Compatibility

Pages SHOULD use standard Markdown wherever possible.

Supported conventions:

```markdown
[[internal-page]]
[[internal-page|display text]]
#tags
YAML frontmatter
```

Avoid depending on community plugins for essential navigation or meaning.

Obsidian-specific syntax MAY be used when it degrades gracefully on GitHub.

Important content MUST remain understandable in a plain Markdown renderer.

---

## 27. GitHub Compatibility

The public repository SHOULD render correctly without Obsidian.

Therefore:

- use relative Markdown links where GitHub rendering is important;
- avoid local absolute file paths;
- use lowercase kebab-case filenames;
- keep image assets inside the repository;
- provide meaningful `README.md` navigation;
- avoid plugin-only embeds as the sole representation of important content.

Where Obsidian wikilinks are used, the publishing workflow MUST support them or convert them.

---

## 28. Public-Site Requirements

The public site SHOULD provide at least three navigation paths:

1. **Learn the pipeline**
2. **Browse OMOP domains**
3. **Solve a transformation challenge**

Suggested homepage sections:

```text
Start here
Engineering lifecycle
OMOP domains
Transformation challenges
Worked examples
Data quality
Glossary
```

---

## 29. File Naming

Use lowercase kebab-case.

Good:

```text
visit-reconstruction.md
lab-unit-normalization.md
condition-occurrence.md
```

Avoid:

```text
Visit Reconstruction FINAL.md
lab_units_v2.md
ConditionOccurrencePage.md
```

Number prefixes MAY be used where sequence is important.

---

## 30. Terminology

Use precise terminology.

Preferred:

```text
source EHR
raw source record
staging
normalization
source concept
standard concept
OMOP domain
OMOP CDM
transformation rule
crosswalk
provenance
reconciliation
release
```

Avoid using **migration** when **transformation** or **ETL** is more accurate.

The EHR remains the source system; OMOP is an analytical representation derived from it.

---

## 31. Documentation Style

Pages SHOULD:

- explain one major idea at a time;
- lead with the transformation problem;
- use small data examples;
- make assumptions explicit;
- prefer deterministic rules over prose ambiguity;
- include incorrect examples where they clarify a risk;
- link to deeper pages instead of repeating long explanations;
- distinguish source-system fact from transformation policy.

The tone SHOULD be technical, concise, and implementation-oriented.

---

## 32. Standard Transformation Page Template

```markdown
---
title: "<Title>"
type: <omop-domain|challenge|engineering>
status: draft
omop_domains:
  - <DOMAIN>
source_entities:
  - <source-entity>
tags:
  - omop
  - ehr
---

# <Title>

## Problem

What transformation problem does this page solve?

## Source Semantics

What does the source record actually mean?

## Minimal Raw EHR Example

| ... |
|---|
| ... |

## Target OMOP Representation

### `<OMOP_TABLE>`

| ... |
|---|
| ... |

## Transformation Rules

1. ...
2. ...
3. ...

## Field Mapping

| Source field | OMOP field | Rule |
|---|---|---|
| ... | ... | ... |

## Mapping Decisions

Document semantic, vocabulary, domain, and timestamp decisions.

## Validation

- ...
- ...
- ...

## Failure Modes

- ...
- ...

## Related Pages

- [[related-engineering-page]]
- [[related-domain-page]]
- [[related-challenge-page]]
- [[related-example]]
```

---

## 33. Definition of Done for a Page

A transformation page is complete when:

- [ ] the problem is stated;
- [ ] source semantics are described;
- [ ] a minimal raw EHR example is present;
- [ ] corresponding OMOP data is present;
- [ ] transformation rules are explicit;
- [ ] field mappings are documented where relevant;
- [ ] vocabulary decisions are documented;
- [ ] timestamp decisions are documented where relevant;
- [ ] provenance is preserved;
- [ ] at least one validation check is defined;
- [ ] major failure modes are documented;
- [ ] related engineering and challenge pages are linked;
- [ ] no real patient data is present;
- [ ] the page renders meaningfully outside Obsidian.

---

## 34. Definition of Done for the Vault

The vault is ready for public release when:

- [ ] every core source entity has documentation;
- [ ] every major OMOP target domain has a page;
- [ ] the engineering lifecycle is documented end-to-end;
- [ ] the challenge index covers the major semantic risks;
- [ ] each transformation page contains raw and OMOP examples;
- [ ] canonical synthetic scenarios are internally consistent;
- [ ] concept mappings are version-aware;
- [ ] release metadata is documented;
- [ ] crosswalk behavior is documented;
- [ ] quality checks are documented;
- [ ] source-to-OMOP reconciliation is demonstrated;
- [ ] internal links are valid;
- [ ] GitHub rendering is verified;
- [ ] no sensitive or real patient data is present.

---

## 35. Initial Build Order

Recommended first implementation sequence:

1. create `README.md` and `index.md`;
2. define the synthetic EHR source model;
3. define one canonical outpatient scenario;
4. document `PERSON`;
5. document `VISIT_OCCURRENCE`;
6. document `CONDITION_OCCURRENCE`;
7. document `MEASUREMENT`;
8. document `DRUG_EXPOSURE`;
9. add vocabulary-mapping documentation;
10. add visit-reconstruction challenge;
11. add medication-order-versus-administration challenge;
12. add laboratory-unit-normalization challenge;
13. add structural and reconciliation checks;
14. document the release process;
15. expand with additional scenarios and edge cases.

This sequence creates a thin but complete vertical slice before the vault grows horizontally.

---

## 36. Architectural Summary

The vault should make every transformation explainable through the following chain:

```text
Clinical meaning in imaginary EHR
        ↓
Raw source representation
        ↓
Transformation rule
        ↓
Vocabulary decision
        ↓
OMOP target representation
        ↓
Validation
        ↓
Release
```

The engineering dimension explains **how the transformation is executed**.

The challenge dimension explains **why particular transformation decisions are necessary**.

The shared raw-and-OMOP examples connect both dimensions and make the vault useful as executable documentation rather than only conceptual reference material.
