---
title: "Structural Checks"
type: quality
status: stable
tags: [omop, data-quality, structural]
---

# Structural Checks

| Check ID | Assertion | Severity |
|---|---|---|
| STR-001 | Primary identifier is non-null and unique in every populated clinical table | critical |
| STR-002 | Every clinical `person_id` resolves to PERSON | critical |
| STR-003 | Non-null visit/detail/specimen foreign keys resolve | critical |
| STR-004 | Required CDM fields are populated | critical |
| STR-005 | Crosswalk target keys exist and are unique for active source identities | critical |
| STR-006 | Target row has lineage; eligible canonical input has target or exclusion | error |

## Example

```sql
-- Expected: zero
select count(*) as orphan_conditions
from condition_occurrence c
left join person p on p.person_id = c.person_id
where p.person_id is null;
```

## Canonical expected results

PERSON IDs 1 and 2 exist; visits 50001–50003 reference them; detail 51001 resolves to visit 50003; facts 90001, 92001–92002, and 93001 resolve to their documented person/visit.

## Failure handling

Critical structural findings quarantine the candidate release. The pipeline fixes upstream keys or rules; checks do not patch target rows.

Related: [[06-omop-transformation]] · [[source-provenance]] · [[source-vs-omop-reconciliation]]
