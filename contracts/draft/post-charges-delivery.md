Records service of the Nota de Culpa on the arguido — the anchor for the 10-working-day response
deadline (art. 355.º).

```contract
POST /companies/{companyId}/cases/{caseId}/charges-delivery
req:
  method          enum: in_person | registered_letter   required
  proof           string                                required
  deliveredAt     date                                  required
201:
  caseId              uuid
  chargesDeliveredAt  date
400 UnknownDeliveryMethod      method is not a recognised delivery method
~ 403 NotAuthorizedToRecordDelivery   caller holds none of management, Instrutor or Secretário do caso standing on this case ← caller holds neither management nor Instrutor standing on this case
404 CaseNotFound               case not in this company
409 ChargesNotIssued           the nota de culpa has not been issued yet, so there is nothing to serve
422 DeliveryDateInTheFuture    deliveredAt is later than today
```
