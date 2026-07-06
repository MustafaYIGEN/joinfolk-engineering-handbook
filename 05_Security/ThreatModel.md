# Threat Model

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is the platform-level threat model draft for JoinFolk.

This is a handbook draft. It is not a code audit, not an accepted implementation contract, and not a confirmed vulnerability report. ThreatModel.md is the specific threat-analysis document under the umbrella SecurityModel.md. SecurityModel.md is the umbrella security document. PermissionMatrix.md holds permission-axis structure. PublicExposureRules.md holds public/private/protected exposure-specific rules.

This document does not define confirmed vulnerabilities, exact severities, exact exploit paths, or accepted mitigations. All exact vulnerabilities and controls remain Unknown / Needs verification.

## 3. Threat Model Definition

The Threat Model describes draft threat categories and verification needs for JoinFolk security-sensitive behavior.

Known facts:

- JoinFolk uses Supabase or Supabase-like backend concepts.
- JoinFolk has RLS, RPC, SECURITY DEFINER, storage policy, and auth concepts.
- JoinFolk has auth/user/profile concepts.
- JoinFolk has personas and tiers.
- JoinFolk has viewer roles.
- JoinFolk has event lifecycle.
- JoinFolk has event ownership and host concepts.
- JoinFolk has ticketing, reservations, wallet/ownership, media/gallery, feed/discovery, notifications, staff scanner/check-in, venue/business tools, host identity transfer, ops/admin concepts, and public sharing.
- JoinFolk has messaging concepts or possible messaging concepts.
- JoinFolk has mobile and dashboard surfaces.
- JoinFolk may have Web/Public surfaces.
- Security-sensitive behavior must not be enforced only by frontend code.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact vulnerabilities.
- Exact threat severity.
- Exact exploit paths.
- Exact mitigations.
- Exact security controls.
- Exact implementation behavior across auth, permissions, public exposure, RLS, RPC, storage, ops/admin, auditability, and production change control.

## 4. Relationship to SecurityModel.md

SecurityModel.md is the umbrella security document.

ThreatModel.md focuses on threat-analysis categories and verification needs. It should not duplicate SecurityModel.md, PermissionMatrix.md, or PublicExposureRules.md in full.

Related documents:

- SecurityModel.md: umbrella security model.
- PermissionMatrix.md: permission-axis structure.
- PublicExposureRules.md: public/private/protected exposure-specific rules.

Unknown / needs verification:

- Exact boundary between ThreatModel.md, SecurityModel.md, PermissionMatrix.md, and PublicExposureRules.md.
- Which verified findings or controls should live in each document.

## 5. Threat Modeling Scope

Draft threat modeling scope includes:

- Frontend-only enforcement risk.
- Auth/session risk.
- Authorization/permission drift risk.
- RLS bypass or incomplete RLS risk.
- RPC / SECURITY DEFINER authority risk.
- Storage policy exposure risk.
- Public exposure risk.
- Feed/discovery exposure risk.
- Media/gallery exposure risk.
- Messaging/privacy exposure risk.
- Ticketing/reservation/wallet ownership risk.
- Staff scanner/check-in authority risk.
- Host identity transfer risk.
- Ops/admin override/moderation risk.
- Notification leakage risk.
- Cross-surface inconsistency risk.
- Production change control risk.
- Audit/logging gap risk.
- Performance/security interaction risk where media, feed, public surfaces, or realtime-like behavior may exist.
- Maintainability/security drift risk.

These are threat categories to verify, not confirmed vulnerabilities.

## 6. Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend/RPC/RLS/storage/auth enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Presentation.
- Navigation.
- Form UX.
- Loading, empty, and error states.
- Client-side validation for usability.
- Non-authoritative display filtering.
- Threat or permission state display where backed by authoritative backend data.

Unknown / needs verification:

- Exact frontend route/component behavior.
- Exact mobile, Dashboard/Ops, and Web/Public behavior.

### What backend/RPC/RLS/storage/auth must enforce

Backend/RPC/RLS/storage/auth must enforce security-sensitive behavior.

Security-sensitive enforcement includes:

- Authentication.
- Authorization.
- Permission checks.
- Public exposure decisions.
- RLS-protected data access.
- RPC execution authority.
- SECURITY DEFINER boundaries.
- Storage upload/read/update/delete authority.
- Ops/admin authority.
- Host identity transfer authority.
- Ticketing, reservation, wallet/ownership, staff scanner/check-in, feed, media, messaging, notification, venue/business, and public sharing authority.
- Production SQL, migrations, functions, RLS, storage, and auth change approval.

Unknown / needs verification:

- Exact auth behavior.
- Exact permission rules.
- Exact public exposure rules.
- Exact RLS policies.
- Exact RPC contracts.
- Exact SECURITY DEFINER behavior.
- Exact storage policies.
- Exact ops/admin behavior.
- Exact audit behavior.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Authentication or authorization.
- Permission decisions.
- Public/private/protected exposure rules.
- RLS-equivalent data access rules.
- RPC authority.
- SECURITY DEFINER boundaries.
- Storage access or upload authorization.
- Ops/admin authorization.
- Host identity transfer authorization.
- Ticketing, reservation, wallet/ownership, staff scanner/check-in, feed visibility, media visibility/upload, messaging, notification, or public sharing security-sensitive behavior.
- Audit log creation or integrity.
- Production SQL, migrations, functions, RLS, storage, or auth change approval.

## 7. Threat Categories Draft

Draft threat categories:

- Frontend-only enforcement risk.
- Auth/session risk.
- Authorization/permission drift risk.
- RLS bypass or incomplete RLS risk.
- RPC / SECURITY DEFINER authority risk.
- Storage policy exposure risk.
- Public exposure risk.
- Feed/discovery exposure risk.
- Media/gallery exposure risk.
- Messaging/privacy exposure risk.
- Ticketing/reservation/wallet ownership risk.
- Staff scanner/check-in authority risk.
- Host identity transfer risk.
- Ops/admin override/moderation risk.
- Notification leakage risk.
- Cross-surface inconsistency risk.
- Production change control risk.
- Audit/logging gap risk.
- Performance/security interaction risk.
- Maintainability/security drift risk.

Unknown / needs verification:

- Whether any threat category maps to confirmed vulnerabilities.
- Exact severity, likelihood, impact, exploit paths, and mitigations.

## 8. Non-Accepted Threat Areas

The following areas are not accepted yet:

- Exact vulnerabilities.
- Exact threat severity.
- Exact exploit paths.
- Exact mitigations.
- Exact auth model.
- Exact permission matrix.
- Exact public exposure rules.
- Exact RLS policies.
- Exact RPC contracts.
- Exact SECURITY DEFINER behavior.
- Exact storage policies.
- Exact ops/admin security model.
- Exact audit logging behavior.
- Exact production change enforcement model.
- Exact cross-surface security consistency.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 9. Authentication Threat Draft

Known facts:

- JoinFolk has auth/user/profile concepts.
- JoinFolk uses Supabase or Supabase-like backend concepts.

Unknown / needs verification:

- Exact auth/session risks.
- Exact auth model.
- Exact session behavior.
- Exact user/profile relationship.
- Exact controls and mitigations.

No authentication threat is confirmed in this draft.

## 10. Authorization / Permission Drift Threat Draft

Known facts:

- JoinFolk has personas and tiers.
- JoinFolk has viewer roles.
- Security-sensitive behavior must not be enforced only by frontend code.

Unknown / needs verification:

- Exact authorization risks.
- Exact permission drift risks.
- Exact permission matrix.
- Whether mobile, Dashboard/Ops, Web/Public, and backend interpret permissions consistently.
- Exact controls and mitigations.

No authorization or permission drift threat is confirmed in this draft.

## 11. RLS Threat Draft

Known facts:

- JoinFolk has RLS concepts.
- Backend/RPC/RLS/storage/auth must enforce security-sensitive behavior.

Unknown / needs verification:

- Exact RLS risks.
- Exact RLS policies.
- Whether any RLS policies are incomplete or bypassed.
- Exact relationship between RLS, RPC, SECURITY DEFINER, auth, ownership, public exposure, and storage.
- Exact controls and mitigations.

No RLS threat is confirmed in this draft.

## 12. RPC / SECURITY DEFINER Threat Draft

Known facts:

- JoinFolk has RPC concepts.
- JoinFolk has SECURITY DEFINER concepts.

Unknown / needs verification:

- Exact RPC risks.
- Exact SECURITY DEFINER risks.
- Exact RPC parameters and return shapes.
- Exact SECURITY DEFINER behavior.
- Whether RPC authority diverges from intended RLS/auth/permission behavior.
- Exact controls and mitigations.

No RPC or SECURITY DEFINER threat is confirmed in this draft.

## 13. Storage Policy Threat Draft

Known facts:

- JoinFolk has storage policy concepts.
- Media visibility and upload behavior are security-sensitive where private/protected/non-public media may be exposed.

Unknown / needs verification:

- Exact storage policy risks.
- Exact storage policies.
- Exact bucket or object access behavior.
- Whether storage object access diverges from media visibility rules.
- Exact controls and mitigations.

No storage policy threat is confirmed in this draft.

## 14. Public Exposure Threat Draft

Known facts:

- JoinFolk has public sharing.
- Public exposure can be security-sensitive where private/protected/non-public data or media may be exposed.

Unknown / needs verification:

- Exact public exposure risks.
- Exact public exposure rules.
- Exact Web/Public behavior.
- Exact public/private/protected visibility model.
- Whether public sharing exposes protected data.
- Exact controls and mitigations.

No public exposure threat is confirmed in this draft.

## 15. Feed / Discovery Threat Draft

Known facts:

- JoinFolk has feed/discovery.
- Feed visibility is security-sensitive where private/protected/non-public data may be exposed.

Unknown / needs verification:

- Exact feed/discovery exposure risks.
- Exact feed visibility behavior.
- Exact event lifecycle filtering behavior.
- Exact public/private/protected filtering behavior.
- Exact host identity and media exposure behavior in feeds.
- Exact controls and mitigations.

No feed/discovery threat is confirmed in this draft.

## 16. Media / Gallery Threat Draft

Known facts:

- JoinFolk has media/gallery.
- Media visibility and upload behavior are security-sensitive where private/protected/non-public media may be exposed.

Unknown / needs verification:

- Exact media/gallery exposure risks.
- Exact media upload risks.
- Exact storage policy relationship.
- Exact public media visibility behavior.
- Exact controls and mitigations.

No media/gallery threat is confirmed in this draft.

## 17. Messaging Threat Draft

Known facts:

- JoinFolk has messaging concepts or possible messaging concepts.
- Messaging is security-sensitive where private conversation, participant-only communication, staff/host communication, or operational messages may be exposed.

Unknown / needs verification:

- Whether messaging exists as accepted behavior.
- Exact messaging privacy risks.
- Exact conversation/thread access behavior.
- Exact participant-only, staff/host, and operational message boundaries.
- Exact realtime-like behavior where applicable.
- Exact controls and mitigations.

No messaging threat is confirmed in this draft.

## 18. Ticketing / Reservation / Wallet Threat Draft

Known facts:

- JoinFolk has ticketing.
- JoinFolk has reservations.
- JoinFolk has wallet/ownership.
- Ticketing, reservation, wallet/ownership, and public sharing can be security-sensitive.

Unknown / needs verification:

- Exact ticketing risks.
- Exact reservation risks.
- Exact wallet/ownership risks.
- Exact ownership, transfer, request, purchase, approval, and management behavior.
- Exact controls and mitigations.

No ticketing, reservation, or wallet threat is confirmed in this draft.

## 19. Staff Scanner / Check-in Threat Draft

Known facts:

- JoinFolk has staff scanner/check-in.
- Staff scanner/check-in can be security-sensitive.

Unknown / needs verification:

- Exact staff scanner risks.
- Exact check-in authority risks.
- Exact queue access behavior.
- Exact staff/scanner permissions.
- Exact controls and mitigations.

No staff scanner/check-in threat is confirmed in this draft.

## 20. Host Identity Transfer Threat Draft

Known facts:

- JoinFolk has host identity transfer.
- Host identity transfer is security-sensitive.

Unknown / needs verification:

- Exact host identity transfer risks.
- Exact transfer approval/execution behavior.
- Exact persona/profile/ownership/public display effects.
- Exact audit behavior.
- Exact controls and mitigations.

No host identity transfer threat is confirmed in this draft.

## 21. Ops/Admin Threat Draft

Known facts:

- JoinFolk has ops/admin concepts.
- Ops/admin behavior is security-sensitive.

Unknown / needs verification:

- Exact ops/admin risks.
- Exact admin role and permission behavior.
- Exact override, moderation, approval, support, rollback, and audit behavior.
- Exact controls and mitigations.

No ops/admin threat is confirmed in this draft.

## 22. Notification Leakage Threat Draft

Known facts:

- JoinFolk has notifications.

Unknown / needs verification:

- Exact notification leakage risks.
- Exact notification creation, delivery, visibility, and recipient behavior.
- Whether notifications expose private/protected/non-public state.
- Exact controls and mitigations.

No notification leakage threat is confirmed in this draft.

## 23. Cross-Surface Consistency Threat Draft

Known facts:

- JoinFolk has mobile and dashboard surfaces.
- JoinFolk may have Web/Public surfaces.
- JoinFolk uses Supabase or Supabase-like backend concepts.

Unknown / needs verification:

- Exact cross-surface security consistency risks.
- Whether mobile, Dashboard/Ops, Web/Public, and backend enforce or display security decisions consistently.
- Exact controls and mitigations.

No cross-surface consistency threat is confirmed in this draft.

## 24. Production Change Control Threat Draft

Known facts:

- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact production change control risks.
- Exact production change enforcement model.
- Exact approval behavior.
- Exact rollback/reversal behavior.
- Exact controls and mitigations.

No production change control threat is confirmed in this draft.

## 25. Auditability / Logging Threat Draft

Known facts:

- Ops/admin behavior is security-sensitive.
- Host identity transfer is security-sensitive.

Unknown / needs verification:

- Exact audit/logging gap risks.
- Exact audit logging behavior.
- Which security-sensitive actions require audit logs.
- Exact audit access, integrity, retention, and visibility behavior.
- Exact controls and mitigations.

No auditability or logging threat is confirmed in this draft.

## 26. Performance / Abuse Threat Draft

Known facts:

- Performance/security interaction risk may exist where media, feed, public surfaces, or realtime-like behavior may exist.

Unknown / needs verification:

- Exact performance/security interaction risks.
- Exact abuse risks.
- Whether media, feed, public surfaces, or realtime-like behavior create performance or abuse concerns.
- Exact controls and mitigations.

No performance or abuse threat is confirmed in this draft.

## 27. Draft Threat Matrix Template

This is a template only. Do not treat any cell as an accepted severity, likelihood, impact, mitigation, or confirmed vulnerability.

| Threat category | Product/domain area | Threat status | Severity | Likelihood | Impact | Mitigation/control |
| --- | --- | --- | --- | --- | --- | --- |
| Frontend-only enforcement risk | Cross-platform | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Auth/session risk | Auth/user/profile | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Authorization/permission drift risk | Personas / viewer roles / permissions | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| RLS risk | Supabase backend | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| RPC / SECURITY DEFINER risk | Supabase backend | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Storage policy exposure risk | Storage / media | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Public exposure risk | Public sharing / Web/Public | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Feed/discovery exposure risk | Feed/discovery | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Media/gallery exposure risk | Media/gallery | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Messaging/privacy exposure risk | Messaging | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Ticketing/reservation/wallet ownership risk | Ticketing / reservations / wallet | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Staff scanner/check-in authority risk | Staff scanner/check-in | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Host identity transfer risk | Host identity transfer | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Ops/admin override/moderation risk | Ops/admin | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Notification leakage risk | Notifications | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Cross-surface inconsistency risk | Mobile / Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Production change control risk | SQL / migrations / functions / RLS / storage / auth | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Audit/logging gap risk | Ops/admin / host identity transfer / product domains | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Performance/security interaction risk | Media / feed / public surfaces / realtime-like behavior | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Maintainability/security drift risk | Cross-platform | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |

## 28. Relationship to Product Domains

### Event lifecycle

Known relationship:

- JoinFolk has event lifecycle.

Unknown / needs verification:

- Exact event lifecycle threat relationship.

### Viewer roles

Known relationship:

- JoinFolk has viewer roles.

Unknown / needs verification:

- Exact viewer role threat relationship.

### Personas and tiers

Known relationship:

- JoinFolk has personas and tiers.

Unknown / needs verification:

- Exact persona/tier threat relationship.

### Ticketing

Known relationship:

- JoinFolk has ticketing.

Unknown / needs verification:

- Exact ticketing threat relationship.

### Reservations

Known relationship:

- JoinFolk has reservations.

Unknown / needs verification:

- Exact reservation threat relationship.

### Wallet/ownership

Known relationship:

- JoinFolk has wallet/ownership.

Unknown / needs verification:

- Exact wallet/ownership threat relationship.

### Media/gallery

Known relationship:

- JoinFolk has media/gallery.

Unknown / needs verification:

- Exact media/gallery threat relationship.

### Feed/discovery

Known relationship:

- JoinFolk has feed/discovery.

Unknown / needs verification:

- Exact feed/discovery threat relationship.

### Messaging

Known relationship:

- JoinFolk has messaging concepts or possible messaging concepts.

Unknown / needs verification:

- Exact messaging threat relationship.

### Notifications

Known relationship:

- JoinFolk has notifications.

Unknown / needs verification:

- Exact notification threat relationship.

### Staff scanner/check-in

Known relationship:

- JoinFolk has staff scanner/check-in.

Unknown / needs verification:

- Exact staff scanner/check-in threat relationship.

### Venue/business tools

Known relationship:

- JoinFolk has venue/business tools.

Unknown / needs verification:

- Exact venue/business tools threat relationship.

### Host identity transfer

Known relationship:

- JoinFolk has host identity transfer.
- Host identity transfer is security-sensitive.

Unknown / needs verification:

- Exact host identity transfer threat relationship.

### Ops/admin

Known relationship:

- JoinFolk has ops/admin concepts.
- Ops/admin behavior is security-sensitive.

Unknown / needs verification:

- Exact ops/admin threat relationship.

### Public sharing

Known relationship:

- JoinFolk has public sharing.
- Public sharing can be security-sensitive.

Unknown / needs verification:

- Exact public sharing threat relationship.

### Supabase backend/storage

Known relationship:

- JoinFolk uses Supabase or Supabase-like backend concepts.
- JoinFolk has RLS, RPC, SECURITY DEFINER, storage policy, and auth concepts.

Unknown / needs verification:

- Exact Supabase backend/storage threat relationship.

## 29. Cross-Surface Consistency Requirements

### Mobile

Unknown / needs verification:

- Exact mobile threat model behavior.
- Which security decisions mobile displays but does not enforce.
- Which mobile behavior must match Dashboard/Ops, Web/Public, and Supabase backend/storage.

### Dashboard/Ops

Unknown / needs verification:

- Exact Dashboard/Ops threat model behavior.
- Which Dashboard/Ops behavior must match mobile, Web/Public, and Supabase backend/storage.
- Exact ops/admin authority and audit behavior.

### Web/Public

Unknown / needs verification:

- Exact Web/Public threat model behavior.
- Exact public exposure, feed, media, messaging, and public sharing threat considerations.
- Which Web/Public behavior must match mobile, Dashboard/Ops, and Supabase backend/storage.

### Supabase Backend / Storage

Unknown / needs verification:

- Exact backend/storage threat model behavior.
- Exact auth, RLS, RPC, SECURITY DEFINER, storage, and production change enforcement.
- Which backend/storage behavior is authoritative for each security-sensitive domain.

## 30. Security Risks

Security risks to verify:

- Frontend-only checks being treated as enforcement.
- Auth/session behavior assumed without accepted model.
- Exact permission rules inferred without verification.
- RLS, RPC, SECURITY DEFINER, storage, or auth behavior assumed without accepted contracts.
- Public exposure rules inferred without verification.
- Ops/admin or host identity transfer authority assumed without accepted contracts.
- Production SQL, migrations, functions, RLS, storage, or auth changes without explicit approval.

No security risk in this section is a confirmed vulnerability unless later verified.

## 31. Privacy Risks

Privacy risks to verify:

- Private/protected/non-public data or media exposed without accepted rules.
- Messaging content or metadata exposed outside accepted participant boundaries.
- Feed/discovery exposing private/protected/non-public data.
- Media/gallery or storage objects exposed outside accepted visibility.
- Notifications exposing private/protected/non-public state.
- Ops/admin or audit data exposed without accepted authority.

No privacy risk in this section is a confirmed vulnerability unless later verified.

## 32. Determinism Risks

Determinism risks to verify:

- Security decisions interpreted differently across mobile, Dashboard/Ops, Web/Public, and Supabase backend/storage.
- Permission, exposure, ownership, and lifecycle state interpreted differently across product domains.
- Frontend display state diverging from backend enforcement.
- RPC or SECURITY DEFINER behavior diverging from RLS expectations.
- Storage access diverging from media visibility rules.
- Audit/logging state diverging from security-sensitive actions.

No determinism risk in this section is a confirmed vulnerability unless later verified.

## 33. Maintainability Risks

Maintainability risks to verify:

- Security logic scattered across frontend, backend, RPCs, RLS, storage policies, and product modules without clear ownership.
- ThreatModel.md duplicating or contradicting SecurityModel.md, PermissionMatrix.md, or PublicExposureRules.md.
- Threat categories being treated as confirmed vulnerabilities before verification.
- Exact severity, exploit paths, or mitigations added without verification.
- Role, permission, exposure state, schema, RPC, RLS policy, route, component, bucket, or storage policy names treated as canonical before verification.

## 34. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk uses Supabase or Supabase-like backend concepts.
- JoinFolk has RLS, RPC, SECURITY DEFINER, storage policy, and auth concepts.
- JoinFolk has auth/user/profile concepts.
- JoinFolk has personas and tiers.
- JoinFolk has viewer roles.
- JoinFolk has event lifecycle.
- JoinFolk has event ownership and host concepts.
- JoinFolk has ticketing, reservations, wallet/ownership, media/gallery, feed/discovery, notifications, staff scanner/check-in, venue/business tools, host identity transfer, ops/admin, and public sharing.
- JoinFolk has messaging concepts or possible messaging concepts.
- JoinFolk has mobile and dashboard surfaces.
- JoinFolk may have Web/Public surfaces.
- Security-sensitive behavior must not be enforced only by frontend code.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact accepted threat model implementation across security-sensitive domains, surfaces, and enforcement layers.

## 35. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact vulnerabilities.
- Exact threat severity.
- Exact exploit paths.
- Exact mitigations.
- Exact auth model.
- Exact permission matrix.
- Exact public exposure rules.
- Exact RLS policies.
- Exact RPC contracts.
- Exact SECURITY DEFINER behavior.
- Exact storage policies.
- Exact ops/admin security model.
- Exact audit logging behavior.
- Exact production change enforcement model.
- Exact cross-surface security consistency.
- Exact threat status for each draft category.
- Exact relationship to SecurityModel.md, PermissionMatrix.md, and PublicExposureRules.md.

## 36. Acceptance Criteria for v1.0

Threat Model v1.0 can be accepted only after verification establishes:

- Accepted threat model vocabulary.
- Accepted threat modeling scope.
- Accepted relationship to SecurityModel.md, PermissionMatrix.md, and PublicExposureRules.md.
- Accepted auth/session threat analysis.
- Accepted authorization/permission drift threat analysis.
- Accepted RLS threat analysis.
- Accepted RPC / SECURITY DEFINER threat analysis.
- Accepted storage policy threat analysis.
- Accepted public exposure threat analysis.
- Accepted feed/discovery threat analysis.
- Accepted media/gallery threat analysis.
- Accepted messaging/privacy threat analysis where applicable.
- Accepted ticketing/reservation/wallet ownership threat analysis.
- Accepted staff scanner/check-in threat analysis.
- Accepted host identity transfer threat analysis.
- Accepted ops/admin threat analysis.
- Accepted notification leakage threat analysis.
- Accepted cross-surface consistency threat analysis.
- Accepted production change control threat analysis.
- Accepted auditability/logging threat analysis.
- Accepted severity, likelihood, impact, mitigation, and ownership model for verified threats.

Until these criteria are met, this document remains non-canonical.

## 37. Open Questions

- What exact vulnerabilities, if any, are verified?
- What threat severity model is accepted?
- What exploit path documentation is accepted?
- What mitigation model is accepted?
- What auth/session threats are accepted?
- What authorization or permission drift threats are accepted?
- What RLS threats are accepted?
- What RPC or SECURITY DEFINER threats are accepted?
- What storage policy threats are accepted?
- What public exposure threats are accepted?
- What feed/discovery exposure threats are accepted?
- What media/gallery exposure threats are accepted?
- What messaging/privacy threats are accepted, if messaging is accepted behavior?
- What ticketing, reservation, or wallet ownership threats are accepted?
- What staff scanner/check-in threats are accepted?
- What host identity transfer threats are accepted?
- What ops/admin threats are accepted?
- What notification leakage threats are accepted?
- What cross-surface consistency threats are accepted?
- What production change control threats are accepted?
- What auditability or logging threats are accepted?
- How should ThreatModel.md relate to SecurityModel.md, PermissionMatrix.md, and PublicExposureRules.md?
- Which surfaces support security-sensitive behavior today: mobile, Dashboard/Ops, Web/Public, and Supabase backend/storage?
