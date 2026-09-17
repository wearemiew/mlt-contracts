Lists the company's members and the company role each holds.

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
  role       enum: …
-                  | CaseManager
-                  | InstructorInternal
-                  | InstructorExternal
-                  | LegalReviewer
-                  | Decider
+                  | Member
```
