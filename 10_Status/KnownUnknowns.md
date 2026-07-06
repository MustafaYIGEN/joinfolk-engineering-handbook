# Known Unknowns

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is the platform-level known unknowns draft for JoinFolk. It separates known handbook facts from Unknown / Needs verification areas.

This is not an accepted risk register, not an accepted blocker list, not a confirmed gap report, not a code audit, and not a patch plan. No unknown is a confirmed bug, confirmed gap, confirmed blocker, accepted risk, accepted backlog item, or accepted remediation item until verified.

## 3. Known Unknowns Definition

Known unknowns are areas where JoinFolk has enough handbook context to identify a verification need, but not enough accepted evidence to claim implementation status, release status, production status, risks, blockers, backlog items, patch plans, ADRs, owners, due dates, priorities, severities, fixes, or remediation status.

Exact unknown classification model, evidence requirements, verification workflow, ownership model, priority model, severity model, due-date model, and resolution model are Unknown / Needs verification.

## 4. Relationship to CurrentStatus.md

CurrentStatus.md is a draft status document. KnownUnknowns.md may support status clarity by separating known facts from verification needs.

This document must not convert draft status into accepted implementation, release, production, risk, blocker, or backlog status.

## 5. Relationship to HardeningBacklog.md

HardeningBacklog.md is a draft hardening backlog document. Known unknowns may inform future hardening intake after verification.

This document must not create accepted hardening backlog items.

## 6. Relationship to AuditIndex.md

AuditIndex.md is a draft audit index. Known unknowns may inform future audit scopes, but they are not accepted audit findings.

Exact audit relationship and evidence requirements are Unknown / Needs verification.

## 7. Relationship to PatchPlanIndex.md

PatchPlanIndex.md is a draft patch plan index. Known unknowns must not become patch plans unless verified and explicitly accepted through a future workflow.

Exact patch-plan relationship is Unknown / Needs verification.

## 8. Relationship to ADRIndex.md

ADRIndex.md is a draft ADR index. Known unknowns may identify unresolved decisions that need future ADR review, but they are not accepted ADR decisions.

Exact ADR relationship is Unknown / Needs verification.

## 9. Authority Model

### What this document may state

- Known handbook facts provided by user-stated and prior audit summary context.
- Areas that need verification.
- Draft categories and templates for tracking unknowns.
- Relationships to status, hardening backlog, audits, patch plans, ADRs, testing, and cross-surface consistency.

### What this document may not claim without verification

- Confirmed gaps.
- Confirmed blockers.
- Accepted risks.
- Backlog items.
- Patch plans.
- ADR decisions.
- Exact implementation status.
- Exact release status.
- Exact production state.
- Owners, due dates, priorities, severities, fixes, files, commands, branches, release gates, rollback plans, remediation status, or resolution status.

### What requires explicit approval before implementation

- Production SQL changes.
- Migrations.
- Functions.
- RLS changes.
- Storage policy changes.
- Auth changes.
- Any uncontrolled production change.
- Any implementation change derived from a known unknown.

## 10. Known Facts Draft

Known facts in this draft:

- JoinFolk has handbook layers for governance, product, architecture, database, modules, security, operations, audits, patch plans, decisions / ADRs, and status/backlog.
- Existing handbook drafts are non-canonical until verified.
- CurrentStatus.md is a draft status document.
- HardeningBacklog.md is a draft hardening backlog document.
- AuditIndex.md is a draft audit index.
- PatchPlanIndex.md is a draft patch plan index.
- ADRIndex.md is a draft ADR index.
- Recently drafted handbook layers include module drafts, security drafts, operations drafts, audit index draft, patch plan index draft, ADR index draft, current status draft, and hardening backlog draft.
- JoinFolk has multiple implementation surfaces: Mobile app, Dashboard, Web/Public where applicable, and Supabase backend/storage/database/auth concepts.
- JoinFolk uses or may use React Native / Expo concepts, Vite / React concepts, Supabase or Supabase-like backend concepts, RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.
- Handbook workflow requires scoped diffs, git status checks, review before commit, no uncontrolled production changes, and no production SQL/migrations/functions/RLS/storage/auth changes without explicit approval.
- Next major work after handbook baseline is expected to be read-only implementation audit before patching.

These facts do not establish implementation readiness, release readiness, production state, accepted risks, accepted blockers, accepted backlog items, accepted patch plans, or accepted ADRs.

## 11. Known Handbook Layers Draft

- Governance: Known handbook layer; accepted state is Unknown / Needs verification.
- Product: Known handbook layer; accepted state is Unknown / Needs verification.
- Architecture: Known handbook layer; accepted state is Unknown / Needs verification.
- Database: Known handbook layer; accepted state is Unknown / Needs verification.
- Modules: Recently drafted layer; accepted state is Unknown / Needs verification.
- Security: Recently drafted layer; accepted state is Unknown / Needs verification.
- Operations: Recently drafted layer; accepted state is Unknown / Needs verification.
- Audits: Audit index draft exists; accepted audit process is Unknown / Needs verification.
- Patch plans: Patch plan index draft exists; accepted patch plan process is Unknown / Needs verification.
- Decisions / ADRs: ADR index draft exists; accepted decision process is Unknown / Needs verification.
- Status/backlog: Current status and hardening backlog drafts exist; accepted status/backlog model is Unknown / Needs verification.

## 12. Known Implementation Surfaces Draft

### Mobile

JoinFolk has a mobile app surface. React Native / Expo concepts may apply. Exact mobile implementation status and readiness are Unknown / Needs verification.

### Dashboard

JoinFolk has a dashboard surface. Vite / React concepts may apply. Exact dashboard implementation status and readiness are Unknown / Needs verification.

### Web/Public

JoinFolk may have Web/Public surfaces. Exact Web/Public implementation status and readiness are Unknown / Needs verification.

### Supabase Backend / Database / Storage / Auth

JoinFolk uses or may use Supabase or Supabase-like backend concepts, including RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations. Exact backend, database, storage, auth, and production status are Unknown / Needs verification.

## 13. Known Product Domains Draft

Known security-sensitive or product domains include:

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

Exact implementation status, accepted behavior, readiness, risks, blockers, and backlog state for each domain are Unknown / Needs verification.

## 14. Non-Accepted Unknown Areas

- Exact implementation status is not accepted yet.
- Exact release readiness is not accepted yet.
- Exact launch readiness is not accepted yet.
- Exact production state is not accepted yet.
- Exact blocker list is not accepted yet.
- Exact risk register is not accepted yet.
- Exact gap report is not accepted yet.
- Exact audit findings are not accepted yet.
- Exact patch plans are not accepted yet.
- Exact ADR decisions are not accepted yet.
- Exact backlog items are not accepted yet.
- Exact hardening items are not accepted yet.
- Exact mobile/dashboard/web/backend readiness is not accepted yet.
- Exact security/database/operations readiness is not accepted yet.
- Exact testing, monitoring, incident, and release readiness is not accepted yet.
- Exact evidence requirements are not accepted yet.
- Exact verification workflow is not accepted yet.
- Exact unknown classification model is not accepted yet.
- Exact ownership, priority, severity, and due-date model is not accepted yet.

## 15. Unknown Categories Draft

### Product unknowns

Product unknowns may identify product-domain areas needing verification. Exact product unknown records are not accepted.

### Architecture unknowns

Architecture unknowns may identify architecture areas needing verification. Exact architecture unknown records are not accepted.

### Database unknowns

Database unknowns may identify database-facing areas needing verification. Exact database unknown records are not accepted.

### Security unknowns

Security unknowns may identify security-sensitive areas needing verification. Exact security unknown records are not accepted.

### Operations unknowns

Operations unknowns may identify build/release, testing, monitoring/logging, or incident response areas needing verification. Exact operations unknown records are not accepted.

### Module unknowns

Module unknowns may identify product-domain module areas needing verification. Exact module unknown records are not accepted.

### Cross-surface unknowns

Cross-surface unknowns may identify consistency areas needing verification across Mobile, Dashboard, Web/Public, Supabase Backend, and Handbook documents. Exact cross-surface unknown records are not accepted.

### Release-readiness unknowns

Release-readiness unknowns may identify readiness areas needing verification. Exact release-readiness unknown records are not accepted.

### Documentation unknowns

Documentation unknowns may identify handbook accuracy, consistency, or verification status questions. Exact documentation unknown records are not accepted.

## 16. Unknown Intake Draft

Unknown intake may follow handbook drafting, CurrentStatus.md updates, HardeningBacklog.md review, read-only implementation audit, audit scoping, patch planning, ADR review, or gap reporting.

Exact intake criteria and acceptance workflow are Unknown / Needs verification.

## 17. Evidence / Verification Draft

Evidence may be needed before an unknown can become a verified fact, audit finding, backlog item, patch plan, ADR, or resolved item.

Exact evidence requirements, verification workflow, evidence format, and acceptance rules are Unknown / Needs verification.

## 18. Known Unknown Template

Template only. Do not treat any field as an accepted unknown record.

| Field | Value |
| --- | --- |
| Unknown ID | Unknown / Needs verification |
| Title | Unknown / Needs verification |
| Status | Unknown / Needs verification |
| Category | Unknown / Needs verification |
| Surface | Unknown / Needs verification |
| Domain | Unknown / Needs verification |
| Evidence | Unknown / Needs verification |
| Owner | Unknown / Needs verification |
| Priority | Unknown / Needs verification |
| Severity | Unknown / Needs verification |
| Next verification step | Unknown / Needs verification |
| Related audit | Unknown / Needs verification |
| Related patch plan | Unknown / Needs verification |
| Related ADR | Unknown / Needs verification |
| Resolution state | Unknown / Needs verification |
| Notes | Unknown / Needs verification |

## 19. Unknown Lifecycle Draft

An unknown lifecycle may include intake, classification, evidence review, verification, conversion to accepted fact or related workflow, resolution, and history updates. This is a draft structure only.

Exact lifecycle steps and acceptance rules are Unknown / Needs verification.

## 20. Priority / Severity Draft

Priority and severity are not accepted yet. Known unknowns must not assign exact priority or severity without a verified and accepted model.

## 21. Verification Safety Draft

Future implementation audits should be read-only first. Verification must not mutate application code, Supabase migrations, production systems, RLS, RPC, storage, auth, or database state unless explicitly approved.

Exact verification safety workflow is Unknown / Needs verification.

## 22. Implementation Safety Draft

Future implementation changes must be scoped and reviewed before commit. Known unknowns must not imply permission to modify application code, Supabase migrations, production systems, or security-sensitive behavior without explicit approval.

Exact implementation workflow is Unknown / Needs verification.

## 23. Production Change Approval Draft

Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Exact production approval workflow is Unknown / Needs verification.

## 24. Testing / Re-Test Relationship Draft

Known unknowns may require testing or re-test after future verification or implementation. Exact testing, re-test, evidence, and acceptance workflow are Unknown / Needs verification.

## 25. Audit Relationship Draft

Known unknowns may inform future audit scope. They are not audit findings until verified through an accepted audit workflow.

Exact audit relationship is Unknown / Needs verification.

## 26. Patch Plan Relationship Draft

Known unknowns may inform patch planning after verification. They are not patch plans, confirmed fixes, or accepted remediation items.

Exact patch plan relationship is Unknown / Needs verification.

## 27. ADR Relationship Draft

Known unknowns may identify unresolved decisions that require future ADR review. They are not accepted ADR decisions.

Exact ADR relationship is Unknown / Needs verification.

## 28. Product Domain Unknowns Draft

- Event lifecycle: Unknowns and verification needs are Unknown / Needs verification.
- Viewer roles: Unknowns and verification needs are Unknown / Needs verification.
- Personas and tiers: Unknowns and verification needs are Unknown / Needs verification.
- Ticketing: Unknowns and verification needs are Unknown / Needs verification.
- Reservations: Unknowns and verification needs are Unknown / Needs verification.
- Wallet/ownership: Unknowns and verification needs are Unknown / Needs verification.
- Media/gallery: Unknowns and verification needs are Unknown / Needs verification.
- Feed/discovery: Unknowns and verification needs are Unknown / Needs verification.
- Messaging: Unknowns and verification needs are Unknown / Needs verification.
- Notifications: Unknowns and verification needs are Unknown / Needs verification.
- Staff scanner/check-in: Unknowns and verification needs are Unknown / Needs verification.
- Venue/business tools: Unknowns and verification needs are Unknown / Needs verification.
- Host identity transfer: Unknowns and verification needs are Unknown / Needs verification.
- Ops/admin: Unknowns and verification needs are Unknown / Needs verification.
- Public sharing: Unknowns and verification needs are Unknown / Needs verification.

## 29. Cross-Surface Consistency Requirements

### Mobile

Mobile unknowns must remain separate from confirmed mobile status, risks, blockers, backlog items, or patch plans.

### Dashboard

Dashboard unknowns must remain separate from confirmed dashboard status, risks, blockers, backlog items, or patch plans.

### Web/Public

Web/Public unknowns, where applicable, must remain separate from confirmed Web/Public status, risks, blockers, backlog items, or patch plans.

### Supabase Backend

Supabase Backend / Database / Storage / Auth unknowns must remain separate from confirmed backend status, production state, risks, blockers, backlog items, or patch plans.

### Handbook

Handbook unknowns must distinguish draft documents from verified accepted facts.

## 30. Security Risks

- Treating known unknowns as confirmed security findings.
- Treating unknowns as accepted risks, blockers, backlog items, patch plans, or ADR decisions.
- Allowing unknowns to imply security-sensitive implementation authority without approval.
- Making production SQL, migrations, functions, RLS, storage, or auth changes without explicit approval.

## 31. Privacy Risks

- Recording sensitive user, profile, event, ticket, reservation, wallet, media, messaging, or ops/admin details in unknown records without accepted evidence rules.
- Treating public exposure, media visibility, messaging, or identity unknowns as accepted facts without verification.

## 32. Determinism Risks

- Mixing known facts with unknowns.
- Assigning status, priority, severity, owner, due date, resolution state, risk, blocker, or remediation state without an accepted model.
- Allowing cross-surface behavior to drift between Mobile, Dashboard, Web/Public, Supabase Backend, and Handbook documents.

## 33. Operational Risks

- Treating this document as a release readiness report.
- Starting patching before read-only audit and approval.
- Creating uncontrolled production changes from unverified unknowns.
- Treating unknowns as blockers or backlog items without verification.

## 34. Maintainability Risks

- Duplicating unknown state across CurrentStatus.md, HardeningBacklog.md, AuditIndex.md, PatchPlanIndex.md, ADRIndex.md, operations, and module documents without a verified relationship model.
- Allowing unknown templates to drift from handbook governance.
- Failing to update unknowns after verification, audits, patch planning, ADR decisions, implementation, testing, release, or re-test once those workflows are accepted.

## 35. Current Known Implementation

Known implementation surfaces and concepts include Mobile app, Dashboard, Web/Public where applicable, Supabase backend/storage/database/auth concepts, React Native / Expo concepts, Vite / React concepts, RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.

Exact implementation status, release status, production status, owners, due dates, priorities, severities, risks, blockers, backlog items, patch plans, ADRs, fixes, files, commands, branches, release gates, rollback plans, and remediation status are Unknown / Needs verification.

## 36. Unknowns / Needs Verification

- Exact implementation status.
- Exact release readiness.
- Exact launch readiness.
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
- Exact testing, monitoring, incident, and release readiness.
- Exact evidence requirements.
- Exact verification workflow.
- Exact unknown classification model.
- Exact ownership, priority, severity, and due-date model.

## 37. Acceptance Criteria for v1.0

For this document to become v1.0, JoinFolk must verify and accept:

- Known unknowns methodology.
- Unknown classification model.
- Evidence requirements.
- Verification workflow.
- Unknown record template.
- Status and resolution model.
- Ownership, priority, severity, and due-date model, if any.
- Relationship to CurrentStatus.md.
- Relationship to HardeningBacklog.md.
- Relationship to AuditIndex.md.
- Relationship to PatchPlanIndex.md.
- Relationship to ADRIndex.md.
- Read-only implementation audit safety model.
- Production change approval constraints for SQL, migrations, functions, RLS, storage, and auth.

## 38. Open Questions

- What methodology should JoinFolk use for known unknowns?
- What unknown classification model should be accepted?
- What evidence is required before an unknown can become a verified fact?
- What verification workflow should be accepted?
- What status and resolution model should unknown records use?
- Should known unknowns have owners, priorities, severities, or due dates?
- How should unknowns relate to CurrentStatus.md?
- How should unknowns become hardening backlog items?
- When should unknowns become audit scopes?
- When should unknowns become patch plans?
- When should unknowns require ADRs?
- What read-only audit safety rules should apply during verification?
- What production approval workflow should apply if an unknown leads to implementation?
- How should known unknowns preserve cross-surface consistency across Mobile, Dashboard, Web/Public, Supabase Backend, and Handbook documents?
