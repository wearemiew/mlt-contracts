# Contracts

A proposal for planning software with agents, in use on one project. It asks one thing of the
people who plan: agree on the seams before anyone builds across them.

## Where we are

Agents have reorganised how code is written. A developer working with one produces more, faster,
and that is not in question. The next step, which every team is taking, is to bring them into
planning: reading sources and interviews, writing requirements, specs and issues. There is no
settled practice for that. Each team is finding out, and this document is what one team found.

## What happened

We planned a product with agents involved in every step. Requirements were written with them;
backend issues were derived from the requirements with them; frontend tasks were derived from the
backend issues with them. Each step was produced from the output of the step before, and each was
faster and larger for it.

Three things went wrong.

**The issues were hard to read.** Written with agents, they were long, uniform in tone and generic
where they should have been specific. The person receiving one could not tell in a minute what it
asked for, what it assumed, or whether it was right — so they were reviewed the way long text is
reviewed: skimmed and accepted.

**The gaps surfaced downstream, one at a time.** Each step checked only against the one before. A
field the screen assumed and the API never sent, a state the design treated as derived and the
backend treated as typed in, an error the client never expected — each showed up as a comment on a
task, found by whoever was working it.

**The base was wrong.** The design the whole chain rested on had errors no step could detect,
because every step took the previous one as given. They became visible when the specified system
was put next to what everyone could see: the design files and a running prototype. By then both
sides had built.

## Why

Two causes, and between them they explain most of it.

**Tacit knowledge.** A product is planned and built by several parties — product, design,
engineering — and most of what each party knows is never written down. Product knows what a feature
is for; design knows what a screen shows and assumes; engineering knows what the model records and
what it derives. Among people who plan and build together, that knowledge travels in conversation
and memory, and it does not need writing while the same people do both. An agent holds none of it.
It works from the context it is given — one party's — and what that party never wrote is not in it.
Two people on a team share more than their documents; two agents share only the documents. Every
gap above is a piece of knowledge one party had, never wrote, and an agent working for another party
built without.

**Maintainability.** What agents do write is cheap to produce and expensive to keep. An issue
written in an hour takes longer than that to read properly, and a hundred of them cannot be read
properly at all: nobody can say which are current, which contradict each other, or which a later
change made wrong. The cost of producing a document fell; the cost of checking one, and of keeping
it true, did not. So the documents carrying the plan were the ones least able to be maintained — and
an assumption wrong at the base was elaborated by every step and caught by none.

Neither cause is fixed by reviewing harder. Review everything and the speed is gone; review nothing
and the first wrong assumption becomes the whole plan.

## The proposal

Keep the agents in planning. Change what planning has to produce.

At every point where one party's work becomes another's input — a screen and the endpoint behind
it, a backend and the AI service it calls — the shape they meet on is written down in one small
file before anyone builds across it. The file has named owners on each side; they approve it in a
pull request; a check enforces that every owner approved the exact version being merged, and a
change after approval needs fresh approval. That file is a **contract**, and a feature is planned
when its contracts are agreed. This is contract-driven development (CDD) — contract as in
agreement, not as in test suite: nothing runs against it but the review.

What this changes for the people who plan:

- **You approve a page, not an issue.** A contract is the size a person reads in a minute and can
  say is right or wrong. Issues keep being written, with agents, at whatever length; they stop being
  where agreement happens.
- **Tacit knowledge is written once, at the seam**, by the party that has it, and the other party
  reads it there. Nothing else needs sharing: the designer does not need the backend's rules, the
  backend does not need the design's, and an agent working for either needs only the file.
- **A wrong base shows at the first contract, not the last task.** Agreeing on a shape makes each
  side say what it assumes, and an assumption the two sides do not share is found before either
  builds.
- **The file stays true because it is the only place the shape lives.** A change is a pull request
  to the same file, reviewed by the same people; the diff is the delta. Removing an endpoint is
  deleting its file.
- **When something is wrong, the file says whose it is.** A response that differs from the file is
  the server's defect; a request that differs is the client's; a shape that turned out wrong is a
  change to the file.

## Scope

The contracts here are endpoints, because that is where the gaps were. The mechanics — one file,
named owners, approval at a commit — serve anything two parties must agree on before building:
shared types, events a server pushes (SSE, SignalR), messages it sends (email, notifications). They
are added as the need appears.

This does not make issues readable, and it does not replace design. It settles the seams;
everything on one side of a seam is still that party's to plan as it likes.

The format is in [CONTRACT-FORMAT.md](CONTRACT-FORMAT.md); a worked one is in
[contracts/EXAMPLE-CONTRACT.md](contracts/EXAMPLE-CONTRACT.md).

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
