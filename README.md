# Contracts

The wire shape of every endpoint two sides meet on — the side that serves it and the side that
consumes it. One file per endpoint, under `contracts/`. The file is the agreement: the server side
builds exactly what it says, the client side consumes exactly what it says, and neither needs the
other's reasons.

The format is in [CONTRACT-FORMAT.md](CONTRACT-FORMAT.md); a worked one is in
[EXAMPLE-CONTRACT.md](EXAMPLE-CONTRACT.md).

## What this is for

When a screen and the API behind it are built by different people — and, increasingly, by different
agents — most defects are not bugs in either side. They are mismatched expectations: a field one side
assumed the other would send, a state one side thought was derived and the other thought was typed
in, an error the client never knew could come back. That knowledge is tacit: it lives in a
conversation, a design file or someone's head, and every new person or agent has to reconstruct it.

A contract makes the one thing both sides depend on explicit and small: the wire shape. Once it is
agreed, the side serving the endpoint builds to the text and the side consuming it builds to the
text, in parallel, without either needing to understand the other's requirements or reasons. When
something is wrong, the contract says whose it is: a response that differs from the file is the
server's defect, a request that differs is the client's, and a shape that turned out wrong is a
change to the file — reviewed by the same people who agreed it.

The approval mechanics exist so this scales past two people in a room. Ownership is per pair of
sides, approval is a GitHub review tied to a specific commit, and a change after approval needs a
fresh one, automatically.

## How a contract is agreed

1. Open a pull request that adds or changes one file under `contracts/`.
2. The owners of the files it touches review it. Ownership is per pair of sides, in
   [`.github/CODEOWNERS`](.github/CODEOWNERS), last matching rule wins — so one area's contracts can
   need one pair of people and another area's a different pair.
3. The **contract-approval** check passes when every owner of every touched file has approved **the
   current head commit** — or authored the pull request. An approval given to an earlier commit does
   not count: a new push means a fresh approval.
4. Merge. The file on `main` is the agreed shape; implementation on either side points at it.

A later change to an endpoint is a new pull request to the same file. The diff is the delta.
Removing an endpoint is deleting its file.
