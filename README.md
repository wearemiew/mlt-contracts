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

**The issues were hard to read.** Written with agents, they were confusing: disorganised, poorly
presented, and low in information for their length, with what mattered scattered across sections
that did not build on one another. The person receiving one could not tell in a minute what it
asked for, what it assumed, or whether it was right.

**The gaps surfaced downstream, one at a time.** Each step checked only against the one before. A
field the screen assumed and the API never sent, a state one side treated as derived and the
other as typed in, an error the client never expected — each showed up as a comment on a
task, found by whoever was working it.

**The base was wrong.** The design the whole chain rested on had errors no step could detect,
because every step took the previous one as given. They became visible when the specified system
was put next to what everyone could see: the design files and a running prototype. By then both
sides had built.

## Why

Two causes, and between them they explain most of it.

**Tacit knowledge.** A product is planned and built by four parties — product, design, frontend,
backend — and each, with its agents, gathers the requirements of its own domain and comes to know
what the others do not. Take one thing: the checklist of a phase. Product knows the practice — the
firm works each phase from a list of steps, so that none is skipped: the facts recorded, competence
checked, an instructor appointed, the deadline under control. Design knows the screen — a
"Checklist da fase" panel with the steps and a count of how many are done. Frontend knows what the
panel needs from the API: whether a step is a tick the user makes or a state the server reports,
what "por rever" (pending review) means, and which message to show when the phase is blocked.
Backend knows the model — some steps are facts it can derive, and those gate the transition; others
are only a person's say-so, and a manual tick must never gate a legal step; so it keeps two things
under the one word, a derived set that blocks and a manual list that does not.

Four true statements about one seam, none of them wrong. What came out of them: an issue that asked
for checklist items computable from the data, with the exit guard wired to them; a manual list,
gating nothing, because the derived side had nothing new to check; a panel showing "0 de 2"; and a
guard named `checklistIncomplete` that never reads the checklist, shown to the user as "Requisitos
da fase".

Most of that knowledge is never written down. Among people who plan and build together it travels
in conversation and memory, and it does not need writing while the same people do both. Nobody
holds the whole of it, and there is no one place where the parts meet: what one party knows reaches
the others only through what it wrote, and what it never wrote does not reach them at all.

**Maintainability.** Where that knowledge was written, it was written into issues. One party's
agent drafted them; the next party appended what it found missing; comments carried corrections; a
later change edited some and not others. Each addition was cheap to make and none was ever
consolidated, so the record grew by accretion — and nobody could say, of any point, which text was
current, which contradicted which, or what had in fact been agreed. The cost of producing text
fell; the cost of keeping it true did not. An assumption wrong at the base was elaborated by every
step and caught by none.

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
- **The dispersed knowledge converges in one file.** Each party writes into the contract what only
  it knows about the seam, and the file is the one representation all of them agree on. Nothing else
  needs sharing: the designer does not need the backend's rules, the backend does not need the
  design's, and an agent working for either needs only the file.
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
