Opens a new disciplinary case.

```contract
POST /companies/{companyId}/cases
req:
  employee                            EmployeeRequest    required
  factsSummary                        string             required
  knowledgeSource                     string             required
  knowledgeDate                       date               required   the art. 329.º/2 anchor — effective knowledge of the infraction; must not be in the future
+ suspicionDate                       date               optional   the art. 352.º anchor — first suspicion of irregular conduct; required when flags.hasPreliminaryInquiry is true
  flags                               CaseFlagsRequest   required
  competenciaConfirmed                bool               required   must be true
- instructorAccountId                 uuid               required
- secretaryAccountId                  uuid               optional
+ appointments                        []CaseAppointmentRequest   optional   who holds which roles on the case from the day it opens; may be empty or omitted, and set later
  instructorLegalTrainingPreference   bool               optional
201:
  caseId   uuid
  status   string
403 NotAuthorizedToOpenCase      caller may not open cases in this company
- 403 InstructorNotEligible       instructorAccountId does not hold eligible standing in this company
+ 404 AccountNotFound             an appointments entry names an account that is not a member of this company
+ 422 UnknownCaseRole             an appointments entry names a role that is not a case role
422 IntakeFieldMissing          a required intake field is missing
422 KnowledgeDateInFuture       knowledgeDate is later than today
+ 422 SuspicionDateInFuture       suspicionDate is later than today
+ 422 SuspicionDateRequiredForInquiry   flags.hasPreliminaryInquiry is true and suspicionDate is missing
422 CompetenciaNotConfirmed     competenciaConfirmed is not true
422 EmployeeEmailInvalid        employee.email does not parse as an email address
```

```types
+ CaseAppointmentRequest
+   accountId  uuid                                                                                    required
+   roles      []enum: instrutor_interno | instrutor_externo | secretario | revisor_juridico | decisor   required   at least one

EmployeeRequest
  name             string                    required
  department       string                    required
  protectedStatus  ProtectedStatusRequest     required
  email            string                    optional   validated as an email address if given

ProtectedStatusRequest
  unionRepresentative      bool   required
  worksCouncilMember       bool   required
  pregnantOrParentalLeave  bool   required

CaseFlagsRequest
  hasPreliminaryInquiry  bool   required
  dismissalIntent        bool   required
```
