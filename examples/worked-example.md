# A worked example

One batch, from the request to the merge. The product is a fictional
scheduling tool; the shape of the work is real.

Read this if the templates feel abstract.

---

## The request, as it arrived

> "Customers keep booking slots that are already taken. Can we fix that?"

This is not a specification. It is a symptom, and it contains an implied
solution ("fix that") that nobody has examined.

---

## Step 1. Frame

Fifteen minutes, with the person who asked.

Two questions changed the batch:

- *How do they end up booking a taken slot?* The list is loaded once and
  not refreshed, so a slot taken by someone else still looks free.
- *What happens today when they try?* The booking fails with a generic
  error message.

So there are two problems: stale availability, and an unhelpful failure. They
have different costs and different fixes.

**Decision:** this batch handles the failure message only. Stale availability
is a separate batch, because fixing it properly means deciding how often to
refresh, which is a trade-off nobody has made yet.

**Written down:**

> **Problem.** When a slot is taken between loading the list and confirming,
> the booking fails with a generic error. The customer does not know what
> happened or what to do next.
>
> **Out of scope.** Refreshing availability. Changing how slots are held. The
> booking confirmation email.

---

## Step 2. Design

Two screens, differing in a way that can be named:

- **A**: the error replaces the slot in the list, with the next three free
  slots underneath.
- **B**: the error appears above the list, and the taken slot is greyed out
  in place.

Shown to the person who asked, who chose **B** in under a minute, for a reason
nobody had anticipated: *"they often want that exact time, and seeing it
greyed tells them to try tomorrow instead of taking something they don't
want."*

This is the value of screens. That sentence could not have come from a written
description.

**States designed:** the error itself, the greyed slot, and what happens if
*every* remaining slot is taken.

---

## Step 3. Plan

The specification, in full, because it is short:

> **Acceptance criteria**
> - [ ] A booking that fails because the slot was taken shows: "This slot was
>   just booked by someone else." above the list
> - [ ] That slot appears greyed and is not selectable
> - [ ] The customer's other form entries are preserved
> - [ ] If no slot remains that day, the empty state offers the next day
> - [ ] The message is reachable by a screen reader when it appears
>
> **What is irreversible.** Nothing. No data is written or deleted.
>
> **Risk.** The "slot was taken" case is hard to reproduce by hand. If we
> cannot trigger it reliably, we cannot verify criterion 1.

That last risk was written before the work started, and it turned out to be
the whole difficulty of the batch.

---

## Step 4. Build

The agent implemented the five criteria and stopped, as its instructions
require, on the risk:

> "I cannot reproduce the 'slot taken' case by hand: it needs two bookings in
> the same second. I can add a way to force the error for testing, but that
> means code in the application whose only purpose is to be triggered by a
> test. Do you want that, or should I verify this case differently?"

This is a stop at the right moment: a trade-off, surfaced before a decision
was made by default.

**Decision:** no test-only code in the application. Instead, the test calls
the booking function twice concurrently and asserts that the second one
produces exactly this message.

---

## Step 5. Verify

Evidence produced:

```
booking: concurrent attempts on the same slot
  ✓ the second attempt is rejected
  ✓ the rejection carries the "already booked" reason
  ✓ the form entries survive the rejection
  ✓ the empty state appears when no slot remains
```

Plus two things that only a person can confirm, opened by hand:

- The message is announced by a screen reader when it appears
- The greyed slot is visibly different, not only disabled

---

## Step 6. Review

Accepted, with one adjustment:

> "The message says 'just booked'. If someone leaves the page open for an
> hour, 'just' is wrong. Say 'This slot is no longer available.'"

Returned to **Build**: right problem, wrong wording. Fixed in ten minutes.

This is the return path being used. A review where nothing ever comes back is
a review that is not reviewing.

---

## Step 7. Log

```
## 2026-03-14: No test-only code in the application

**What was decided.** The concurrent-booking case is verified by calling the
booking function twice from a test, not by adding a trigger in the
application.

**Why.** Code whose only purpose is to be triggered by a test lives in the
product forever, and will eventually be reachable by something other than the
test.

**What was rejected, and why.** A hidden parameter that forces the error.
Faster to write, and it would have made the failure path reachable from
outside.

**What would change this.** If we ever need to demonstrate this case to a
customer live, a controlled way to trigger it becomes worth the risk.
```

---

## What the batch cost

| Step | Time |
|---|---|
| Frame | 15 min |
| Design | 40 min |
| Plan | 15 min |
| Build | 2 h |
| Verify | 30 min |
| Review and adjust | 20 min |
| Log | 10 min |

Four hours and ten minutes. About ninety minutes of that is the method.

What it bought: a batch that did not also try to fix availability, a message
chosen by the person who understands the customers, a trade-off decided
deliberately rather than by default, and a written reason that will still be
there when someone proposes the hidden parameter again.
