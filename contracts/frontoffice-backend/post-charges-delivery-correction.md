Corrects a previously-recorded Nota de Culpa service — a distinct, audited event, never a silent
overwrite of the original record. Because the response deadline (art. 355.º) is computed live from the
service date, correcting it recomputes the deadline immediately; the response flags when that recompute
shortens or has already elapsed a deadline the arguido may already have been told about.

```contract
POST /companies/{companyId}/cases/{caseId}/charges-delivery/correction
req:
  reason        string                                required
  method        enum: in_person | registered_letter   optional
  proof         string                                optional   required when method or deliveredAt is given
  deliveredAt   date                                  optional
201:
  caseId                      uuid
  previousMethod              enum: in_person | registered_letter
  previousProof               string
  previousDeliveredAt         date
  method                      enum: in_person | registered_letter
  proof                       string
  deliveredAt                 date
  correctedBy                 uuid
  correctedAt                 datetime
  previousResponseDueDate     date
  responseDueDate             date
  responseDeadlineShortened   bool
400 CorrectionReasonRequired         no reason supplied
403 NotAuthorizedToRecordDelivery    caller holds none of management, Instrutor or Secretário do caso standing on this case
404 CaseNotFound                     case not in this company
409 ChargesNotIssued                 the nota de culpa has not been issued yet
409 NoDeliveryToCorrect              no prior service record exists to correct
422 DeliveryDateInTheFuture          deliveredAt is later than today
422 UnknownDeliveryMethod            method is not a recognised delivery method
422 EvidenceRequiredForCorrection    proof is required when method or deliveredAt is being corrected
```
