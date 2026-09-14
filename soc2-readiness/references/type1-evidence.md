# Type 1 evidence discipline

## What a Type 1 opines on

A Type 1 is a CPA attesting, as of a **single date**, to two things:

- **Designed** — the control, if it runs, would actually achieve the criterion (it's the *right* control).
- **Implemented** — it genuinely exists and is in operation as of the date: configured, turned on, in use
  — not just a paragraph in a policy.

No operating history is tested. A control stood up the week before the as-of date is perfectly valid for a
Type 1 — it just can't have a track record yet.

## The as-of date is a hard line

The auditor is signing their name to: "As of [date], these controls were suitably designed and
implemented." So:

- **Every artifact must be dated on or before the as-of date.** A control turned on after the date
  describes a different company than the one the report covers — full stop.
- **A written policy alone is not "implemented."** Attestation means independent verification; the CPA
  needs a corroborating artifact — a config screenshot, a ticket, a completed checklist — showing the
  control actually exists and ran. That's why they ask for "one example": confirming it's real, not
  aspirational.

## Periodic controls: "designed and implemented; first cycle not yet due"

For annual/periodic controls (access review, performance review, BC/DR tabletop, pen test), "implemented"
means **the mechanism exists and is scheduled** — you do **not** need a completed cycle if the cycle isn't
due yet. A defensible Type I posture — subject to the auditor's agreement — is:

> "Designed and implemented; first [annual] cycle scheduled for [date]." (Attach the scheduling evidence —
> a calendar entry, a GRC task with a due date.)

Framing matters:

- Use **"newly implemented; cycles not yet due,"** not "young company" — a company with a year of
  operating history but a *newly stood-up SOC 2 program* is telling the truth with the first framing and
  contradicting its own asset/hire records with the second.
- Never let it read as **"no events during the review period"** — that's Type 2 language and makes a
  new program look like a year in which controls never fired. (See `threat-lens.md` on catching this in
  the auditor's own draft.)

### The five-part shape that lands (use every time)

The difference between a "not yet due" assertion the auditor accepts and one they kick back is entirely
whether it has all five parts. A loose "we do this annually" with no citation, cadence, or date gets
returned; this doesn't:

1. **Documented** — name the policy and section that establishes the control.
2. **Trigger defined** — what causes it to run (termination, role change, incident, an annual date).
3. **Evidence path defined** — where the artifact will live when the event occurs.
4. **Attestation** — a statement that no qualifying event occurred between the control's adoption and the as-of date.
5. **Cadence + planned date** — the schedule and a *specific* execution date, not "later this year."

This shape applied across roughly eight controls on a real Type 1 (access review, backup-restore test, IR
tabletop, BC/DR walkthrough, lessons-learned, physical-access review, board minutes, and more). It is the
reusable template for the entire "designed and implemented, not yet due" posture.

## "No instances" — only where genuinely true

Some controls have no evidence because nothing triggered them yet: no terminations → no access
revocations; no internal transfers → no access-change approvals; no device disposals; no incidents. These
are legitimately "no instances," but the auditor will want **proof of non-occurrence**, not just your word
(see `auditor-qa-patterns.md`). Only claim "no instances" where it's actually true — a company that has
onboarded people *does* have onboarding approvals and *must* produce the real records.

## The one genuinely dangerous move: backdating

Do not backdate or fabricate. A backdated artifact doesn't just misstate the company's records — it makes
the **auditor's signed opinion false, on their professional license.** "Not yet due" is true and costs
nothing; a fabricated date risks the whole report. This is the brightest line in the engagement. Flag it,
never do it — including not presenting template "Example:" rows in a register as if they were real events.

## Type 1 as a dress rehearsal — don't over-document

Whatever controls you claim in the Type 1 get tested for **operating effectiveness** in the Type 2. Every
extra control and every tighter-than-real commitment you add now becomes an evidentiary burden — and a
potential exception — in the Type 2 window. Claim what's real and lean; that's what survives the movie.
