# Running the engagement: the tactical playbook

The other references cover *what* to write. This one covers *how the engagement actually runs* — where
the schedule risk lives, how to work the platform and the auditor, and the control-strategy calls that
recur. All of it learned in the field, not from the standard.

## Client-side configuration is the critical path — not your writing

Everything the advisor controls moves fast. Everything requiring a client platform change (a CI job, a
GCP setting, endpoint compliance on a laptop) is slow — and it accounts for essentially **all** the
schedule risk. Plan around this from day one.

**The fix is a wording fix.** Vague asks to busy engineers return vague answers on a multi-day lag.
Specific asks return specific answers same-day. Don't ask "what are your CI job names." Ask, per repo:

> Go to Settings → Actions. Give me the exact job name as it appears. Does it run on every repo, or just
> this one? Does it run on every PR to main? Is it a security test or an integration test?

Issue client config requests with **per-repo, per-setting precision**. One correctly-named CI job can
satisfy several platform controls at once — high value per unit of engineer attention, so the precision
pays for itself. Follow up every other day during the config push.

## Calibrate the auditor early — and don't assume the next one

Auditors vary widely in how much they challenge. One may accept "no cycle yet; policy states X on Y
cadence" with no follow-up; another will press. Read yours early, because it changes how much rigor to
**front-load**. But calibration is per-auditor: don't carry one auditor's low bar into the next
engagement, and don't over-engineer for a bar you haven't measured. Pre-stage the Q&A loop either way
(see `auditor-qa-patterns.md`) — "one example," then "prove it didn't happen."

## Declining or arguing a control (compensating-control pattern)

Sometimes the right call is *not* to run a control the auditor expects — if the criterion doesn't strictly
require it and you have defensible alternatives. The pentest / CC7.1 case is the canonical example:

- **The criterion is arguable.** The 2017 TSC treats points of focus as *illustrative*, not mandatory.
  CC4.1 lists penetration testing inside a menu (vuln scans, security assessments, third-party
  assessments). An entity performing *some* structured periodic evaluation satisfies the criterion.
- **Auditors expect a pentest anyway** for pragmatic reasons — it's the single easiest artifact that
  convincingly shows the evaluation happened.
- **The price of arguing it is non-negotiable.** If you decline the obvious artifact, you must bring the
  rigor: a defined methodology, scope matching the SOC 2 system boundary, a schedule, a written report,
  and tracked remediation, delivered by your compensating controls (e.g. SAST + dependency scanning +
  scheduled external scans against staging + merge review). Argue the criterion *without* that rigor and
  you take a finding you could have avoided — the worst outcome, because you paid nothing and got nothing.
- **It's "not now," not "not ever."** Buyers will ask for a pentest regardless of what the auditor
  accepted; a scoped test (API + tenant-isolation boundary) belongs on the post-Type-1 list.

General form: a control you decline is a **documented risk-acceptance with compensating controls**, not a
gap you hope no one notices.

## Right-sizing vendor management (CC9.2)

Vendor work is where a startup engagement quietly over-scopes. What CC9.2 actually requires is modest: a
vendor inventory, risk-based tiering, and evidence you reviewed your critical vendors' SOC 2 reports (clean
opinion, relevant coverage, in-period) at least annually, plus a DPA on file for anyone processing customer
data. That is the bar. Two ways it balloons past it — resist both:

- **Deep-reviewing every vendor.** Tier first. Deep-review the handful that touch customer data or sit in
  the service-delivery path; inventory-only the rest. A 30-SaaS startup needs 4–5 real vendor reviews, not
  30. (An ~8-person shop with ~9 material vendors reviewing all 9 is fine — scale by data exposure, not
  vendor count.)
- **Tracking every subservice org's CUECs to closure.** Each vendor's SOC 2 lists Complementary User Entity
  Controls it expects you to run ("enforce MFA," "require 2FA," "pin CI actions"). Reading them and acting on
  the material ones is good hygiene — and most already map onto your own CC6 controls, so you are not
  uncovered. But building and closing a full CUEC obligation tracker (often 100+ rows) is above the Type I
  bar and usually ends as a spreadsheet of "Pending" rows. Do it only when the client will work the list, a
  customer contractually requires it, or regulated data is in play. The starter pack ships one as *optional
  depth*, clearly labeled — don't let it set the default scope.

The tell either way: an artifact that won't be maintained or acted on is adding audit surface, not reducing
risk. Vendor management is the same lean-vs-comprehensive call as everything else in this skill.

## GRC platform scoping (Secureframe specifics)

Security-only scope doesn't cleanly deselect the other TSCs — you inherit a tail of Confidentiality and
Availability controls, tests, and questions that don't apply. Budget ~20 minutes to handle it, and
understand the three tools:

- **Override** — at the *control* level. Flips the control to Healthy with your justification in the audit
  record. Does **not** silence the tests underneath it.
- **Disable test** — for a test that asks for evidence of an event that didn't occur or doesn't apply to
  your environment.
- **Mark N/A** — same effect as disable on the test side; the label varies by build.

Also remove out-of-scope policies (Processing Integrity, Privacy) from the package entirely rather than
leaving them present-but-unmapped.

**The accounting that matters: auditors evaluate *controls*, not your dashboard.** Test failures under an
overridden control don't appear in the report. Your dashboard will still show red — a red dashboard is not
a finding. Fix what integrations can pass, upload evidence to manual tests, disable what's irrelevant,
override the rest, and **stop optimizing the number.**

## Rightsizing cloud IAM vs. accepting the grant (empirical, not rhetorical)

When the auditor flags an over-privileged service account, both "rightsize" and "accept" are valid closes.
Which is correct is determined **empirically**, not by argument:

- Run the cloud provider's IAM recommender and policy simulator against a policy with the suspect role
  removed. If nothing breaks, remove it. If something breaks, justify the grant or replace it with the
  specific permissions that broke (often a small custom role).
- Grant impersonation/user roles at the **resource** level, not the project level (project-level lets the
  account act on *every* account in the project).
- If you keep the broad role, write the justification honestly as **risk acceptance** ("we chose not to
  maintain a custom role for a low-risk CI account"), not as technical necessity — necessity is a claim an
  auditor can disprove.

## Scheduling: slip early, not late

Auditors need roughly ~10 days of lead time to enter their queue, so a date slip is cheap if you make it
early and expensive if you make it late. If the open items are client-side config (they usually are) and
your advisor-controlled work is on track, say so early and move the date rather than present a
half-configured instance. During the final push, a daily-to-every-other-day follow-up cadence is right.

## Working environment

A single directory holding all documentation, evidence screenshots, auditor requests, and the SOC 2
requirements — with an agent operating over it — makes the highest-volume task (justification writing)
fast and keeps the schedule estimate honest. Worth exploring next time: the GRC platform's API (evidence
status and test state are the obvious targets; the manual dashboard walk is the most tedious recurring
task).
