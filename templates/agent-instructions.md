# Agent instructions: template

> Copy this to the file your agent reads at the start of every session.
> Replace everything in angle brackets. Delete the guidance in quotes.
>
> **Fill this before the first line of code.** Half the failures in the
> anti-pattern catalogue are prevented by this file.

---

## Mission

<One paragraph: what this project is, and who it is for. Not the architecture.
The intention.>

## Stack

<The technologies, with versions where a version matters. Keep it short: the
agent can read the files. This section exists so it does not guess.>

---

## Non-negotiable constraints

> These are the rules whose cost you cannot absorb. Write the reason next to
> each one: a rule without its reason gets worked around the first time it is
> inconvenient.

- **<Constraint>.** <Why. What happens if it is broken.>
- **<Constraint>.** <Why.>

### Examples of the kind of thing that belongs here

- **Never move, rename, edit or delete `<the secrets file>`.** It holds
  credentials and belongs to one person. To test without it, use a separate
  working copy rather than touching the file.
- **Data changes are never applied directly.** They are written as reviewable
  files, guarded on the value they expect to replace, and tested in three
  directions before anyone applies them.
- **A privileged function revokes the default execute permission.** Creating
  one is two steps, not one.
- **Nothing is pushed without approval.** No action on any shared
  environment (database, hosting, domain) without being asked for by name.

---

## Vocabulary

> The words this product uses, and the words it never uses. This prevents a
> slow drift that is very expensive to reverse once it is in forty files.

| Always write | Never write | Why |
|---|---|---|
| <term> | <term> | <reason> |

---

## Checks before delivery

> The commands, and what counts as passing. An agent that does not know how to
> verify will report success by default.

```
<build command>
<lint command>
<type check command>
<test command>
```

Also required before saying a batch is done:

- <Any manual check: a page opened, a measurement taken>
- <Anything that must be rendered rather than compiled>

---

## What is never touched

- `<file or directory>`: <why>
- `<environment>`: <why>

---

## How to behave

> These three sentences are worth more than most of the rest.

- **If something in a specification is wrong or impossible, stop and say so.**
  Do not work around it silently.
- **Report what you did not do**, and why, as plainly as what you did.
- **When you are unsure between two defensible options, ask.** Do not choose
  and justify afterwards.

---

## End of session

- Write a summary in `<location>`
- Record structural decisions in `<decision log>`
- Update any documentation this batch made wrong
