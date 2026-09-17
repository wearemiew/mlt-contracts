Reads a case's current legal deadlines.

```contract
GET /companies/{companyId}/cases/{caseId}/deadlines
200:
  items  []DeadlineRow
404 CaseNotFound  case not in this company
```

```types
DeadlineRow
  rule                string                caducidade | prescricao | resposta | pareceres | inicioDiligente | notaDeCulpaAposInquerito
  dueDate             date
  basis               string                a sentence, per rule
  elapsed             bool
  remainingDays        int
  dayKind             enum: calendar | working
~ anchorConfirmed      bool                  for inicioDiligente, true once a material diligência (per the diligência-materiality rule: formal instructor appointment, a hearing held, evidence collected, an analysis begun, or a documented refusal — not a merely scheduled or agendada-only diligência) has occurred on the case; unconditionally true for every other rule ← for inicioDiligente, always false regardless of diligências recorded; unconditionally true for every other rule
  noResponseRecorded  bool                  only on the resposta row; null elsewhere
```
