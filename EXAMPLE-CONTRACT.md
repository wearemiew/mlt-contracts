# Example contract

What a file under `contracts/` looks like. This one describes a real endpoint — issuing a certidão
(a certified copy) of a case item the arguido has already been given access to — and would live at
`contracts/frontoffice-backend/post-certidoes.md`.

A sentence or two like the paragraph below the rule is welcome: what the endpoint is for, in plain
words, so a reader who has never seen the product can place it. The block is the contract.

---

Issues a certidão of one case item. The item must already be disclosed to the portal.

```contract
POST /companies/{companyId}/cases/{caseId}/certidoes
req:
  itemId          uuid        required   a case item already disclosed to the portal
201:
  certidaoId      uuid
403 NotAuthorizedToPublishPortalItems   caller has no management standing on the case
404 CaseNotFound                        case not in this company
404 ItemNotDisclosed                    item was never disclosed to the portal
```
