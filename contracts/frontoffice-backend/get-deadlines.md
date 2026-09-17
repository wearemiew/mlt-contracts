Reads a case's current legal deadlines.

```contract
GET /companies/{companyId}/cases/{caseId}/deadlines
200:
  items  []DeadlineRow
404 CaseNotFound  case not in this company
```

```types
DeadlineRow
  rule                enum: …               existing members unchanged
+                            | prescricao     1 calendar year from suspicionDate (CT art. 329.º/1); reported under the same condition as caducidade — while chargesDeliveredAt is unset
~ dueDate             date                  for inicioDiligente, 30 calendar days from suspicionDate; for prescricao, 1 calendar year from suspicionDate ← for inicioDiligente, 30 calendar days from knowledgeDate
~ basis               string                a sentence, per rule; for inicioDiligente now cites suspicionDate as its anchor ← for inicioDiligente, cited knowledgeDate as its anchor
  elapsed             bool
  remainingDays        int
  dayKind             enum: calendar | working
~ anchorConfirmed      bool                  for inicioDiligente, true once a material diligência (per the diligência-materiality rule: formal instructor appointment, a hearing held, evidence collected, an analysis begun, or a documented refusal — not a merely scheduled or agendada-only diligência) has occurred on the case; unconditionally true for every other rule ← for inicioDiligente, always false regardless of diligências recorded; unconditionally true for every other rule
  noResponseRecorded  bool                  only on the resposta row; null elsewhere
```
