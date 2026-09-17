Reads one case's full detail.

```contract
GET /companies/{companyId}/cases/{caseId}
200:
  id                     uuid
  status                 string
  flags                  CaseFlagsReadDto
  employee               CaseEmployeeReadDto
  factsSummary           string
  knowledgeSource        string
  competenciaConfirmed   bool
  instructor             CaseInstructorReadDto
  milestones             CaseMilestonesReadDto
  outcome                string
  callerRole             string
  permissions             []string
  phasePath               []CasePhasePathStepDto
  chargesReview            ChargesReviewReadDto
  requiresPareceres        bool
  preventiveSuspension      PreventiveSuspensionReadDto
  inquiryStatus              InquiryStatusReadDto
  secretary                   CaseSecretaryReadDto
404 CaseNotFound  case not in this company
```

```types
CaseMilestonesReadDto
  knowledgeDate      date         the art. 329.º/2 anchor — effective knowledge of the infraction
+ suspicionDate      date         the art. 352.º anchor — first suspicion of irregular conduct; null when the case has no preliminary inquiry
  inquiryOpenedAt    date
```
