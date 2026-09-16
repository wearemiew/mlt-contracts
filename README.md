# Contracts

The wire shape of every endpoint two sides meet on — the side that serves it and the side that
consumes it. One file per endpoint, under `contracts/`. The file is the agreement: the server side
builds exactly what it says, the client side consumes exactly what it says, and neither needs the
other's reasons.

The format is in [CONTRACT-FORMAT.md](CONTRACT-FORMAT.md).

## How a contract is agreed

1. Open a pull request that adds or changes one file under `contracts/`.
2. The owners of the files it touches review it. Ownership is per path in
   [`.github/CODEOWNERS`](.github/CODEOWNERS), last matching rule wins — so one area's contracts can
   need one pair of people and another area's a different pair.
3. The **contract-approval** check passes when every owner of every touched file has approved **the
   current head commit** — or authored the pull request. An approval given to an earlier commit does
   not count: a new push means a fresh approval.
4. Merge. The file on `main` is the agreed shape; implementation on either side points at it.

A later change to an endpoint is a new pull request to the same file. The diff is the delta.
Removing an endpoint is deleting its file.
