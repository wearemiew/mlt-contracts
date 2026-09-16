# Contract format

A contract is the wire shape of one endpoint. It exists only where more than one party must agree: work that adds or
changes no endpoint has no contract.

## One file per endpoint

`contracts/<area>/<verb>-<slug>.md`, where `<area>` is a CODEOWNERS area — the people who must agree — for example
`contracts/frontoffice-backend/post-certidoes.md`. The file holds the endpoint's **current** full
shape as one block, optionally preceded by a sentence or two saying what the endpoint is for — see
[contracts/EXAMPLE-CONTRACT.md](contracts/EXAMPLE-CONTRACT.md):

````
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
````

| Line | Form |
|---|---|
| Header | `METHOD /path`, path params in `{braces}` |
| `req:` | One field per line: `name  type  required\|optional  [constraint]`. Nested fields indented under their parent |
| `2xx:` | One field per line: `name  type`. A bare `204` means no body |
| Error | `status ErrorKey  condition` — one line per error the caller can receive. The condition is written in the product's language: it is the sentence the client shows for the code, lifted from here rather than invented |

Types: `string` `int` `bool` `date` `datetime` `uuid` `enum: A | B | C` `[]type` `{}`. Wire names
as they travel — camelCase, exact.

Who may call the endpoint, and in what state, is not a field: it is the error lines. A `403` and its
code say who is refused; a `409` and its code say when. Every refusal the caller can meet is one
line with its condition — that is the knowledge that is otherwise tacit.

## Named types

A field whose shape is more than a scalar is a **type**, defined once by name and referenced from
the endpoint block. Types are defined in a `types` block in the file of the endpoint that owns
them, below the endpoint; a file that reuses another file's type references it by name and file
(`[]Deliverable (get-deliverable.md)`) and never redefines it. The grammar inside a `types` block is
the field grammar above — pseudo-typed, the same on every side of the wire.

````
```contract
GET /companies/{companyId}/cases/{caseId}/deadlines
200:
  items           []DeadlineRow
404 CaseNotFound  case not in this company
```

```types
DeadlineRow
  rule            enum: …            existing members unchanged
+                       | decisao    30 calendar days from the anchor; reported only while the case is in FinalReport
  dueDate         date
  basis           string             a sentence, per rule
  elapsed         bool
  remainingDays   int
  dayKind         enum: calendar | working
  anchorConfirmed bool
+ anchor          enum: pareceres | evidenceConcluded | null    decisao only; the art. 357.º/1–2 anchor that applied
```
````

`…` inside an enum means *the members it already has, unchanged*; only members this contract touches
are written. A type is written in full the first time it is contracted and thereafter only as its
current fields plus the markers below — never as every value every discriminator could produce.

**Extending a type.** A shape that is another type plus more fields is written `Name : Base` with
only the extra lines beneath — `DeliverableDetail : Deliverable` / `downloadPath string`. An endpoint
then names the extended type; a block never says "plus".

**Fields present only for one value of a discriminator.** Inside a type, a qualified group
`[kind=RelatorioFinal]` lists fields that are on the wire — flat, at that level — only when the
discriminator has that value, and null or absent otherwise, as the group's first line says. It is
the same qualifier the request side uses, and it is how a shared row carries kind-specific columns
without reading as if every row gained them.

**Served rows, not shape.** When a read returns *instances* of a type that this contract fixes —
the `FieldSchema` rows a deliverable kind serves — they go in a `rows` block, one value per line in
the type's column order, never as fields of the type:

````
```rows [kind=RelatorioFinal]
"Factos provados"   multiline   input   required
"Sanção proposta"   enum        input   required   options are the OutcomeOption members
```
````

## Changing what already exists

When an endpoint or a type the server already serves gains, loses or changes a line, the line
carries a marker in the first column:

```
  name  type  …        unchanged — the server already serves it exactly so
+ name  type  …        added by this contract
~ name  type  …  ← old form   changed; what it was, after the arrow
- name  type  …        removed by this contract
```

Markers sit **where the change is**. A new row on a read is a `+` member on its type, not a rewritten
endpoint block; an endpoint block whose own shape did not change carries no marker at all. Markers
describe the delta from the previous agreed version of the file — or, for an endpoint contracted for
the first time, from what the server serves today. The next contract on the same file clears the old
markers and sets its own, so a file on `main` reads as *the current shape, with the latest agreed
change visible in it*. Error lines take the same markers.

## Endpoints discriminated by a type

Some writes take several request shapes behind one path, chosen by a field — a deliverable `kind`,
a transition `command`. Their file holds the **common request once**, and each discriminator value
that carries its own fields or refusals gets a **qualified group**, written only for the values this
contract touches:

````
```contract
POST /companies/{companyId}/cases/{caseId}/deliverables
req:
  kind            enum: …   required
+                       | RelatorioFinal
  mode            enum: Generated | Uploaded   required
  fields          {}        required   per kind, below
+ [kind=RelatorioFinal]
+   "Factos provados"     multiline   required   keyed by label, as this mechanism keys its fields
+   "Sanção proposta"     enum        required   an option of the kind's fields read
201:
  deliverableId   uuid
+ 422 DeliverableFieldRequired  [kind=RelatorioFinal]   a required section is empty; carries `field`
```
````

| Line | Form |
|---|---|
| Group | `[<discriminator>=<value>]` on its own line, under the field it discriminates; the value's fields indented beneath |
| Qualified error | `status ErrorKey  [<discriminator>=<value>]  condition` — a refusal only that value can meet |

Two further types serve these groups: `multiline` — a `string` that holds paragraphs and is rendered
as one — and `enum` without an inline member list, meaning the members are served by a read named in
the constraint, not fixed in the contract.

## What a contract never contains

Reasons. Alternatives. Questions. Decisions and their history. Design ids, claim ids, status,
owner, estimate, links. Text beyond the short orientation above the block. A `TBD` or a `?` makes it
a draft, not a contract.
