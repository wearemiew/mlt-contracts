# Contracts

A planning tool for work split across parties — product, design, engineering — and the agents each
of them works with.

An agent plans and builds from the context it is given: one party's. It holds none of what that
party knows but never wrote down — the field a screen assumes, the state a model treats as derived,
the error a client never expected. That is tacit knowledge: it travels only with the people who
hold it, and every agent boundary is a place it stops. The gap shows only after both sides have
built.

A contract writes down the one part of that knowledge every party depends on: the shape they meet
on. It is a **first-class product of planning**. Planning a feature is arriving at the contracts it
needs; work that crosses a party boundary is plannable only once its contract is agreed; an agreed
contract is the unit that is scheduled and built — in parallel, by anyone or any agent. The
agreement is a file, its approval is a pull request review, and implementation on every side
follows from it. This is **contract-driven development (CDD)** — contract as in agreement, not as
in test suite: nothing runs against it but the review.

The contracts here are endpoints. The shape is not fixed to that: the same mechanics — one file,
named owners, approval at a commit — serve anything two or more parties must agree on before
building, and further shapes are added as the need for them appears. Shared types are one; events
a server pushes (SSE, SignalR) and the messages it sends (email, notifications) are others.

The format is in [CONTRACT-FORMAT.md](CONTRACT-FORMAT.md); a worked one is in
[contracts/EXAMPLE-CONTRACT.md](contracts/EXAMPLE-CONTRACT.md).

## What an agreed contract settles

Each party builds to the text, without the others' requirements or reasons: the designer does not
need the backend's rules, the backend does not need the design's, and an agent working for either
needs only the file. When something is wrong, the contract says whose it is: a response that
differs from the file is the server's defect, a request that differs is the client's, and a shape
that turned out wrong is a change to the file — reviewed by the same people who agreed it.

## Who agrees on what

[`.github/CODEOWNERS`](.github/CODEOWNERS) is the authority; this table mirrors it.

| Area | Must agree |
|---|---|
| `contracts/portal-backend/` | @pauloedspinho20 · @nunosilva · @henriq350 |
| `contracts/portal-ai/` | @pauloedspinho20 · @nunosilva |
| `contracts/backend-ai/` | @nunosilva · @henriq350 |
| `contracts/frontoffice-backend/` | @pauloedspinho20 · @henriq350 |

To add an area: one line in CODEOWNERS with its path and everyone who must agree, and a row here. A
contract file under `contracts/` that no rule covers fails the check — the folder is the decision.

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
