# Testing Strategy

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is the platform-level testing strategy operations draft for JoinFolk.

This is a handbook draft. It is not a code audit and is not an accepted testing contract. This document describes draft testing boundaries and verification needs.

No test command, manual QA flow, automated test suite, CI check, smoke test, regression test, or release gate is accepted until verified. Prior implementation notes must not be treated as canonical.

## 3. Testing Strategy Definition

Testing strategy defines how JoinFolk should reason about verification across implementation surfaces, product domains, security-sensitive backend behavior, and release readiness.

Known facts:

- JoinFolk has multiple implementation surfaces: mobile app, dashboard, Web/Public where applicable, and Supabase backend/storage/database/auth concepts.
- Testing strategy must preserve product, architecture, database, security, module, and cross-surface consistency.
- Frontend build success alone does not prove backend, RLS, storage, auth, data, or production correctness.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact test strategy.
- Exact automated test suite.
- Exact manual QA process.
- Exact release test gates.
- Exact evidence/reporting behavior.

## 4. Relationship to BuildAndRelease.md

TestingStrategy.md describes verification boundaries and testing needs.

BuildAndRelease.md describes build/release boundaries and release operations. Testing strategy may inform release gates, but exact release gates are not accepted in this draft.

Unknown / needs verification:

- Exact relationship between test results and release approval.
- Exact build/release gates that require testing.
- Exact release evidence requirements.

## 5. Relationship to Handbook Governance

Handbook workflow requires:

- Scoped diffs.
- Git status checks.
- Review before commit.
- No uncontrolled production changes.
- No production SQL/migrations/functions/RLS/storage/auth changes without explicit approval.

Testing strategy may interact with:

- Product specs.
- Architecture specs.
- Database specs.
- Security specs.
- Modules.
- Audits.
- Patch plans.
- Decisions / ADRs.
- Status/backlog.
- Build and release operations.

Unknown / needs verification:

- Exact governance workflow for accepting testing requirements.
- Exact relationship between audits, patch plans, ADRs, and testing scope.

## 6. Authority Model

### What local tests may prove

Local tests may provide evidence that a specific behavior, surface, or check passed in the local environment.

Known build-like check concepts:

- TypeScript build.
- Vite build.
- Dashboard build.
- Mobile checks.

These names are known concepts only and are not accepted canonical tests, commands, QA flows, release gates, or coverage contracts until verified.

Unknown / needs verification:

- Exact test commands.
- Exact automated test suites.
- Exact expected outputs.
- Exact coverage and confidence of local tests.

### What local tests do not prove

Local tests do not by themselves prove:

- Backend correctness.
- RLS correctness.
- Storage policy correctness.
- Auth correctness.
- Production correctness.
- Cross-surface consistency.
- Database migration safety.
- SECURITY DEFINER safety.
- Public exposure safety.
- Release readiness.

Unknown / needs verification:

- Exact required verification beyond local tests.

### What security-sensitive tests require

Security-sensitive tests require authoritative verification of backend/RPC/RLS/storage/auth behavior and must not rely only on frontend behavior.

Unknown / needs verification:

- Exact security-sensitive test model.
- Exact environment requirements.
- Exact approval requirements for tests involving production SQL, migrations, functions, RLS, storage, or auth.

## 7. Known Testing Surfaces Draft

### Mobile

Known facts:

- JoinFolk has a mobile app.
- JoinFolk uses or may use React Native / Expo concepts for mobile.
- Prior project context mentioned mobile checks and mobile behavior.

Unknown / needs verification:

- Exact mobile testing behavior.
- Exact mobile test commands.
- Exact mobile manual QA process.

### Dashboard

Known facts:

- JoinFolk has dashboard surfaces.
- JoinFolk uses or may use Vite / React concepts for dashboard or web surfaces.
- Prior project context mentioned dashboard behavior and dashboard build.

Unknown / needs verification:

- Exact dashboard testing behavior.
- Exact dashboard test commands.
- Exact dashboard manual QA process.

### Web/Public

Known facts:

- JoinFolk may have Web/Public surfaces.
- JoinFolk uses or may use Vite / React concepts for dashboard or web surfaces.

Unknown / needs verification:

- Exact Web/Public testing behavior.
- Exact Web/Public test commands.
- Exact public exposure test behavior.

### Supabase Backend / Database / Storage / Auth

Known facts:

- JoinFolk uses or may use Supabase or Supabase-like backend concepts.
- JoinFolk uses or may use RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.

Unknown / needs verification:

- Exact Supabase/backend testing behavior.
- Exact RLS/RPC/storage/auth testing behavior.
- Exact production verification behavior.

## 8. Known Test Concepts Draft

Prior project context mentioned build-like checks such as:

- TypeScript build.
- Vite build.
- Dashboard build.
- Mobile checks.

Prior project context mentioned manual testing or QA-like concerns around:

- Mobile behavior.
- Dashboard behavior.
- Venue editor behavior.
- Ticketing.
- Reservations.
- Media/gallery.
- Staff scanner/check-in.
- Host identity transfer.
- Public exposure.
- Feed/discovery.
- Messaging where applicable.
- Storage/RLS/auth/security-sensitive behavior.

These names are known concepts only and must not be treated as accepted canonical tests, commands, QA flows, release gates, or coverage contracts until verified.

## 9. Non-Accepted Testing Areas

The following areas are not accepted yet:

- Exact test strategy.
- Exact test commands.
- Exact automated test suite.
- Exact manual QA process.
- Exact smoke test process.
- Exact regression test process.
- Exact release test gate.
- Exact CI test behavior.
- Exact mobile testing behavior.
- Exact dashboard testing behavior.
- Exact Web/Public testing behavior.
- Exact Supabase/backend testing behavior.
- Exact RLS/RPC/storage/auth testing behavior.
- Exact production verification behavior.
- Exact test evidence or reporting behavior.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 10. Local Verification Draft

Known facts:

- Prior project context mentioned TypeScript build, Vite build, dashboard build, and mobile checks.
- Handbook workflow requires scoped diffs, git status checks, and review before commit.

Unknown / needs verification:

- Exact local verification commands.
- Exact local test coverage.
- Exact required local checks by surface or change type.
- Exact relationship between local verification and release readiness.

No exact local verification process is accepted in this draft.

## 11. Automated Testing Draft

Known facts:

- Exact automated test suite is not accepted yet.

Unknown / needs verification:

- Whether automated test suites exist as accepted behavior.
- Exact automated test commands.
- Exact automated test coverage.
- Exact CI behavior.
- Exact release gate relationship.

No automated testing behavior is accepted in this draft.

## 12. Manual QA Draft

Known facts:

- Prior project context mentioned manual testing or QA-like concerns around multiple product domains.
- Exact manual QA process is not accepted yet.

Unknown / needs verification:

- Exact manual QA flows.
- Exact manual QA checklist.
- Exact QA ownership.
- Exact QA evidence requirements.
- Exact relationship between manual QA and release approval.

No manual QA process is accepted in this draft.

## 13. Smoke Testing Draft

Known facts:

- Exact smoke test process is not accepted yet.

Unknown / needs verification:

- Exact smoke test scope.
- Exact smoke test commands or flows.
- Exact smoke test ownership.
- Exact smoke test relationship to release gates.

No smoke test process is accepted in this draft.

## 14. Regression Testing Draft

Known facts:

- Exact regression test process is not accepted yet.

Unknown / needs verification:

- Exact regression test scope.
- Exact regression test commands or flows.
- Exact regression test ownership.
- Exact regression test relationship to release gates.

No regression test process is accepted in this draft.

## 15. Mobile Testing Draft

Known facts:

- JoinFolk has a mobile app.
- Prior project context mentioned mobile behavior and mobile checks.

Unknown / needs verification:

- Exact mobile testing behavior.
- Exact mobile test commands.
- Exact mobile manual QA flows.
- Exact mobile release test gates.
- Exact relationship between mobile testing and backend/RLS/storage/auth behavior.

No mobile testing behavior is accepted in this draft.

## 16. Dashboard Testing Draft

Known facts:

- JoinFolk has dashboard surfaces.
- Prior project context mentioned dashboard behavior and dashboard build.

Unknown / needs verification:

- Exact dashboard testing behavior.
- Exact dashboard test commands.
- Exact dashboard manual QA flows.
- Exact dashboard release test gates.
- Exact relationship between dashboard testing and backend/RLS/storage/auth behavior.

No dashboard testing behavior is accepted in this draft.

## 17. Web/Public Testing Draft

Known facts:

- JoinFolk may have Web/Public surfaces.

Unknown / needs verification:

- Exact Web/Public testing behavior.
- Exact public exposure testing behavior.
- Exact public event or discovery test flows.
- Exact Web/Public release test gates.
- Exact relationship between Web/Public testing and backend/RLS/storage/auth behavior.

No Web/Public testing behavior is accepted in this draft.

## 18. Supabase / Backend Testing Draft

Known facts:

- JoinFolk uses or may use Supabase or Supabase-like backend concepts.
- JoinFolk uses or may use migrations.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact Supabase/backend testing behavior.
- Exact database testing behavior.
- Exact migration testing behavior.
- Exact production verification behavior.
- Exact approval requirements for backend-sensitive testing.

No Supabase/backend testing behavior is accepted in this draft.

## 19. RLS / RPC / SECURITY DEFINER Testing Draft

Known facts:

- JoinFolk uses or may use RLS, RPC, and SECURITY DEFINER concepts.
- Storage/RLS/auth/security-sensitive behavior was mentioned as a manual testing or QA-like concern.

Unknown / needs verification:

- Exact RLS testing behavior.
- Exact RPC testing behavior.
- Exact SECURITY DEFINER testing behavior.
- Exact permission/public exposure test coverage.
- Exact test evidence requirements.

No RLS, RPC, or SECURITY DEFINER testing behavior is accepted in this draft.

## 20. Storage / Auth Testing Draft

Known facts:

- JoinFolk uses or may use storage policies and auth.
- Storage/RLS/auth/security-sensitive behavior was mentioned as a manual testing or QA-like concern.

Unknown / needs verification:

- Exact storage testing behavior.
- Exact auth testing behavior.
- Exact media visibility/upload test coverage.
- Exact public/private/protected storage behavior testing.

No storage or auth testing behavior is accepted in this draft.

## 21. Product Domain Testing Draft

### Event lifecycle

Unknown / needs verification:

- Exact event lifecycle testing behavior.

### Viewer roles

Unknown / needs verification:

- Exact viewer role testing behavior.

### Personas and tiers

Unknown / needs verification:

- Exact persona/tier testing behavior.

### Ticketing

Known facts:

- Prior project context mentioned ticketing as a manual testing or QA-like concern.

Unknown / needs verification:

- Exact ticketing testing behavior.

### Reservations

Known facts:

- Prior project context mentioned reservations as a manual testing or QA-like concern.

Unknown / needs verification:

- Exact reservation testing behavior.

### Wallet/ownership

Unknown / needs verification:

- Exact wallet/ownership testing behavior.

### Media/gallery

Known facts:

- Prior project context mentioned media/gallery as a manual testing or QA-like concern.

Unknown / needs verification:

- Exact media/gallery testing behavior.

### Feed/discovery

Known facts:

- Prior project context mentioned feed/discovery as a manual testing or QA-like concern.

Unknown / needs verification:

- Exact feed/discovery testing behavior.

### Messaging

Known facts:

- Prior project context mentioned messaging where applicable as a manual testing or QA-like concern.

Unknown / needs verification:

- Whether messaging testing applies.
- Exact messaging testing behavior.

### Notifications

Unknown / needs verification:

- Exact notification testing behavior.

### Staff scanner/check-in

Known facts:

- Prior project context mentioned staff scanner/check-in as a manual testing or QA-like concern.

Unknown / needs verification:

- Exact staff scanner/check-in testing behavior.

### Venue/business tools

Known facts:

- Prior project context mentioned venue editor behavior as a manual testing or QA-like concern.

Unknown / needs verification:

- Exact venue/business tools testing behavior.
- Exact venue editor testing behavior.

### Host identity transfer

Known facts:

- Prior project context mentioned host identity transfer as a manual testing or QA-like concern.

Unknown / needs verification:

- Exact host identity transfer testing behavior.

### Ops/admin

Unknown / needs verification:

- Exact ops/admin testing behavior.

### Public sharing

Known facts:

- Prior project context mentioned public exposure as a manual testing or QA-like concern.

Unknown / needs verification:

- Exact public sharing/public exposure testing behavior.

## 22. Test Evidence / Reporting Draft

Known facts:

- Exact test evidence or reporting behavior is not accepted yet.

Unknown / needs verification:

- Exact test reporting behavior.
- Exact evidence format.
- Exact storage or retention of test evidence.
- Exact relationship between test evidence, release approval, audits, patch plans, ADRs, status, and backlog.

No test evidence or reporting behavior is accepted in this draft.

## 23. Release Gate Relationship Draft

Known facts:

- Exact release test gate is not accepted yet.
- Testing strategy may interact with build and release operations.

Unknown / needs verification:

- Exact release test gates.
- Exact relationship between tests and release approval.
- Exact blocking behavior for failed tests.
- Exact release risk acceptance behavior.

No release test gate behavior is accepted in this draft.

## 24. Cross-Surface Consistency Requirements

### Mobile

Unknown / needs verification:

- Exact mobile cross-surface test requirements.
- Exact relationship between mobile tests and dashboard, Web/Public, and Supabase backend behavior.

### Dashboard

Unknown / needs verification:

- Exact dashboard cross-surface test requirements.
- Exact relationship between dashboard tests and mobile, Web/Public, and Supabase backend behavior.

### Web/Public

Unknown / needs verification:

- Exact Web/Public cross-surface test requirements.
- Exact relationship between Web/Public tests and mobile, dashboard, and Supabase backend behavior.

### Supabase Backend

Unknown / needs verification:

- Exact Supabase backend cross-surface test requirements.
- Exact relationship between backend/RLS/RPC/storage/auth tests and all frontend surfaces.

## 25. Security Risks

Security risks to verify:

- Frontend build success being treated as backend/RLS/storage/auth correctness.
- Security-sensitive behavior being tested only through frontend UX.
- RLS, RPC, SECURITY DEFINER, storage, or auth behavior not tested before release.
- Production SQL, migrations, functions, RLS, storage, or auth changes without explicit approval.
- Release gates missing security-sensitive test coverage.

No security risk in this section is a confirmed testing defect unless later verified.

## 26. Privacy Risks

Privacy risks to verify:

- Public/private/protected behavior not tested before release.
- Media, feed, messaging, ticketing, reservation, wallet/ownership, venue/business, or public sharing behavior exposing protected data due to insufficient verification.
- Cross-surface test gaps exposing privacy-sensitive behavior.

No privacy risk in this section is a confirmed testing defect unless later verified.

## 27. Determinism Risks

Determinism risks to verify:

- Local tests differing from CI/CD or production behavior.
- Manual QA producing inconsistent results without accepted flows.
- Cross-surface behavior tested inconsistently.
- Backend/RLS/RPC/storage/auth assumptions diverging from frontend test assumptions.
- Test evidence or reporting interpreted differently across release decisions.

## 28. Operational Risks

Operational risks to verify:

- Missing accepted test commands.
- Missing accepted manual QA process.
- Missing accepted automated tests.
- Missing accepted smoke or regression process.
- Missing accepted release test gate.
- Missing accepted production verification process.

## 29. Maintainability Risks

Maintainability risks to verify:

- Testing strategy scattered across product domains, surfaces, CI/CD, and release process without clear ownership.
- Commands, test suites, QA flows, checklists, release gates, CI jobs, or production verification methods treated as canonical before verification.
- TestingStrategy.md duplicating or contradicting product, architecture, database, security, module, audit, patch plan, ADR, status/backlog, or BuildAndRelease.md documents.

## 30. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has multiple implementation surfaces: mobile app, dashboard, Web/Public where applicable, and Supabase backend/storage/database/auth concepts.
- JoinFolk uses or may use React Native / Expo concepts for mobile.
- JoinFolk uses or may use Vite / React concepts for dashboard or web surfaces.
- JoinFolk uses or may use Supabase or Supabase-like backend concepts.
- JoinFolk uses or may use RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.
- Prior context mentioned build-like checks and manual testing or QA-like concerns, but none are accepted canonical tests, commands, QA flows, release gates, or coverage contracts.
- Handbook workflow requires scoped diffs, git status checks, review before commit, no uncontrolled production changes, and no production SQL/migrations/functions/RLS/storage/auth changes without explicit approval.

Unknown / needs verification:

- Exact accepted testing implementation across mobile, dashboard, Web/Public, Supabase backend, database, storage, auth, RLS, RPC, SECURITY DEFINER, product domains, CI/CD, release gates, and production verification.

## 31. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact test strategy.
- Exact test commands.
- Exact automated test suite.
- Exact manual QA process.
- Exact smoke test process.
- Exact regression test process.
- Exact release test gate.
- Exact CI test behavior.
- Exact mobile testing behavior.
- Exact dashboard testing behavior.
- Exact Web/Public testing behavior.
- Exact Supabase/backend testing behavior.
- Exact RLS/RPC/storage/auth testing behavior.
- Exact production verification behavior.
- Exact test evidence or reporting behavior.
- Exact local verification process.
- Exact product domain testing behavior.
- Exact cross-surface testing requirements.

## 32. Acceptance Criteria for v1.0

Testing Strategy v1.0 can be accepted only after verification establishes:

- Accepted testing vocabulary.
- Accepted testing surfaces and ownership.
- Accepted local verification process.
- Accepted automated testing process.
- Accepted manual QA process.
- Accepted smoke testing process.
- Accepted regression testing process.
- Accepted mobile testing behavior.
- Accepted dashboard testing behavior.
- Accepted Web/Public testing behavior where applicable.
- Accepted Supabase/backend testing behavior.
- Accepted RLS/RPC/SECURITY DEFINER testing behavior.
- Accepted storage/auth testing behavior.
- Accepted product-domain testing behavior.
- Accepted test evidence/reporting behavior.
- Accepted release gate relationship.
- Accepted CI test behavior.
- Accepted production verification behavior.
- Accepted cross-surface consistency testing requirements.
- Accepted relationship to product, architecture, database, security, module, audit, patch plan, ADR, status/backlog, and BuildAndRelease.md documents.

Until these criteria are met, this document remains non-canonical.

## 33. Open Questions

- What test strategy is accepted?
- What test commands are accepted?
- What automated test suites are accepted?
- What manual QA process is accepted?
- What smoke test process is accepted?
- What regression test process is accepted?
- What CI test behavior is accepted?
- What release test gates are accepted?
- What mobile testing behavior is accepted?
- What dashboard testing behavior is accepted?
- What Web/Public testing behavior is accepted?
- What Supabase/backend testing behavior is accepted?
- What RLS/RPC/SECURITY DEFINER testing behavior is accepted?
- What storage/auth testing behavior is accepted?
- What production verification behavior is accepted?
- What test evidence or reporting behavior is accepted?
- How should testing strategy preserve product, architecture, database, security, module, and cross-surface consistency?
- How should TestingStrategy.md relate to BuildAndRelease.md?
- Which surfaces are part of the accepted testing process today: mobile, dashboard, Web/Public, and Supabase backend?
