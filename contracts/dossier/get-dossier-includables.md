What the case holds that an export may carry, so the request that starts an export is made from a
list the server served rather than from ids the client assembled. Each entry is one includable part:
a deliverable, an evidence item, a portal disclosure. The fixed sections of the dossier — the cover,
the timeline, the checklist, the evidence index, the audit — are not listed here: they are always
included and are not the caller's to choose.

```contract
GET /companies/{companyId}/cases/{caseId}/dossier/includables
200:
  items           []Includable
403 NotAuthorizedToExportDossier   Não tem permissão para exportar o dossiê deste processo.
404 CaseNotFound                   Processo não encontrado.
```

```types
Includable
  itemId          uuid
  kind            enum: Deliverable | Evidence | PortalDisclosure
  title           string                     what the item is called on the case
  occurredAt      datetime                   when it entered the case, for ordering
  available       bool                       false when the stored file cannot be read; an export that includes it will fail
  unavailableReason  string | null            why it cannot be read, in the product's language; null when available
```
