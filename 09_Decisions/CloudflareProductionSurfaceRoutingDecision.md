# Cloudflare Production Surface Routing Decision

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Source confidence: operator-provided Cloudflare dashboard evidence
- Decision type: Production surface routing / deployment ownership
- Applies to: JoinFolk web-facing surfaces, Cloudflare Pages projects, custom domains, production branch alignment
- Last evidence date: 2026-07-08

## 2. Decision

JoinFolk web-facing surfaces must be managed through explicit Cloudflare Pages project ownership.

The production application domain `app.join-folk.com` is attached to the Cloudflare Pages project `joinfolk-dashboard-live`.

The RC-2.1 dashboard/application production deployment target is therefore:

| Field | Required value |
| --- | --- |
| Cloudflare project | `joinfolk-dashboard-live` |
| Custom domain | `app.join-folk.com` |
| Git repository | `MustafaYIGEN/joinfolk-web` |
| Production branch | `refactor/joinfolk-stabilization-p0` |
| Root directory | `dashboard` |
| Build command | `npm run build` |
| Build output directory | `dist` |
| Node version | `20` |

Do not connect `joinfolk-dashboard-live` to `MustafaYIGEN/joinfolk-platform` for dashboard production deploys.

## 3. Current Cloudflare Surface Map

| Cloudflare project | Custom domain evidence | Repository evidence | Interpretation |
| --- | --- | --- | --- |
| `joinfolk-web` | `join-folk.com`, `www.join-folk.com` | `MustafaYIGEN/joinfolk-web` | Root/marketing web surface |
| `joinfolk-dashboard` | No custom production domain | `MustafaYIGEN/joinfolk-web` | Non-production/staging/dev dashboard surface |
| `joinfolk-dashboard-live` | `app.join-folk.com` | Repository connection currently requires validation/connect | Production application/dashboard surface |
| `joinfolk-web-live` | No custom production domain | `MustafaYIGEN/joinfolk-platform` | Not selected for RC-2.1 dashboard production deploy |

## 4. RC-2.1 Required Release References

| Area | Release reference |
| --- | --- |
| Web/dashboard tag | `joinfolk-v1-rc2-web-dashboard` |
| Web/dashboard commit | `86dba59b4155efaeba13d0be369409367174bb68` |
| Platform/DB tag | `joinfolk-v1-rc2-platform-db` |
| Platform/DB commit | `72d3a9f3c795e8b9e06060ab7b78fe88b690353c` |
| Mobile tag | `joinfolk-v1-rc2.1-mobile` |
| Mobile commit | `6a1224bac1ed4c2a451b50611dc1967fc1e99d07` |

## 5. Production Deployment Rule

Production deployment for `app.join-folk.com` is blocked unless all of the following are true:

| Gate | Required state |
| --- | --- |
| Cloudflare project | `joinfolk-dashboard-live` |
| Domain | `app.join-folk.com` active |
| Repository | `MustafaYIGEN/joinfolk-web` |
| Production branch | `refactor/joinfolk-stabilization-p0` |
| Root directory | `dashboard` |
| Build command | `npm run build` |
| Output directory | `dist` |
| Environment variables | Production Supabase URL and anon key present |
| Node version | `20` |
| Latest deployment source | `refactor/joinfolk-stabilization-p0` |
| Latest deployment commit | `86dba59b4155efaeba13d0be369409367174bb68` or a later explicitly approved release commit |

## 6. Blocked Misconfigurations

The following configurations are explicitly blocked:

| Misconfiguration | Reason |
| --- | --- |
| `joinfolk-dashboard-live` connected to `MustafaYIGEN/joinfolk-platform` | Wrong source repository for dashboard RC-2.1 deploy |
| Production branch set to `main` for RC-2.1 deploy | RC-2.1 evidence is on `refactor/joinfolk-stabilization-p0` |
| Root directory empty with build command `npm run build` and output `dist` | Would build from repo root, not dashboard package |
| Root directory `dashboard` with build command `npm --prefix dashboard run build` | Double-dashboard path risk |
| Treating Preview deployment as Production | `app.join-folk.com` uses Production deployment |
| Deploying from `release/ios-v17-media-performance` | Mobile branch must not be used for web/dashboard production deploy |

## 7. Operational Interpretation

- `joinfolk-dashboard-live` is the production app/dashboard project because it owns `app.join-folk.com`.
- `joinfolk-dashboard` is not production because it has no production custom domain.
- `joinfolk-web` owns the root marketing domains `join-folk.com` and `www.join-folk.com`.
- `joinfolk-web-live` is not selected for RC-2.1 dashboard production deploy.
- The dashboard production source of truth is `C:\dev\joinfolk-web` on branch `refactor/joinfolk-stabilization-p0`.
- The platform/DB source of truth remains `C:\dev\hostos`.
- The mobile source of truth remains `C:\dev\hostos\apps\mobile`, even though its remote currently points to `MustafaYIGEN/joinfolk-web`.

## 8. Required Next Gates

| Next gate | Required before | Status |
| --- | --- | --- |
| Connect `joinfolk-dashboard-live` to `MustafaYIGEN/joinfolk-web` | Any production dashboard deploy | Required |
| Set production branch to `refactor/joinfolk-stabilization-p0` | Any RC-2.1 production deploy | Required |
| Confirm root/build/output config | Any RC-2.1 production deploy | Required |
| Confirm latest deployment commit | Release checkpoint | Required |
| Record final deployment evidence | Release closure | Required |

## 9. No-Modification Confirmation

- No application code was modified by this handbook task.
- No dashboard/mobile/web code was modified by this handbook task.
- No Supabase tree was modified by this handbook task.
- No SQL was executed by this handbook task.
- No production mutation was executed by this handbook task.
- No Cloudflare setting was changed by this handbook task.
- No Supabase CLI was run by this handbook task.
- No files were staged or committed by this handbook task.
- Only `09_Decisions/CloudflareProductionSurfaceRoutingDecision.md` was created or modified.
