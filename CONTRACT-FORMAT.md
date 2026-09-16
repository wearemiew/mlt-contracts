# Contract format

A contract is the wire shape of one endpoint. It exists only where two sides meet: work that adds or
changes no endpoint has no contract.

## One file per endpoint

`contracts/<sides>/<verb>-<slug>.md`, where `<sides>` names the two sides that meet on it — for example
`contracts/frontoffice-backend/post-certidoes.md`. The file holds
the endpoint's **current** full shape, as one block and nothing else:

````
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
````

| Line | Form |
|---|---|
| Header | `METHOD /path`, path params in `{braces}` |
| `who:` | Roles that may call it, `\|`-separated. Omit for a portal-token endpoint |
| `when:` | Phases in which the call is accepted, `\|`-separated, or `any`. Omit when not phase-gated |
| `req:` | One field per line: `name  type  required\|optional  [constraint]`. Nested fields indented under their parent |
| `2xx:` | One field per line: `name  type`. A bare `204` means no body |
| Error | `status ErrorKey  condition` — one line per error the caller can receive |

Types: `string` `int` `bool` `date` `datetime` `uuid` `enum: A | B | C` `[]type` `{}`. Wire names
as they travel — camelCase, exact.

## What a contract never contains

Reasons. Alternatives. Questions. Decisions and their history. Design ids, claim ids, status,
owner, estimate, links. Prose outside the block. A `TBD` or a `?` makes it a draft, not a contract.
