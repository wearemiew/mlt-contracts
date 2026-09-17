# Contracts

A proposal for contract-driven development (CDD): a way to bring agents into the planning of a
team project that stays maintainable and scales.

## The shift

Agents have reorganised how code is written. A developer working with one produces more, faster,
and that is not in question. The next step, which every team is taking, is to bring them into
planning: learning the rules the product must follow and the people who will use it, writing
requirements, specs and issues. There is no settled practice for that. Each team is finding out;
this document is a proposal, drawn from one product planned that way.

## In short

On one product planned with agents in every step, the mistakes that cost most were the ones
no step could see — one word that meant one thing to the lawyers, another on the screen and two
things in the model, found after both sides had built. The proposal: at every point where one
team's work becomes another's input, the shape they meet on is negotiated and agreed in one small
file, by named people, before anyone builds across it. Agreeing those files is most of what
planning is, and it is the part agents can be brought into at scale — an agent reads and drafts
the file; a person signs it and answers for it. Each side then knows what it owes the others and
builds without waiting for them. It is set up for one project; nothing in the mechanics is specific
to it.

## What happened

A product was planned with agents involved in every step. Requirements were written with them;
backend issues were derived from the requirements with them; frontend tasks were derived from the
backend issues with them. Each step was produced from the output of the step before, and each was
faster and larger for it.

Three things went wrong.

**The issues were hard to read.** Written with agents, they were confusing: disorganised, poorly
presented, and low in information for their length, with what mattered scattered across sections
that did not build on one another. The person receiving one could not tell in a minute what it
asked for, what it assumed, or whether it was right.

**The gaps surfaced downstream, one at a time.** Each step checked only against the one before. A
field the screen assumed and the API never sent, a state one side treated as derived and the other
as typed in, an error the client never expected — each showed up as a comment on a task, found by
whoever was working it.

**The base was wrong.** The model the requirements assumed — what the system records, what it
derives, which step gates which — had errors no step could detect, because every step took the
previous one as given. They became visible when the specified system was put next to what everyone
could see: the design files and a running prototype. By then both sides had built.

## Why

Two causes suggest themselves, and between them they explain most of it.

**Tacit knowledge.** A product is planned and built by four parties — product, design, frontend,
backend — and in this project a fifth, the AI service. Each, with its agents, gathers the
requirements of its own domain and comes to know what the others do not. Take one thing: the
checklist of a phase. Product knows the practice — the firm works each phase from a list of steps,
so that none is skipped: the facts recorded, competence checked, an instructor appointed, the
deadline under control. Design knows the screen — a "Checklist da fase" panel with the steps and a
count of how many are done. Frontend knows what the panel needs from the API: whether a step is a
tick the user makes or a state the server reports, what "por rever" (pending review) means, and
which message to show when the phase is blocked. Backend knows the model — some steps are facts it
can derive, and those gate the transition; others are only a person's say-so, and a manual tick
must never gate a legal step; so it keeps two things under the one word, a derived set that blocks
and a manual list that does not.

Four true statements about one seam, none of them wrong. What came out of them: an issue that asked
for checklist items computable from the data, with the exit guard wired to them; a manual list,
gating nothing, because the derived side had nothing new to check; a panel showing "0 de 2"; and a
guard named `checklistIncomplete` that never reads the checklist, shown to the user as "Requisitos
da fase".

That knowledge is tacit. It cannot be written down in full, and trying to recreates the record
described next: long documents nobody can keep true. Among people who plan and build together it
does not need writing — it travels in conversation and memory, and it acts when one of them says
"that is not how it works". Nobody holds the whole of it, and there is no one place where the parts
meet: what each party knows shows only in what it can and cannot accept, and with agents doing the
drafting, nobody was asked.

Agents add a second layer of it. Much of what a person knows is told to their agent — in a session,
as corrections, as decisions taken along the way — and it lives in that conversation. An agent's
context is tacit knowledge too: it belongs to one party, it is not a document, it does not survive
the session, and it does not cross to another party's agent. Exporting it does not help; a context
written out is the same long text nobody can keep true.

**Maintainability — of the plan, not the code.** Where that knowledge was written down anyway, it
went into issues. One party's agent drafted them; the next party appended what it found missing;
comments carried corrections; a later change edited some and not others. Each addition was cheap to
make and none was ever consolidated, so the record grew by accretion — and nobody could say, of any
point, which text was current, which contradicted which, or what had in fact been agreed. The cost
of producing text fell; the cost of keeping it true did not.

The map from causes to symptoms: the unreadable issues and the wrong base are the record grown by
accretion; the gaps found downstream are knowledge each party had and was never asked for. Both had a
second cost: because the shape of a seam existed nowhere until one side had built it, the other
side either waited on that pull request to start or guessed and rebuilt, and nobody could say
whose the mismatch was. Neither cause is fixed by reviewing harder. Review everything at every
step and the speed is gone; review nothing and the first wrong assumption becomes the whole plan.

## The proposal

Keep the agents in planning. Plan by agreeing the seams.

At every point where one party's work becomes another's input — a screen and the endpoint behind
it, a backend and the AI service it calls — the shape they meet on is written down in one small
file before anyone builds across it. Negotiating it is where each side's knowledge acts: a shape
one side cannot accept is objected to, and the reason need not be written. The file has named
owners on each side, and they approve it in a pull request. A check enforces that every owner approved the exact version being merged; a change
after approval needs fresh approval. That file is a **contract**, and a feature is planned when its
contracts are agreed. This is contract-driven development (CDD) — contract as in agreement, not as
in test suite: nothing runs against it but the review.

For the checklist above, the contract for the phase's read would have had to name two lists — the
steps a person marks and the requirements the server derives — and which of them gates the exit
would have been a question asked before either was built, not a comment found after.

What this would change for the people who plan:

- **You approve a page, not an issue.** A contract is the size a person reads in a minute and can
  say is right or wrong. Issues keep being written, with agents, at whatever length; they stop being
  where agreement happens.
- **Tacit knowledge stays tacit; the contract represents it.** Nobody is made to write down what
  they know. Each party — the person, and the agent from the context it holds — reads the proposed
  shape against what it knows and accepts or objects, and the file records only the outcome: the
  shape all of them could accept. Nothing else needs sharing: the
  designer does not need the backend's rules, the backend does not need the design's, and an agent
  working for either needs only the file.
- **A wrong assumption is found where two sides disagree, not where one side finishes.** Agreeing
  on a shape makes each side say what it assumes, and the shape either side cannot accept is the
  planning conversation, held before anything is built on it.
- **The file stays true because it is the only place the shape lives.** A change is a pull request
  to the same file, reviewed by the same people, opened by the party whose knowledge was missing;
  the diff is the delta. Removing an endpoint is deleting its file.
- **Nobody waits on another side's pull request.** Once the file is agreed, each side builds
  against it — the screen against the shape, not the running backend; the backend against the
  shape, not the screen — and the dependency is on the file, which exists first.
- **When something is wrong, the file says whose it is.** A response that differs from the file is
  the server's defect; a request that differs is the client's; a shape that turned out wrong is a
  change to the file.

## Scope

The contracts here are endpoints, because that is where the gaps were. The mechanics — one file,
named owners, approval at a commit — serve anything two parties must agree on before building:
shared types, events a server pushes (SSE, SignalR), messages it sends (email, notifications). They
are added as the need appears.

This would not make issues readable, and it does not replace design. It settles the seams;
everything on one side of a seam is still that party's to plan as it likes.

The format is in [CONTRACT-FORMAT.md](CONTRACT-FORMAT.md); a worked one is in
[contracts/EXAMPLE-CONTRACT.md](contracts/EXAMPLE-CONTRACT.md).

## Who agrees on what

[`.github/CODEOWNERS`](.github/CODEOWNERS) is the authority; this table mirrors it.

| Area | Must agree |
|---|---|
| `contracts/portal-backend/` | @pauloedspinho20 · @NunoSilvaMiew · @henriq350 |
| `contracts/portal-ai/` | @pauloedspinho20 · @NunoSilvaMiew |
| `contracts/backend-ai/` | @NunoSilvaMiew · @henriq350 |
| `contracts/frontoffice-backend/` | @pauloedspinho20 · @henriq350 |
| `contracts/draft/` | @henriq350 |

To add an area: one line in CODEOWNERS with its path and everyone who must agree, and a row here. A
contract file under `contracts/` that no rule covers fails the check — the folder is the decision.

`contracts/draft/` is the exception that proves it: one owner, so a contract there is agreed by one
party. A file stays there while the shape is still being worked out, and moving it into the area of
the parties it binds is the act of putting it to them.

## How a contract is agreed

1. **Open one pull request per piece of work that can be agreed and built on its own.** Scope it
   the way the work is scoped — a task, a slice of a feature, a whole feature when it is small —
   and by one test: nothing in it waits on anything outside it, and nothing outside it waits on it.
   The point is parallel work: the moment a request is agreed, the people and agents on every side
   of it start building, without waiting for any other contract to be agreed. Too fine and parties
   collide contracting the same endpoint at once; too coarse and every party sits on one moving
   head, where each push resets everyone else's approval and nobody starts. When two pieces turn
   out to share something both need agreed first, that shared thing is its own, smaller pull
   request, agreed before either.
2. The owners of the files it touches review it. Ownership is per area, in
   [`.github/CODEOWNERS`](.github/CODEOWNERS), last matching rule wins — an area names as many
   people as must agree on its contracts, and different areas name different people. Every area
   also names **one reviewer outside its parties** — a tech lead — because acceptability across
   parties is not correctness: three sides can each accept a shape that is wrong against the
   domain, and someone has to read it against the domain rather than against their own side.
3. The **contract-approval** check passes when every owner of every touched file has approved **the
   current head commit** — or authored the pull request. An approval given to an earlier commit does
   not count: a new push means a fresh approval. Because the author counts as approved on every
   file in the request, **who opens it decides whose approval is implied**: a party opening a
   request silently satisfies its own side, including on files it did not write. The outside
   reviewer opens contract pull requests by default, so every party must approve.
4. Merge. The file on `main` is the agreed shape; every implementation points at it. When the check
   cannot run — a private repository whose Actions are unpaid, a runner that never started — a red
   check is not a verdict: the person merging reads the approvals at the head by hand and says so
   in the merge commit.

**A change after agreement is owed by the party whose knowledge was missing.** The server refuses
something no error line listed — the server side's amendment. A screen needs a field it never
asked for — the client side's. A drawn state that no field expresses — the design's. That party
opens the pull request to the same file and says, in its body, what it did not know or did not
look for. Finding it late does not move it: the record of what was missed is the point — it is the
tacit knowledge the file exists to represent, written down at the moment it stopped being tacit.
The diff is the delta. Removing an endpoint is deleting its file.

**Endpoint shapes in issues are sketches.** An issue may carry a request and response to make its
ask concrete; the contract round exists to test that sketch against what each party knows, and a
contract departs from it wherever a party's knowledge says so — a field moved into the kind it
belongs to, a status dropped because the write does not move the aggregate, a clock anchored on a
different rule. An issue is updated to match an agreed contract, never the reverse.
