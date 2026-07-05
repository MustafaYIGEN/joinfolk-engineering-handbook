# Permission Matrix

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is the platform-level permission matrix draft for JoinFolk.

This is a handbook draft. It is not a code audit and is not an accepted implementation contract. PermissionMatrix.md is the specific permission-axis document under the umbrella SecurityModel.md. SecurityModel.md is the umbrella security document. PublicExposureRules.md will hold public/private/protected exposure-specific rules. ThreatModel.md will hold threat-specific analysis.

This document does not define exact allow/deny permissions. All exact permission values remain Unknown / Needs verification.

## 3. Permission Matrix Definition

The Permission Matrix describes the axes that must be verified before JoinFolk can accept platform-level permission rules.

Known facts:

- JoinFolk has auth/user/profile concepts.
- JoinFolk has personas and tiers.
- JoinFolk has viewer roles.
- JoinFolk has event lifecycle.
- JoinFolk has event ownership and host concepts.
- JoinFolk has ticketing, reservations, wallet/ownership, media/gallery, feed/discovery, notifications, staff scanner/check-in, venue/business tools, host identity transfer, ops/admin concepts, and public sharing.
- JoinFolk has messaging concepts or possible messaging concepts.
- JoinFolk uses Supabase or Supabase-like backend concepts.
- JoinFolk has RLS, RPC, SECURITY DEFINER, storage policy, and auth concepts.
- Security-sensitive behavior must not be enforced only by frontend code.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact permission matrix.
- Exact actor, resource, action, context, surface, and enforcement rules.
- Exact permission behavior across product domains.

## 4. Relationship to SecurityModel.md

SecurityModel.md is the umbrella security document.

PermissionMatrix.md focuses on permission axes and the future matrix structure. It should not duplicate SecurityModel.md in full.

Related documents:

- SecurityModel.md: umbrella security model.
- PublicExposureRules.md: public/private/protected exposure-specific rules.
- ThreatModel.md: threat-specific analysis.

Unknown / needs verification:

- Exact boundary between PermissionMatrix.md, SecurityModel.md, PublicExposureRules.md, and ThreatModel.md.
- Which verified rules should live in each document.

## 5. Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend/RPC/RLS/storage/auth enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Permission-aware presentation.
- Navigation visibility.
- Disabled or hidden controls for usability.
- Loading, empty, and error states.
- Client-side validation for usability.
- Non-authoritative display filtering.

Unknown / needs verification:

- Exact frontend route/component permission behavior.
- Exact mobile, Dashboard/Ops, and Web/Public UX behavior.

### What backend/RPC/RLS/storage/auth must enforce

Backend/RPC/RLS/storage/auth must enforce security-sensitive permissions.

Security-sensitive enforcement includes:

- Authentication.
- Authorization.
- Viewer-role permissions.
- Persona/tier permissions.
- Staff role permissions.
- Host permissions.
- Ops/admin permissions.
- Ownership permissions.
- Public/private/protected access.
- Storage upload/read/update/delete.
- RPC execution authority.
- SECURITY DEFINER boundaries.
- Production change approval.

Unknown / needs verification:

- Exact auth behavior.
- Exact RLS policies.
- Exact RPC contracts.
- Exact SECURITY DEFINER behavior.
- Exact storage policies.
- Exact permission matrix values.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Exact allow/deny decisions for security-sensitive behavior.
- Viewer-role permissions.
- Persona/tier permissions.
- Staff role permissions.
- Host permissions.
- Ops/admin permissions.
- Ownership permissions.
- Public/private/protected exposure rules.
- Storage upload/read/update/delete permissions.
- RPC execution authority.
- SECURITY DEFINER boundaries.
- Production SQL, migrations, functions, RLS, storage, and auth change approval.

## 6. Permission Axes Draft

### Actor axis

Known actor concepts include users, profiles, viewer roles, personas, tiers, staff roles, hosts, and ops/admin actors.

Unknown / needs verification:

- Exact actor model.
- Exact actor hierarchy.
- Exact actor-to-permission relationships.

### Resource axis

Known resource domains include events, tickets, reservations, wallet/ownership, media/gallery, feed/discovery, messaging, notifications, staff scanner/check-in, venue/business tools, host identity transfer, ops/admin, public sharing, storage, RPC, RLS, and production changes.

Unknown / needs verification:

- Exact resource model.
- Exact resource ownership boundaries.
- Exact resource-to-permission relationships.

### Action axis

Known action families include view, create, edit, publish, manage, purchase, request, own, transfer, check in, approve, reject, upload, replace, remove, moderate, notify, execute, and audit.

Unknown / needs verification:

- Exact action model.
- Exact action names.
- Exact action-to-permission relationships.

### Context axis

Known context concepts include event lifecycle, event ownership, participation, ticket/reservation state, wallet/ownership, public/private/protected visibility, personas, tiers, viewer roles, staff assignment, venue/business context, and ops/admin context.

Unknown / needs verification:

- Exact context model.
- Exact context combinations.
- Exact context-to-permission relationships.

### Surface axis

Known surfaces include mobile, Dashboard/Ops, Web/Public, and Supabase backend.

Unknown / needs verification:

- Exact surface ownership.
- Exact cross-surface permission behavior.
- Which surfaces display permission state versus enforce permission state.

### Enforcement axis

Known enforcement concepts include auth, RLS, RPC, SECURITY DEFINER, storage policies, backend logic, and production change approval.

Unknown / needs verification:

- Exact enforcement model.
- Exact source of truth for each permission.
- Exact relationship between frontend UX and backend enforcement.

## 7. Known Actor Concepts Draft

### Viewer roles

Known viewer role concepts from prior project context:

- `guest`
- `authenticated_non_participant`
- `ticket_holder`
- `participant`
- `checked_in`
- `host`
- `staff`

These names are known concepts only and must not be treated as accepted canonical permission values until verified.

Unknown / needs verification:

- Exact viewer-role model.
- Exact viewer-role permissions.

### Personas

Known persona concepts from prior project context:

- `personal`
- `host`

These names are known concepts only and must not be treated as accepted canonical permission values until verified.

Unknown / needs verification:

- Exact persona model.
- Exact persona permissions.

### Tiers

Known tier concepts from prior project context:

- `user`
- `semi_pro`
- `pro`

These names are known concepts only and must not be treated as accepted canonical permission values until verified.

Unknown / needs verification:

- Exact tier model.
- Exact tier permissions.

### Staff roles

Known staff role or concept names from prior project context:

- `scanner`
- `manager`

These names are known concepts only and must not be treated as accepted canonical permission values until verified.

Unknown / needs verification:

- Exact staff role model.
- Exact staff role permissions.

### Host

Known facts:

- JoinFolk has event ownership and host concepts.

Unknown / needs verification:

- Exact host model.
- Exact host permissions.
- Exact relationship between host, event ownership, venue/business tools, ticketing, reservations, media, feed, messaging, notifications, staff scanner/check-in, and public sharing.

### Ops/admin

Known facts:

- JoinFolk has ops/admin concepts.
- Ops/admin behavior can be security-sensitive.

Unknown / needs verification:

- Exact ops/admin role model.
- Exact ops/admin permissions.
- Exact approval, support, override, moderation, rollback, and audit permissions.

## 8. Known Resource Domains Draft

Known security-sensitive permission domains:

- Event visibility and lifecycle access.
- Event creation, publishing, editing, and management.
- Ticket purchase, request, ownership, transfer, and check-in.
- Reservation creation, ownership, approval, and management.
- Wallet/ownership access.
- Media/gallery upload, view, replace, remove, and moderation.
- Feed/discovery inclusion and visibility.
- Messaging visibility and access where applicable.
- Notification creation, delivery, and visibility.
- Staff/scanner queue access and check-in authority.
- Venue/business tooling access.
- Host identity transfer approval/execution.
- Ops/admin support, override, moderation, approval, rollback, and audit access.
- Public sharing and Web/Public exposure.
- Storage upload/read/update/delete.
- RPC execution authority.
- SECURITY DEFINER boundaries.
- Production change approval.

Unknown / needs verification:

- Exact resources.
- Exact resource ownership and access rules.
- Exact resource-specific permission values.

## 9. Known Action Families Draft

Known action families:

- View.
- Create.
- Edit.
- Publish.
- Manage.
- Purchase.
- Request.
- Own.
- Transfer.
- Check in.
- Approve.
- Reject.
- Upload.
- Replace.
- Remove.
- Moderate.
- Notify.
- Execute.
- Audit.
- Rollback.
- Approve production changes.

Unknown / needs verification:

- Exact action names.
- Exact action semantics.
- Exact allow/deny behavior for each actor/resource/context/surface/enforcement combination.

## 10. Non-Accepted Permission Areas

The following areas are not accepted yet:

- Exact permission matrix.
- Exact viewer-role permissions.
- Exact persona/tier permissions.
- Exact staff role permissions.
- Exact host permissions.
- Exact ops/admin permissions.
- Exact public/private/protected permission rules.
- Exact RLS policies.
- Exact RPC contracts.
- Exact SECURITY DEFINER behavior.
- Exact storage policies.
- Exact frontend route/component permission behavior.
- Exact cross-surface permission consistency.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 11. Event Lifecycle Permission Draft

Known permission domains:

- Event visibility and lifecycle access.
- Event creation, publishing, editing, and management.

Unknown / needs verification:

- Exact event lifecycle permissions.
- Exact event visibility permissions.
- Exact event creation, publishing, editing, and management permissions.
- Exact relationship to viewer roles, personas, tiers, host, staff, ops/admin, public sharing, RLS, RPC, and frontend behavior.

No exact event lifecycle permission behavior is accepted in this draft.

## 12. Ticketing Permission Draft

Known permission domains:

- Ticket purchase, request, ownership, transfer, and check-in.

Unknown / needs verification:

- Exact ticketing permissions.
- Exact ticket purchase, request, ownership, transfer, and check-in permissions.
- Exact relationship to viewer roles, personas, tiers, host, staff, checked-in state, wallet/ownership, reservations, RPC, RLS, and public sharing.

No exact ticketing permission behavior is accepted in this draft.

## 13. Reservation Permission Draft

Known permission domains:

- Reservation creation, ownership, approval, and management.

Unknown / needs verification:

- Exact reservation permissions.
- Exact reservation creation, ownership, approval, and management permissions.
- Exact relationship to event reservations, venue reservations, viewer roles, personas, tiers, host, staff, ops/admin, ticketing, wallet/ownership, RPC, and RLS.

No exact reservation permission behavior is accepted in this draft.

## 14. Wallet / Ownership Permission Draft

Known permission domains:

- Wallet/ownership access.

Unknown / needs verification:

- Exact wallet/ownership permissions.
- Exact ownership access, transfer, display, and management permissions.
- Exact relationship to ticketing, reservations, event ownership, host identity transfer, viewer roles, personas, tiers, RPC, and RLS.

No exact wallet/ownership permission behavior is accepted in this draft.

## 15. Media / Gallery Permission Draft

Known permission domains:

- Media/gallery upload, view, replace, remove, and moderation.
- Storage upload/read/update/delete.

Unknown / needs verification:

- Exact media/gallery permissions.
- Exact storage permissions.
- Exact upload, view, replace, remove, moderation, public sharing, and Web/Public exposure permissions.
- Exact relationship to viewer roles, personas, tiers, host, staff, ops/admin, storage policies, RLS, RPC, and frontend behavior.

No exact media/gallery permission behavior is accepted in this draft.

## 16. Feed / Discovery Permission Draft

Known permission domains:

- Feed/discovery inclusion and visibility.

Unknown / needs verification:

- Exact feed/discovery permissions.
- Exact feed visibility and inclusion permissions.
- Exact relationship to event lifecycle, public sharing, viewer roles, personas, tiers, host identity, media, ops/admin, RLS, RPC, and Web/Public exposure.

No exact feed/discovery permission behavior is accepted in this draft.

## 17. Messaging Permission Draft

Known permission domains:

- Messaging visibility and access where applicable.

Unknown / needs verification:

- Whether messaging is accepted behavior.
- Exact messaging permissions.
- Exact conversation/thread access permissions.
- Exact direct, host/participant, event-level, reservation/ticket-related, staff/scanner, and ops/admin messaging permissions where applicable.
- Exact relationship to viewer roles, personas, tiers, event participation, ownership, notifications, RLS, RPC, and realtime behavior.

No exact messaging permission behavior is accepted in this draft.

## 18. Notifications Permission Draft

Known permission domains:

- Notification creation, delivery, and visibility.

Unknown / needs verification:

- Exact notification permissions.
- Exact notification creation, delivery, visibility, preferences, read state, and admin permissions.
- Exact relationship to product-domain triggers, viewer roles, personas, tiers, ownership, RPC, RLS, and frontend behavior.

No exact notification permission behavior is accepted in this draft.

## 19. Staff Scanner / Check-in Permission Draft

Known permission domains:

- Staff/scanner queue access and check-in authority.

Unknown / needs verification:

- Exact staff scanner permissions.
- Exact check-in permissions.
- Exact queue access permissions.
- Exact staff role permissions for scanner and manager concepts.
- Exact relationship to ticketing, reservations, wallet/ownership, event lifecycle, viewer roles, personas, tiers, RPC, and RLS.

No exact staff scanner/check-in permission behavior is accepted in this draft.

## 20. Venue / Business Tools Permission Draft

Known permission domains:

- Venue/business tooling access.

Unknown / needs verification:

- Exact venue/business permissions.
- Exact venue listing, viewing, creation, editing, archiving, reservation, media, layout/editor, product-section mapping, and public sharing permissions.
- Exact relationship to host, staff, ops/admin, viewer roles, personas, tiers, RPC, RLS, storage policies, and frontend behavior.

No exact venue/business tools permission behavior is accepted in this draft.

## 21. Host Identity Transfer Permission Draft

Known permission domains:

- Host identity transfer approval/execution.

Unknown / needs verification:

- Exact host identity transfer permissions.
- Exact request, risk check, recipient verification, party approval, JoinFolk approval, execution, audit, notification, and rollback permissions.
- Exact relationship to ops/admin, host, personas, profiles, ownership, public sharing, RPC, RLS, and audit logs.

No exact host identity transfer permission behavior is accepted in this draft.

## 22. Ops/Admin Permission Draft

Known permission domains:

- Ops/admin support, override, moderation, approval, rollback, and audit access.

Unknown / needs verification:

- Exact ops/admin permissions.
- Exact support, override, moderation, approval, rollback, audit, and production change permissions.
- Exact relationship to admin roles, product domains, RPC, RLS, SECURITY DEFINER, storage policies, and audit logs.

No exact ops/admin permission behavior is accepted in this draft.

## 23. Public Sharing / Web/Public Permission Draft

Known permission domains:

- Public sharing and Web/Public exposure.

Unknown / needs verification:

- Exact public sharing permissions.
- Exact public/private/protected exposure permissions.
- Exact Web/Public permissions.
- Exact relationship to event lifecycle, feed/discovery, media/gallery, messaging where applicable, viewer roles, personas, tiers, ownership, RPC, RLS, and storage policies.

No exact public sharing or Web/Public permission behavior is accepted in this draft.

## 24. Storage / RPC / RLS Permission Draft

Known permission domains:

- Storage upload/read/update/delete.
- RPC execution authority.
- SECURITY DEFINER boundaries.
- Production change approval.

Unknown / needs verification:

- Exact storage permissions.
- Exact RPC execution permissions.
- Exact RLS policies.
- Exact SECURITY DEFINER behavior.
- Exact production change approval permissions.
- Exact relationship between auth, RLS, RPC, SECURITY DEFINER, storage policies, ops/admin, and frontend behavior.

No exact storage, RPC, RLS, SECURITY DEFINER, or production change permission behavior is accepted in this draft.

## 25. Draft Matrix Template

This is a template only. Do not treat any cell as an accepted allow/deny value.

| Actor concept | Resource domain | Action family | Context | Surface | Enforcement | Permission value |
| --- | --- | --- | --- | --- | --- | --- |
| Unknown / Needs verification | Event lifecycle | View / create / edit / publish / manage | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Unknown / Needs verification | Ticketing | Purchase / request / own / transfer / check in | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Unknown / Needs verification | Reservations | Create / own / approve / manage | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Unknown / Needs verification | Wallet/ownership | View / own / transfer / manage | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Unknown / Needs verification | Media/gallery | Upload / view / replace / remove / moderate | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Backend/RPC/RLS/storage | Unknown / Needs verification |
| Unknown / Needs verification | Feed/discovery | Include / view / expose / moderate | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Unknown / Needs verification | Messaging | View / send / moderate / notify | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Unknown / Needs verification | Notifications | Create / deliver / view / manage | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Unknown / Needs verification | Staff scanner/check-in | Queue access / check in / manage | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Unknown / Needs verification | Venue/business tools | View / create / edit / manage | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Backend/RPC/RLS/storage | Unknown / Needs verification |
| Unknown / Needs verification | Host identity transfer | Request / approve / execute / audit / rollback | Unknown / Needs verification | Dashboard/Ops / Backend | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Unknown / Needs verification | Ops/admin | Support / override / moderate / approve / audit | Unknown / Needs verification | Dashboard/Ops / Backend | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Unknown / Needs verification | Public sharing | Expose / view / manage | Unknown / Needs verification | Web/Public / Mobile / Dashboard/Ops / Backend | Backend/RPC/RLS/storage/auth | Unknown / Needs verification |
| Unknown / Needs verification | Storage / RPC / RLS | Upload / read / update / delete / execute | Unknown / Needs verification | Backend | Backend/RPC/RLS/storage/auth | Unknown / Needs verification |

## 26. Cross-Surface Consistency Requirements

### Mobile

Unknown / needs verification:

- Exact mobile permission behavior.
- Which permission decisions mobile displays but does not enforce.
- Which mobile behavior must match Dashboard/Ops, Web/Public, and Supabase backend.

### Dashboard/Ops

Unknown / needs verification:

- Exact Dashboard/Ops permission behavior.
- Exact admin route/component permission behavior.
- Which Dashboard/Ops behavior must match mobile, Web/Public, and Supabase backend.

### Web/Public

Unknown / needs verification:

- Exact Web/Public permission behavior.
- Exact public/private/protected exposure permission behavior.
- Which Web/Public behavior must match mobile, Dashboard/Ops, and Supabase backend.

### Supabase Backend

Unknown / needs verification:

- Exact backend permission behavior.
- Exact auth, RLS, RPC, SECURITY DEFINER, storage, and production change enforcement.
- Which backend behavior is authoritative for each permission domain.

## 27. Security Risks

Security risks to verify:

- Frontend-only checks being treated as permission enforcement.
- Exact allow/deny rules inferred without verification.
- Viewer roles, personas, tiers, staff roles, host, or ops/admin concepts treated as canonical before verification.
- RLS, RPC, SECURITY DEFINER, storage, or auth behavior assumed without accepted contracts.
- Product domains implementing inconsistent permission rules.
- Production SQL, migrations, functions, RLS, storage, or auth changes without explicit approval.

## 28. Privacy Risks

Privacy risks to verify:

- Private/protected/non-public data exposed due to incorrect permissions.
- Messaging, media, feed, ticketing, reservation, wallet/ownership, venue/business, or public sharing permissions exposing data to unauthorized actors.
- Ops/admin or audit access exposed without accepted permissions.
- Web/Public exposure permissions diverging from backend authority.

## 29. Determinism Risks

Determinism risks to verify:

- Permissions interpreted differently across mobile, Dashboard/Ops, Web/Public, and Supabase backend.
- Actor concepts mapped differently across product domains.
- Resource/action/context combinations evaluated inconsistently.
- Frontend display state diverging from backend enforcement.
- SECURITY DEFINER or RPC behavior diverging from RLS expectations.

## 30. Maintainability Risks

Maintainability risks to verify:

- Permission logic scattered across frontend, backend, RPCs, RLS, storage policies, and product modules without clear ownership.
- PermissionMatrix.md duplicating or contradicting SecurityModel.md, PublicExposureRules.md, or ThreatModel.md.
- Exact permission values added without verification.
- Role, permission, policy, schema, RPC, route, component, or storage policy names treated as canonical before verification.

## 31. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has auth/user/profile concepts.
- JoinFolk has personas and tiers.
- JoinFolk has viewer roles.
- JoinFolk has event lifecycle.
- JoinFolk has event ownership and host concepts.
- JoinFolk has ticketing, reservations, wallet/ownership, media/gallery, feed/discovery, notifications, staff scanner/check-in, venue/business tools, host identity transfer, ops/admin concepts, and public sharing.
- JoinFolk has messaging concepts or possible messaging concepts.
- JoinFolk uses Supabase or Supabase-like backend concepts.
- JoinFolk has RLS, RPC, SECURITY DEFINER, storage policy, and auth concepts.
- Prior role/persona/tier/staff names are known concepts only and not accepted canonical permission values.
- Security-sensitive behavior must not be enforced only by frontend code.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact accepted permission implementation across actors, resources, actions, contexts, surfaces, and enforcement layers.

## 32. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact permission matrix.
- Exact viewer-role permissions.
- Exact persona/tier permissions.
- Exact staff role permissions.
- Exact host permissions.
- Exact ops/admin permissions.
- Exact public/private/protected permission rules.
- Exact RLS policies.
- Exact RPC contracts.
- Exact SECURITY DEFINER behavior.
- Exact storage policies.
- Exact frontend route/component permission behavior.
- Exact cross-surface permission consistency.
- Exact event lifecycle permissions.
- Exact ticketing permissions.
- Exact reservation permissions.
- Exact wallet/ownership permissions.
- Exact media/gallery permissions.
- Exact feed/discovery permissions.
- Exact messaging permissions where applicable.
- Exact notification permissions.
- Exact staff scanner/check-in permissions.
- Exact venue/business tools permissions.
- Exact host identity transfer permissions.
- Exact production change approval permissions.

## 33. Acceptance Criteria for v1.0

Permission Matrix v1.0 can be accepted only after verification establishes:

- Accepted permission domain vocabulary.
- Accepted actor model.
- Accepted resource model.
- Accepted action model.
- Accepted context model.
- Accepted surface model.
- Accepted enforcement model.
- Accepted viewer-role permissions.
- Accepted persona/tier permissions.
- Accepted staff role permissions.
- Accepted host permissions.
- Accepted ops/admin permissions.
- Accepted public/private/protected permission rules.
- Accepted RLS policies.
- Accepted RPC contracts and SECURITY DEFINER boundaries.
- Accepted storage policies.
- Accepted cross-surface permission consistency.
- Accepted relationship to SecurityModel.md, PublicExposureRules.md, and ThreatModel.md.
- Accepted maintainability ownership for permission logic across frontend, backend, RPC, RLS, storage, auth, ops/admin, and production change control.

Until these criteria are met, this document remains non-canonical.

## 34. Open Questions

- What is the accepted permission matrix?
- What actor model is accepted?
- What resource model is accepted?
- What action model is accepted?
- What context model is accepted?
- What surface model is accepted?
- What enforcement model is accepted?
- Which viewer roles are accepted, and what permissions do they have?
- Which personas and tiers are accepted, and what permissions do they have?
- Which staff roles are accepted, and what permissions do they have?
- What host permissions are accepted?
- What ops/admin permissions are accepted?
- What public/private/protected permission rules are accepted?
- What RLS policies enforce permissions?
- What RPC contracts and SECURITY DEFINER boundaries enforce permissions?
- What storage policies enforce permissions?
- What frontend route/component permission behavior is accepted?
- How should PermissionMatrix.md relate to SecurityModel.md, PublicExposureRules.md, and ThreatModel.md?
- Which surfaces support permission-sensitive behavior today: mobile, Dashboard/Ops, Web/Public, and Supabase backend?
