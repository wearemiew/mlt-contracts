Sets the roles a member holds on a case — the whole set, replacing what they hold. A role may have
several holders, and one member may hold several roles on the same case. A role already held keeps
the `assignedBy` and `assignedAt` it was given.

The roles offered for a member are preselected from the titles they hold in the company
(get-members.md): a member titled `instrutor` is offered `instrutor_interno` ticked. Preselection is
a default, not a limit — any role may be set for any member, and the server checks nothing against
the titles.

```contract
PUT /companies/{companyId}/cases/{caseId}/appointments/{accountId}
req:
  roles  []enum: instrutor_interno | instrutor_externo | secretario | revisor_juridico | decisor   required   empty takes the member off the case
200:
  appointments  []CaseAppointment
403 NotAuthorizedToAppoint   caller is not this company's hr_admin and is not a super admin
404 CaseNotFound             case not in this company
404 AccountNotFound          accountId is not a member of this company
409 CaseAlreadyClosed        the case is in Arquivo or otherwise closed
422 UnknownCaseRole          a member of roles is not a case role
```

```types
CaseAppointment
  caseId      uuid
  accountId   uuid
  role        enum: instrutor_interno | instrutor_externo | secretario | revisor_juridico | decisor
  assignedBy  uuid
  assignedAt  datetime
```
