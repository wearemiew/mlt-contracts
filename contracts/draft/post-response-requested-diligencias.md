Records a diligência the defence requested in its response to the Nota de Culpa. Reuses
`RecordDiligenciaRequest`/`RecordDiligenciaResult` from `post-inquiry-actions.md` (not yet contracted
there in full). Partial contract — only the changed lower bound on `startedOn`.

```contract
POST /companies/{companyId}/cases/{caseId}/response/requested-diligencias
req:
~ startedOn  date  required  not before suspicionDate, not after today ← not before knowledgeDate, not after today
~ 422 InvalidDiligenciaStartDate  startedOn is before suspicionDate or after today ← startedOn is before knowledgeDate or after today
```
