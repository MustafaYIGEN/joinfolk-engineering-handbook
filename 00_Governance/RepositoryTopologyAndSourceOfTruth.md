# Repository Topology and Source of Truth

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: local workspace structure and committed handbook workflow
- canonical: false
- Governance status: Proposed source-of-truth map
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering governance only; not legal advice

## 2. Purpose

This document defines the repository/workspace topology used by JoinFolk handbook, audits, decisions, patch plans, and implementation planning.

The purpose is to prevent confusion between:

- Handbook governance artifacts.
- Product source repositories.
- Supabase/migration source roots.
- Web/dashboard source roots.
- Backup/non-canonical roots.
- Temporary context files.

This is not implementation.

This does not authorize source changes.

This does not authorize SQL.

This does not authorize migration creation.

This does not authorize production access.

This does not authorize Supabase CLI.

## 3. Repository Alias Map

| Alias | Local workspace name | Canonical role | May be used as audit evidence? | May be modified by handbook tasks? | Notes |
|---|---|---|---|---|---|
| [handbook] | joinfolk-engineering-handbook | Governance / audits / decisions / patch plans / status source of truth | Yes | Yes, documentation only | Canonical handbook repo. |
| [hostos] | hostos | Mobile app + Supabase/migrations/backend source root | Yes, read-only unless explicit implementation task | No during handbook tasks | Product/source evidence root. |
| [joinfolk-web] | joinfolk-web | Web/dashboard source root | Yes, read-only unless explicit implementation task | No during handbook tasks | Product/source evidence root. |
| [joinfolk-web-backups] | joinfolk-web-backups | Backup/non-canonical historical copy | Usually no; only if explicitly approved for comparison | No | Must not override canonical source. |
| [.vscode] | .vscode | Local tooling/editor configuration | No, except tooling context | No | Not product truth. |
| [tmp-context] | tmp_context_files.txt | Temporary context file | No, unless explicitly referenced | No | Non-canonical temporary artifact. |

## 4. Canonical Source Rules

- [handbook] is canonical for governance, audits, decisions, patch plans, status gates, readiness reports, and evidence classification.
- [hostos] is canonical for mobile/Supabase/migration/backend source evidence when local source review is approved.
- [joinfolk-web] is canonical for web/dashboard source evidence when local source review is approved.
- [joinfolk-web-backups] is not canonical unless a separate comparison task explicitly authorizes it.
- Temporary files are never canonical.
- Production remains separate from local source and must not be inferred from local evidence unless explicitly production-verified.

## 5. Evidence Classification Rules

| Evidence label | Meaning | Example use | Claim boundary |
|---|---|---|---|
| Handbook evidence | Evidence from [handbook] docs | decisions, audits, patch plans | Governance truth, not runtime proof. |
| Local source evidence | Evidence from [hostos] or [joinfolk-web] source | call-site or migration review | Local-only; production not verified. |
| Local migration evidence | Evidence from [hostos] migrations | function definition or grant history | Local-only unless production parity verified. |
| Production metadata evidence | Approved read-only sanitized production metadata | exact grants/proconfig/schema state | Requires owner-approved gate. |
| Runtime behavior evidence | Manual/app/dashboard behavior observation | feature works/fails by role/lifecycle/city | Requires test record. |
| Backup evidence | Evidence from backup roots | historical comparison only | Non-canonical unless explicitly approved. |

## 6. Path Reference Convention

Future handbook documents should use aliases:

- [handbook]/...
- [hostos]/...
- [joinfolk-web]/...
- [joinfolk-web-backups]/...

Avoid absolute local paths in committed handbook documents.

Do not include hostnames, full project refs, connection strings, credentials, or secret-bearing paths.

Storage object names and storage object paths must not be committed unless a separate privacy-safe policy explicitly allows it.

## 7. Modification Rules by Root

| Root | Default permission during handbook workflow | Modification allowed only when |
|---|---|---|
| [handbook] | documentation edits allowed | within requested docs/audit/decision/patch/status scope. |
| [hostos] | read-only evidence source | explicit implementation task approved. |
| [joinfolk-web] | read-only evidence source | explicit implementation task approved. |
| [joinfolk-web-backups] | no modification | almost never; only explicit backup maintenance task. |
| [.vscode] | no modification | explicit tooling task. |
| [tmp-context] | no canonical modification | temporary local context only. |

## 8. Relationship to SecurityDefiner Workstream

The SecurityDefiner/function grant workstream uses:

- [handbook] for decisions, patch plans, status gates, and reports.
- [hostos] for local Supabase migration evidence.
- [joinfolk-web] for local web/dashboard call-site evidence.
- Production metadata only after an explicit approval gate.
- Local evidence must be labeled as local-only and not production-verified.

Current chain:

- `09_Decisions/SecurityDefinerAndFunctionGrantHardeningDecision.md`
- `08_PatchPlans/SecurityDefinerAndFunctionGrantHardeningPatchPlan.md`
- `00_Status/SecurityDefinerFunctionGrantHardeningOwnerReviewGate.md`
- `07_Audits/SecurityDefinerFunctionGrantInventoryClassification.md`
- `00_Status/SecurityDefinerFunctionGrantClassificationCompletenessReview.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantMetadataCollectionPlan.md`
- `00_Status/SecurityDefinerFunctionGrantMetadataCollectionApprovalGate.md`
- `07_Audits/SecurityDefinerFunctionGrantCollectedMetadataReport.md`

## 9. Relationship to Feature Runtime Audits

Future feature/runtime completeness audits, such as Memory Wall city-specific behavior, should use:

- [handbook] for expected product/security/lifecycle rules.
- [hostos] and [joinfolk-web] for local implementation/call-site evidence.
- Runtime/manual test records for actual behavior.
- Production metadata only after explicit approval where needed.

Local source evidence can explain likely causes.

Runtime behavior evidence is required to claim a feature works or fails in production.

City-specific behavior must not be inferred from code alone.

## 10. Non-Canonical / Blocked Sources

- Backup folders unless explicitly approved.
- Temporary files.
- Screenshots alone without text/context.
- Private production data.
- Secrets or credentials.
- Storage object paths.
- Payload dumps.
- Unreviewed AI output.
- Unstaged/uncommitted source changes outside requested scope.

## 11. Future Use Requirements

Before future audit or patch work, the assistant/operator should identify:

- Which root is being read.
- Whether the root is canonical for the task.
- Whether evidence is handbook, local source, local migration, production metadata, runtime, or backup evidence.
- Whether the output claims production truth or only local evidence.
- Whether a status gate is required before execution.

## 12. Risk Position

Repository topology confusion can create false implementation assumptions.

Local evidence must not be treated as production proof.

Backup evidence must not override canonical source.

Production claims require explicit production evidence.

This document reduces process risk but does not itself make the platform launch-ready.

## 13. Implementation Authorization Status

Implementation remains not authorized.

No source change, SQL, migration, production mutation, Supabase CLI action, dashboard action, verification query, RPC invocation, metadata collection execution, or deployment action is authorized by this topology document.

A separate owner-approved implementation prompt is required for any app/dashboard/mobile/web/backend/Supabase source modification.

## 14. Explicitly Blocked Claims

- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim security hardened.
- Do not claim repository parity with production.
- Do not claim backups are canonical.
- Do not claim local source evidence proves production behavior.
- Do not claim implementation authorized.

## 15. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- No production mutation was executed.
- Supabase CLI was not run.
- No dashboard action was performed.
- No verification query was executed.
- No RPC/function was invoked.
- No private rows were inspected.
- No storage objects were listed.
- No builds/tests/installs were run.
- No secret or environment variable value was inspected, copied, printed, rotated, or changed.
- No credentials, hostnames, full project refs, service_role keys, database passwords, connection strings, webhook secrets, API keys, environment variable values, or secrets were included.
- No files were staged or committed.
- Only `00_Governance/RepositoryTopologyAndSourceOfTruth.md` was created/modified.
