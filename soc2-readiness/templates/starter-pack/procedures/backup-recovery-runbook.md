# Backup & Recovery Runbook

**Owner:** {{SECURITY_OWNER}}

This runbook is the recovery procedure and the control narrative. Evidence lives in the systems themselves (see Controls & Evidence) — there is no hand-kept log to maintain.

Scope: hosted infrastructure serving customers.

---

## Environment

Fill once; the procedures below reference these.

| Item | Value |
|------|-------|
| {{CLOUD_PROVIDER}} project | `<project-id>` |
| {{DATA_STORE}} org / project owner | {{SECURITY_OWNER}} |
| {{DATA_STORE}} cluster | `<cluster-name>` |
| {{DATA_STORE}} backup config | confirm in {{DATA_STORE}} → Backup → Configure |
| {{CLOUD_PROVIDER}} artifact registry | `<project/repo>` |
| {{SECRETS_MANAGER}} project | `<project-id>` |
| IaC repo | `<{{VCS}}-repo>` |
| Terraform state bucket | `<{{CLOUD_PROVIDER}}-bucket>` (Object Versioning on) |
| {{DATA_STORE}} service | `<service-name>` |
| Alert channel | `<PagerDuty/{{CHAT_PLATFORM}} channel>` |

---

## Recovery Objectives

Policy commitment is RTO 24h / RPO 24h overall (BCP/DR Policy §2). Internal per-system operational targets are tighter to maintain headroom.

---

## Triage: What Just Failed?

| Symptom | First check | First action |
|---------|------------|--------------|
| Health checks failing across the board | {{CLOUD_PROVIDER}} pod logs, recent deploys | Roll back to previous deployment |
| 500s on one endpoint after a deploy | {{CLOUD_PROVIDER}} pod logs | Roll back to previous deployment |
| DB errors / timeouts | {{DATA_STORE}} cluster metrics, alerts | If cluster healthy → suspect app; if unhealthy → escalate to {{DATA_STORE}} support |
| Suspected data corruption | Identify the time window | PITR restore to a new cluster, validate, then plan cutover |
| Secret rotated wrong | {{SECRETS_MANAGER}} version history | Disable the bad version |

---

## {{DATA_STORE}} (Operational Database)

Always restore to a new cluster. Never restore over production.

1. {{DATA_STORE}} console → cluster → Backup → Restore
2. Target: new cluster, same region and provider as the snapshot
3. Pick the PITR point (Date & Time or Oplog Timestamp)
4. Wait for completion
5. Validate before any cutover ({{DATA_STORE}} client: confirm document counts match expected magnitude; recent docs present; indexes present)
6. Real incident: update the database connection-string secret and redeploy (see Secrets, then {{CLOUD_PROVIDER}})
7. Test: tear down the cluster after validation

For data corruption: find when the bad writes started — correlate with deploy history or app logs — restore to just before that timestamp. The restore is mechanical; picking the point is the judgment call.

---

## {{CLOUD_PROVIDER}} (Application Layer)

Stateless — recovery is rollback or redeploy.

Roll back a bad deploy:
```bash
kubectl rollout history deployment/<service> -n <namespace>
kubectl rollout undo deployment/<service> -n <namespace>
# Or roll back to a specific revision:
kubectl rollout undo deployment/<service> -n <namespace> --to-revision=<N>
```

Redeploy from a known-good image:
```bash
kubectl set image deployment/<service> <container>=<registry>/<image>:<tag> -n <namespace>
# Or re-apply manifest from the IaC repo:
kubectl apply -f <manifest>.yaml -n <namespace>
```

Validate: `kubectl rollout status deployment/<service>`; health endpoint returns 200.

---

## Secrets ({{SECRETS_MANAGER}})

Roll back a bad secret:
```bash
gcloud secrets versions list <secret>
gcloud secrets versions disable <bad-version> --secret <secret>
```

Apps reading `latest` pick up the previous enabled version on next read. Apps that cached the value at startup need a redeploy. Apps pinned to a specific version number were never affected.

Rotation on suspected compromise: generate a new value at the source, add as a new version, redeploy consumers, then disable the old version.

---

## Infrastructure (Terraform / IaC)

Rebuild from code:
```bash
git clone <repo> && cd <infra-dir>
terraform init
terraform plan      # verify before applying
terraform apply
```

Recover lost state from a prior generation:
```bash
gcloud storage ls --all-versions gs://<state-bucket>/<state-file>
gcloud storage cp gs://<state-bucket>/<state-file>#<generation> ./<state-file>
gcloud storage cp ./<state-file> gs://<state-bucket>/<state-file>
```

---

## {{DATA_STORE}} (Observability)

Non-blocking — restore after service is live. {{DATA_STORE}} console → service → Backups → select snapshot → restore to a new service.

---

## Source Code

{{VCS}} is primary (replicated across {{VCS}} infrastructure). Recover by re-cloning. If {{VCS}} is unavailable, the most recent developer-laptop clones are the fallback.

---

## Full-Loss Recovery (empty project, worst case)

{{SECURITY_OWNER}} declares the disaster and approves cutover. Customer comms follow the Incident Response Policy.

Bootstrap if needed: create the {{CLOUD_PROVIDER}} project and attach billing using the break-glass credential; recreate the Terraform state bucket with Object Versioning; then proceed below.

Order:
1. Infrastructure — `terraform apply` (networking, {{CLOUD_PROVIDER}}, IAM)
2. Secrets — recreate / restore {{SECRETS_MANAGER}} versions (everything except the database connection string)
3. {{DATA_STORE}} — PITR restore to a new cluster
4. Database connection string — update with the new cluster's string
5. Application — redeploy to {{CLOUD_PROVIDER}} from the last good image
6. {{DATA_STORE}} — restore observability last

---

## Controls & Evidence

| Control claim | System of record |
|---|---|
| Restores work | `dr-restore-test` run history + {{DATA_STORE}} → Backup → Restore history |
| Backups run | {{DATA_STORE}} snapshot list; {{DATA_STORE}} Backups |
| Backup failures captured | {{DATA_STORE}} backup-failure alert → `<channel>` |
| Deploys & rollbacks | {{CLOUD_PROVIDER}} rollout history + CI run history |
| Secret changes | {{SECRETS_MANAGER}} version history + {{CLOUD_PROVIDER}} audit logs |
| IaC changes | Git commit / PR history |
| Who can/did restore | IAM policy + {{CLOUD_PROVIDER}} audit logs |

Encryption at rest: {{DATA_STORE}}, {{CLOUD_PROVIDER}} storage, and {{SECRETS_MANAGER}} all use provider-managed encryption by default.

Restore and backup-config access is privileged, limited per the Access Control Policy.
