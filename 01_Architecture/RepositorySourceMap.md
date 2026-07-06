# Repository Source Map

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Local read-only source-map audit
- canonical: false

## 2. Purpose

This document is a draft repository/directory source map for JoinFolk local development paths.

It is not a cleanup plan, not a patch plan, not a migration plan, and not a source-of-truth decision unless explicitly supported by evidence. It separates confirmed local facts from unresolved source-of-truth questions. Production SQL evidence remains separate from local source-map evidence.

## 3. Confirmed Repository Map

Observed top-level `C:\dev` entries:

- `hostos`
- `joinfolk-engineering-handbook`
- `joinfolk-web`
- `joinfolk-web-backups`
- `.vscode`

Repository observations:

| Path | Exists | Git repo root | Actual git toplevel | Branch | Remote origin | Worktree status | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `C:\dev\hostos` | Yes | Yes | `C:/dev/hostos` | `refactor/joinfolk-stabilization-p0` | `MustafaYIGEN/joinfolk-platform.git` | Clean | Platform monorepo-looking root. Latest commit: `f353051f feat(commerce): support standing section tickets`. |
| `C:\dev\hostos\supabase` | Yes | No | `C:/dev/hostos` | Same as `hostos` | Same as `hostos` | Clean from parent status | Subdirectory inside `hostos`. |
| `C:\dev\hostos\apps\mobile` | Yes | Yes | `C:/dev/hostos/apps/mobile` | `release/ios-v17-media-performance` | `MustafaYIGEN/joinfolk-web.git` | Modified mobile venue buyer UI files according to audit | Nested git repo inside `hostos`; major source-map ambiguity. Latest commit: `eed9bf5 fix(mobile): stabilize venue buyer geometry rendering`. |
| `C:\dev\joinfolk-web` | Yes | Yes | `C:/dev/joinfolk-web` | `refactor/joinfolk-stabilization-p0` | `MustafaYIGEN/joinfolk-web.git` | Modified dashboard files; untracked dashboard simulator and Supabase migration | Web repo root. Latest commit: `3887381 fix(dashboard): stabilize shape split preview geometry`. |
| `C:\dev\joinfolk-web\dashboard` | Yes | No | `C:/dev/joinfolk-web` | Same as `joinfolk-web` | Same as `joinfolk-web` | Modified dashboard files visible relative to subdir | Dashboard app subdirectory, not separate repo. |
| `C:\dev\joinfolk-engineering-handbook` | Yes | Yes | `C:/dev/joinfolk-engineering-handbook` | `master` | None observed | Added `08_PatchPlans/SupabaseProofCheckinRpcHardeningPlan.md` | Handbook repo. Latest commit: `cfb3ec9 docs(audits): add supabase focused backend follow-up draft`. |

## 4. Component Source Map

| Component | Expected path | Evidence | Confidence | Notes |
| --- | --- | --- | --- | --- |
| Handbook | `C:\dev\joinfolk-engineering-handbook` | Git root; audit files under `07_Audits`; draft plan under `08_PatchPlans` | High | Active handbook repo. |
| Supabase/backend | `C:\dev\hostos\supabase` | Exists; 186 migrations; functions include `push-dispatch`, `send-test-push`, `snapshot`, `transactional-email` | Medium | Plausible backend source, but competing Supabase directories exist. No `config.toml` present. |
| Mobile app | `C:\dev\hostos\apps\mobile` | Nested git root; `package.json` name `mobile`; Expo `app.json` name `JoinFolk`, slug `mobile`; `eas.json` present | Medium | Plausible mobile app, but nested repo under `hostos` with `joinfolk-web` remote is ambiguous. |
| Web SPA | `C:\dev\joinfolk-web` | Git root; `package.json` name `joinfolk-web`; scripts include dev/build/preview; `app.json` oddly says Expo name/slug `mobile` | Medium | Plausible web repo, but contains duplicated web package and Supabase directory. |
| Dashboard | `C:\dev\joinfolk-web\dashboard` | `package.json` name `joinfolk-dashboard`; Vite config present; modified dashboard files in status | High | Plausible dashboard app, not its own git repo. |
| Supabase functions | Expected under backend | `hostos\supabase\functions` has 4 functions; `joinfolk-web\supabase` has no functions; mobile nested Supabase has `notify-transfer-invite`, `notify-transfer-review` | Medium | Function source-of-truth is split/ambiguous. |
| Supabase migrations | Expected under backend | `hostos\supabase\migrations` has 186 files; `joinfolk-web\supabase\migrations` has 54 files; mobile nested Supabase has 103 files | Medium | Multiple migration histories with zero filename overlap between `hostos` and `joinfolk-web`. |

Known expected paths:

- Handbook: `C:\dev\joinfolk-engineering-handbook`
- Supabase/backend expected path: `C:\dev\hostos\supabase`
- Mobile app expected path: `C:\dev\hostos\apps\mobile`
- Web SPA expected path: `C:\dev\joinfolk-web`
- Dashboard expected path: `C:\dev\joinfolk-web\dashboard`
- Production public domain: `app.join-folk.com`

## 5. Ambiguous / Duplicate Path Findings

### F-001 Multiple Supabase Directories

Confirmed paths:

- `C:\dev\hostos\supabase`
- `C:\dev\joinfolk-web\supabase`
- `C:\dev\hostos\apps\mobile\supabase`

Risk: backend reports may conflate local source trees.

Confidence: High.

### F-002 Multiple Dashboard-Looking Roots

Confirmed paths:

- `C:\dev\joinfolk-web\dashboard`
- `C:\dev\hostos\apps\dashboard`

Risk: dashboard findings or patches could target the wrong app.

Confidence: High.

### F-003 Nested Mobile Git Repo Inside Hostos

Confirmed path:

- `C:\dev\hostos\apps\mobile`

Risk: parent `hostos` status does not represent mobile worktree state.

Confidence: High.

### F-004 Duplicate Web Package Roots

Confirmed paths:

- `C:\dev\joinfolk-web\package.json`
- `C:\dev\joinfolk-web\web\package.json`

Risk: build/deploy assumptions may be path-sensitive.

Confidence: Medium.

### F-005 joinfolk-web-backups Exists

Confirmed path:

- `C:\dev\joinfolk-web-backups`

Risk: Unknown because contents were not inspected.

Confidence: Low.

## 6. Supabase Source-Path Assessment

`C:\dev\hostos\supabase` is the primary plausible backend source for current audit purposes because it has the largest observed migration set and Edge Functions including `push-dispatch`.

However, competing Supabase paths exist:

- `C:\dev\joinfolk-web\supabase`
- `C:\dev\hostos\apps\mobile\supabase`

Observed Supabase migration evidence:

- `hostos\supabase\migrations`: 186 files, latest observed `20260626_commerce_standing_tickets_v1.sql`
- `joinfolk-web\supabase\migrations`: 54 files, latest includes `temp_rpc.sql`
- Mobile nested Supabase: 103 files

Assessment:

- Expected backend path has migrations/functions, but no `config.toml`.
- Competing paths also contain migrations/functions.
- Local source path uncertainty does not directly affect production SQL evidence.
- Local source path uncertainty does affect local-code interpretations such as `push-dispatch`.
- Supabase/backend source-of-truth remains Unknown / Needs verification until competing Supabase directories are classified.

## 7. Impact on Existing Audit Reports

| Report | Foundation risk | Reason | Recheck if needed |
| --- | --- | --- | --- |
| `SupabaseBackendGapReport` | Medium | Explicitly references `C:\dev\hostos\supabase`, `joinfolk-web\dashboard`, and `hostos\apps\mobile`; those are plausible, but competing paths exist. | Recheck local-code findings against chosen canonical source paths, especially `push-dispatch`. |
| `SupabaseProductionParityVerificationReport` | Low for production SQL; Medium for local-code carryover | Production SQL/operator evidence is separate, but `push-dispatch` local-code posture inherits path assumptions. | Recheck local-code carryover after backend source path is confirmed. |
| `SupabaseFocusedBackendFollowUpReport` | Low to Medium | `push-dispatch` was outside focused SQL pass and production-not-visible; SQL conclusions are less affected. | Recheck only local-code assumptions if used later. |
| `SupabaseProofCheckinRpcHardeningPlan` draft | Medium | Source-map ambiguity should be resolved before using it as canonical implementation basis. | Confirm which Supabase migrations/functions tree the plan targets. |

## 8. Source-of-Truth Unknowns

- Which Supabase directory is canonical for backend migrations?
- Whether `C:\dev\joinfolk-web\supabase` is stale, partial, dashboard-specific, or active.
- Whether `C:\dev\hostos\apps\mobile\supabase` is mobile-owned backend history or stale.
- Whether `C:\dev\hostos\apps\dashboard` is active or superseded by `C:\dev\joinfolk-web\dashboard`.
- Whether `C:\dev\joinfolk-web\web` is active or duplicate of repo root.
- What `C:\dev\joinfolk-web-backups` contains.

## 9. Current Working Rule Until Canonical Decision

- Handbook: `C:\dev\joinfolk-engineering-handbook` is the active handbook repo.
- Dashboard: `C:\dev\joinfolk-web\dashboard` is the active dashboard app for current dashboard work, but `C:\dev\hostos\apps\dashboard` remains an ambiguity until ruled out.
- Mobile: `C:\dev\hostos\apps\mobile` is plausible active mobile app, but it is a nested repo and must be checked separately.
- Supabase/backend: `C:\dev\hostos\supabase` is the primary plausible backend source for current audit purposes, but not canonical until competing Supabase directories are classified.
- Production SQL evidence outranks local source assumptions for production-state claims.
- Local-code interpretations must name the source path they used.
- No future patch plan should be treated as implementation-ready until source path is named and verified.

## 10. Required Verification Before Future Patches

Before future patches are treated as implementation-ready, verify:

- The canonical Supabase/backend migrations and functions path.
- Whether competing Supabase directories are active, stale, partial, or component-specific.
- Whether dashboard work should target `C:\dev\joinfolk-web\dashboard` or any `hostos` dashboard path.
- Whether mobile work should be checked independently from the `hostos` parent repo because it is a nested git repo.
- Whether web build/deploy expectations use `C:\dev\joinfolk-web` root or `C:\dev\joinfolk-web\web`.
- Whether local-code findings in audit reports used the same source path that future implementation work will use.

## 11. Non-Goals

- This document does not choose a canonical source of truth.
- This document does not clean up duplicate directories.
- This document does not propose migrations.
- This document does not provide a patch plan.
- This document does not modify production state.
- This document does not inspect or modify application code.
- This document does not supersede production SQL evidence.

## 12. Open Questions

- Which Supabase directory is canonical for backend migrations and Edge Functions?
- Is `C:\dev\joinfolk-web\supabase` active, stale, partial, dashboard-specific, or experimental?
- Is `C:\dev\hostos\apps\mobile\supabase` active mobile-owned backend history or stale?
- Is `C:\dev\hostos\apps\dashboard` still active?
- Is `C:\dev\joinfolk-web\web` active or duplicate of the repository root?
- Should `C:\dev\joinfolk-web-backups` be ignored, archived, or documented separately?
- Which repository and path feed production deploys for web, dashboard, mobile, and Supabase/backend?

## 13. No-Modification Confirmation

For this source-map draft:

- No JoinFolk application code files were modified. This document was created from the supplied read-only repository metadata audit.
- No production connection was made.
- Supabase CLI was not run.
- No migrations were run or generated.
- No builds were run.
- No tests were run.
- No cleanup plan was created.
- No patch plan was created.
- No files were staged or committed.
