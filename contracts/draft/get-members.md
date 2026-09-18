Lists the company's members and the titles each holds.

```contract
GET /companies/{companyId}/members
200:
  members  []CompanyMember
403 NotAMember         caller is not a member of this company
404 CompanyNotFound    company doesn't exist
```

```types
CompanyMember
  accountId  uuid
  email      string
~ roles      []enum: hr_admin | gestor | instrutor | secretario | revisor | decisor   ← role enum: HrAdmin | Member
```
