# Web Dashboard RC-2.1 Tag And Manual Deploy Evidence Report

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Source confidence: terminal-provided git/build/deploy/QA evidence
- Repository: `C:\dev\joinfolk-web`
- Branch: `refactor/joinfolk-stabilization-p0`
- RC-2.1 tag: `joinfolk-v1-rc2.1-web-dashboard`
- Tagged commit: `cf7e8296d1460169de1a824e4e6c1e0eb2b52056`
- Tag date: 2026-07-08
- Previous tag: `joinfolk-v1-rc2-web-dashboard`
- Previous tag commit: `86dba59`
- Related patches:
  - `691a294 fix(dashboard): guard staff scanner event access`
  - `7b39a04 fix(dashboard): guard event detail routes by owner`
  - `65bd5e3 fix(dashboard): align event owner guard filters`
  - `cf7e829 fix(dashboard): harden staff scanner authorization`

## 2. Tag Evidence

Terminal evidence confirmed:

| Check | Result |
| --- | --- |
| `git status -sb --untracked-files=all` | clean |
| HEAD | `cf7e829 fix(dashboard): harden staff scanner authorization` |
| Remote branch HEAD | `origin/refactor/joinfolk-stabilization-p0 = cf7e829` |
| Build command | `npm --prefix dashboard run build` |
| Build result | PASS |
| Build modules | 314 modules transformed |
| Build duration | `built in 3.00s` |
| Tag created | `joinfolk-v1-rc2.1-web-dashboard` |
| Tag target | `cf7e8296d1460169de1a824e4e6c1e0eb2b52056` |
| Tag pushed | PASS |
| `git tag --points-at HEAD` | `joinfolk-v1-rc2.1-web-dashboard` |

Known non-blocking build warning:

- Vite chunk size warning remains present.
- This warning is not related to the authorization hardening and is not treated as a blocker for this checkpoint.

## 3. Authorization Hardening Summary

RC-2.1 web/dashboard includes:

| Area | Commit | Result |
| --- | --- | --- |
| WA-03 staff scanner route authorization | `691a294` + `cf7e829` | Scanner guidance is denied by default unless current user is host owner or assigned staff |
| WA-04 event detail owner route guard | `7b39a04` | `/events/:id/*` mounts only after `EventOwnerGuard` passes |
| WA-04 event owner guard filter alignment | `65bd5e3` | `EventOwnerGuard` now mirrors `fetchHostEvents()` event eligibility filtering |

## 4. Live Manual Deploy Evidence

Production surface:

- Domain: `https://app.join-folk.com`
- Cloudflare Pages project: `joinfolk-dashboard-live`
- Deployment method used for this checkpoint: manual Cloudflare Pages upload from `dashboard/dist`
- Reason: Cloudflare Git repository connection is not configured/resolved for this project; UI showed `Git repository: Connect`.

Manual deployment result:

- Local build output from `C:\dev\joinfolk-web\dashboard\dist` was manually deployed through Cloudflare Pages.
- Live WA-03 negative test then returned the expected denied state.

## 5. Live QA Evidence

WA-03 live negative QA:

| Field | Value |
| --- | --- |
| User id | `80362115-0203-4544-beda-b8b043f17e7b` |
| Event id | `8fcf1064-8e1d-443e-8307-2c032568284b` |
| DB expected access | `DENY` |
| Reason | user is not host owner and not assigned staff |
| Live URL | `/staff/scan/8fcf1064-8e1d-443e-8307-2c032568284b` |
| Live result | `Access denied` / `You are not assigned to this event scanner.` |
| QA result | PASS |

WA-04 live positive QA:

| Field | Result |
| --- | --- |
| Owner host event detail | PASS |
| `/events/:id` | PASS |
| `/events/:id/content` | PASS |
| `/events/:id/staff` | PASS |
| `/events/:id/venue` | PASS |

WA-04 guest negative QA:

| Field | Result |
| --- | --- |
| Guest direct access to `/events/:id` | PASS - login required |
| Guest direct access to `/staff/scan/:eventId` | PASS - login required |

## 6. Remaining Gaps

This report records a code checkpoint and manual deploy evidence.

It does not close the permanent deployment pipeline issue.

Remaining:

- Cloudflare Git repository connection for `joinfolk-dashboard-live` must be configured.
- Expected permanent config:
  - Repository: `MustafaYIGEN/joinfolk-web`
  - Production branch: `refactor/joinfolk-stabilization-p0`
  - Root directory: `dashboard`
  - Build command: `npm run build`
  - Output directory: `dist`
  - Node version: `20`
- Future production deploys should be Git-backed instead of manual upload.
- Non-owner-host WA-04 negative QA remains not separately proven unless another host account is used.

## 7. Final Status

| Item | Status |
| --- | --- |
| RC-2.1 web/dashboard code checkpoint | DONE |
| RC-2.1 tag pushed | DONE |
| Manual production deploy for live QA | DONE |
| WA-03 live negative QA | PASS |
| WA-04 live owner/guest QA | PASS |
| Cloudflare Git-backed deploy pipeline | OPEN |
| Full production launch process closure | NOT YET |

## 8. No-Modification Confirmation

- No application code was modified by this handbook task.
- No dashboard/mobile/web code was modified by this handbook task.
- No Supabase tree was modified by this handbook task.
- No SQL was executed by this handbook task.
- No Cloudflare setting was changed by this handbook task.
- No Supabase CLI was run by this handbook task.
- Only `00_Status/WebDashboardRC21TagAndManualDeployEvidenceReport.md` was created.
