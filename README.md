# SOC 2 Skills

Three [Agent Skills](https://docs.claude.com/en/docs/claude-code/skills) for running lean SOC 2 engagements, from [Adversis](https://adversis.io). They encode a working method, not a template dump: **lean, honest, low-liability** — policies name controls, procedures name products, don't over-document, never backdate.

Scope: Security (Common Criteria), Type I first with a Type II runway. Calibrated for small, cloud-native startups.

## The three skills

- **`soc2-readiness`** — *author the program.* Scope (TSC / Type I vs II / as-of date / system boundary), replace GRC-templated policy sludge with a lean principles-based set, stand up procedures and evidence infrastructure, make vendor/subservice calls, and QA the auditor's draft. Ships an ACME'd **starter pack** (`soc2-readiness/templates/starter-pack/`): ~17 policies, procedures, registers, and a system description, with a `REPLACE.md` token manifest — copy it and fill it in.
- **`soc2-audit-prep`** — *finalize and hand off.* Turn the authored program into the submission the auditor actually tests: the real control matrix (CC1–CC9), the system-description verification walk, the evidence repository, the seven go-live gates, and hand-off hygiene. Ships a `control-matrix.csv` starter.
- **`vendor-risk-analyzer`** — *the consumer side.* Evaluate someone else's SOC 2 (plus MSA/DPA/BAA) with realistic, adversary-informed threat modeling — the mirror of the two above.

`soc2-readiness` authors the program, `soc2-audit-prep` submits it, and `vendor-risk-analyzer` evaluates the other side's.

```
soc2-readiness/         SKILL.md · references/ · templates/{evidence-index.md, starter-pack/}
soc2-audit-prep/        SKILL.md · references/ · templates/control-matrix.csv
vendor-risk-analyzer/   SKILL.md · references/threat-model-index.md
```

## Install

Copy (or symlink) the folders into your Agent skills directory:

```sh
cp -R soc2-readiness soc2-audit-prep vendor-risk-analyzer ~/.claude/skills/
```

They trigger on SOC 2 tasks automatically, or you can invoke them by name. Each skill's `SKILL.md` is the entry point; `references/` holds the depth; `templates/` holds the copy-and-fill artifacts.

## Use

1. `soc2-readiness` → scope and author. Copy `templates/starter-pack/`, then work through its `REPLACE.md`: find/replace `ACME`, fill the `{{TOKENS}}`, confirm the calibrated defaults.
2. `soc2-audit-prep` → build the control matrix, walk the system description against real evidence, run the go-live gates, and hand off to the auditor.
3. `vendor-risk-analyzer` → point it at a vendor's SOC 2 (and any MSA/DPA/BAA) to get a threat-modeled risk assessment for an integration decision.

## Where this stops

Starters and structure, not a finished audit. These skills don't:

- test or verify your actual infrastructure or configuration — the audit-prep verification walk exists precisely because you must confirm each claim against real evidence;
- make the scoping call for regulated data — they're calibrated for a startup's Security-only Type I; HIPAA, PCI-DSS, FedRAMP, or heavy privacy exposure need specialist judgment;
- replace an AICPA-licensed CPA firm or its judgment, which governs your actual report;
- remediate — they identify gaps and structure evidence; they don't fix your controls, configs, or code.

The templates are a calibrated baseline to *tailor* (the control matrix especially is a CC1–CC9 seed to complete, not a sufficient control set). Not legal or audit advice — verify against the current AICPA Trust Services Criteria and your auditor.

## Attribution & license

MIT — see [`LICENSE`](LICENSE). The starter-pack templates under `soc2-readiness/templates/starter-pack/` are CC0-1.0 (public domain — use them in your own program, no attribution needed); see [`NOTICE`](NOTICE). The policy base is derived from [Tailscale's security-policies](https://github.com/tailscale/security-policies) (CC0).
