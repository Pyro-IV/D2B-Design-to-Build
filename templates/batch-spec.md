# Batch specification: template

> One batch. One mergeable change. One decision to accept it.
> If this cannot end in something that runs, split it.

---

**Batch** <name>
**Date** <date>
**Lane** Full cycle / Fast lane, *fast lane is never used for data,
security, deletion, or anything touching production*

---

## The problem

<One paragraph. What is wrong today, and for whom. Not the solution.>

## Out of scope

> The most valuable section. An agent fills silence with initiative.

- <What this batch deliberately does not touch>
- <A nearby thing that is also broken, and is a separate batch>

## Screens

> The specification itself. Link them, or attach them.
> If there are no screens, say why, some batches have no visible surface.

- <link or reference>
- States covered: empty / loading / error / focus / overflow

## What the screens cannot say

> Behaviour over time, edge cases, and rules that are not visual.

- <rule>
- <edge case and what should happen>

## Acceptance criteria

> Each one checkable by someone who did not write it.

- [ ] <criterion>
- [ ] <criterion>

## What is irreversible

> So the stopping points are known before the work starts, not discovered
> during it. Write "nothing" if that is true, it usually is not.

- <action>, approved by <who>, before <when>

## Risks

| Risk | If it happens | Mitigation |
|---|---|---|
| <risk> | <consequence> | <what we do> |
