# Ops/Admin System

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a platform-level draft specification for JoinFolk operations/admin behavior.

This is a handbook draft. It is not a code audit and is not an accepted implementation contract. Ops/admin behavior is security-sensitive and may affect users, personas, events, venues, commerce, ticketing, reservations, media, notifications, staff scanner, host identity transfer, public sharing, and audit history.

All schema names, RPC names, audit event names, dashboard route names, and field names in this document are known concepts only until verified. Prior implementation notes must not be treated as canonical.

## 3. Ops/Admin System Definition

Ops/admin is the JoinFolk platform area for security-sensitive operational review, support, approval, override, transfer, moderation, and audit workflows.

Known facts:

- JoinFolk has dashboard or ops/admin tooling.
- Ops/admin behavior may support operational review, support, approval, override, transfer, moderation, and audit workflows.
- Ops/admin behavior is security-sensitive.
- Backend/RPC/RLS must enforce security-sensitive ops/admin behavior.
- Frontend/dashboard admin behavior is UX only where security-sensitive.
- Audit logging is required or expected for security-sensitive admin behavior.
- Dashboard/Ops may expose admin tools.

Unknown / needs verification:

- Exact ops/admin schema.
- Exact admin role model.
- Exact admin permission model.
- Exact approval, override, moderation, support, audit, and rollback behavior.
- Exact dashboard route/component/service ownership.
- Exact RPC contracts and RLS policies.

## 4. Authority Model

### What frontend/dashboard may own

Frontend/dashboard surfaces may own user experience concerns, subject to backend/RPC/RLS enforcement for security-sensitive behavior.

Frontend/dashboard-owned behavior may include:

- Admin tool presentation.
- Operational review UX.
- Support workflow UX.
- Approval workflow UX.
- Override/correction workflow UX.
- Moderation workflow UX.
- Audit log display UX.
- Transfer dashboard UX.
- Loading, empty, and error states.
- Dashboard/Ops workflow composition.

Unknown / needs verification:

- Exact dashboard route/component/service ownership.
- Exact dashboard admin behavior.
- Exact frontend state model.
- Exact transfer dashboard behavior.

### What backend/RPC/RLS must enforce

Backend, RPC, and RLS must enforce security-sensitive ops/admin behavior.

Security-sensitive behavior includes:

- Admin role authority.
- Admin permission authority.
- Approval workflow authority.
- Override/correction authority.
- Moderation authority.
- Support workflow authority where security-sensitive.
- Audit log creation and integrity.
- Rollback/reversal authority where applicable.
- Host identity transfer authority where applicable.
- Any admin behavior affecting personas, profile identity, event lifecycle, event ownership, venue/business tools, ticketing, reservations, wallet/ownership, media/gallery, notifications, staff scanner/check-in, public sharing, audit logs, or production/backend changes.

Unknown / needs verification:

- Exact RPC contracts.
- Exact RLS policies.
- Exact backend authority boundaries.
- Exact audit log behavior.
- Exact production migration policy enforcement by ops/admin.

### What must never be frontend-only

The following must never rely only on frontend/dashboard checks:

- Admin role checks.
- Admin permission checks.
- Approval decisions.
- Override or correction execution.
- Moderation decisions or execution.
- Security-sensitive support actions.
- Host identity transfer approval or execution.
- Audit log creation or integrity.
- Rollback/reversal behavior.
- Production migration or backend change enforcement where applicable.
- Any operation involving users, personas, profiles, ownership, public sharing, ops/admin authority, or audit logs.

## 5. Known Ops/Admin Concepts Draft

Known ops/admin concepts:

- Dashboard or ops/admin tooling.
- Operational review.
- Support workflows.
- Approval workflows.
- Override workflows.
- Transfer workflows.
- Moderation workflows.
- Audit workflows.
- Audit logging.
- Admin role model.
- Admin permission model.
- Rollback/reversal.
- Production migration or backend change relationship where applicable.
- Transfer dashboard behavior.

Known related product areas:

- Personas and tiers.
- Profile identity.
- Event lifecycle.
- Event ownership.
- Venue/business tools.
- Ticketing.
- Reservations.
- Wallet/ownership.
- Media/gallery.
- Notifications.
- Staff scanner/check-in.
- Host identity transfer.
- Public sharing.
- Audit logs.
- Migrations or backend changes where applicable.

Unknown / needs verification:

- Exact domain vocabulary.
- Which concepts exist as persisted backend entities.
- Which concepts exist only as Dashboard/Ops UX.
- Which concepts are authoritative in backend/RPC/RLS.

## 6. Known Schema / RPC / Audit Concept Names Draft

Prior project context mentioned audit event-like concepts.

Prior project context mentioned migration-like concepts.

Prior project context mentioned admin/RPC-like behavior, such as:

- `admin_execute_host_identity_transfer_v1`

This name is a known concept only and must not be treated as an accepted canonical RPC contract until verified.

Unknown / needs verification:

- Whether `admin_execute_host_identity_transfer_v1` exists in the accepted backend.
- Whether this name is current.
- Whether any ops/admin schema names exist.
- Whether any audit event names are accepted.
- Whether any dashboard route names are accepted.
- Whether any migration-like names are accepted.
- Exact schema names.
- Exact field names.
- Exact RPC parameters.
- Exact RPC return shapes.
- Exact audit event names.
- Exact RLS behavior.

## 7. Non-Accepted Ops/Admin Areas

The following areas are not accepted yet:

- Exact ops/admin schema.
- Exact admin role model.
- Exact admin permission model.
- Exact approval workflow behavior.
- Exact override behavior.
- Exact moderation behavior.
- Exact support workflow behavior.
- Exact audit log behavior.
- Exact dashboard route/component/service ownership.
- Exact RPC contracts.
- Exact RLS policies.
- Exact rollback/reversal behavior.
- Exact production migration policy enforcement by ops/admin.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 8. Admin Role / Permission Draft

Known facts:

- Ops/admin behavior is security-sensitive.
- Backend/RPC/RLS must enforce security-sensitive ops/admin behavior.
- Exact admin role model is not accepted yet.
- Exact admin permission model is not accepted yet.

Unknown / needs verification:

- Exact admin role behavior.
- Exact admin permission behavior.
- Which admin roles or permissions may perform operational review, support, approval, override, transfer, moderation, audit, rollback/reversal, or production/backend-change actions.
- Whether admin roles interact with personas and tiers, profile identity, event ownership, venue/business tools, ticketing, reservations, wallet/ownership, media/gallery, notifications, staff scanner/check-in, host identity transfer, or public sharing.
- Which backend/RPC/RLS behavior enforces admin roles and permissions.

No exact admin role or permission behavior is accepted in this draft.

## 9. Approval Workflow Draft

Known facts:

- Ops/admin behavior may support approval workflows.
- Ops/admin approval may be involved in host identity transfer.
- Prior desired host transfer flow included request/application, risk check, recipient verification, party approval, JoinFolk-approved transfer, and audit log.
- Exact approval workflow behavior is not accepted yet.

Unknown / needs verification:

- Exact approval behavior.
- Whether approval workflows apply to host identity transfer, users, personas, events, venues, commerce, ticketing, reservations, media, notifications, staff scanner/check-in, public sharing, or other domains.
- Which admin roles or permissions may review, approve, reject, or execute approvals.
- Whether approvals require risk check, recipient verification, party approval, JoinFolk approval, audit log, or other steps.
- Which backend/RPC/RLS behavior enforces approval workflows.

No exact approval workflow behavior is accepted in this draft.

## 10. Operational Review / Support Draft

Known facts:

- Ops/admin behavior may support operational review and support workflows.
- Exact support workflow behavior is not accepted yet.

Unknown / needs verification:

- Exact operational review behavior.
- Exact support workflow behavior.
- Which product domains can be reviewed or supported by ops/admin.
- Which admin roles or permissions may perform support actions.
- Whether support workflows affect users, personas, profiles, events, venues, commerce, ticketing, reservations, wallet/ownership, media/gallery, notifications, staff scanner/check-in, host identity transfer, public sharing, or audit logs.
- Which backend/RPC/RLS behavior enforces support workflow authority.

No exact operational review or support workflow behavior is accepted in this draft.

## 11. Override / Correction Draft

Known facts:

- Ops/admin behavior may support override workflows.
- Exact override behavior is not accepted yet.

Unknown / needs verification:

- Exact override behavior.
- Exact correction behavior.
- Which product domains can be overridden or corrected by ops/admin.
- Which admin roles or permissions may perform overrides or corrections.
- Whether overrides require approval, audit logging, rollback/reversal support, or notifications.
- Whether overrides affect personas, profile identity, event lifecycle, event ownership, venue/business tools, ticketing, reservations, wallet/ownership, media/gallery, notifications, staff scanner/check-in, host identity transfer, public sharing, or audit logs.
- Which backend/RPC/RLS behavior enforces override/correction authority.

No exact override or correction behavior is accepted in this draft.

## 12. Moderation Draft

Known facts:

- Ops/admin behavior may support moderation workflows.
- Exact moderation behavior is not accepted yet.

Unknown / needs verification:

- Exact moderation behavior.
- Which product domains are subject to moderation.
- Which admin roles or permissions may moderate.
- Whether moderation affects profile identity, events, venues, media/gallery, notifications, public sharing, personas, ticketing, reservations, or audit logs.
- Whether moderation requires approval, audit logging, rollback/reversal support, or notifications.
- Which backend/RPC/RLS behavior enforces moderation authority.

No exact moderation behavior is accepted in this draft.

## 13. Audit Logging Draft

Known facts:

- Audit logging is required or expected for security-sensitive admin behavior.
- Prior project context mentioned audit event-like concepts.
- Exact audit log behavior is not accepted yet.

Unknown / needs verification:

- Exact audit log behavior.
- Exact audit schema.
- Exact audit event names.
- Which ops/admin actions require audit logging.
- Whether audit logs record operational review, support, approval, override, transfer, moderation, rollback/reversal, production/backend changes, or errors.
- Who can view audit logs.
- Whether audit logs are exposed in Dashboard/Ops, mobile, Web/Public, or backend-only surfaces.
- Which backend/RPC/RLS behavior enforces audit log creation, integrity, and access.

No exact audit log behavior is accepted in this draft.

## 14. Host Identity Transfer Relationship Draft

Known facts:

- Host identity transfer may be an ops/admin capability.
- Ops/admin approval may be involved in host identity transfer.
- Prior desired host transfer flow included request/application, risk check, recipient verification, party approval, JoinFolk-approved transfer, and audit log.
- Prior project context mentioned transfer dashboard behavior.
- Prior project context mentioned admin/RPC-like behavior such as `admin_execute_host_identity_transfer_v1`.

Unknown / needs verification:

- Exact host identity transfer relationship.
- Exact ops/admin authority for host identity transfer.
- Exact transfer dashboard behavior.
- Exact approval behavior for host identity transfer.
- Exact audit behavior for host identity transfer.
- Exact rollback/reversal behavior for host identity transfer.
- Which backend/RPC/RLS behavior enforces host identity transfer admin authority.

No exact host identity transfer ops/admin behavior is accepted in this draft.

## 15. Migration / Production Change Relationship Draft

Known facts:

- Ops/admin may interact with migrations or backend changes where applicable.
- Prior project context mentioned migration-like concepts.
- Exact production migration policy enforcement by ops/admin is not accepted yet.

Unknown / needs verification:

- Whether ops/admin behavior has any accepted role in migration or production backend change workflows.
- Exact migration/production-change enforcement behavior.
- Whether ops/admin can approve, block, audit, view, or manage production/backend changes.
- Whether production/backend changes require audit logging, approvals, rollback/reversal, or support workflows.
- Which backend/RPC/RLS or external process enforces migration/production-change policy where applicable.

No migration or production-change enforcement behavior is accepted in this draft.

## 16. Relationship to Product Domains

### Personas and tiers

Known relationship:

- Ops/admin may interact with personas and tiers.

Unknown / needs verification:

- Exact relationship between ops/admin behavior, personas, and tiers.

### Profile identity

Known relationship:

- Ops/admin may interact with profile identity.

Unknown / needs verification:

- Exact profile identity review, support, approval, override, moderation, transfer, public sharing, or audit behavior.

### Event lifecycle

Known relationship:

- Ops/admin may interact with event lifecycle.

Unknown / needs verification:

- Exact event lifecycle relationship.

### Event ownership

Known relationship:

- Ops/admin may interact with event ownership.

Unknown / needs verification:

- Exact event ownership review, support, approval, override, transfer, rollback/reversal, or audit behavior.

### Venue/business tools

Known relationship:

- Ops/admin may interact with venue/business tools.

Unknown / needs verification:

- Exact venue/business relationship.

### Ticketing

Known relationship:

- Ops/admin may interact with ticketing.

Unknown / needs verification:

- Exact ticketing relationship.

### Reservations

Known relationship:

- Ops/admin may interact with reservations.

Unknown / needs verification:

- Exact reservation relationship.

### Wallet/ownership

Known relationship:

- Ops/admin may interact with wallet/ownership.

Unknown / needs verification:

- Exact wallet/ownership relationship.

### Media/gallery

Known relationship:

- Ops/admin may interact with media/gallery.

Unknown / needs verification:

- Exact media/gallery relationship, including moderation where applicable.

### Notifications

Known relationship:

- Ops/admin may interact with notifications.

Unknown / needs verification:

- Exact notification relationship, including whether admin actions trigger notifications.

### Staff scanner/check-in

Known relationship:

- Ops/admin may interact with staff scanner/check-in.

Unknown / needs verification:

- Exact staff scanner/check-in relationship.

### Host identity transfer

Known relationship:

- Host identity transfer may be an ops/admin capability.
- Ops/admin approval may be involved in host identity transfer.

Unknown / needs verification:

- Exact host identity transfer relationship and authority boundary.

### Public sharing

Known relationship:

- Ops/admin may interact with public sharing.

Unknown / needs verification:

- Exact public sharing review, support, approval, override, moderation, visibility, or audit behavior.

### Audit logs

Known relationship:

- Audit logging is required or expected for security-sensitive admin behavior.

Unknown / needs verification:

- Exact audit log relationship, visibility, retention, and authority.

## 17. Cross-Surface Consistency Requirements

### Dashboard/Ops

Known facts:

- Dashboard/Ops may expose admin tools.
- Frontend/dashboard admin behavior is UX only where security-sensitive.

Unknown / needs verification:

- Exact dashboard route/component/service ownership.
- Exact dashboard admin behavior.
- Exact relationship between Dashboard/Ops state and backend admin authority.

### Mobile

Known relationship:

- Ops/admin may affect product domains that mobile may consume.

Unknown / needs verification:

- Whether mobile displays or consumes ops/admin state.
- Whether mobile reflects admin changes to personas, profiles, events, venues, ticketing, reservations, wallet/ownership, media/gallery, notifications, staff scanner/check-in, host identity transfer, or public sharing.
- Which mobile behavior must match Dashboard/Ops, Web/Public, or backend behavior.

### Web/Public

Known relationship:

- Ops/admin may interact with public sharing.

Unknown / needs verification:

- Exact Web/Public behavior after admin review, support, approval, override, moderation, transfer, rollback/reversal, or audit-related changes.
- Exact public visibility rules for admin-affected data.
- Whether Web/Public exposes admin state or only resulting product-domain state.

### Supabase Backend

Known requirement:

- Backend/RPC/RLS must enforce security-sensitive ops/admin behavior.

Unknown / needs verification:

- Exact schema.
- Exact RPC contracts.
- Exact RLS policies.
- Exact backend ownership boundaries.
- Exact enforcement model for admin roles, permissions, approvals, overrides, moderation, support workflows, audit logging, host identity transfer, rollback/reversal, and production/backend-change policy where applicable.

## 18. Security Risks

Known risks:

- Ops/admin behavior is security-sensitive.
- Ops/admin may affect users, personas, events, venues, commerce, ticketing, reservations, media, notifications, staff scanner, host identity transfer, public sharing, and audit history.
- Backend/RPC/RLS must enforce security-sensitive ops/admin behavior.
- Frontend/dashboard admin behavior is UX only where security-sensitive.
- Audit logging is required or expected for security-sensitive admin behavior.

Security risks to verify:

- Unauthorized admin access.
- Unauthorized admin role or permission escalation.
- Unauthorized approval, override, correction, moderation, support, transfer, or rollback/reversal action.
- Missing or incomplete audit logging.
- Admin action affecting product domains without accepted authority.
- Frontend-only checks being treated as enforcement.
- Unauthorized public exposure of admin state or audit history.
- Migration or backend change enforcement being assumed without accepted policy.

## 19. Determinism Risks

Known determinism risks:

- Ops/admin may affect many product domains and audit history.
- Exact approval, override, moderation, support, audit, rollback/reversal, and production-change behavior is not accepted yet.

Risks to verify:

- Admin roles or permissions interpreted differently across surfaces.
- Approval state diverging from backend authority.
- Override/correction behavior producing inconsistent product-domain state.
- Moderation results differing across Dashboard/Ops, mobile, Web/Public, and backend.
- Audit logs diverging from actual admin actions.
- Rollback/reversal behavior, if accepted, producing inconsistent state.
- Public sharing showing stale or inconsistent admin-affected data.

## 20. Maintainability Risks

Known maintainability risks:

- Exact dashboard route/component/service ownership is not accepted yet.
- Exact RPC contracts are not accepted yet.
- Exact RLS policies are not accepted yet.
- Prior schema, RPC, audit event, dashboard route, migration, and field names are known concepts only, not accepted contracts.

Risks to verify:

- Prior implementation names being treated as canonical before verification.
- Ops/admin logic scattered across Dashboard/Ops, backend, product domains, public sharing, and audit logging without clear ownership.
- Frontend code encoding security-sensitive admin rules.
- Approval, override, moderation, support, transfer, rollback/reversal, production/backend-change, and audit behavior duplicated without accepted ownership documentation.

## 21. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has dashboard or ops/admin tooling.
- Ops/admin behavior may support operational review, support, approval, override, transfer, moderation, and audit workflows.
- Ops/admin behavior is security-sensitive.
- Backend/RPC/RLS must enforce security-sensitive ops/admin behavior.
- Frontend/dashboard admin behavior is UX only where security-sensitive.
- Audit logging is required or expected for security-sensitive admin behavior.
- Dashboard/Ops may expose admin tools.
- Host identity transfer may be an ops/admin capability.
- Ops/admin approval may be involved in host identity transfer.
- Prior desired host transfer flow included request/application, risk check, recipient verification, party approval, JoinFolk-approved transfer, and audit log.
- Prior context mentioned transfer dashboard behavior, audit event-like concepts, migration-like concepts, and admin/RPC-like behavior, but none are accepted canonical contracts.

Unknown / needs verification:

- Exact accepted implementation across Dashboard/Ops, mobile, Web/Public, Supabase backend, personas, profiles, event lifecycle, event ownership, venue/business tools, ticketing, reservations, wallet/ownership, media/gallery, notifications, staff scanner/check-in, host identity transfer, public sharing, audit logs, and migrations/backend changes where applicable.

## 22. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact ops/admin schema.
- Exact admin role model.
- Exact admin permission model.
- Exact approval workflow behavior.
- Exact override behavior.
- Exact moderation behavior.
- Exact support workflow behavior.
- Exact audit log behavior.
- Exact dashboard route/component/service ownership.
- Exact RPC contracts.
- Exact RLS policies.
- Exact rollback/reversal behavior.
- Exact production migration policy enforcement by ops/admin.
- Exact relationship between ops/admin and personas/tiers.
- Exact relationship between ops/admin and profile identity.
- Exact relationship between ops/admin and event lifecycle.
- Exact relationship between ops/admin and event ownership.
- Exact relationship between ops/admin and venue/business tools.
- Exact relationship between ops/admin and ticketing.
- Exact relationship between ops/admin and reservations.
- Exact relationship between ops/admin and wallet/ownership.
- Exact relationship between ops/admin and media/gallery.
- Exact relationship between ops/admin and notifications.
- Exact relationship between ops/admin and staff scanner/check-in.
- Exact relationship between ops/admin and host identity transfer.
- Exact relationship between ops/admin and public sharing.
- Exact relationship between ops/admin and audit logs.

## 23. Acceptance Criteria for v1.0

Ops/Admin v1.0 can be accepted only after verification establishes:

- Accepted ops/admin domain vocabulary.
- Accepted ops/admin schema.
- Accepted admin role model.
- Accepted admin permission model.
- Accepted approval workflow behavior.
- Accepted operational review and support workflow behavior.
- Accepted override/correction behavior.
- Accepted moderation behavior.
- Accepted audit log behavior.
- Accepted host identity transfer relationship.
- Accepted migration/production-change relationship where applicable.
- Accepted rollback/reversal behavior, if any.
- Accepted RPC contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted RLS policies.
- Accepted cross-surface ownership for Dashboard/Ops, mobile, Web/Public, and Supabase backend.
- Accepted security-sensitive enforcement boundaries.
- Accepted maintainability ownership for dashboard tooling, backend contracts, RLS, admin roles, permissions, approvals, overrides, moderation, support, host identity transfer, audit logs, rollback/reversal, public sharing, and migration/production-change policy where applicable.

Until these criteria are met, this document remains non-canonical.

## 24. Open Questions

- What is the accepted ops/admin schema?
- What is the accepted admin role model?
- What is the accepted admin permission model?
- Which admin roles or permissions can access each Dashboard/Ops tool?
- What approval workflow behavior is accepted?
- What operational review and support workflow behavior is accepted?
- What override or correction behavior is accepted?
- What moderation behavior is accepted?
- What audit log behavior is accepted?
- Which admin actions require audit logging?
- What rollback or reversal behavior is accepted, if any?
- What dashboard routes, components, and services own ops/admin tools?
- What RPC contracts and RLS policies enforce ops/admin behavior?
- How does ops/admin interact with personas and tiers?
- How does ops/admin interact with profile identity?
- How does ops/admin interact with event lifecycle and event ownership?
- How does ops/admin interact with venue/business tools?
- How does ops/admin interact with ticketing, reservations, and wallet/ownership?
- How does ops/admin interact with media/gallery and notifications?
- How does ops/admin interact with staff scanner/check-in?
- How does ops/admin interact with host identity transfer?
- How does ops/admin interact with public sharing?
- What production migration or backend-change enforcement behavior is accepted, if any?
- Which surfaces support ops/admin today: Dashboard/Ops, mobile, Web/Public, and Supabase backend?
