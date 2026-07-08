# WA-03 Staff Scanner Authorization Patch Report

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Source confidence: terminal-provided git/build evidence
- Related audit: `07_Audits/DashboardAuthorizationMatrixReport.md`
- Related gap: WA-03
- Patch repository: `C:\dev\joinfolk-web`
- Patch branch: `refactor/joinfolk-stabilization-p0`
- Patch commit: `691a294 fix(dashboard): guard staff scanner event access`
- Follow-up commit: `cf7e829 fix(dashboard): harden staff scanner authorization`
- Patch date: 2026-07-08

## 2. Original Gap

WA-03 identified that web staff route-level authorization was unclear.

Specific risk:
`/staff/scan/:eventId` was protected by authentication but did not prove page-level event-specific staff or host authorization before showing scanner guidance/title.

After initial WA-03 patch, live QA found that an authenticated user who was neither event host nor assigned staff still saw scanner guidance on the deployed surface. DB verification for user `80362115-0203-4544-beda-b8b043f17e7b` and event `8fcf1064-8e1d-443e-8307-2c032568284b` returned:
- is_host_owner: false
- is_assigned_staff: false
- expected_staff_scan_access: DENY

Follow-up fix:
`StaffScannerPage.tsx` now uses `supabase.auth.getUser()`, treats missing user and query errors as denied, and keeps deny-by-default behavior before rendering event title or scanner guidance.

## 3. Patch Scope

Changed only:

| File | Purpose |
| --- | --- |
| `dashboard/src/pages/staff/StaffScannerPage.tsx` | Add event-specific access check before rendering scanner guidance/title |

Explicitly not changed:

- No host account delegation.
- No business account membership.
- No dashboard team access system.
- No DB migration.
- No RPC/RLS/security schema change.
- No Cloudflare change.
- No mobile change.
- No broad formatter.

## 4. Implemented Access Rule

`/staff/scan/:eventId` now allows scanner guidance/title only when the authenticated user satisfies one of these conditions:

| Actor | Required evidence |
| --- | --- |
| Event host owner | `events.id = eventId`, `events.host_id = session.user.id`, `events.created_under_persona = "host"` |
| Assigned staff user | `event_staff_assignments.event_id = eventId`, `event_staff_assignments.staff_user_id = session.user.id` |

Unauthorized users should see:

- `Access denied`
- `You are not assigned to this event scanner.`

Unauthorized users should not see the event title or scanner guidance.

## 5. Verification Evidence

Terminal evidence provided:

| Check | Result |
| --- | --- |
| `git diff --check` | PASS |
| Mojibake scan on added lines | PASS / empty |
| Console/AUDIT scan on added lines | PASS / empty |
| `npm --prefix dashboard run build` | PASS |
| Build modules | 314 modules transformed |
| Build result | `built in 2.90s` |
| Commit | `691a294 fix(dashboard): guard staff scanner event access` |
| Push | `86dba59..691a294 refactor/joinfolk-stabilization-p0 -> refactor/joinfolk-stabilization-p0` |
| Post-push status | clean |
| Follow-up commit | `cf7e829 fix(dashboard): harden staff scanner authorization` |
| Follow-up push | `65bd5e3..cf7e829 refactor/joinfolk-stabilization-p0 -> refactor/joinfolk-stabilization-p0` |
| Follow-up build | `npm --prefix dashboard run build` PASS |
| Build modules | 314 modules transformed |
| Build result | `built in 2.95s` |
| Live manual deployment method | Cloudflare Pages manual upload from `dashboard/dist` via Yol B |
| Live negative QA | PASS |
| Live negative QA result | `Access denied` / `You are not assigned to this event scanner.` |

Known non-blocking build warning:

- Vite chunk size warning remains present.
- This warning is not related to WA-03 and is not treated as a release blocker for this patch.

## 6. Gap Resolution Status

| Gap | Status | Reason |
| --- | --- | --- |
| WA-03 | Code patched and live negative QA passed | Code patched and live negative QA passed for guest and unassigned authenticated user; assigned-staff positive QA remains optional/manual if separately required. |

## 7. Required Manual QA

Manual QA must verify:

| Actor | Expected result |
| --- | --- |
| Guest | Redirected/blocked by auth flow |
| Normal authenticated user with no assignment | Access denied |
| Staff assigned to the exact event | Scanner guidance visible |
| Staff assigned to a different event | Access denied |
| Event host owner | Scanner guidance visible |

## 8. Remaining Release Blockers

WA-04 remains open.

WA-04 concerns dashboard event nested tab owner authorization coverage and must be resolved or proven before production release/deploy.

Cloudflare Git repository connection remains unresolved; current successful live test used manual deployment. Permanent Git-based production deployment evidence is still required before final launch process closure.

## 9. No-Modification Confirmation

- No application code was modified by this handbook task.
- No dashboard/mobile/web code was modified by this handbook task.
- No Supabase tree was modified by this handbook task.
- No SQL was executed by this handbook task.
- No production mutation was executed by this handbook task.
- No Cloudflare setting was changed by this handbook task.
- No Supabase CLI was run by this handbook task.
- No files were staged or committed by this handbook task.
- Only `00_Status/WA03StaffScannerAuthorizationPatchReport.md` was created.
