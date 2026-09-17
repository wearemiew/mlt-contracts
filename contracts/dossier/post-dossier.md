Starts an export of the case's dossier — the legal record compiled from every phase. The work is
asynchronous: this write answers with the export's id and the caller polls it
(get-dossier-export.md). An export always carries the fixed sections; `itemIds` chooses which of the
case's includable items (get-dossier-includables.md) travel with them. Nothing is compiled here —
the export begins in `Generating` and its outcome is read, not returned.

```contract
POST /companies/{companyId}/cases/{caseId}/dossier
req:
  includeAuditLog bool        required   the whole audit trail, not a window of it
  itemIds         []uuid      required   ids from the includables read; empty exports the fixed sections alone
202:
  exportId        uuid
  status          enum: Generating
400 UnknownIncludable             O dossiê inclui um elemento que não pertence a este processo.
403 NotAuthorizedToExportDossier  Não tem permissão para exportar o dossiê deste processo.
404 CaseNotFound                  Processo não encontrado.
409 ExportAlreadyRunning          Já está em curso uma exportação deste dossiê; aguarde que termine.
```
