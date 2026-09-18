Reads the signed-in account and the companies it belongs to. `isSuperAdmin` is an account-level
capability, never a title; `memberships` is how the client learns which companies it may enter.

```contract
GET /me
200:
  id            uuid
  email         string
  isSuperAdmin  bool
  memberships   []Membership
401 Unauthorized   no session
```

```types
Membership
  companyId        uuid
  companyName      string
  companySlug      string
~ roles            []enum: hr_admin | gestor | instrutor | secretario | revisor | decisor   ← role string
  hasWorksCouncil  bool
```
