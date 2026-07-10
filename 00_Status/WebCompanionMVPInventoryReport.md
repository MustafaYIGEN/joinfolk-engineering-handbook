# Web Companion MVP Inventory Report

## 1. Status

- Inventory completed
- Web Companion MVP not implemented yet

## 2. Decision Source

- `09_Decisions/WebCompanionSurfaceDecision.md`
- Required distinction:
  - Mobile App = main consumer experience
  - Dashboard = host/business/ops management
  - Web Companion = lightweight public + personal browser surface

## 3. Current Architecture

- `joinfolk-web` currently has separated dashboard and web SPA surfaces.
- dashboard SPA owns host/business/ops management.
- web SPA currently acts mainly as public gateway/deep-link surface.

## 4. Dashboard SPA Inventory

- Existing dashboard routes include:
  - `/login`
  - `/*`
  - `/messages`
  - `/events`
  - `/venues`
  - `/reservations`
  - `/ops/*`
  - `/staff/*`
- Existing guards include:
  - `EventOwnerGuard`
  - `SemiProGuard`
  - `OpsGuard`
- Dashboard remains host/business/ops surface.
- No evidence that dashboard should be reused as buyer Web Companion.

## 5. Web SPA Existing Routes

- `/`
- `/e/:id`
- `/claim/:token`
- `/v/:token`
- `/e/:id` supports public event sharing and Cloudflare Functions SSR/OG use case.
- Existing web routes are public/deep-link oriented.

## 6. Missing Web Companion MVP Routes

- `/organizers/:id` or `/u/:handle` is missing.
- `/personal` is missing.
- `/personal/tickets` is missing.
- `/download` is missing.
- buyer-facing `/login` inside web SPA is missing.
- web SPA auth guards are missing.

## 7. Gap Classification

- Public event share: partially present.
- Public organizer profile: missing.
- Personal buyer home: missing.
- Personal tickets: missing.
- Buyer web login: missing.
- Download/app handoff route: missing.
- Web auth/session handling: missing.
- Web Companion MVP: not implemented.

## 8. Security Boundary

- Dashboard should remain isolated as host/business/ops surface.
- Web Companion should not weaken dashboard guards.
- Public web routes should expose only public-safe event/profile data.
- Personal web routes must require authenticated user session.
- Staff scanner and dashboard owner routes remain separate from Web Companion.

## 9. Recommended Implementation Gate

- WC-01: Web Companion route shell and auth boundary.
- Implement only minimal web SPA routes first:
  - `/login`
  - `/personal`
  - `/personal/tickets`
  - `/download`
  - `/organizers/:id` or `/u/:handle`
- Preserve existing `/e/:id`, `/claim/:token`, and `/v/:token` behavior.
- Do not alter dashboard ownership/tier/ops guards.

## 10. Final Status

- Web Companion MVP inventory: CLOSED.
- Web Companion implementation gap: OPEN.
- Next recommended gate: WC-01 route shell and auth boundary.
