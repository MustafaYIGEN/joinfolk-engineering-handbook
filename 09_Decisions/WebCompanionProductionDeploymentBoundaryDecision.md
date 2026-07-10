# Web Companion Production Deployment Boundary Decision

## 1. Status

- Accepted
- Boundary locked
- Implementation source checkpoint exists
- Production/live checkpoint not yet closed

## 2. Trigger

- WC-01 implemented web SPA route shell and buyer auth boundary.
- Repo: `C:\dev\joinfolk-web`
- Branch: `refactor/joinfolk-stabilization-p0`
- Commit: `2395092 feat(web): add companion route shell and buyer auth boundary`
- Branch push: PASS
- Local web build: PASS
- Local web runtime auth smoke: PASS

## 3. Decision

- `app.join-folk.com` remains the dashboard production surface for the current RC line.
- dashboard `/personal` is not the Web Companion `/personal` surface.
- WC-01 must not be tagged as production-live until a Web Companion deployment surface is provisioned and verified.
- Web Companion production must use either:
  - a separate Cloudflare Pages project for the web SPA, or
  - an explicitly documented routing layer that separates web companion routes from dashboard routes.
- Silent route collision between dashboard `/login`, dashboard `/personal`, and web `/login`, web `/personal` is not allowed.
- Dashboard host/business/ops routes must not be weakened or exposed through Web Companion routing.

## 4. Current Surface Boundary

- Dashboard SPA:
  - host/business/ops management
  - current production domain: `app.join-folk.com`
  - owns dashboard login, event management, venue management, ops, staff surfaces
- Web SPA:
  - lightweight public and personal companion surface
  - source exists under `web/src`
  - WC-01 routes exist in source
  - production domain/routing is not yet verified

## 5. WC-01 Source Scope

- Added web SPA routes:
  - `/login`
  - `/download`
  - `/organizers/:id`
  - `/u/:handle`
  - `/personal`
  - `/personal/tickets`
- Preserved web SPA routes:
  - `/`
  - `/e/:id`
  - `/claim/:token`
  - `/v/:token`
- Added auth-only buyer boundary through WebAuthGuard.
- Did not modify dashboard source.
- Did not modify mobile source.
- Did not modify backend, Supabase, RLS, RPCs, migrations, or DB schema.

## 6. Required Production Deployment Shape

If using a separate Cloudflare Pages project:
- Repo: `MustafaYIGEN/joinfolk-web`
- Branch: `refactor/joinfolk-stabilization-p0`
- Root directory: `web`
- Build command: `npm run build`
- Output directory: `dist`
- Node version: `20`
- Required env:
  - `VITE_SUPABASE_URL`
  - `VITE_SUPABASE_ANON_KEY`
  - `VITE_DASHBOARD_URL=https://app.join-folk.com`
- SPA fallback:
  - `web/public/_redirects` must remain active

## 7. Required Production Smoke Before Tag

Web Companion production smoke must verify:
- `/` opens public Home
- `/login` opens buyer web login
- `/download` opens public download handoff
- `/organizers/test` opens public organizer shell
- `/u/test` opens public organizer shell
- `/personal` when logged out redirects to `/login`
- `/personal/tickets` when logged out redirects to `/login`
- valid login opens `/personal`
- `/personal/tickets` opens after auth
- sign out returns to public surface
- `/e/:id` still opens public event share
- `/claim/:token` still opens claim handoff
- `/v/:token` still opens verification handoff
- `app.join-folk.com` dashboard `/login` still works
- `app.join-folk.com` dashboard `/events` still works
- staff routes remain unaffected

## 8. Tagging Rule

- Do not create a production RC tag for WC-01 until production deployment smoke passes.
- Suggested future tag only after live verification:
  - `joinfolk-v1-rc2.5-web-companion`

## 9. Final Boundary

- WC-01 source checkpoint: CLOSED
- WC-01 production deployment boundary: LOCKED
- WC-01 production evidence: OPEN
- Next gate: provision and verify Web Companion production deployment surface
