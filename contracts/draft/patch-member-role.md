Sets the titles a member holds in the company — the whole set, replacing what they hold. Two of
them are standings that act on every case (`hr_admin`, `gestor`); the other four say what the
person is in the firm, and preselect the roles offered when they are appointed on a case. No title
grants anything on a case by itself: what a member does on a case comes from the appointments the
case holds.

```contract
PATCH /companies/{companyId}/members/{accountId}
req:
~ roles  []enum: hr_admin | gestor | instrutor | secretario | revisor | decisor   required   the full set; empty leaves the member with no title   ← role enum: HrAdmin | Member
200:
  accountId  uuid
~ roles      []enum: hr_admin | gestor | instrutor | secretario | revisor | decisor   ← role enum: HrAdmin | Member
400 RolesRequired                roles is missing
403 NotAuthorizedToManage        caller is not this company's hr_admin or a super admin
403 NotAuthorizedForHrAdmin      only a super admin may grant or remove hr_admin
404 CompanyNotFound              company doesn't exist
404 MemberNotFound               target is not a member of this company
409 LastHrAdminCannotBeRemoved   the set removes hr_admin from the last member holding it
~ 422 UnknownTitle               a member of roles is not a company title   ← 422 InvalidRole
```
