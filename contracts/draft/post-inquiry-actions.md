Records a diligência during Inquérito Prévio. Partial contract — only the changed lower bound on
`startedOn`; the endpoint's full request/response shape (kind-discriminated fields) is not yet
contracted here.

```contract
POST /companies/{companyId}/cases/{caseId}/inquiry-actions
req:
~ startedOn  date  required  not before suspicionDate, not after today ← not before knowledgeDate, not after today
~ 422 InvalidDiligenciaStartDate  startedOn is before suspicionDate or after today ← startedOn is before knowledgeDate or after today
```
