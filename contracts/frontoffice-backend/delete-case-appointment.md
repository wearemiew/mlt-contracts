Removes one appointment from a case. A role can be left with no holder: the actions that require it
refuse until someone is appointed.

```contract
DELETE /companies/{companyId}/cases/{caseId}/appointments/{role}/{accountId}
204:
403 NotAuthorizedToAppoint   caller is not this company's hr_admin and is not a super admin
404 CaseNotFound             case not in this company
404 AppointmentNotFound      accountId does not hold this role on this case
409 CaseAlreadyClosed        the case is in Arquivo or otherwise closed
```
