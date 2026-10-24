# A decision log, filled in

Five entries from a real project, rewritten so that nothing identifies the
product, the client or the organisation. They are kept because each one
records a decision that was later questioned, which is exactly what a
decision log is for.

Newest first.

---

## 2026-05-02: Content is edited in production, not in a preview

**What was decided.** Content changes are made in the production
administration interface. Preview environments show a visible banner saying
that editing there will not refresh the public site.

**Why.** Previews share the production database. Editing from a preview writes
the correct data and reports success honestly, but the cache refresh runs in
the environment that triggered it. The preview updates; production does not.
Nothing anywhere reports the discrepancy, and a private browsing window rules
out the browser cache, which sends the investigation in the wrong direction.

**What was rejected, and why.** Blocking editing in previews entirely. A
preview exists so that changes can be tried; making it read-only removes most
of its value.

**What would change this.** A preview with its own copy of the data. Then
editing there would be safe, and the banner should say something different.

---

## 2026-04-18: One cache label per source of data, applied at the read

**What was decided.** Each source of data carries one label, and that label is
its name. Pages and routes declare nothing.

**Why.** Two hand-maintained lists existed: one mapping sources to labels,
another listing which labels each page needed. Both were correct on the day
they were written, and neither had any reason to stay correct. Adding a read
to a page without adding its label produced a page that silently stopped
refreshing.

**What was rejected, and why.** A helper that would apply the label
automatically, making the omission impossible. It cannot exist: it would need
to receive the query as a function, and a function cannot cross the caching
boundary. Measured, not assumed, the build fails and falls back silently.

**What would change this.** If the framework ever allows a cached function to
receive behaviour rather than values, the helper becomes possible and should
be preferred.

---

## 2026-03-27: Two dependency versions pinned exactly

**What was decided.** Two packages are declared at an exact version rather
than a range, unlike the rest.

**Why.** Adding a new dependency triggered a known package-manager issue with
optional platform binaries. The only repair is to delete the lock file and
reinstall, which re-resolves every range and silently advances versions
unrelated to the work. Three approaches were measured; only "clean install
with those two versions pinned" produced both a complete lock file and no
version drift.

**What was rejected, and why.** Accepting the drift. Both packages were
unrelated to the batch, and one of them renders every icon in the product.
Also rejected: repairing the lock file afterwards, which produced a lock file
missing all platform binaries, a deployment failure that does not reproduce
locally.

**What would change this.** When those packages need to advance, relax the
pins and do a clean install in that batch, deliberately.

---

## 2026-02-11: The administration pages are rendered in the tests

**What was decided.** A test signs in and loads every page behind
authentication, failing on any console error, any failed request, or any
near-empty page.

**Why.** Two hundred and ten tests passed over a change that made two
administration pages return an error. None of them rendered a page behind
authentication, so none of them crossed the boundary where the fault was.

**What was rejected, and why.** Adding unit tests around the component that
broke. The fault was not in the component: it was in what was passed across a
boundary, which no unit test observes.

**What would change this.** Nothing foreseeable. The negative control was run:
reintroducing the fault fails this test on six pages while the unit tests
stay green.

---

## 2026-01-09: Data guards are written against stored values

**What was decided.** A guard on a data correction matches the exact value it
expects to replace, taken from a dump of the real data.

**Why.** The first version guarded on a modification timestamp, intending to
skip rows edited by hand. Something else already wrote to those rows, so every
row looked modified and the correction applied to nothing, silently, with a
normal-looking report.

**What was rejected, and why.** Guarding against the text in the source files.
The stored values had drifted from the repository months earlier, in exactly
the rows the guard was about.

**What would change this.** Nothing. This one has been tested in both
directions and has held.
