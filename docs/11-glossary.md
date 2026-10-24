# Glossary

Shared vocabulary. The point is not precision for its own sake: it is that a
team using the same words stops having the same argument twice.

---

**Acceptance criterion.** A statement that can be checked by someone who did
not write it. "The list loads in under 200 ms with 500 rows" is a criterion.
"The list is fast" is a wish.

**Agent.** A system that produces work from instructions. In this method it
holds the Build role and part of Design. It never holds Decide.

**Batch.** The unit of work. One batch, one mergeable change, one decision to
accept it. If it cannot end in something that runs, it is too big.

**Batch report.** What a batch produces at the end: what was done, what was
measured, what was given up, what is still open.

**Challenge.** The role that produces questions and reviews, and never
produces the deliverable. The moment the challenger writes the answer, nobody
is left to question it.

**Claim.** Anything the project states as fact to the outside world. A number,
a capability, a comparison, a date. If nobody can point to where it comes
from, it does not ship.

**Coding agent.** See *Agent*.

**Decide.** The fourth role. Arbitration, acceptance, release. Always held by
a person.

**Decision log.** The project's memory of *why*. Why this approach and not
the obvious one; what was tried and abandoned. It is what lets anyone pick up
the work instead of re-deciding it.

**Design system.** The set of decisions made once, spacing, type, colour
roles, component shapes, states, so that no screen has to negotiate them
again. The contract between Design and Build.

**Evidence.** Output you can look at. A test run, a measured number, a
screenshot. "It works" is a claim; "here is what it printed" is evidence.

**Fast lane.** The shortened cycle, for changes whose worst outcome is
cosmetic. Never used for data, security, deletion, or anything touching
production.

**Frame.** The first step of the cycle. Writing the problem, and writing what
is explicitly out of scope.

**Guard.** The condition on a data change that decides which rows it may
touch. Written against the value you expect to replace, never against a
modification timestamp.

**Human decision point.** One of six moments where the cycle stops: scope,
trade-off, irreversible action, acceptance, release, claim.

**Instruction file.** The file an agent reads at the start of every session,
holding constraints that must never be re-explained. Not a description of the
codebase.

**Irreversible action.** Delete, publish, send, grant, charge, write to
production data. Prepared by anyone, approved individually by a person.

**Out of scope.** The section of a specification that says what the batch
deliberately does not do. The most valuable section and the most often
skipped, because an agent fills silence with initiative.

**Review assistant.** See *Challenge*.

**Screen.** The specification, in this method. Readable by anyone with a
stake in the product, which a written description is not.

**Verify.** The step that produces evidence. Distinct from Build, because
something that compiles has not been verified, and distinct from Review,
because evidence is not yet a decision.
