Reassigns or clears the Secretário do caso. `Case.SecretaryAccountId` is set once at intake today, with
no way to change it — this is what a company must call before it can suspend or remove a member who is
the Secretário on an open case.

```contract
PATCH /companies/{companyId}/cases/{caseId}/secretary
req:
  accountId  uuid  optional   omitted or null clears the case's secretary
200:
  caseId              uuid
  secretaryAccountId  uuid
403 NotAuthorizedToReassignCaseRole   caller is neither this company's hr_admin/case manager nor a super admin
404 CaseNotFound                      case not in this company
404 AccountNotFound                   accountId is not a member of this company
409 CaseAlreadyClosed                 the case is in Arquivo or otherwise closed
422 SecretaryNotEligible              accountId does not hold Secretário-eligible standing in this company
```
