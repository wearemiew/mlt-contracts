Reassigns the Instrutor on a case. `Case.InstructorAccountId` is set once at intake today, with no way
to change it — this is what a company must call before it can suspend or remove a member who is the
Instrutor on an open case.

```contract
PATCH /companies/{companyId}/cases/{caseId}/instructor
req:
  accountId  uuid  required
200:
  caseId               uuid
  instructorAccountId  uuid
403 NotAuthorizedToReassignCaseRole   caller is neither this company's hr_admin/case manager nor a super admin
404 CaseNotFound                      case not in this company
404 AccountNotFound                   accountId is not a member of this company
409 CaseAlreadyClosed                 the case is in Arquivo or otherwise closed
422 InstructorNotEligible             accountId does not hold Instrutor-eligible standing in this company
```
