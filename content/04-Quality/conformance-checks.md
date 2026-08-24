---
title: "Conformance Checks"
type: quality
status: stable
tags: [omop, data-quality, conformance, vocabulary]
---

# Conformance Checks

| Check ID | Assertion | Severity |
|---|---|---|
| CON-001 | Standard concept fields contain a valid standard concept or permitted zero | critical |
| CON-002 | Standard concept domain is compatible with target table | critical |
| CON-003 | Source concepts/relationships are valid at the event date and snapshot | error |
| CON-004 | Unit concept is compatible with measurement concept | error |
| CON-005 | Type concepts match the documented evidence source | error |
| CON-006 | Every zero concept has preserved source value and mapping reason | error |

## Example

For MEASUREMENT 93001, the final release must show a valid standard HbA1c Measurement-domain concept and compatible percent unit. Documentation placeholders are accepted only in the documentation fixture—not a production CDM snapshot.

## Vocabulary upgrade gate

Compare concept validity, standard status, domain routing, map coverage, multi-map count, and zero rates before/after the snapshot. Any domain reroute receives targeted reconciliation and clinical review.

## Failure handling

Invalid mappings return to the mapping backlog or quarantine; they are never replaced by a guessed concept solely to pass the check.

Related: [[05-vocabulary-mapping]] · [[domain-assignment]] · [[unmapped-concepts]]
