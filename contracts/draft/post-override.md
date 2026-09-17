Overrides a blocked case transition (e.g. proceeding past the caducidade deadline). Every override is
justified in writing and left auditable on the case.

```contract
POST /companies/{companyId}/cases/{caseId}/transitions/override
req:
  command         OverrideCommand   required
  justification   string            required
  idempotencyKey  string            optional
200:
  status          string
  transitionedAt  datetime
400 JustificationRequired      no justification supplied
~ 403 NotAuthorizedToOverride    caller holds neither Instrutor standing on this case nor Revisor Jurídico company standing, and is not super admin ← caller does not hold Revisor Jurídico company standing, and is not super admin
404 CaseNotFound               case not in this company
409 GuardNotOverridable        the surviving block on this transition cannot be overridden
409 NothingToOverride          the transition was not blocked
```

```types
OverrideCommand
  type            string      required   the transition being overridden
  reason          string      optional
```
