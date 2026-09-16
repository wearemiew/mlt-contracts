```contract
POST /companies/{companyId}/cases/{caseId}/certidoes
who:  HrAdmin | CaseManager | SuperAdmin
when: any
req:
  itemId          uuid        required   a case item already disclosed to the portal
201:
  certidaoId      uuid
403 NotAuthorizedToPublishPortalItems   caller has no management standing on the case
404 CaseNotFound                        case not in this company
404 ItemNotDisclosed                    item was never disclosed to the portal
```
