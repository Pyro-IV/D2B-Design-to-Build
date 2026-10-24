# Acceptance checklist: template

> What you open, in what order, and what counts as a failure.
> Written before the review, not during it.

---

**Batch** <name>
**Reviewer** <who>
**Environment** <where this is being checked, and whether it shares
production data>

---

## Before opening anything

- [ ] The batch report exists, and sections 3 and 4 are not empty
- [ ] Every acceptance criterion from the specification is listed below
- [ ] If this batch changes data, the data safety flow was followed

## What to open, in order

| # | What | Looking for | Fails if |
|---|---|---|---|
| 1 | <page or screen> | <what should be true> | <what would be wrong> |
| 2 | <page or screen> | | |

## Checks that are not visual

- [ ] <keyboard navigation, focus visible>
- [ ] <empty state, error state>
- [ ] <behaviour at a small screen size>
- [ ] <anything measured: timing, contrast, size>

## Irreversible actions in this batch

- [ ] <action>, approved by <who>, on <date>

## Outcome

- [ ] **Accept** : ready to release, as a separate decision
- [ ] **Adjust** : right problem, specific changes listed below
- [ ] **Reject** : wrong problem, returns to Frame

<Notes>
