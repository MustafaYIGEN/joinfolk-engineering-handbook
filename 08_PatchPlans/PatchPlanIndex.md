# Patch Plan Index

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is the platform-level patch plan index draft for JoinFolk. It defines an operating structure for future patch plans across handbook layers, implementation surfaces, security-sensitive domains, operations, audits, ADRs, status/backlog, and cross-surface consistency.

This is not a code patch, not an accepted patch plan, and not a confirmed remediation list. No patch, bug, fix, file, implementation step, owner, priority, due date, release gate, rollback path, or remediation status is accepted until verified.

## 3. Patch Plan Index Definition

The Patch Plan Index is a non-canonical handbook entry point for organizing future patch planning. Patch plans may be created from verified audit findings, accepted decisions, accepted backlog items, or explicitly approved remediation needs.

Patch plans must not be created from unverified findings as accepted remediation. Exact patch plan methodology, template, prioritization model, workflow, review process, release relationship, rollback model, and history format are Unknown / Needs verification.

## 4. Relationship to Handbook Governance

Patch planning must preserve handbook governance requirements:

- Scoped diffs.
- Git status checks.
- Review before commit.
- No uncontrolled production changes.
- No production SQL, migrations, functions, RLS, storage, or auth changes without explicit approval.

Existing handbook drafts are non-canonical until verified.

## 5. Relationship to AuditIndex.md

AuditIndex.md is a draft index for future audits. Patch plans may be created from verified audit findings, but must not treat unverified audit notes as accepted remediation.

Exact audit-to-patch workflow is Unknown / Needs verification.

## 6. Relationship to ADRIndex.md

Patch plans may feed ADRs or be created from accepted ADRs. Exact ADR-to-patch workflow, decision ownership, and implementation relationship are Unknown / Needs verification.

Patch plans must not be treated as accepted architecture decisions.

### Recent PP-01 Security Patch Plan

The following Draft / Proposed patch plans are indexed as future implementation planning artifacts. They do not authorize implementation, SQL, migration creation, production access, Supabase CLI actions, dashboard actions, source changes, or production mutation.

| Patch plan | Patch plan status | Implementation status | Source decisions / related gates | Risk area | Next gate | Blocked actions |
| --- | --- | --- | --- | --- | --- | --- |
| `08_PatchPlans/SecurityDefinerAndFunctionGrantHardeningPatchPlan.md` | Draft / Proposed | Not authorized | `09_Decisions/SecurityDefinerAndFunctionGrantHardeningDecision.md`; `09_Decisions/RLSPolicyAndGrantMatrixClassification.md`; `09_Decisions/SupabaseMigrationSourceOfTruthDecision.md` | P0/P1 security hardening candidate | Owner approval before implementation prompt | No SQL; no migration; no production mutation; no source change. |
| `08_PatchPlans/SecurityDefinerFunctionGrantMetadataCollectionPlan.md` | Draft / Proposed | Not authorized | Follows `00_Status/SecurityDefinerFunctionGrantClassificationCompletenessReview.md` and `00_Status/SecurityDefinerFunctionGrantMetadataCollectionApprovalGate.md` | P0/P1 security metadata prerequisite | Owner review of collected metadata report before implementation | No SQL; no migration; no production mutation; no source change; no RPC invocation; no private row/storage object inspection. |

`07_Audits/SecurityDefinerFunctionGrantCollectedMetadataReport.md` is an audit/evidence artifact, not a patch plan. It informs patch planning but does not authorize implementation; local source, migration, and call-site evidence does not prove production behavior, and production metadata remains TBD until approved production metadata collection is performed.

### Current Execution Order

1. Hardening decision and hardening patch plan created.
2. Owner preparation gate completed.
3. Inventory classification completed.
4. Classification completeness review completed.
5. Metadata collection plan created.
6. Bounded metadata collection approval gate completed.
7. Collected metadata report shell created and local-only evidence added.
8. Remaining step: owner review of local-only evidence and unresolved production metadata gaps.
9. Implementation remains blocked until collected metadata, rollback plan, and verification plan are owner-approved.

### Recent Notification Delivery Closure Plan

| Patch plan | Patch plan status | Implementation status | Source decision | Scope | Next gate |
| --- | --- | --- | --- | --- | --- |
| `08_PatchPlans/NotificationDeliveryClosurePatchPlan.md` | Draft / Controlled closure plan | Not executed | `09_Decisions/NotificationDeliveryBoundaryDecision.md` | Remaining mobile rollout, closed-app reminder device acceptance, server push regression observation, and legacy RPC Phase B revoke. | [10_Status/NotificationDeliveryStatusGates.md](../10_Status/NotificationDeliveryStatusGates.md) |

## 7. Relationship to Status / Backlog

Patch plans may feed status/backlog tracking after verification. Exact status values, backlog handoff, owner model, due-date model, and remediation status model are Unknown / Needs verification.

## 8. Relationship to BuildAndRelease.md

Patch plans may affect operations and release planning. Exact release gates, deployment process, rollback process, and production approval workflow are Unknown / Needs verification.

## 9. Relationship to TestingStrategy.md

Patch plans may require testing and re-test after implementation. Exact test plan, automated test suite, manual QA flow, release gate, and re-test process are Unknown / Needs verification.

## 10. Patch Plan Authority Model

### What a patch plan may define

- Proposed scope.
- Proposed rationale.
- Proposed verification needs.
- Proposed relationship to audits, ADRs, status/backlog, operations, testing, and release planning.
- Proposed implementation boundaries, if verified later.

### What a patch plan may not claim without verification

- Confirmed bugs or fixes.
- Accepted implementation steps.
- Accepted files, commands, branches, or code ownership.
- Accepted priority, severity, owner, due date, or remediation status.
- Accepted release gate, rollback path, or test plan.
- Accepted mobile, dashboard, web, Supabase, RLS, RPC, storage, auth, database, security, module, release, testing, monitoring, logging, incident, or audit behavior.

### What requires explicit approval before implementation

- Production SQL changes.
- Migrations.
- Functions.
- RLS changes.
- Storage policy changes.
- Auth changes.
- Any uncontrolled production change.

## 11. Known Patch Surfaces Draft

### Mobile

JoinFolk has a mobile app surface. React Native / Expo concepts may apply. Exact mobile patch workflow is Unknown / Needs verification.

### Dashboard

JoinFolk has a dashboard surface. Vite / React concepts may apply. Exact dashboard patch workflow is Unknown / Needs verification.

### Web/Public

JoinFolk may have Web/Public surfaces. Exact Web/Public patch workflow is Unknown / Needs verification.

### Supabase Backend / Database / Storage / Auth

JoinFolk uses or may use Supabase or Supabase-like backend concepts, including RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations. Exact backend, database, storage, and auth patch workflow is Unknown / Needs verification.

### Handbook documents

JoinFolk has handbook layers for governance, product, architecture, database, modules, security, operations, audits, patch plans, decisions / ADRs, and status/backlog. Exact handbook patch workflow is Unknown / Needs verification.

## 12. Known Patch Domains Draft

Known patch domains include:

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

Exact domain boundaries are Unknown / Needs verification.

## 13. Non-Accepted Patch Plan Areas

- Exact patch plan methodology is not accepted yet.
- Exact patch plan template is not accepted yet.
- Exact prioritization model is not accepted yet.
- Exact severity model is not accepted yet.
- Exact owner model is not accepted yet.
- Exact due-date model is not accepted yet.
- Exact remediation workflow is not accepted yet.
- Exact implementation workflow is not accepted yet.
- Exact review workflow is not accepted yet.
- Exact test/re-test workflow is not accepted yet.
- Exact release gate relationship is not accepted yet.
- Exact rollback/reversal model is not accepted yet.
- Exact production approval workflow is not accepted yet.
- Exact status/backlog handoff is not accepted yet.
- Exact audit-to-patch workflow is not accepted yet.
- Exact ADR-to-patch workflow is not accepted yet.
- Exact patch history format is not accepted yet.
- Exact code ownership or file ownership model is not accepted yet.

## 14. Patch Plan Categories Draft

### Product patch

Product patches may address verified product-spec needs. Exact scope and workflow are Unknown / Needs verification.

### Architecture patch

Architecture patches may address verified architecture needs or accepted ADR outcomes. Exact scope and workflow are Unknown / Needs verification.

### Database patch

Database patches may involve database-facing concepts. Production SQL, migrations, functions, RLS, storage, and auth changes require explicit approval. Exact scope and workflow are Unknown / Needs verification.

### Security patch

Security patches may address verified security-sensitive needs. Exact scope, verification, and approval workflow are Unknown / Needs verification.

### Operations patch

Operations patches may address build/release, testing, monitoring/logging, or incident response drafts. Exact scope and workflow are Unknown / Needs verification.

### Module patch

Module patches may address verified product-domain module needs. Exact scope and workflow are Unknown / Needs verification.

### Cross-surface patch

Cross-surface patches may address verified consistency needs across Mobile, Dashboard, Web/Public, Supabase Backend, and Handbook documents. Exact scope and workflow are Unknown / Needs verification.

### Release-readiness patch

Release-readiness patches may relate to verified release preparation needs. Exact release gates and readiness criteria are Unknown / Needs verification.

### Documentation patch

Documentation patches may update handbook content. Exact documentation review and acceptance workflow are Unknown / Needs verification.

## 15. Patch Plan Intake Draft

Patch plan intake may originate from verified audit findings, accepted decisions, accepted backlog items, or explicitly approved remediation needs.

Exact intake criteria and approval workflow are Unknown / Needs verification.

## 16. Patch Plan Evidence Draft

Patch plan evidence may reference verified audit findings, accepted ADRs, accepted backlog items, or explicitly approved remediation needs.

Exact evidence requirements, evidence format, and evidence acceptance rules are Unknown / Needs verification.

## 17. Patch Plan Template

Template only. Do not treat any field as accepted patch data.

| Field | Value |
| --- | --- |
| Patch ID | Unknown / Needs verification |
| Title | Unknown / Needs verification |
| Category | Unknown / Needs verification |
| Domain | Unknown / Needs verification |
| Surface | Unknown / Needs verification |
| Status | Unknown / Needs verification |
| Priority | Unknown / Needs verification |
| Severity | Unknown / Needs verification |
| Owner | Unknown / Needs verification |
| Due date | Unknown / Needs verification |
| Evidence | Unknown / Needs verification |
| Scope | Unknown / Needs verification |
| Implementation | Unknown / Needs verification |
| Test plan | Unknown / Needs verification |
| Rollback | Unknown / Needs verification |
| Release | Unknown / Needs verification |
| Re-test | Unknown / Needs verification |
| Notes | Unknown / Needs verification |

## 18. Patch Lifecycle Draft

A patch lifecycle may include intake, scoping, evidence review, plan drafting, approval, implementation, review, testing, release planning, re-test, and history updates. This is a draft structure only.

Exact lifecycle steps and acceptance rules are Unknown / Needs verification.

## 19. Priority / Severity Draft

Priority and severity are not accepted yet. Patch plans must not assign exact priority or severity without a verified and accepted model.

## 20. Scope Control Draft

Patch plans must preserve scoped diffs and reviewed implementation changes. Exact scope-control process, branch strategy, file ownership, and implementation boundaries are Unknown / Needs verification.

## 21. Implementation Safety Draft

Future implementation changes must be scoped and reviewed before commit. Patch plans must not imply permission to modify application code, Supabase migrations, production systems, or security-sensitive behavior without explicit approval.

Exact implementation workflow is Unknown / Needs verification.

## 22. Production Change Approval Draft

Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Exact production approval workflow is Unknown / Needs verification.

## 23. Testing / Re-Test Relationship Draft

Patch plans may require testing and re-test. Exact test plan, test commands, manual QA flow, automated test suite, release gate, and re-test workflow are Unknown / Needs verification.

## 24. Release / Rollback Relationship Draft

Patch plans may relate to release planning and rollback or reversal. Exact release gate behavior, rollback procedure, deployment process, and release readiness criteria are Unknown / Needs verification.

## 25. ADR Relationship Draft

Patch plans may require ADRs when decisions are needed, or may implement accepted ADR outcomes. Exact ADR relationship and handoff process are Unknown / Needs verification.

## 26. Patch Log / History Draft

Patch history may track patch plan activity and changes over time. Exact patch log format, retention, ownership, and acceptance process are Unknown / Needs verification.

## 27. Product Domain Patch Draft

- Event lifecycle: Patch scope and status are Unknown / Needs verification.
- Viewer roles: Patch scope and status are Unknown / Needs verification.
- Personas and tiers: Patch scope and status are Unknown / Needs verification.
- Ticketing: Patch scope and status are Unknown / Needs verification.
- Reservations: Patch scope and status are Unknown / Needs verification.
- Wallet/ownership: Patch scope and status are Unknown / Needs verification.
- Media/gallery: Patch scope and status are Unknown / Needs verification.
- Feed/discovery: Patch scope and status are Unknown / Needs verification.
- Messaging: Patch scope and status are Unknown / Needs verification.
- Notifications: Patch scope and status are Unknown / Needs verification.
- Staff scanner/check-in: Patch scope and status are Unknown / Needs verification.
- Venue/business tools: Patch scope and status are Unknown / Needs verification.
- Host identity transfer: Patch scope and status are Unknown / Needs verification.
- Ops/admin: Patch scope and status are Unknown / Needs verification.
- Public sharing: Patch scope and status are Unknown / Needs verification.

## 28. Cross-Surface Consistency Requirements

### Mobile

Mobile patch plans must remain consistent with approved product, architecture, database, security, operations, module, audit, ADR, and status/backlog facts. Exact mobile consistency checks are Unknown / Needs verification.

### Dashboard

Dashboard patch plans must remain consistent with approved handbook facts and must not invent dashboard behavior. Exact dashboard consistency checks are Unknown / Needs verification.

### Web/Public

Web/Public patch plans must remain consistent with approved public exposure and product-domain facts. Exact Web/Public consistency checks are Unknown / Needs verification.

### Supabase Backend

Backend patch plans must remain consistent with approved database, security, RLS, RPC, storage, auth, and production-change controls. Exact backend consistency checks are Unknown / Needs verification.

### Handbook

Handbook patch plans must distinguish draft, non-canonical content from verified accepted facts. Exact handbook consistency checks are Unknown / Needs verification.

## 29. Security Risks

- Treating unverified findings as accepted remediation.
- Allowing patch plans to imply security-sensitive implementation authority without approval.
- Creating frontend-only fixes for security-sensitive behavior.
- Making production SQL, migrations, functions, RLS, storage, or auth changes without explicit approval.

## 30. Privacy Risks

- Planning changes that affect private or protected user, profile, event, ticket, reservation, wallet, media, messaging, or ops/admin data without verified scope.
- Recording sensitive evidence in patch plans without accepted evidence rules.

## 31. Determinism Risks

- Assigning priority, severity, status, owner, due date, test plan, rollback, or release state without an accepted model.
- Mixing proposed patches with accepted remediation.
- Allowing cross-surface behavior to drift between Mobile, Dashboard, Web/Public, Supabase Backend, and Handbook documents.

## 32. Operational Risks

- Treating patch plans as release gates before release-readiness criteria are accepted.
- Starting implementation from unverified findings.
- Creating uncontrolled production changes from patch work.

## 33. Maintainability Risks

- Duplicating patch state across audits, patch plans, ADRs, and backlog without a verified relationship model.
- Allowing patch templates to drift from handbook governance.
- Failing to update patch history after implementation, testing, release, or re-test once those workflows are accepted.

## 34. Current Known Implementation

Known implementation surfaces and concepts include Mobile app, Dashboard, Web/Public where applicable, Supabase backend/storage/database/auth concepts, React Native / Expo concepts, Vite / React concepts, RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.

Exact implementation behavior, patch plans, bugs, fixes, files, commands, branches, owners, due dates, priority, severity, release gates, rollback paths, remediation status, and re-test status are Unknown / Needs verification.

## 35. Unknowns / Needs Verification

- Exact patch plan methodology.
- Exact patch plan template.
- Exact prioritization model.
- Exact severity model.
- Exact owner model.
- Exact due-date model.
- Exact remediation workflow.
- Exact implementation workflow.
- Exact review workflow.
- Exact test/re-test workflow.
- Exact release gate relationship.
- Exact rollback/reversal model.
- Exact production approval workflow.
- Exact status/backlog handoff.
- Exact audit-to-patch workflow.
- Exact ADR-to-patch workflow.
- Exact patch history format.
- Exact code ownership or file ownership model.

## 36. Acceptance Criteria for v1.0

For this document to become v1.0, JoinFolk must verify and accept:

- Patch plan methodology.
- Patch plan template.
- Intake criteria.
- Evidence requirements.
- Priority and severity model.
- Owner and due-date model, if any.
- Remediation workflow.
- Implementation and review workflow.
- Scope-control process.
- Testing and re-test workflow.
- Release and rollback relationship.
- Production approval workflow.
- Status/backlog handoff.
- Audit-to-patch and ADR-to-patch workflows.
- Patch log / history format.
- Explicit production-change approval constraints for SQL, migrations, functions, RLS, storage, and auth.

## 37. Open Questions

- What patch plan methodology should JoinFolk accept?
- What patch plan template should be accepted?
- What intake criteria should allow creation of a patch plan?
- What evidence is required before a patch plan can be accepted?
- What priority and severity model should be used?
- Who can own patch plans, and are due dates part of the model?
- How should audit findings become patch plans?
- How should ADRs create or constrain patch plans?
- What implementation and review workflow should apply?
- What scope-control process should be required before commit?
- What testing and re-test process should be required?
- What release gate relationship should patch plans have?
- What rollback or reversal model should be accepted?
- What production approval workflow should apply during patch implementation?
- What patch log or history format should be used?
- How should patch plans preserve cross-surface consistency across Mobile, Dashboard, Web/Public, Supabase Backend, and Handbook documents?
