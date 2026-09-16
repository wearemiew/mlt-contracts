# Contracts

The wire shape of every endpoint the MyLegalTeam frontend and backend meet on. One file per
endpoint, under `contracts/`. The file is the agreement: the backend serves exactly what it says,
the frontend consumes exactly what it says, and neither side needs the other's reasons.

The format is in [CONTRACT-FORMAT.md](CONTRACT-FORMAT.md).

## How a contract is agreed

1. Open a pull request that adds or changes one file under `contracts/`.
2. The people named in [`.github/CODEOWNERS`](.github/CODEOWNERS) review it.
3. The **contract-approval** check passes when every code owner has approved **the current head
   commit** — or authored the pull request. An approval given to an earlier commit does not count:
   a new push means a fresh approval.
4. Merge. The file on `main` is the agreed shape; implementation on either side points at it.

A later change to an endpoint is a new pull request to the same file. The diff is the delta.
Removing an endpoint is deleting its file.
