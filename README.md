# Contracts

The wire shape of every endpoint, agreed by everyone with a stake in it — whoever serves it, whoever
consumes it, whoever designed the screen it feeds, whoever answers for the system as a whole — and
the agents each of them works with. One file per endpoint, under `contracts/`. The file is the
agreement: each side builds exactly what it says, and none of them needs the others' reasons.

This is **contract-driven development (CDD)**: the contract is written and agreed first, and
implementation on every side follows from it. Nothing about an endpoint is negotiated in code
review, in a design tool or in a chat; it is negotiated once, here, in a pull request that touches
one file.

The format is in [CONTRACT-FORMAT.md](CONTRACT-FORMAT.md); a worked one is in
[contracts/EXAMPLE-CONTRACT.md](contracts/EXAMPLE-CONTRACT.md).

## What this is for

When a screen and the API behind it are built by different people — and, increasingly, by different
agents — most defects are not bugs in anyone's code. They are mismatched expectations: a field one
person assumed another would send, a state the design treated as derived and the backend treated as
typed in, an error the client never knew could come back. That knowledge is tacit: it lives in a
conversation, a design file or someone's head, and every new person or agent has to reconstruct it.

A contract makes the one thing everyone depends on explicit and small: the wire shape. Once it is
agreed, each party builds to the text, in parallel, without having to understand the others'
requirements or reasons — the designer does not need to know the backend's rules, the backend does
not need to know the design's, and an agent working for either needs only the file. When something
is wrong, the contract says whose it is: a response that differs from the file is the server's
defect, a request that differs is the client's, and a shape that turned out wrong is a change to
the file — reviewed by the same people who agreed it.

The approval mechanics exist so this scales past a few people in a room. Ownership names, per area,
everyone who must agree; approval is a GitHub review tied to a specific commit; and a change after
approval needs fresh approvals, automatically.

## How a contract is agreed

1. Open a pull request that adds or changes one file under `contracts/`.
2. The owners of the files it touches review it. Ownership is per area, in
   [`.github/CODEOWNERS`](.github/CODEOWNERS), last matching rule wins — an area names as many
   people as must agree on its contracts, and different areas name different people.
3. The **contract-approval** check passes when every owner of every touched file has approved **the
   current head commit** — or authored the pull request. An approval given to an earlier commit does
   not count: a new push means a fresh approval.
4. Merge. The file on `main` is the agreed shape; every implementation points at it.

A later change to an endpoint is a new pull request to the same file. The diff is the delta.
Removing an endpoint is deleting its file.
