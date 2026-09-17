Appoints a member to a case role. A case holds a set of appointments: a role may have several
holders, and one member may hold several roles on the same case.

```contract
POST /companies/{companyId}/cases/{caseId}/appointments
req:
  accountId  uuid                                                                                    required
  role       enum: instrutor_interno | instrutor_externo | secretario | revisor_juridico | decisor   required
201:
  appointment  CaseAppointment
403 NotAuthorizedToAppoint   caller is not this company's hr_admin and is not a super admin
404 CaseNotFound             case not in this company
404 AccountNotFound          accountId is not a member of this company
409 CaseAlreadyClosed        the case is in Arquivo or otherwise closed
409 AlreadyAppointed         accountId already holds this role on this case
```

```types
CaseAppointment
  caseId     uuid
  accountId  uuid
  role       enum: instrutor_interno | instrutor_externo | secretario | revisor_juridico | decisor
```
