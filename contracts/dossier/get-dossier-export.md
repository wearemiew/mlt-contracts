One export, as it stands. The caller polls this until the status settles. A `Ready` export carries
its manifest — what the artifact contains, part by part — and the path to download it; a `Failed`
export carries the parts that failed and why, and offers no download: an incomplete legal record is
never handed over as a complete one. The `DossierExport` type and the `DossierItem` family
it is built from are owned here.

```contract
GET /companies/{companyId}/cases/{caseId}/dossier/{exportId}
200:
  DossierExport
403 NotAuthorizedToExportDossier   Não tem permissão para exportar o dossiê deste processo.
404 CaseNotFound                   Processo não encontrado.
404 ExportNotFound                 Esta exportação não pertence a este processo.
```

```types
DossierExport
  exportId        uuid
  status          enum: Generating | Ready | Failed
  requestedAt     datetime
  requestedBy     uuid                        the account that started it
  settledAt       datetime | null             when it became Ready or Failed; null while Generating
  includeAuditLog bool                        as requested
  [status=Ready]                              flat on the wire, null otherwise
    manifest      []DossierPart               every part the artifact contains, in the order it contains them
    contentType   string                      the artifact's media type
    byteSize      int
    sha256        string                      of the artifact as served, so a copy can be checked against it
    downloadPath  string                      relative, exchanged for a short-lived authorised URL as evidence is
  [status=Failed]                             flat on the wire, null otherwise
    failures      []DossierFailure            every part that could not be compiled; never empty
```

```types
DossierItem
  itemId          uuid | null                 the case item this is; null only for a fixed section, which only a part can be
  kind            enum: Cover | Timeline | Checklist | EvidenceIndex | Audit | Deliverable | Evidence | PortalDisclosure
  title           string                      what it is called — the part's heading in the artifact, the item's name on the case

DossierPart : DossierItem
  sha256          string | null               of the file this part carries; null for a part the export rendered itself

DossierFailure : DossierItem
  reason          string                      why it could not be compiled, in the product's language — shown as it stands
```

The fixed sections every export contains, in order — the parts of the manifest that are not the
caller's to choose:

```rows [kind=fixed]
Cover           "Capa"                      the case, its parties, and when the export was made
Timeline        "Cronologia do processo"    every phase transition, in order
Checklist       "Checklist do processo"     each phase's items and their standing
EvidenceIndex   "Índice da prova"           every evidence item with its SHA-256, whether or not its file travels
Audit           "Registo de auditoria"      the whole audit trail; present only when includeAuditLog is true
```
