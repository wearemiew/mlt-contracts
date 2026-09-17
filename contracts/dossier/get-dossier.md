Every export of this case's dossier, newest first. An export is itself a disclosure of the whole
record, so who made one and when is part of the case's history and readable as such. A running
export appears here too, so a caller arriving on the screen sees one already under way rather than
starting a second.

```contract
GET /companies/{companyId}/cases/{caseId}/dossier
200:
  items           []DossierExportSummary
403 NotAuthorizedToExportDossier   Não tem permissão para exportar o dossiê deste processo.
404 CaseNotFound                   Processo não encontrado.
```

```types
DossierExportSummary
  exportId        uuid
  status          enum: Generating | Ready | Failed   (get-dossier-export.md)
  requestedAt     datetime
  requestedBy     uuid
  settledAt       datetime | null
  partCount       int | null                  how many parts the artifact contains; null unless Ready
  byteSize        int | null                  null unless Ready
```
