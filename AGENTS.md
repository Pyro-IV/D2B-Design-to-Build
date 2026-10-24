# Working on this repository

Instructions for anyone, or anything, contributing here.

This file is named `AGENTS.md` rather than after any particular assistant, for
the same reason the method itself names no tool: a repository that describes
roles should not be organised around one vendor's product. Assistants that look
for an instruction file under a product name will not find one here. That is
deliberate, and this is the file they should read.

---

## Non-negotiable

### No tool signature anywhere

Nothing produced for this repository carries the name of the assistant that
helped produce it. Specifically:

- **No trailer in commit messages.** No `Co-Authored-By` naming an assistant,
  no `Generated-by`, no equivalent.
- **No footer in pull request descriptions.** No "Generated with", no "Created
  by", no badge, no link.
- **No mention inside the documents themselves.**

Commits are authored by the person responsible for the work. That person is
responsible for it whether an assistant helped or not, which is the whole
point of the Decide role.

**Why this matters here more than elsewhere.** This repository argues that a
method should be described in terms of roles rather than products, and that a
human decides. A document making that argument while carrying a vendor's
signature in every commit would undercut itself on both counts.

### No tool, product or vendor named

The method describes **roles**: a design tool, a review assistant, a coding
agent, a human decision-maker. Never a product name, a vendor, a model, or an
editor.

This is not neutrality for its own sake. A method tied to one product expires
when that product does, and cannot be adopted by a team that uses another.

### No employer, client or confidential project

No organisation names. No sector detail precise enough to identify one. No
figure, date or URL that could be traced back to a real engagement.

Examples are invented, and say so. When an example is drawn from practice, the
situation is kept and everything identifying is removed: exact counts, what a
package does, anything that pins the account to one project on one day.

### English only

Simple, precise English, readable by people who do not speak it natively.
Short sentences. No idiom that needs a native ear.

### No em dash

The character U+2014, the long dash, appears nowhere: not in documents, not in
diagrams, not in commit messages. Use a colon, a comma, parentheses, or a full
stop, whichever the sense calls for. Never a hyphen as a substitute.

Check before committing:

```bash
grep -rn "$(printf '\u2014')" --exclude-dir=.git .
```

---

## Diagrams

All diagrams are Mermaid, so that they render natively without an image
pipeline.

**No markdown formatting inside node labels.** Bold or italic inside a plain
quoted label renders as literal asterisks on the hosting platform. If a label
needs emphasis, restructure the label.

Keep diagrams readable at the width of a documentation page. A diagram that
needs horizontal scrolling explains nothing.

---

## Writing

**Every document follows the same shape**: what it is, why, how to do it, how
it fails, how to check it. The last two are what make a document usable, and
the ones most often skipped.

**Two audiences, one text.** Project leads read the principles, the roles, the
cycle and the design document without needing to code. Each technical section
opens with what the failure costs the project, in plain language, before the
detail.

**Anti-patterns follow a fixed form**: the symptom, why review misses it, what
catches it, the rule. The second of these is the valuable one, and the one
that takes thought.

**Claims are sourced or removed.** No figure, comparison or capability
statement that nobody can point to the origin of. This applies to the method's
own claims most of all.

---

## Before opening a pull request

- [ ] No tool signature in the commits or in the description
- [ ] No product, vendor or editor named
- [ ] No organisation or client identifiable
- [ ] No em dash
- [ ] Every internal link resolves, including anchors
- [ ] Mermaid labels contain no markdown
- [ ] Any new document follows the five-part shape
