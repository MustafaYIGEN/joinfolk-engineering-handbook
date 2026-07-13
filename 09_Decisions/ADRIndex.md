# ADR Index

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is the platform-level Architecture Decision Record index draft for JoinFolk. It defines an operating structure for future ADRs across handbook layers, implementation surfaces, security-sensitive domains, operations, audits, patch plans, status/backlog, and cross-surface consistency.

This is not an accepted ADR set and not a list of accepted architecture decisions. No decision, status, owner, date, consequence, implementation requirement, supersession rule, or approval history is accepted until verified.

## 3. ADR Index Definition

The ADR Index is a non-canonical handbook entry point for organizing future architecture decision records. ADRs may be created from unresolved design decisions, verified audit findings, patch planning needs, architecture changes, database/security-sensitive decisions, or product/module tradeoffs.

ADRs must not be created from unverified assumptions as accepted decisions. Exact ADR methodology, template, status model, owner model, approval workflow, date/history format, supersession workflow, and review cadence are Unknown / Needs verification.

## 4. Relationship to Handbook Governance

ADR work must preserve handbook governance requirements:

- Scoped diffs.
- Git status checks.
- Review before commit.
- No uncontrolled production changes.
- No production SQL, migrations, functions, RLS, storage, or auth changes without explicit approval.

Existing handbook drafts are non-canonical until verified.

## 5. Relationship to AuditIndex.md

AuditIndex.md is a draft index for future audits. ADRs may be created from verified audit findings or unresolved audit decisions.

Exact audit-to-ADR workflow is Unknown / Needs verification.

## 6. Relationship to PatchPlanIndex.md

PatchPlanIndex.md is a draft index for future patch plans. ADRs may feed patch plans, and patch planning needs may identify decisions that require ADRs.

Exact ADR-to-patch workflow and patch-to-ADR workflow are Unknown / Needs verification.

## 7. Relationship to Status / Backlog

ADRs may feed status/backlog tracking after acceptance. Exact backlog handoff, decision status values, owners, dates, and implementation tracking are Unknown / Needs verification.

## 8. Relationship to Architecture Documents

ADRs may record accepted architecture decisions after verification and approval. Exact architecture decision authority and relationship to architecture documents are Unknown / Needs verification.

## 9. Relationship to Database and Security Documents

ADRs may be required for database or security-sensitive decisions. Exact database/security decision authority and approval workflow are Unknown / Needs verification.

Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

### Recent PP-01 Security Decision Records

The following Draft / Proposed / Not implemented decision records are indexed as PP-01 follow-up governance artifacts. They do not authorize implementation, production access, SQL, migrations, Supabase CLI actions, source changes, or launch-readiness claims.

| Decision record | Decision status | Implementation status | Domain | Relationship to PP-01 | Next artifact or dependent patch plan |
| --- | --- | --- | --- | --- | --- |
| `09_Decisions/SecurityDefinerAndFunctionGrantHardeningDecision.md` | Draft / Proposed / Not implemented | Not authorized | Security | Classifies broad function EXECUTE grants, SECURITY DEFINER posture, missing proconfig/search_path candidates, and future verifier role implications from PP-01 metadata evidence. | `08_PatchPlans/SecurityDefinerAndFunctionGrantHardeningPatchPlan.md` |
| `09_Decisions/RLSDisabledRelationTriageDecision.md` | Draft / Proposed / Not implemented | Not authorized | RLS | Defines triage classes for PP-01 RLS-disabled backup/legacy/view relation findings before any RLS/grant patching. | `09_Decisions/RLSPolicyAndGrantMatrixClassification.md`; future `RLSDisabledRelationPatchPlan.md` only after classification and owner approval. |
| `09_Decisions/RLSPolicyAndGrantMatrixClassification.md` | Draft / Proposed / Not implemented | Not authorized | RLS | Defines combined table grant, RLS state, policy, relation class, caller-role, and app dependency classification required by PP-01 evidence gaps. | Future `RLSGrantMatrixPatchPlan.md` only after matrix classification and owner approval. |
| `09_Decisions/StorageBucketExposureDecision.md` | Draft / Proposed / Not implemented | Not authorized | Storage | Defines storage bucket public/private, storage policy, operation, signed URL, and media exposure classification for PP-01 storage evidence gaps. | Future `StorageBucketExposurePatchPlan.md` only after bucket/policy classification and owner approval. |
| `09_Decisions/EdgeFunctionDeploymentInventoryDecision.md` | Draft / Proposed / Not implemented | Not authorized | Edge | Defines deployment inventory, endpoint exposure, auth/CORS/webhook, secret boundary, and runtime dependency classification for PP-01 Edge Function evidence gaps. | Future `EdgeFunctionDeploymentPatchPlan.md` only after deployment inventory classification and owner approval. |
| `09_Decisions/SupabaseMigrationSourceOfTruthDecision.md` | Draft / Proposed / Not implemented | Not authorized | Migration provenance | Defines migration provenance, drift, source-of-truth, rollback, and release-governance classification for unresolved PP-01 migration evidence. | Required input to future SQL/migration patch plans, including `08_PatchPlans/SecurityDefinerAndFunctionGrantHardeningPatchPlan.md`. |

## 10. ADR Authority Model

### What an ADR may define

- Proposed or accepted decision context, after the status model is verified.
- Alternatives considered, if evidence is verified.
- Consequences, implementation impact, verification needs, and relationships to audits or patch plans, once accepted.
- Decision boundaries across product, architecture, database, security, modules, operations, audits, patch plans, status/backlog, and surfaces.

### What an ADR may not claim without acceptance

- Accepted decisions.
- Accepted decision IDs.
- Accepted statuses.
- Accepted owners or dates.
- Accepted alternatives, consequences, or implementation requirements.
- Accepted supersession/deprecation rules.
- Accepted release gates, rollback paths, patch relationships, or approval history.

### What requires explicit approval before implementation

- Production SQL changes.
- Migrations.
- Functions.
- RLS changes.
- Storage policy changes.
- Auth changes.
- Any uncontrolled production change.

## 11. Known Decision Surfaces Draft

### Mobile

JoinFolk has a mobile app surface. React Native / Expo concepts may apply. Exact mobile decision scope is Unknown / Needs verification.

### Dashboard

JoinFolk has a dashboard surface. Vite / React concepts may apply. Exact dashboard decision scope is Unknown / Needs verification.

### Web/Public

JoinFolk may have Web/Public surfaces. Exact Web/Public decision scope is Unknown / Needs verification.

### Supabase Backend / Database / Storage / Auth

JoinFolk uses or may use Supabase or Supabase-like backend concepts, including RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations. Exact backend, database, storage, and auth decision scope is Unknown / Needs verification.

### Handbook documents

JoinFolk has handbook layers for governance, product, architecture, database, modules, security, operations, audits, patch plans, decisions / ADRs, and status/backlog. Exact handbook decision scope is Unknown / Needs verification.

## 12. Known Decision Domains Draft

Known decision domains include:

- Governance.
- Product.
- Architecture.
- Database.
- Modules.
- Security.
- Operations.
- Audits.
- Patch plans.
- Decisions / ADRs.
- Status/backlog.
- Cross-surface consistency.

Exact domain boundaries and decision authority are Unknown / Needs verification.

## 13. Non-Accepted ADR Areas

- Exact ADR methodology is not accepted yet.
- Exact ADR template is not accepted yet.
- Exact ADR status model is not accepted yet.
- Exact decision owner model is not accepted yet.
- Exact decision approval workflow is not accepted yet.
- Exact decision date/history format is not accepted yet.
- Exact supersession/deprecation workflow is not accepted yet.
- Exact ADR-to-patch workflow is not accepted yet.
- Exact audit-to-ADR workflow is not accepted yet.
- Exact backlog/status handoff is not accepted yet.
- Exact implementation workflow from ADRs is not accepted yet.
- Exact release gate relationship is not accepted yet.
- Exact production approval workflow for ADR-driven changes is not accepted yet.
- Exact decision review cadence is not accepted yet.
- Exact architecture decision authority is not accepted yet.
- Exact database/security decision authority is not accepted yet.

## 14. ADR Categories Draft

### Product decision

Product decisions may address verified product or module tradeoffs. Exact scope and approval workflow are Unknown / Needs verification.

### Architecture decision

Architecture decisions may address verified architecture changes. Exact authority and workflow are Unknown / Needs verification.

### Database decision

Database decisions may involve database-facing concepts. Production SQL, migrations, functions, RLS, storage, and auth changes require explicit approval. Exact workflow is Unknown / Needs verification.

### Security decision

Security decisions may address security-sensitive domains, public exposure, permissions, RLS, RPC, storage, auth, or ops/admin boundaries. Exact workflow is Unknown / Needs verification.

### Operations decision

Operations decisions may address build/release, testing, monitoring/logging, or incident response. Exact workflow is Unknown / Needs verification.

### Module decision

Module decisions may address verified product-domain module tradeoffs. Exact workflow is Unknown / Needs verification.

### Cross-surface decision

Cross-surface decisions may address consistency across Mobile, Dashboard, Web/Public, Supabase Backend, and Handbook documents. Exact workflow is Unknown / Needs verification.

### Release-readiness decision

Release-readiness decisions may relate to verified release preparation needs. Exact release gates and readiness criteria are Unknown / Needs verification.

### Documentation decision

Documentation decisions may affect handbook structure or accepted content. Exact documentation decision workflow is Unknown / Needs verification.

## 15. ADR Intake Draft

ADR intake may originate from unresolved design decisions, verified audit findings, patch planning needs, architecture changes, database/security-sensitive decisions, or product/module tradeoffs.

Exact intake criteria and approval workflow are Unknown / Needs verification.

## 16. ADR Evidence Draft

ADR evidence may reference verified handbook facts, verified audit findings, accepted patch planning needs, accepted backlog items, or explicitly scoped implementation evidence.

Exact evidence requirements, evidence format, and evidence acceptance rules are Unknown / Needs verification.

## 17. ADR Template

Template only. Do not treat any field as accepted ADR data.

| Field | Value |
| --- | --- |
| ADR ID | Unknown / Needs verification |
| Title | Unknown / Needs verification |
| Status | Unknown / Needs verification |
| Owner | Unknown / Needs verification |
| Date | Unknown / Needs verification |
| Context | Unknown / Needs verification |
| Decision | Unknown / Needs verification |
| Alternatives | Unknown / Needs verification |
| Consequences | Unknown / Needs verification |
| Implementation impact | Unknown / Needs verification |
| Patch relationship | Unknown / Needs verification |
| Verification | Unknown / Needs verification |
| Supersession | Unknown / Needs verification |
| Review | Unknown / Needs verification |
| Notes | Unknown / Needs verification |

## 18. ADR Lifecycle Draft

An ADR lifecycle may include intake, evidence review, draft, review, acceptance, implementation relationship, verification, supersession/deprecation if needed, and history updates. This is a draft structure only.

Exact lifecycle steps and acceptance rules are Unknown / Needs verification.

## 19. Status Model Draft

ADR status values are not accepted yet. ADR documents must not assign exact decision status without a verified and accepted model.

## 20. Ownership / Approval Draft

Decision ownership and approval workflow are not accepted yet. ADRs must not assign owners, dates, or approval history without verification.

## 21. Supersession / Deprecation Draft

Supersession and deprecation behavior are not accepted yet. ADRs must not claim that a decision replaces or deprecates another decision without a verified model and accepted decision history.

## 22. Implementation Safety Draft

Future implementation changes must be scoped and reviewed before commit. ADRs must not imply permission to modify application code, Supabase migrations, production systems, or security-sensitive behavior without explicit approval.

Exact implementation workflow from ADRs is Unknown / Needs verification.

## 23. Production Change Approval Draft

Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Exact production approval workflow for ADR-driven changes is Unknown / Needs verification.

## 24. Testing / Verification Relationship Draft

ADRs may require testing and verification after implementation. Exact verification process, test plan, re-test process, and evidence requirements are Unknown / Needs verification.

## 25. Release / Rollback Relationship Draft

ADRs may affect release planning and rollback or reversal needs. Exact release gate behavior, rollback procedure, deployment process, and release readiness criteria are Unknown / Needs verification.

## 26. Audit Relationship Draft

ADRs may be created from verified audit findings or may trigger future audits and re-audits. Exact audit relationship and workflow are Unknown / Needs verification.

## 27. Patch Plan Relationship Draft

ADRs may feed patch plans, and patch planning may identify decisions that require ADRs. Exact patch relationship and handoff process are Unknown / Needs verification.

## 28. ADR Log / History Draft

ADR history may track decision activity and changes over time. Exact ADR log format, history format, review cadence, retention, and acceptance process are Unknown / Needs verification.

## 29. Product Domain Decision Draft

- Event lifecycle: Decision scope and status are Unknown / Needs verification.
- Viewer roles: Decision scope and status are Unknown / Needs verification.
- Personas and tiers: Decision scope and status are Unknown / Needs verification.
- Ticketing: Decision scope and status are Unknown / Needs verification.
- Reservations: Decision scope and status are Unknown / Needs verification.
- Wallet/ownership: Decision scope and status are Unknown / Needs verification.
- Media/gallery: Decision scope and status are Unknown / Needs verification.
- Feed/discovery: Decision scope and status are Unknown / Needs verification.
- Messaging: Decision scope and status are Unknown / Needs verification.
- Notifications: Decision scope and status are Unknown / Needs verification.
- Staff scanner/check-in: Decision scope and status are Unknown / Needs verification.
- Venue/business tools: Decision scope and status are Unknown / Needs verification.
- Host identity transfer: Decision scope and status are Unknown / Needs verification.
- Ops/admin: Decision scope and status are Unknown / Needs verification.
- Public sharing: Decision scope and status are Unknown / Needs verification.

## 30. Cross-Surface Consistency Requirements

### Mobile

Mobile ADRs must remain consistent with approved product, architecture, database, security, operations, module, audit, patch plan, and status/backlog facts. Exact mobile consistency checks are Unknown / Needs verification.

### Dashboard

Dashboard ADRs must remain consistent with approved handbook facts and must not invent dashboard behavior. Exact dashboard consistency checks are Unknown / Needs verification.

### Web/Public

Web/Public ADRs must remain consistent with approved public exposure and product-domain facts. Exact Web/Public consistency checks are Unknown / Needs verification.

### Supabase Backend

Backend ADRs must remain consistent with approved database, security, RLS, RPC, storage, auth, and production-change controls. Exact backend consistency checks are Unknown / Needs verification.

### Handbook

Handbook ADRs must distinguish draft, non-canonical content from verified accepted facts. Exact handbook consistency checks are Unknown / Needs verification.

## 31. Security Risks

- Treating draft ADRs as accepted security or architecture decisions.
- Allowing ADRs to imply security-sensitive implementation authority without approval.
- Creating frontend-only decisions for security-sensitive behavior.
- Making production SQL, migrations, functions, RLS, storage, or auth changes without explicit approval.

## 32. Privacy Risks

- Recording sensitive user, profile, event, ticket, reservation, wallet, media, messaging, or ops/admin details in ADR evidence without accepted evidence rules.
- Making public exposure, media, messaging, or identity decisions without verified privacy review.

## 33. Determinism Risks

- Assigning ADR status, owner, date, consequences, implementation impact, supersession, or review state without an accepted model.
- Mixing proposed decisions with accepted decisions.
- Allowing cross-surface behavior to drift between Mobile, Dashboard, Web/Public, Supabase Backend, and Handbook documents.

## 34. Operational Risks

- Treating ADRs as release gates before release-readiness criteria are accepted.
- Starting implementation from unaccepted decisions.
- Creating uncontrolled production changes from ADR-driven work.

## 35. Maintainability Risks

- Duplicating decision state across ADRs, audits, patch plans, and backlog without a verified relationship model.
- Allowing ADR templates to drift from handbook governance.
- Failing to update ADR history after review, acceptance, implementation, supersession, or re-audit once those workflows are accepted.

## 36. Current Known Implementation

Known implementation surfaces and concepts include Mobile app, Dashboard, Web/Public where applicable, Supabase backend/storage/database/auth concepts, React Native / Expo concepts, Vite / React concepts, RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.

Exact implementation behavior, accepted ADRs, decision IDs, statuses, owners, dates, consequences, implementation requirements, supersession rules, approval history, release gates, rollback paths, and patch relationships are Unknown / Needs verification.

## 37. Unknowns / Needs Verification

- Exact ADR methodology.
- Exact ADR template.
- Exact ADR status model.
- Exact decision owner model.
- Exact decision approval workflow.
- Exact decision date/history format.
- Exact supersession/deprecation workflow.
- Exact ADR-to-patch workflow.
- Exact audit-to-ADR workflow.
- Exact backlog/status handoff.
- Exact implementation workflow from ADRs.
- Exact release gate relationship.
- Exact production approval workflow for ADR-driven changes.
- Exact decision review cadence.
- Exact architecture decision authority.
- Exact database/security decision authority.

## 38. Acceptance Criteria for v1.0

For this document to become v1.0, JoinFolk must verify and accept:

- ADR methodology.
- ADR template.
- ADR intake criteria.
- ADR evidence requirements.
- ADR status model.
- Decision owner and approval workflow.
- Date/history format.
- Supersession/deprecation workflow.
- ADR-to-patch workflow.
- Audit-to-ADR workflow.
- Status/backlog handoff.
- Implementation safety workflow.
- Testing and verification relationship.
- Release and rollback relationship.
- Production approval workflow for ADR-driven changes.
- Decision review cadence.
- Architecture, database, and security decision authority.
- Explicit production-change approval constraints for SQL, migrations, functions, RLS, storage, and auth.

## 39. Recent Notification Delivery Decision

| Decision record | Decision status | Implementation status | Domain | Relationship to current notification work | Next artifact |
| --- | --- | --- | --- | --- | --- |
| `09_Decisions/NotificationDeliveryBoundaryDecision.md` | Accepted | Server notification delivery is production-proven; reminder local delivery is implemented but not yet device-accepted | Notifications / push / reminders | Locks the server notification boundary, local reminder ownership, dispatch secret requirement, and legacy RPC rollout dependence. | [10_Status/NotificationDeliveryStatusGates.md](../10_Status/NotificationDeliveryStatusGates.md) |

## 40. Event Visual Decision

| Decision record | Decision status | Implementation status | Domain | Relationship to current event visual work | Next artifact |
| --- | --- | --- | --- | --- | --- |
| `09_Decisions/EventVisualCanonicalImageOrVideoDecision.md` | Accepted | Product contract frozen; implementation and release closure remain open | Event visuals / poster video / feed playback / dashboard parity | Locks IMAGE xor VIDEO, bounded feed loop authority, host-plus-pro entitlement, static fallback requirement, and public derivative requirement without authorizing broad protected-player rewrites. | [10_Status/EventVisualStatusGates.md](../10_Status/EventVisualStatusGates.md) |

## 41. Open Questions

- What ADR methodology should JoinFolk accept?
- What ADR template should be accepted?
- What status model should ADRs use?
- Who can own ADRs, and what approval workflow should apply?
- What date and history format should be used?
- How should supersession and deprecation work?
- What evidence is required before a decision can be accepted?
- When should audits create ADRs?
- When should patch plans create ADRs?
- How should ADRs feed status/backlog?
- What implementation workflow should apply to accepted ADRs?
- What testing and verification should be required for ADR-driven work?
- What release and rollback relationship should ADRs have?
- What production approval workflow should apply to ADR-driven database, RLS, RPC, storage, auth, or security-sensitive changes?
- What decision review cadence should be accepted?
- How should ADRs preserve cross-surface consistency across Mobile, Dashboard, Web/Public, Supabase Backend, and Handbook documents?

## 42. Auth Email Canonical Host Decision

| Decision record | Decision status | Implementation status | Domain | Relationship to AUTH-EMAIL-01 | Next artifact |
| --- | --- | --- | --- | --- | --- |
| `09_Decisions/AuthEmailCanonicalLinkHostDecision.md` | Blocked | Not authorized | Auth / universal links / reset / confirmation | Freezes the browser-first auth-email contract, decides `join-folk.com` as canonical host, rejects `app.join-folk.com` as the current auth-email surface, and authorizes browser-first closure work under the accepted patch plan. | [10_Status/AuthEmailStatusGates.md](../10_Status/AuthEmailStatusGates.md) |
