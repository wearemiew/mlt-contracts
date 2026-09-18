Invites people to the company by email, each with the titles they will hold on joining.

```contract
POST /companies/{companyId}/invites
req:
  invites            []InviteItem   required   at most 100
201:
  results            []InviteResult
400 InvitesRequired               no invites supplied
403 NotAuthorizedToInvite         caller is not this company's hr_admin or a super admin
403 NotAuthorizedToGrantHrAdmin   only a super admin may grant hr_admin
404 CompanyNotFound               company doesn't exist
422 TooManyInvites                more than 100 invites in one request
```

```types
InviteItem
  email  string                                                                   required
~ roles  []enum: hr_admin | gestor | instrutor | secretario | revisor | decisor   required   may be empty; a row naming an unknown title is Rejected   ← role enum: HrAdmin | Member

InviteResult
  email   string
  status  enum: Invited | Skipped | Rejected
```
