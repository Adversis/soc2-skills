# SOC 2 Skills

Two [Agent Skills](https://docs.claude.com/en/docs/claude-code/skills) for running lean SOC 2 engagements, from [Adversis](https://adversis.io). They encode a working method, not a template dump: **lean, honest, low-liability** — policies name controls, procedures name products, don't over-document, never backdate.

Scope: Security (Common Criteria), Type I first with a Type II runway. Calibrated for small, cloud-native startups.

## The two skills

- **`soc2-readiness`** — *author the program.* Scope (TSC / Type I vs II / as-of date / system boundary), replace GRC-templated policy sludge with a lean principles-based set, stand up procedures and evidence infrastructure, make vendor/subservice calls, and QA the auditor's draft. Ships an ACME'd **starter pack** (`soc2-readiness/templates/starter-pack/`): ~17 policies, procedures, registers, and a system description, with a `REPLACE.md` token manifest — copy it and fill it in.
- **`soc2-audit-prep`** — *finalize and hand off.* Turn the authored program into the submission the auditor actually tests: the real control matrix (CC1–CC9), the system-description verification walk, the evidence repository, the seven go-live gates, and hand-off hygiene. Ships a `control-matrix.csv` starter.

`soc2-readiness` authors the program; `soc2-audit-prep` submits it.

```
soc2-readiness/    SKILL.md · references/ · templates/{evidence-index.md, starter-pack/}
soc2-audit-prep/   SKILL.md · references/ · templates/control-matrix.csv
```

## Install

Copy (or symlink) the two folders into your Agent skills directory:

```sh
cp -R soc2-readiness soc2-audit-prep ~/.claude/skills/
```

They trigger on SOC 2 tasks automatically, or you can invoke them by name. Each skill's `SKILL.md` is the entry point; `references/` holds the depth; `templates/` holds the copy-and-fill artifacts.

## Use

1. `soc2-readiness` → scope and author. Copy `templates/starter-pack/`, then work through its `REPLACE.md`: find/replace `ACME`, fill the `{{TOKENS}}`, confirm the calibrated defaults.
2. `soc2-audit-prep` → build the control matrix, walk the system description against real evidence, run the go-live gates, and hand off to the auditor.

## Caveats

- **Starters, not turnkey.** The templates are a calibrated baseline to *tailor*. The control matrix especially is a CC1–CC9 seed to complete, not a sufficient control set.
- **Security-only Type I focus.** Verify against the current AICPA Trust Services Criteria and your own auditor's expectations.
- **Not legal or audit advice.**
- References a companion `vendor-risk-analyzer` skill (the consumer side — reading someone else's report), which is not included here.

## Attribution & license

The policy base is adapted from [Tailscale's open-source security policies](https://github.com/tailscale/security-policies) (MIT). MIT licensed — see [`LICENSE`](LICENSE).
