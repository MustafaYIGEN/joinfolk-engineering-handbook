# Cloudflare Git-Backed Production Deploy Evidence Report

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Source confidence: terminal-provided git/build/Cloudflare/browser QA evidence
- Production domain: `https://app.join-folk.com`
- Cloudflare Pages project: `joinfolk-dashboard-live`
- Repository: `MustafaYIGEN/joinfolk-web`
- Local repo: `C:\dev\joinfolk-web`
- Branch: `refactor/joinfolk-stabilization-p0`
- Current production commit: `e11c5ce fix(dashboard): make venue shared imports repo local`
- Previous RC-2.1 tag: `joinfolk-v1-rc2.1-web-dashboard`
- Previous RC-2.1 tag commit: `cf7e829 fix(dashboard): harden staff scanner authorization`
- Evidence date: 2026-07-09

## 2. Deployment Pipeline Fix Summary

Cloudflare Git-backed deployment is now configured and working for the dashboard production surface.

Final Cloudflare production configuration:

| Setting | Value |
| --- | --- |
| Repository | `MustafaYIGEN/joinfolk-web` |
| Production branch | `refactor/joinfolk-stabilization-p0` |
| Root directory | `dashboard` |
| Build command | `npm run build` |
| Build output directory | `dist` |
| Node version | `20` |
| Automatic deployments | Enabled |
| Production domain | `app.join-folk.com` |

## 3. Cloudflare Clean Build Portability Fix

Cloudflare initially failed Git-backed builds because the dashboard resolved `venue-shared` from a repo-external local path:

`../../hostos/packages/venue-shared`

That path exists on the developer workstation but does not exist in a clean Cloudflare clone of `MustafaYIGEN/joinfolk-web`.

Fix committed in web repo:

`e11c5ce fix(dashboard): make venue shared imports repo local`

Files changed:

| File | Purpose |
| --- | --- |
| `dashboard/tsconfig.json` | Point `venue-shared` alias to repo-local package |
| `dashboard/vite.config.ts` | Point Vite alias to repo-local package |
| `packages/venue-shared/package.json` | Add repo-local package metadata |
| `packages/venue-shared/index.ts` | Add minimal `TopologyMeta` export |
| `packages/venue-shared/venue-types.ts` | Add ASCII-only canonical venue type exports |

## 4. Cloudflare Build Evidence

Cloudflare deployment evidence:

| Field | Result |
| --- | --- |
| Deployment type | Git-backed production deployment |
| Repository | `MustafaYIGEN/joinfolk-web` |
| Branch | `refactor/joinfolk-stabilization-p0` |
| Commit | `e11c5ce` |
| Status | Success |
| Duration | 46s |
| Alias | `app.join-folk.com` |
| Deployment URL example | `https://b379896f.joinfolk-dashboard-live.pages.dev` |

## 5. Supabase Environment Fix Evidence

After Git-backed deploy, the live dashboard initially showed `Invalid API key`.

Root cause:
The production bundle contained the fallback value `placeholder-key`, proving that `VITE_SUPABASE_ANON_KEY` was not correctly injected into the Vite build.

Verified correct Supabase values:

| Check | Result |
| --- | --- |
| Supabase URL | `https://nufwrcqjiwbtydgcsukv.supabase.co` |
| Verified anon key length | 208 characters |
| Verified anon key prefix | `eyJhbGciOiJIUzI1NiIs` |
| `/auth/v1/settings` with verified key | 200 OK |
| Full key recorded in handbook | No |

Bundle verification before fix:

| Check | Result |
| --- | --- |
| Main JS asset | `/assets/index-ARmwkGT4.js` |
| Contains qji Supabase URL | True |
| Contains qjj typo | False |
| Contains `placeholder-key` | True |
| Contains verified key prefix | False |

Bundle verification after fix:

| Check | Result |
| --- | --- |
| Main JS asset | `/assets/index-D3tPXONG.js` |
| Contains qji Supabase URL | True |
| Contains qjj typo | False |
| Contains `placeholder-key` | False |
| Contains verified key prefix | True |

## 6. Live QA Evidence

WA-03 staff scanner route:

| Actor | URL | Expected | Result |
| --- | --- | --- | --- |
| Unassigned authenticated user | `/staff/scan/8fcf1064-8e1d-443e-8307-2c032568284b` | Access denied | PASS |
| Host owner | `/staff/scan/8fcf1064-8e1d-443e-8307-2c032568284b` | Scanner guidance / mobile app message | PASS |

Observed unassigned-auth result:

`Access denied`
`You are not assigned to this event scanner.`

Observed host-owner result:

`Please use the mobile app`

WA-04 event detail owner routes:

| URL | Expected | Result |
| --- | --- | --- |
| `/events/8fcf1064-8e1d-443e-8307-2c032568284b` | Opens for host owner | PASS |
| `/events/8fcf1064-8e1d-443e-8307-2c032568284b/content` | Opens for host owner | PASS |
| `/events/8fcf1064-8e1d-443e-8307-2c032568284b/staff` | Opens for host owner | PASS |
| `/events/8fcf1064-8e1d-443e-8307-2c032568284b/venue` | Opens for host owner | PASS |

## 7. Final Status

| Item | Status |
| --- | --- |
| Cloudflare Git repository connection | DONE |
| Git-backed production build | PASS |
| Repo-local `venue-shared` portability fix | DONE |
| Production Supabase env injection | FIXED |
| Live bundle placeholder-key issue | FIXED |
| WA-03 live QA on Git-backed production | PASS |
| WA-04 live owner route QA on Git-backed production | PASS |
| Manual-only deployment dependency | CLOSED |
| Full production launch process | Still subject to remaining non-web release gates |

## 8. No-Modification Confirmation

- No application code was modified by this handbook task.
- No dashboard/mobile/web code was modified by this handbook task.
- No Supabase tree was modified by this handbook task.
- No SQL was executed by this handbook task.
- No Cloudflare setting was changed by this handbook task.
- No Supabase CLI was run by this handbook task.
- No secrets or full Supabase keys were recorded.
- Only `00_Status/CloudflareGitBackedProductionDeployEvidenceReport.md` was created.
