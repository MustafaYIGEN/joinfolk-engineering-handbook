# Security Model

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is the platform-level umbrella security model draft for JoinFolk.

This is a handbook draft. It is not a code audit and is not an accepted implementation contract. PermissionMatrix.md, PublicExposureRules.md, and ThreatModel.md will hold more specific security views; this document does not duplicate those files in full.

All exact implementation behavior remains Unknown / Needs verification unless verified later. Backend/RPC/RLS/storage/auth must enforce security-sensitive behavior. Frontend behavior is UX only where security-sensitive. Prior implementation notes must not be treated as canonical.

## 3. Security Model Definition

The Security Model describes platform-level security boundaries for authentication, authorization, backend authority, RLS, RPC, storage, public exposure, personas, viewer roles, ownership, ops/admin, auditability, production change control, and cross-surface consistency.

Known facts:

- JoinFolk uses Supabase or Supabase-like backend concepts.
- JoinFolk has RLS and SECURITY DEFINER RPC concepts.
- JoinFolk has storage buckets/policies concepts.
- JoinFolk has auth/user/profile concepts.
- JoinFolk has personas and tiers.
- JoinFolk has viewer roles.
- JoinFolk has event lifecycle.
- JoinFolk has ticketing, reservations, wallet/ownership, media/gallery, notifications, staff scanner/check-in, venue/business tools, feed/discovery, messaging, host identity transfer, ops/admin, and public sharing concepts.
- Security-sensitive behavior must not be enforced only by frontend code.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact auth model.
- Exact permission matrix.
- Exact public exposure rules.
- Exact RLS policies.
- Exact RPC contracts and SECURITY DEFINER behavior.
- Exact storage policies.
- Exact audit logging and production change enforcement behavior.

## 4. Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend/RPC/RLS/storage/auth enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Presentation.
- Navigation.
- Form UX.
- Loading, empty, and error states.
- Client-side validation for usability.
- Local draft state.
- Non-authoritative display filtering.

Unknown / needs verification:

- Exact frontend ownership for mobile, Dashboard/Ops, and Web/Public.
- Exact frontend service boundaries.
- Exact route/component ownership.

### What backend/RPC/RLS/storage/auth must enforce

Backend/RPC/RLS/storage/auth must enforce security-sensitive behavior.

Security-sensitive behavior includes:

- Authentication.
- Authorization.
- Permission checks.
- Persona/tier boundaries.
- Viewer role boundaries.
- Ownership boundaries.
- Public exposure boundaries.
- Private/protected/non-public data visibility.
- Storage upload, read, update, and delete authority.
- RPC and SECURITY DEFINER authority.
- Ops/admin authority.
- Host identity transfer authority.
- Ticketing, reservation, wallet/ownership, staff scanner/check-in, feed visibility, media visibility/upload, messaging, and public sharing authority.
- Auditability where required or expected.
- Production SQL, migrations, functions, RLS, storage, and auth change approval.

Unknown / needs verification:

- Exact auth behavior.
- Exact permissions.
- Exact RLS policies.
- Exact RPC contracts.
- Exact SECURITY DEFINER behavior.
- Exact storage policies.
- Exact audit behavior.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Authentication or authorization.
- Permission decisions.
- RLS-equivalent data access rules.
- RPC authority.
- SECURITY DEFINER boundaries.
- Storage access or upload authorization.
- Public exposure decisions.
- Persona/tier authorization.
- Viewer role authorization.
- Ownership authorization.
- Ops/admin authorization.
- Host identity transfer authorization.
- Ticketing, reservation, wallet/ownership, staff scanner/check-in, feed visibility, media visibility/upload, messaging, or public sharing security-sensitive rules.
- Audit log creation or integrity.
- Production SQL, migrations, functions, RLS, storage, or auth change approval.

## 5. Security Domains Draft

Security domains covered by this umbrella model:

- Authentication.
- Authorization.
- Backend authority.
- Frontend UX limits.
- RLS.
- RPC / SECURITY DEFINER boundaries.
- Storage policy boundaries.
- Public exposure boundaries.
- Persona/tier boundaries.
- Viewer role boundaries.
- Ownership boundaries.
- Ops/admin boundaries.
- Auditability.
- Change control / production change approval.
- Cross-surface consistency across mobile, Dashboard/Ops, Web/Public, and Supabase backend.

Unknown / needs verification:

- Exact domain ownership.
- Exact accepted policies for each domain.
- Exact relationship between this umbrella model and PermissionMatrix.md, PublicExposureRules.md, and ThreatModel.md.

## 6. Authentication Draft

Known facts:

- JoinFolk has auth/user/profile concepts.
- JoinFolk uses Supabase or Supabase-like backend concepts.

Unknown / needs verification:

- Exact auth model.
- Exact user/profile relationship.
- Exact session, identity, or authentication behavior.
- Exact cross-surface auth behavior across mobile, Dashboard/Ops, Web/Public, and Supabase backend.
- Which auth behavior is enforced by backend/auth infrastructure.

No exact auth behavior is accepted in this draft.

## 7. Authorization Draft

Known facts:

- JoinFolk has personas and tiers.
- JoinFolk has viewer roles.
- Security-sensitive behavior must not be enforced only by frontend code.

Unknown / needs verification:

- Exact authorization model.
- Exact permission behavior.
- Exact permission matrix.
- Exact relationship between viewer roles, personas, tiers, ownership, and product-domain authority.
- Exact cross-surface authorization behavior.

No exact permission behavior is accepted in this draft.

## 8. RLS Draft

Known facts:

- JoinFolk has RLS concepts.
- Backend/RPC/RLS/storage/auth must enforce security-sensitive behavior.

Unknown / needs verification:

- Exact RLS policies.
- Which tables or entities are protected by RLS.
- How RLS interacts with RPC, SECURITY DEFINER, storage policies, auth, personas, viewer roles, ownership, and public exposure.
- Exact RLS behavior across product domains.

No exact RLS policy is accepted in this draft.

## 9. RPC / SECURITY DEFINER Draft

Known facts:

- JoinFolk has SECURITY DEFINER RPC concepts.
- JoinFolk has RPC concepts.

Unknown / needs verification:

- Exact RPC contracts.
- Exact RPC parameters and return shapes.
- Exact SECURITY DEFINER behavior.
- Which RPCs are security-sensitive.
- Which RPCs use SECURITY DEFINER behavior.
- How RPCs enforce or interact with RLS, auth, storage, personas, viewer roles, ownership, ops/admin, and public exposure.

No exact RPC or SECURITY DEFINER behavior is accepted in this draft.

## 10. Storage Policy Draft

Known facts:

- JoinFolk has storage buckets/policies concepts.
- Media visibility and upload behavior are security-sensitive where private/protected/non-public media may be exposed.

Unknown / needs verification:

- Exact storage bucket model.
- Exact storage policies.
- Exact storage upload, read, update, delete, and public access behavior.
- Exact relationship between storage policies, media/gallery, public sharing, feed/discovery, venue/business tools, profiles/avatars, and backend/RPC/RLS.

No exact storage policy is accepted in this draft.

## 11. Public Exposure Draft

Known facts:

- JoinFolk has public sharing concepts.
- Feed visibility is security-sensitive where private/protected/non-public data may be exposed.
- Media visibility and upload behavior are security-sensitive where private/protected/non-public media may be exposed.
- Messaging is security-sensitive where private conversation, participant-only communication, staff/host communication, or operational messages may be exposed.

Unknown / needs verification:

- Exact public exposure rules.
- Exact public/private/protected visibility model.
- Exact Web/Public exposure behavior.
- Exact relationship between public exposure, event lifecycle, viewer roles, personas, tiers, ownership, media, feed, messaging, ticketing, reservations, venue/business tools, and ops/admin.

No exact public exposure rule is accepted in this draft.

## 12. Persona / Tier / Viewer Role Security Draft

Known facts:

- JoinFolk has personas and tiers.
- JoinFolk has viewer roles.

Unknown / needs verification:

- Exact persona security boundaries.
- Exact tier security boundaries.
- Exact viewer role boundaries.
- Exact relationship between personas, tiers, viewer roles, ownership, public exposure, and product-domain authorization.

No exact persona, tier, or viewer role security behavior is accepted in this draft.

## 13. Ownership Security Draft

Known facts:

- JoinFolk has wallet/ownership concepts.
- Ticketing, reservation, wallet/ownership, staff scanner/check-in, and public sharing can be security-sensitive.
- Host identity transfer may affect identity, persona, ownership, public display, event/business authority, and audit history.

Unknown / needs verification:

- Exact ownership security model.
- Exact event ownership behavior.
- Exact ticket ownership behavior.
- Exact wallet ownership behavior.
- Exact reservation ownership behavior.
- Exact venue/business ownership behavior.
- Exact host identity transfer ownership effects.

No exact ownership security behavior is accepted in this draft.

## 14. Ops/Admin Security Draft

Known facts:

- Ops/admin behavior is security-sensitive.
- Backend/RPC/RLS/storage/auth must enforce security-sensitive behavior.

Unknown / needs verification:

- Exact ops/admin security model.
- Exact admin role model.
- Exact admin permission model.
- Exact support, approval, override, moderation, rollback/reversal, and audit behavior.
- Exact relationship between ops/admin and product-domain security.

No exact ops/admin security rule is accepted in this draft.

## 15. Auditability Draft

Known facts:

- Auditability is required or expected for security-sensitive admin behavior.
- Host identity transfer may affect audit history.

Unknown / needs verification:

- Exact audit logging behavior.
- Which security-sensitive actions require audit logs.
- Exact audit log schema, access, retention, integrity, and visibility.
- Exact relationship between audit logs, ops/admin, host identity transfer, production changes, and product-domain actions.

No exact audit behavior is accepted in this draft.

## 16. Production Change Security Draft

Known facts:

- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact production change enforcement model.
- Exact approval process for production SQL, migrations, functions, RLS, storage, and auth changes.
- Exact rollback/reversal model.
- Exact auditability for production changes.
- Which systems enforce production change approval.

No exact production migration or change enforcement behavior is accepted in this draft.

## 17. Relationship to Product Domains

### Event lifecycle

Known relationship:

- JoinFolk has event lifecycle.

Unknown / needs verification:

- Exact event lifecycle security relationship.

### Viewer roles

Known relationship:

- JoinFolk has viewer roles.

Unknown / needs verification:

- Exact viewer role security relationship.

### Personas and tiers

Known relationship:

- JoinFolk has personas and tiers.

Unknown / needs verification:

- Exact persona/tier security relationship.

### Ticketing

Known relationship:

- Ticketing can be security-sensitive.

Unknown / needs verification:

- Exact ticketing security relationship.

### Reservations

Known relationship:

- Reservations can be security-sensitive.

Unknown / needs verification:

- Exact reservation security relationship.

### Wallet/ownership

Known relationship:

- Wallet/ownership can be security-sensitive.

Unknown / needs verification:

- Exact wallet/ownership security relationship.

### Media/gallery

Known relationship:

- Media visibility and upload behavior are security-sensitive where private/protected/non-public media may be exposed.

Unknown / needs verification:

- Exact media/gallery security relationship.

### Feed/discovery

Known relationship:

- Feed visibility is security-sensitive where private/protected/non-public data may be exposed.

Unknown / needs verification:

- Exact feed/discovery security relationship.

### Messaging

Known relationship:

- Messaging is security-sensitive where private conversation, participant-only communication, staff/host communication, or operational messages may be exposed.

Unknown / needs verification:

- Exact messaging security relationship.

### Notifications

Known relationship:

- JoinFolk has notifications concepts.

Unknown / needs verification:

- Exact notification security relationship.

### Staff scanner/check-in

Known relationship:

- Staff scanner/check-in can be security-sensitive.

Unknown / needs verification:

- Exact staff scanner/check-in security relationship.

### Venue/business tools

Known relationship:

- JoinFolk has venue/business tools concepts.

Unknown / needs verification:

- Exact venue/business tools security relationship.

### Host identity transfer

Known relationship:

- Host identity transfer is security-sensitive.

Unknown / needs verification:

- Exact host identity transfer security model.

### Ops/admin

Known relationship:

- Ops/admin behavior is security-sensitive.

Unknown / needs verification:

- Exact ops/admin security model.

### Public sharing

Known relationship:

- Public sharing can be security-sensitive.

Unknown / needs verification:

- Exact public sharing security relationship.

## 18. Cross-Surface Consistency Requirements

### Mobile

Known relationship:

- JoinFolk has mobile surfaces.

Unknown / needs verification:

- Exact mobile security behavior.
- Which security decisions mobile may display but not enforce.
- Which mobile behavior must match Dashboard/Ops, Web/Public, and Supabase backend behavior.

### Dashboard/Ops

Known relationship:

- Ops/admin behavior is security-sensitive.

Unknown / needs verification:

- Exact Dashboard/Ops security behavior.
- Exact admin role and permission enforcement.
- Which Dashboard/Ops behavior must match mobile, Web/Public, and Supabase backend behavior.

### Web/Public

Known relationship:

- JoinFolk has public sharing concepts.
- Web/Public exposure can be security-sensitive.

Unknown / needs verification:

- Exact Web/Public security behavior.
- Exact public/private/protected visibility behavior.
- Which Web/Public behavior must match mobile, Dashboard/Ops, and Supabase backend behavior.

### Supabase Backend

Known relationship:

- JoinFolk uses Supabase or Supabase-like backend concepts.
- Backend/RPC/RLS/storage/auth must enforce security-sensitive behavior.

Unknown / needs verification:

- Exact Supabase backend security behavior.
- Exact auth, RLS, RPC, SECURITY DEFINER, storage, and audit behavior.
- Exact backend ownership boundaries.

## 19. Security Risks

Security risks to verify:

- Frontend-only checks being treated as enforcement.
- Missing or incomplete RLS policies.
- RPCs bypassing intended security boundaries.
- SECURITY DEFINER behavior expanding authority without accepted rules.
- Storage policies exposing private/protected/non-public files.
- Public exposure of private/protected/non-public data.
- Incorrect persona/tier or viewer-role enforcement.
- Incorrect ownership enforcement.
- Unauthorized ops/admin access.
- Unauthorized host identity transfer.
- Unauthorized ticketing, reservation, wallet/ownership, staff scanner/check-in, feed, media, messaging, or public sharing behavior.
- Production SQL, migrations, functions, RLS, storage, or auth changes without explicit approval.

## 20. Privacy Risks

Privacy risks to verify:

- Private/protected/non-public user, profile, event, media, feed, messaging, ticketing, reservation, wallet/ownership, venue/business, or public sharing data exposed to unauthorized viewers.
- Messaging metadata or content exposed outside accepted participant boundaries.
- Media or storage objects exposed outside accepted visibility.
- Feed/discovery exposing private or protected events.
- Public sharing exposing data without accepted rules.
- Ops/admin data or audit history exposed without accepted authority.

## 21. Determinism Risks

Determinism risks to verify:

- Security decisions differing across mobile, Dashboard/Ops, Web/Public, and Supabase backend.
- Viewer roles, personas, tiers, and ownership interpreted differently across product domains.
- Public exposure differing from backend/RPC/RLS/storage authority.
- RPC behavior diverging from RLS expectations.
- Storage access differing from media visibility rules.
- Ops/admin actions producing inconsistent security state.
- Production change approval state not reflected consistently.

## 22. Maintainability Risks

Maintainability risks to verify:

- Security logic scattered across frontend, backend, RPCs, RLS, storage policies, and product modules without clear ownership.
- Frontend code encoding security-sensitive rules.
- Product domains implementing inconsistent authorization patterns.
- PermissionMatrix.md, PublicExposureRules.md, ThreatModel.md, and this SecurityModel.md duplicating or contradicting each other.
- Schema, RPC, RLS policy, storage policy, role, permission, route, or component names being treated as canonical before verification.

## 23. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk uses Supabase or Supabase-like backend concepts.
- JoinFolk has RLS and SECURITY DEFINER RPC concepts.
- JoinFolk has storage buckets/policies concepts.
- JoinFolk has auth/user/profile concepts.
- JoinFolk has personas and tiers.
- JoinFolk has viewer roles.
- JoinFolk has event lifecycle.
- JoinFolk has ticketing, reservations, wallet/ownership, media/gallery, notifications, staff scanner/check-in, venue/business tools, feed/discovery, messaging, host identity transfer, ops/admin, and public sharing concepts.
- Security-sensitive behavior must not be enforced only by frontend code.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact accepted implementation across auth, RLS, RPC, SECURITY DEFINER, storage, permissions, public exposure, auditability, ops/admin, host identity transfer, production changes, and product domains.

## 24. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact auth model.
- Exact RLS policies.
- Exact RPC contracts.
- Exact SECURITY DEFINER behavior.
- Exact storage policies.
- Exact permission matrix.
- Exact public exposure rules.
- Exact audit logging behavior.
- Exact ops/admin security model.
- Exact host identity transfer security model.
- Exact production change enforcement model.
- Exact persona/tier security boundaries.
- Exact viewer role boundaries.
- Exact ownership security boundaries.
- Exact product-domain security boundaries.
- Exact cross-surface security behavior across mobile, Dashboard/Ops, Web/Public, and Supabase backend.
- Exact relationship between SecurityModel.md, PermissionMatrix.md, PublicExposureRules.md, and ThreatModel.md.

## 25. Acceptance Criteria for v1.0

Security Model v1.0 can be accepted only after verification establishes:

- Accepted security domain vocabulary.
- Accepted auth model.
- Accepted authorization model.
- Accepted permission matrix.
- Accepted RLS policies.
- Accepted RPC contracts and SECURITY DEFINER boundaries.
- Accepted storage policy model.
- Accepted public exposure rules.
- Accepted persona/tier/viewer-role security boundaries.
- Accepted ownership security boundaries.
- Accepted ops/admin security model.
- Accepted auditability behavior.
- Accepted production change approval and enforcement model.
- Accepted cross-surface ownership for mobile, Dashboard/Ops, Web/Public, and Supabase backend.
- Accepted relationship to PermissionMatrix.md, PublicExposureRules.md, and ThreatModel.md.
- Accepted maintainability ownership for security-sensitive frontend, backend, RPC, RLS, storage, auth, ops/admin, audit, and production change behavior.

Until these criteria are met, this document remains non-canonical.

## 26. Open Questions

- What is the accepted auth model?
- What is the accepted authorization model?
- What permission matrix is accepted?
- What RLS policies are accepted?
- What RPC contracts are accepted?
- What SECURITY DEFINER behavior is accepted?
- What storage policies are accepted?
- What public exposure rules are accepted?
- What persona, tier, and viewer-role boundaries are accepted?
- What ownership boundaries are accepted?
- What ops/admin security model is accepted?
- What host identity transfer security model is accepted?
- What audit logging behavior is accepted?
- What production change approval and enforcement model is accepted?
- How should SecurityModel.md relate to PermissionMatrix.md, PublicExposureRules.md, and ThreatModel.md?
- Which security decisions are displayed by frontend surfaces but enforced only by backend/RPC/RLS/storage/auth?
- Which surfaces support security-sensitive behavior today: mobile, Dashboard/Ops, Web/Public, and Supabase backend?
