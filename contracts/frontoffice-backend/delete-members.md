Removes a member from the company.

```contract
DELETE /companies/{companyId}/members/{accountId}
204:
404 CompanyNotFound                  company doesn't exist
403 NotAuthorizedToManage            caller is not this company's hr_admin or a super admin
404 MemberNotFound                   target is not a member of this company
403 NotAuthorizedForHrAdmin          only a super admin may remove an hr_admin
409 LastHrAdminCannotBeRemoved       target is the last remaining hr_admin
+ 409 MemberHoldsUnreassignedCaseRoles  target is the Instrutor or Secretário do caso on a case not yet closed; reassign those roles first — carries `caseIds`
```
