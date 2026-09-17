Changes a member's company role. A member holds one company role; what they do on a case comes from
the appointments the case holds, not from here.

```contract
PATCH /companies/{companyId}/members/{accountId}
req:
  role  enum: …          required
-             | CaseManager
-             | InstructorInternal
-             | InstructorExternal
-             | LegalReviewer
-             | Decider
+             | Member
200:
  accountId  uuid
  role       enum: HrAdmin | Member
403 NotAuthorizedToManage        caller is not this company's hr_admin or a super admin
403 NotAuthorizedForHrAdmin      only a super admin may grant, change or remove hr_admin
404 CompanyNotFound              company doesn't exist
404 MemberNotFound               target is not a member of this company
409 LastHrAdminCannotBeRemoved   target is the last remaining hr_admin
422 InvalidRole                  role is not one of the company roles
```
