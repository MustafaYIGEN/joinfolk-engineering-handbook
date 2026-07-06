# Platform Consistency Matrix

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a handbook draft for defining a future platform-level consistency matrix for JoinFolk.

It is not an accepted consistency report, confirmed gap report, code audit, patch plan, release readiness report, or production status report. It defines a draft structure for future cross-surface consistency verification across handbook layers and implementation surfaces.

No surface, domain, behavior, permission, lifecycle, module, backend rule, frontend rule, database rule, security rule, release state, or documentation state should be treated as aligned, misaligned, complete, incomplete, ready, blocked, accepted, or production-ready based on this document alone.

## 3. Platform Consistency Matrix Definition

The platform consistency matrix is intended to become a read-only verification structure for comparing documented expectations and observed implementation behavior across JoinFolk surfaces.

The matrix may be used in future audits to record whether handbook drafts, product behavior, architecture assumptions, database behavior, security rules, operational workflows, module boundaries, and release expectations have been verified against implementation evidence.

At this draft stage, all consistency results are `Unknown / Needs verification`.

## 4. Relationship to CurrentStatus.md

`CurrentStatus.md` is a draft status document.

This matrix may reference `CurrentStatus.md` in the future, but it must not inherit or confirm implementation status from it without separate verification. Current platform consistency status remains `Unknown / Needs verification`.

## 5. Relationship to KnownUnknowns.md

`KnownUnknowns.md` is a draft known unknowns document.

This matrix may use future accepted unknown identifiers to connect consistency questions to unresolved areas. Until such links are verified and accepted, related unknowns remain `Unknown / Needs verification`.

## 6. Relationship to HardeningBacklog.md

`HardeningBacklog.md` is a draft hardening backlog document.

This matrix must not convert draft hardening notes into accepted backlog items, risks, blockers, priorities, severities, owners, due dates, or remediation status. Any relationship to hardening work remains `Unknown / Needs verification`.

## 7. Relationship to AuditIndex.md

`AuditIndex.md` is a draft audit index.

This matrix may point to future read-only audit records after those records exist and are reviewed. It must not treat audit index entries as confirmed findings unless separately accepted through the handbook workflow.

## 8. Relationship to PatchPlanIndex.md

`PatchPlanIndex.md` is a draft patch plan index.

This matrix must not create or imply patch plans. Any future patch plan relationship must come after read-only verification, scoped review, and explicit approval for implementation work.

## 9. Relationship to ADRIndex.md

`ADRIndex.md` is a draft ADR index.

This matrix must not create or imply accepted ADR decisions. Any ADR relationship remains `Unknown / Needs verification` until the relevant decision record is drafted, reviewed, and accepted through the handbook workflow.

## 10. Authority Model

### What this matrix may state

- The matrix is a draft handbook structure.
- JoinFolk has multiple handbook layers and implementation surfaces.
- Existing handbook drafts are non-canonical until verified.
- Future implementation audits should be read-only first.
- Future implementation changes must be scoped and reviewed before commit.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.
- Current consistency results are `Unknown / Needs verification`.

### What this matrix may not claim without verification

- Confirmed cross-surface alignment.
- Confirmed cross-surface gaps.
- Confirmed implementation status.
- Confirmed release readiness.
- Confirmed production state.
- Confirmed blockers, risks, backlog items, patch plans, ADR decisions, owners, due dates, priorities, severities, fixes, branches, release gates, rollback plans, or remediation status.
- Exact mobile, dashboard, web, Supabase, RLS, RPC, storage, auth, database, security, module, release, testing, monitoring, logging, incident, audit, patch, ADR, backlog, status, or production behavior.

### What requires explicit approval before implementation

- Production SQL changes.
- Migration changes.
- Function changes.
- RLS changes.
- Storage policy changes.
- Auth changes.
- Backend/database changes.
- Security-sensitive implementation changes.
- Any scoped patching after read-only audit findings.

## 11. Known Facts Draft

- JoinFolk has handbook layers for governance, product, architecture, database, modules, security, operations, audits, patch plans, decisions / ADRs, and status/backlog.
- Existing handbook drafts are non-canonical until verified.
- `CurrentStatus.md` is a draft status document.
- `HardeningBacklog.md` is a draft hardening backlog document.
- `KnownUnknowns.md` is a draft known unknowns document.
- `AuditIndex.md` is a draft audit index.
- `PatchPlanIndex.md` is a draft patch plan index.
- `ADRIndex.md` is a draft ADR index.
- Recently drafted handbook layers include module drafts, security drafts, operations drafts, audit index draft, patch plan index draft, ADR index draft, current status draft, hardening backlog draft, and known unknowns draft.
- JoinFolk has multiple implementation surfaces: Mobile app, Dashboard, Web/Public where applicable, and Supabase backend/storage/database/auth concepts.
- JoinFolk uses or may use React Native / Expo concepts, Vite / React concepts, Supabase or Supabase-like backend concepts, RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.
- Handbook workflow requires scoped diffs, git status checks, review before commit, no uncontrolled production changes, and no production SQL/migrations/functions/RLS/storage/auth changes without explicit approval.
- The next major work after handbook baseline is expected to be read-only implementation audit before patching.

## 12. Known Surfaces Draft

### Mobile

- Surface: Mobile app.
- Possible concepts: React Native / Expo.
- Consistency status: `Unknown / Needs verification`.

### Dashboard

- Surface: Dashboard.
- Possible concepts: Vite / React.
- Consistency status: `Unknown / Needs verification`.

### Web/Public

- Surface: Web/Public where applicable.
- Possible concepts: Vite / React.
- Consistency status: `Unknown / Needs verification`.

### Supabase Backend / Database / Storage / Auth

- Surface: Supabase backend/storage/database/auth concepts.
- Possible concepts: RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.
- Consistency status: `Unknown / Needs verification`.

### Handbook documents

- Surface: Handbook documents across governance, product, architecture, database, modules, security, operations, audits, patch plans, ADRs, and status/backlog.
- Consistency status: `Unknown / Needs verification`.

## 13. Known Product Domains Draft

- Event lifecycle.
- Viewer roles.
- Personas and tiers.
- Ticketing.
- Reservations.
- Wallet/ownership.
- Media/gallery.
- Feed/discovery.
- Messaging where applicable.
- Notifications.
- Staff scanner/check-in.
- Venue/business tools.
- Host identity transfer.
- Ops/admin.
- Public sharing.

All domain behavior and consistency results are `Unknown / Needs verification`.

## 14. Non-Accepted Consistency Areas

- Exact platform consistency matrix.
- Exact consistency status model.
- Exact consistency criteria.
- Exact cross-surface alignment.
- Exact cross-surface gaps.
- Exact implementation status.
- Exact release readiness.
- Exact production state.
- Exact blocker list.
- Exact risk register.
- Exact gap report.
- Exact audit findings.
- Exact patch plans.
- Exact ADR decisions.
- Exact backlog items.
- Exact hardening items.
- Exact mobile/dashboard/web/backend readiness.
- Exact security/database/operations readiness.
- Exact evidence requirements.
- Exact verification workflow.
- Exact ownership, priority, severity, and due-date model.

## 15. Consistency Categories Draft

### Product consistency

Draft category for future verification of product behavior across surfaces. Status: `Unknown / Needs verification`.

### Architecture consistency

Draft category for future verification of architecture assumptions and implementation structure. Status: `Unknown / Needs verification`.

### Database consistency

Draft category for future verification of database behavior, schema expectations, and data access patterns. Status: `Unknown / Needs verification`.

### Security consistency

Draft category for future verification of security-sensitive behavior, permissions, policies, and access boundaries. Status: `Unknown / Needs verification`.

### Operations consistency

Draft category for future verification of operational workflows and production-change controls. Status: `Unknown / Needs verification`.

### Module consistency

Draft category for future verification of module-level behavior and boundaries. Status: `Unknown / Needs verification`.

### Cross-surface consistency

Draft category for future verification of behavior across Mobile, Dashboard, Web/Public, Supabase Backend, and handbook documents. Status: `Unknown / Needs verification`.

### Release-readiness consistency

Draft category for future verification of release-readiness claims. Status: `Unknown / Needs verification`.

### Documentation consistency

Draft category for future verification of handbook and implementation documentation alignment. Status: `Unknown / Needs verification`.

## 16. Consistency Evidence Draft

Future consistency evidence may include read-only audit notes, reviewed handbook references, implementation observations, and reviewed cross-surface comparisons.

No exact evidence requirement is accepted yet. No current evidence is recorded by this draft. Evidence status is `Unknown / Needs verification`.

## 17. Consistency Status Model Draft

The draft status model is intentionally conservative:

- `Unknown / Needs verification`: No accepted consistency result has been recorded.

No other consistency status is accepted by this draft.

## 18. Platform Consistency Matrix Template

Template only:

| Area | Mobile | Dashboard | Web/Public | Supabase Backend | Handbook | Evidence | Status | Owner | Priority | Severity | Related unknown | Related audit | Related patch plan | Related ADR | Resolution state |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Product consistency | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Architecture consistency | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Database consistency | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Security consistency | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Operations consistency | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Module consistency | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Cross-surface consistency | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Release-readiness consistency | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Documentation consistency | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |

## 19. Domain Consistency Matrix Draft

| Domain | Mobile | Dashboard | Web/Public | Supabase Backend | Handbook | Status |
| --- | --- | --- | --- | --- | --- | --- |
| Event lifecycle | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Viewer roles | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Personas and tiers | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Ticketing | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Reservations | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Wallet/ownership | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Media/gallery | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Feed/discovery | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Messaging | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Notifications | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Staff scanner/check-in | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Venue/business tools | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Host identity transfer | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Ops/admin | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |
| Public sharing | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification | Unknown / Needs verification |

## 20. Lifecycle Draft

The expected future lifecycle is:

1. Maintain handbook baseline as draft unless verified.
2. Run read-only implementation audit before patching.
3. Compare Supabase/backend, Dashboard, Mobile, Web/Public where applicable, and handbook documents.
4. Produce a handbook vs implementation gap report only after verification.
5. Produce a P0/P1 patch plan only after reviewed audit findings.
6. Perform controlled patching only after approval.

Current lifecycle acceptance status: `Unknown / Needs verification`.

## 21. Priority / Severity Draft

The exact ownership, priority, severity, and due-date model is not accepted yet.

All priority and severity fields in this matrix must remain `Unknown / Needs verification` until an accepted model exists and specific findings are verified.

## 22. Verification Safety Draft

Future implementation audits should be read-only first.

Verification should not modify production SQL, migrations, functions, RLS, storage, auth, application code, release settings, or production configuration without explicit approval.

## 23. Implementation Safety Draft

Future implementation changes must be scoped and reviewed before commit.

This document does not authorize any code change, migration, Supabase change, release change, patch plan execution, or production operation.

## 24. Production Change Approval Draft

Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

No production change is approved by this document.

## 25. Testing / Re-Test Relationship Draft

Future verified findings may require test or re-test evidence, but exact testing requirements are not accepted yet.

Testing and re-test status for all matrix rows is `Unknown / Needs verification`.

## 26. Audit Relationship Draft

The expected read-only implementation audit sequence is:

1. Supabase/backend.
2. Dashboard.
3. Mobile.
4. Web/Public where applicable.
5. Handbook vs implementation gap report.
6. P0/P1 patch plan.
7. Controlled patching only after approval.

No audit finding is accepted by this matrix.

## 27. Patch Plan Relationship Draft

Patch plans must not be inferred from this matrix. A future patch plan may be related only after read-only verification, review, and approval.

Current patch plan relationship status: `Unknown / Needs verification`.

## 28. ADR Relationship Draft

ADR decisions must not be inferred from this matrix. A future ADR may be related only after drafting, review, and acceptance through the handbook workflow.

Current ADR relationship status: `Unknown / Needs verification`.

## 29. Current Known Implementation

Current known implementation status is `Unknown / Needs verification`.

This document does not confirm mobile, dashboard, web, Supabase, RLS, RPC, storage, auth, database, security, module, release, testing, monitoring, logging, incident, audit, patch, ADR, backlog, status, or production behavior.

## 30. Security Risks

Security risks are `Unknown / Needs verification`.

This document does not accept or confirm any security risk register, blocker, severity, owner, due date, mitigation, or remediation status.

## 31. Privacy Risks

Privacy risks are `Unknown / Needs verification`.

This document does not accept or confirm any privacy risk register, blocker, severity, owner, due date, mitigation, or remediation status.

## 32. Determinism Risks

Determinism risks are `Unknown / Needs verification`.

This document does not accept or confirm any determinism risk register, blocker, severity, owner, due date, mitigation, or remediation status.

## 33. Operational Risks

Operational risks are `Unknown / Needs verification`.

This document does not accept or confirm any operational risk register, blocker, severity, owner, due date, mitigation, or remediation status.

## 34. Maintainability Risks

Maintainability risks are `Unknown / Needs verification`.

This document does not accept or confirm any maintainability risk register, blocker, severity, owner, due date, mitigation, or remediation status.

## 35. Unknowns / Needs Verification

- Exact platform consistency matrix.
- Exact consistency status model.
- Exact consistency criteria.
- Exact cross-surface alignment.
- Exact cross-surface gaps.
- Exact implementation status.
- Exact release readiness.
- Exact production state.
- Exact blocker list.
- Exact risk register.
- Exact gap report.
- Exact audit findings.
- Exact patch plans.
- Exact ADR decisions.
- Exact backlog items.
- Exact hardening items.
- Exact mobile/dashboard/web/backend readiness.
- Exact security/database/operations readiness.
- Exact evidence requirements.
- Exact verification workflow.
- Exact ownership, priority, severity, and due-date model.

## 36. Acceptance Criteria for v1.0

For this matrix to become a v1.0 accepted handbook artifact, it should have:

- An accepted consistency status model.
- Accepted consistency criteria.
- Accepted evidence requirements.
- Accepted verification workflow.
- Reviewed read-only audit evidence.
- Reviewed relationships to accepted unknowns, audits, patch plans, and ADRs where applicable.
- No unverified claims presented as confirmed facts.
- Explicit approval boundaries for production SQL, migrations, functions, RLS, storage, auth, and other security-sensitive changes.

## 37. Open Questions

- What consistency status model should be accepted for future matrix versions?
- What evidence is required before a surface or domain can move out of `Unknown / Needs verification`?
- Which handbook documents should be considered required references for each product domain?
- What review process should accept matrix updates after read-only audits?
- What approval process should govern any future implementation changes related to matrix findings?
