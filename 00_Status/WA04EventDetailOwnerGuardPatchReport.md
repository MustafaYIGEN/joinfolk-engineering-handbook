# WA-04 Event Detail Owner Guard Patch Report

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Source confidence: terminal-provided git/build evidence
- Related audit: `07_Audits/DashboardAuthorizationMatrixReport.md`
- Related gap: WA-04
- Patch repository: `C:\dev\joinfolk-web`
- Patch branch: `refactor/joinfolk-stabilization-p0`
- Patch commit: `7b39a04 fix(dashboard): guard event detail routes by owner`
- Previous related patch: `691a294 fix(dashboard): guard staff scanner event access`
- Patch date: 2026-07-08

## 2. Original Gap

WA-04 identified that dashboard event nested tab owner authorization coverage was not explicit enough.

Specific risk:
`/events/:id/*` nested dashboard tabs were protected by `AuthGuard` and `HostGuard`, and `EventDetailPage` performed a parent event lookup through `useEventDetail(id)`, but the route itself was not explicitly wrapped in `EventOwnerGuard`.

Additional concern:
`EventDetailPage` calls event-scoped hooks such as `useEventModules(id)` and `useEventStats(id)` during the component lifecycle. These hooks are enabled by `!!eventId`, so a route-level owner guard is the correct frontend protection layer before mounting the event detail shell.

## 3. Patch Scope

Changed only:

| File | Purpose |
| --- | --- |
| `dashboard/src/App.tsx` | Wrap `/events/:id` route with existing `EventOwnerGuard` |

Exact route change:

Before:

```tsx
<Route path="events/:id" element={<EventDetailPage />}>
```

After:

```tsx
<Route path="events/:id" element={<EventOwnerGuard><EventDetailPage /></EventOwnerGuard>}>
```

Explicitly not changed:

- No child routes were modified.
- No `EventDetailPage` logic was modified.
- No hooks/API/RPC/Supabase code was modified.
- No DB migration was created.
- No RPC/RLS/security schema change was made.
- No Cloudflare change was made.
- No mobile change was made.
- No broad formatter was run.

## 4. Implemented Access Rule

`/events/:id` and its nested dashboard tabs now mount `EventDetailPage` only after the existing `EventOwnerGuard` accepts the current authenticated user as the event owner.

The existing `EventOwnerGuard` checks:

| Required evidence | Source |
| --- | --- |
| Current authenticated user id | `useAuth()` |
| Route event id | `useParams()` |
| Matching event owner | `events.host_id = current user id` |
| Host-created event persona | `events.created_under_persona = "host"` |

Expected result:

| Actor | Expected result |
| --- | --- |
| Event host owner | Existing event detail tabs mount normally |
| Non-owner host | Redirected away by `EventOwnerGuard` before `EventDetailPage` mounts |
| Standard authenticated user | Already blocked from dashboard by `HostGuard` |
| Guest | Already blocked by `AuthGuard` |

## 5. Verification Evidence

Terminal evidence provided:

| Check | Result |
| --- | --- |
| `git diff --check` | PASS |
| Route diff | `/events/:id` route wrapped with `EventOwnerGuard` |
| Modified file count | 1 |
| Modified file | `dashboard/src/App.tsx` |
| Commit | `7b39a04 fix(dashboard): guard event detail routes by owner` |
| Previous related patch | `691a294 fix(dashboard): guard staff scanner event access` |

Build evidence should remain attached to the release checklist if separately produced. This handbook task did not run builds or tests.

## 6. Gap Resolution Status

| Gap | Status | Reason |
| --- | --- | --- |
| WA-04 | Code patched; awaiting manual negative QA | The `/events/:id` route now uses the existing owner guard before mounting event detail child tabs. Manual negative authorization QA is still required before full closure. |

## 7. Required Manual QA

Manual QA must verify:

| Actor | Expected result |
| --- | --- |
| Guest | Redirected/blocked by auth flow |
| Normal authenticated user | Blocked from dashboard event detail route |
| Non-owner host | Cannot access another host's `/events/:id/*` tabs |
| Event host owner | Can access own event detail tabs normally |
| Existing `/event-reservations/:id` owner guard | Behavior remains unchanged |

## 8. Remaining Release Blockers

WA-03 has been patched but remains awaiting manual negative QA.

WA-04 has been patched but remains awaiting manual negative QA.

WA-06 semi-pro/pro semantics still require owner review or closure.

Cloudflare production deploy remains blocked until release gates are clean.

## 9. No-Modification Confirmation

- No application code was modified by this handbook task.
- No dashboard/mobile/web code was modified by this handbook task.
- No Supabase tree was modified by this handbook task.
- No SQL was executed by this handbook task.
- No production mutation was executed by this handbook task.
- No Cloudflare setting was changed by this handbook task.
- No Supabase CLI was run by this handbook task.
- No files were staged or committed by this handbook task.
- Only `00_Status/WA04EventDetailOwnerGuardPatchReport.md` was created.
