# Manual Deployment Path Confirmation Report

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Manual UI deployment-path checks
- canonical: false

## 2. Purpose

This document is a draft manual deployment path confirmation report for JoinFolk Supabase/backend source-path evidence.

It is not a canonical source-of-truth decision, not a cleanup plan, not a patch plan, and not a migration plan. It does not authorize modifying any Supabase tree.

This report separates confirmed manual UI facts from interpretation. It also separates local source classification from production SQL evidence. Production SQL evidence remains valid separately from local deployment-path ambiguity.

## 3. Confirmation Scope

This report covers supplied manual evidence from:

- GitHub Actions UI for `MustafaYIGEN/joinfolk-platform`.
- GitHub Actions UI for `MustafaYIGEN/joinfolk-web`.
- Local read-only workflow path checks supplied by the operator.
- GitHub repository source browsing for `MustafaYIGEN/joinfolk-platform`.
- GitHub repository settings/environments for `joinfolk-platform`.
- Supabase Dashboard Edge Functions UI.

This report does not inspect or modify application code files directly, connect to production, run Supabase CLI, run migrations, run builds, or run tests.

## 4. Manual Evidence Summary

Confirmed manual/UI facts supplied by the operator:

- GitHub Actions pages for `MustafaYIGEN/joinfolk-platform` and `MustafaYIGEN/joinfolk-web` showed “Get started with GitHub Actions”.
- No configured workflow list was visible in either repository.
- Local workflow path checks returned no workflow directory or workflow files for:
  - `C:\dev\hostos\.github\workflows`
  - `C:\dev\joinfolk-web\.github\workflows`
  - `C:\dev\hostos\apps\mobile\.github\workflows`
- GitHub `MustafaYIGEN/joinfolk-platform` contains a top-level `supabase` directory.
- The GitHub `joinfolk-platform/supabase` tree contains `migrations` and `functions`.
- Function names visible or mentioned under that tree include `push-dispatch`, `send-test-push`, `transactional-email`, and possibly `snapshot` from local source audit.
- GitHub Environments for `joinfolk-platform` showed `Preview` and `Production`.
- Supabase Dashboard Edge Functions page showed “Deploy your first Edge Function” and options such as Via Editor, AI Assistant, and Via CLI.
- No deployed Edge Function list was visible in the Supabase Dashboard project.

Interpretation:

- GitHub Actions workflow evidence does not currently confirm a canonical Supabase deployment path.
- `C:\dev\hostos\supabase` is strengthened as the primary plausible backend source because GitHub `joinfolk-platform` has matching `supabase/migrations` and `supabase/functions`.
- GitHub Environments `Preview` and `Production` exist, but they do not prove Supabase migration deployment or Edge Function deployment source by themselves.
- Local Edge Function folders exist, but deployed Supabase Edge Functions were not visible in the Dashboard UI.

## 5. GitHub Actions Findings

Evidence:

- `MustafaYIGEN/joinfolk-platform` Actions UI showed “Get started with GitHub Actions”.
- `MustafaYIGEN/joinfolk-web` Actions UI showed “Get started with GitHub Actions”.
- No configured workflow list was visible in either repository.
- Supplied local checks found no `.github\workflows` directory/results under:
  - `C:\dev\hostos`
  - `C:\dev\joinfolk-web`
  - `C:\dev\hostos\apps\mobile`

Interpretation:

- Supabase migrations/functions do not appear to be deployed through GitHub Actions in these repositories.
- No GitHub workflow evidence currently confirms a canonical Supabase deployment path.
- GitHub Actions deployment path: Not found / no workflow evidence.

## 6. GitHub Repository Source Findings

Evidence:

- Operator viewed `MustafaYIGEN/joinfolk-platform` on GitHub.
- The repository contains a top-level `supabase` directory.
- Inside that `supabase` directory, the operator saw `migrations` and `functions`.
- Function names visible or mentioned include `push-dispatch`, `send-test-push`, `transactional-email`, and possibly `snapshot`.
- This matches the local path `C:\dev\hostos\supabase`.

Interpretation:

- `hostos\supabase` strongly matches the GitHub `joinfolk-platform/supabase` source tree.
- This strengthens `C:\dev\hostos\supabase` as the primary plausible backend source.
- This still does not prove the production migration deployment path or Edge Function deployment source.

## 7. GitHub Environments / Deployments Findings

Evidence:

- GitHub repository settings/environments for `joinfolk-platform` showed:
  - `Preview`
  - `Production`
- Operator reported that opening the deployment/environment area showed mostly environment-variable UI and no special deployment-source proof.

Interpretation:

- GitHub Environments `Preview` and `Production` exist.
- These environment entries do not prove Supabase migration deployment source.
- These environment entries do not prove Supabase Edge Function deployment source.
- They may relate to generic GitHub deployment/environment metadata.

## 8. Supabase Dashboard Edge Functions Findings

Evidence:

- Supabase Dashboard Edge Functions page showed “Deploy your first Edge Function”.
- The page presented options such as Via Editor, AI Assistant, and Via CLI.
- No deployed Edge Function list was visible.
- Clicking relevant tabs did not show any function list, according to the operator.

Interpretation:

- No deployed Edge Functions are currently visible in the Supabase Dashboard project.
- Local Edge Function folders exist, but they do not appear production-active from the Dashboard UI.
- `push-dispatch`, `send-test-push`, `snapshot`, `transactional-email`, `notify-transfer-invite`, and `notify-transfer-review` should be treated as local/future-deployment or non-current-production-active unless further evidence proves deployment.

## 9. Current Deployment Path Classification

| Area | Current classification | Confidence | Notes |
| --- | --- | --- | --- |
| GitHub Actions deployment path | Not found / no workflow evidence | Medium | Actions UI and local workflow path checks did not show workflows. |
| Supabase Edge Functions production status | No deployed Edge Functions visible in Dashboard | Medium | Dashboard UI showed first-deploy state. |
| GitHub Environments | Preview/Production exist, but not deployment-source proof | Medium | Environment entries alone do not prove Supabase migration/function deployment. |
| `C:\dev\hostos\supabase` | Strongest primary plausible backend source; not canonical | Medium-High | GitHub `joinfolk-platform/supabase` matches local tree shape. |
| Canonical migration deployment path | Unknown / Needs verification | Low | No workflow or deployment-source proof was found. |

Production SQL evidence remains separate and still valid for production-state claims. Local deployment-path ambiguity does not invalidate production SQL outputs already gathered by the operator.

## 10. Impact on Existing Reports and Patch Plans

| Document | Impact | Updated note |
| --- | --- | --- |
| `SupabaseBackendGapReport` | Medium for local-code findings, especially `push-dispatch` | `push-dispatch` appears local but was not visible as a deployed Edge Function in the Supabase Dashboard. |
| `SupabaseProductionParityVerificationReport` | Low for production SQL; Medium for local-code carryover | Edge Function production-active status appears negative from Dashboard UI; production SQL evidence remains valid separately. |
| `SupabaseFocusedBackendFollowUpReport` | Low to Medium | Focused SQL findings remain mostly unaffected. Dormant/local-code Edge Function language remains source-path dependent. |
| `SupabaseProofCheckinRpcHardeningPlan` | Medium | Implementation target path is still not confirmed; migration/patch implementation remains blocked. |

The proof check-in patch plan cannot be implementation-ready until the target Supabase tree is confirmed.

## 11. Updated Working Rule

- Treat `C:\dev\hostos\supabase` as the strongest primary plausible backend source for documentation and local-code audits.
- Do not treat `C:\dev\hostos\supabase` as canonical deployment source yet.
- Treat local Edge Functions as not currently production-active unless Supabase Dashboard or deployment evidence proves otherwise.
- Treat production SQL evidence as stronger than local source assumptions for production-state claims.
- Block migration/patch implementation until canonical migration source path is confirmed.
- Do not modify any Supabase tree until source-of-truth is confirmed.

## 12. Remaining Unknowns

- Which path is used by the real production migration process.
- Whether Supabase migrations are applied manually, through another tool, or through a deployment process not visible in GitHub Actions.
- Whether GitHub Environments `Preview` and `Production` are connected to any Supabase deployment process.
- Whether Edge Functions were intentionally never deployed, removed, deployed to another Supabase project, or managed outside the visible Dashboard project.
- Whether `C:\dev\joinfolk-web\supabase` is draft, stale, dashboard-specific, or active.
- Whether `C:\dev\hostos\apps\mobile\supabase` is mobile-owned backend history, component-specific, deployed elsewhere, or stale.

## 13. Required Next Manual Confirmation

Required manual confirmation before any source-of-truth decision:

- Confirm the actual migration deployment path or process.
- Confirm whether Supabase migrations are applied manually, through a local CLI workflow, through a hosted process, or through another system.
- Confirm whether GitHub Environments are linked to any Supabase deployment mechanism.
- Confirm whether Edge Functions are deployed in another Supabase project or intentionally absent.
- Confirm whether `joinfolk-platform/supabase` is the accepted source tree for future backend work.
- Confirm whether `joinfolk-web\supabase` and mobile `supabase` are active, stale, partial, or component-specific.

## 14. Non-Goals

- This document does not claim final source-of-truth.
- This document does not provide cleanup instructions.
- This document does not provide migration instructions.
- This document does not include SQL.
- This document does not include production commands.
- This document does not authorize modifying any Supabase tree.
- This document does not supersede production SQL evidence.
- This document does not approve any patch plan.

## 15. Open Questions

- What process applies Supabase migrations to production?
- Is `joinfolk-platform/supabase` the accepted source tree for future backend migrations?
- Are Supabase migrations currently applied manually?
- Are GitHub Environments connected to any deploy process, or are they only metadata/config containers?
- Are Edge Functions intentionally absent from production?
- Were Edge Functions deployed to a different Supabase project?
- Should local Edge Function findings be retained only as future-deployment guard concerns?
- Which Supabase source tree should receive any future accepted proof check-in RPC hardening work?

## 16. No-Modification Confirmation

For this manual deployment path confirmation report:

- No application code files were modified. This draft was created from supplied manual UI evidence and handbook sources.
- No production connection was made.
- Supabase CLI was not run.
- No migrations were run or generated.
- No builds were run.
- No tests were run.
- No cleanup plan was created.
- No patch plan was created.
- No Supabase tree modification was authorized.
- No files were staged or committed.
