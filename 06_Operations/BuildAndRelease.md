# Build and Release

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is the platform-level build and release operations draft for JoinFolk.

This is a handbook draft. It is not a code audit and is not an accepted release contract. This document describes draft build/release boundaries and verification needs.

No build, deployment, CI/CD, TestFlight, App Store, Play Store, Cloudflare, Supabase, migration, or production process is accepted until verified. Prior implementation notes must not be treated as canonical.

## 3. Build and Release Definition

Build and release operations cover the verification, packaging, deployment, approval, rollback, and release logging concepts required to move JoinFolk changes across implementation surfaces.

Known surfaces:

- Mobile app.
- Dashboard.
- Web/Public where applicable.
- Supabase backend/storage/database/auth concepts.

Known requirement:

- Release operations must preserve product, security, database, module, and cross-surface consistency.
- Frontend build success alone does not prove backend, RLS, storage, auth, or production correctness.

Unknown / needs verification:

- Exact CI/CD process.
- Exact branch strategy.
- Exact release gates.
- Exact build commands.
- Exact deployment process.
- Exact rollback process.
- Exact production approval process.

## 4. Relationship to Handbook Governance

Handbook workflow requires:

- No production SQL/migrations/functions/RLS/storage/auth changes without explicit approval.
- Scoped diffs.
- Git status checks.
- Review before commit.
- No uncontrolled production changes.

Build/release operations may interact with:

- Product specs.
- Architecture specs.
- Database specs.
- Security specs.
- Modules.
- Audits.
- Patch plans.
- Decisions / ADRs.
- Status/backlog.

Unknown / needs verification:

- Exact workflow for promoting handbook guidance into accepted release policy.
- Exact relationship between audits, patch plans, ADRs, and release gates.

## 5. Authority Model

### What local build checks may prove

Local build checks may provide evidence that a specific surface can compile or pass a local verification step in the checked environment.

Examples of known build-like concepts:

- TypeScript build.
- Vite build.
- Dashboard build.
- Mobile checks.

These are known concepts only and are not accepted canonical commands or release gates until verified.

Unknown / needs verification:

- Exact commands.
- Exact expected outputs.
- Exact failure handling.
- Exact relationship between local checks and release readiness.

### What local build checks do not prove

Local build checks do not by themselves prove:

- Backend correctness.
- RLS correctness.
- Storage policy correctness.
- Auth correctness.
- Production readiness.
- Cross-surface consistency.
- Database migration safety.
- SECURITY DEFINER safety.
- Public exposure safety.
- App store or market review readiness.

Unknown / needs verification:

- Exact required verification beyond local build checks.

### What production changes require

Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact production approval process.
- Exact approver model.
- Exact rollback process.
- Exact release logging or audit requirements.

## 6. Known Surfaces Draft

### Mobile

Known facts:

- JoinFolk has a mobile app.
- JoinFolk uses or may use React Native / Expo concepts for mobile.
- Prior project context mentioned mobile checks.

Unknown / needs verification:

- Exact mobile build process.
- Exact mobile release process.
- Exact TestFlight, App Store, or Play Store behavior.

### Dashboard

Known facts:

- JoinFolk has a dashboard surface.
- JoinFolk uses or may use Vite / React concepts for dashboard or web surfaces.
- Prior project context mentioned dashboard build.

Unknown / needs verification:

- Exact dashboard build process.
- Exact dashboard deployment process.
- Exact dashboard release gates.

### Web/Public

Known facts:

- JoinFolk may have Web/Public surfaces.
- JoinFolk uses or may use Vite / React concepts for dashboard or web surfaces.

Unknown / needs verification:

- Exact Web/Public build process.
- Exact Web/Public deployment process.
- Exact Cloudflare/web hosting behavior.

### Supabase Backend / Database / Storage / Auth

Known facts:

- JoinFolk uses or may use Supabase or Supabase-like backend concepts.
- JoinFolk uses or may use RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.
- Supabase production concepts were mentioned in prior project context.

Unknown / needs verification:

- Exact Supabase migration/release process.
- Exact production database release process.
- Exact storage/RLS/auth release behavior.

## 7. Known Build Concepts Draft

Prior project context mentioned build commands or build-like checks such as:

- TypeScript build.
- Vite build.
- Dashboard build.
- Mobile checks.

These names are known concepts only and must not be treated as accepted canonical commands, CI/CD process, deployment target, or environment contract until verified.

Unknown / needs verification:

- Exact command names.
- Exact local verification process.
- Exact required build checks per surface.
- Exact relationship between build success and release eligibility.

## 8. Known Release Concepts Draft

Prior project context mentioned release or deployment surfaces such as:

- TestFlight.
- App store / market review concepts.
- Cloudflare Pages or web hosting concepts.
- Supabase production concepts.

These names are known concepts only and must not be treated as accepted canonical release process, CI/CD process, deployment target, command, or environment contract until verified.

Unknown / needs verification:

- Exact release targets.
- Exact release channels.
- Exact deployment environments.
- Exact production process.

## 9. Non-Accepted Build / Release Areas

The following areas are not accepted yet:

- Exact CI/CD process.
- Exact branch strategy.
- Exact release gates.
- Exact build commands.
- Exact deployment process.
- Exact rollback process.
- Exact production approval process.
- Exact app store/TestFlight/Play Store process.
- Exact Cloudflare/web deployment process.
- Exact Supabase migration/release process.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 10. Local Verification Draft

Known facts:

- Prior project context mentioned TypeScript build, Vite build, dashboard build, and mobile checks.
- Handbook workflow requires scoped diffs, git status checks, and review before commit.

Unknown / needs verification:

- Exact local verification commands.
- Exact expected local verification coverage.
- Exact required order of checks.
- Exact relationship between local verification and release approval.

No exact local verification process is accepted in this draft.

## 11. Dashboard Build Draft

Known facts:

- JoinFolk has a dashboard surface.
- Prior project context mentioned dashboard build.
- JoinFolk uses or may use Vite / React concepts for dashboard or web surfaces.

Unknown / needs verification:

- Exact dashboard build command.
- Exact dashboard build environment.
- Exact dashboard build artifacts.
- Exact dashboard release gate behavior.

No exact dashboard build process is accepted in this draft.

## 12. Mobile Build Draft

Known facts:

- JoinFolk has a mobile app.
- JoinFolk uses or may use React Native / Expo concepts for mobile.
- Prior project context mentioned mobile checks.

Unknown / needs verification:

- Exact mobile build commands.
- Exact mobile check commands.
- Exact mobile build artifacts.
- Exact mobile release channel behavior.

No exact mobile build process is accepted in this draft.

## 13. Web/Public Build Draft

Known facts:

- JoinFolk may have Web/Public surfaces.
- JoinFolk uses or may use Vite / React concepts for dashboard or web surfaces.

Unknown / needs verification:

- Exact Web/Public build process.
- Exact Web/Public build command.
- Exact Web/Public artifact model.
- Exact public hosting relationship.

No exact Web/Public build process is accepted in this draft.

## 14. Supabase / Database Release Draft

Known facts:

- JoinFolk uses or may use Supabase or Supabase-like backend concepts.
- JoinFolk uses or may use migrations.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact Supabase migration process.
- Exact database release process.
- Exact production approval workflow.
- Exact database rollback/reversal behavior.

No exact Supabase or database release process is accepted in this draft.

## 15. Storage / RLS / Auth Release Draft

Known facts:

- JoinFolk uses or may use RLS, RPC, SECURITY DEFINER, storage policies, and auth.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact storage policy release behavior.
- Exact RLS release behavior.
- Exact auth release behavior.
- Exact SECURITY DEFINER release behavior.
- Exact approval and rollback behavior for these changes.

No storage, RLS, auth, RPC, or SECURITY DEFINER release behavior is accepted in this draft.

## 16. CI/CD Draft

Known facts:

- Exact CI/CD process is not accepted yet.

Unknown / needs verification:

- Whether CI/CD exists as an accepted process.
- Exact CI/CD jobs.
- Exact CI/CD environments.
- Exact CI/CD release gates.
- Exact relationship between CI/CD and production approval.

No CI/CD behavior is accepted in this draft.

## 17. TestFlight / App Store / Play Store Draft

Known facts:

- Prior project context mentioned TestFlight.
- Prior project context mentioned app store / market review concepts.

Unknown / needs verification:

- Exact TestFlight process.
- Exact App Store process.
- Exact Play Store process.
- Exact market review behavior.
- Exact mobile release approval workflow.

No TestFlight, App Store, or Play Store behavior is accepted in this draft.

## 18. Web Hosting / Cloudflare Draft

Known facts:

- Prior project context mentioned Cloudflare Pages or web hosting concepts.

Unknown / needs verification:

- Exact Cloudflare process.
- Exact web hosting process.
- Exact deployment target.
- Exact environment model.
- Exact release and rollback behavior.

No Cloudflare or web hosting behavior is accepted in this draft.

## 19. Release Gate Draft

Known facts:

- Exact release gates are not accepted yet.
- Release operations must preserve product, security, database, module, and cross-surface consistency.

Unknown / needs verification:

- Exact release gates.
- Exact product spec gate.
- Exact architecture spec gate.
- Exact database spec gate.
- Exact security spec gate.
- Exact module consistency gate.
- Exact audit, patch plan, ADR, status, or backlog gate.

No release gate behavior is accepted in this draft.

## 20. Rollback / Reversal Draft

Known facts:

- Exact rollback process is not accepted yet.

Unknown / needs verification:

- Exact rollback process.
- Exact reversal process.
- Exact rollback approval behavior.
- Exact rollback logging behavior.
- Exact rollback behavior for mobile, dashboard, Web/Public, Supabase backend, database, storage, RLS, auth, and production changes.

No rollback or reversal behavior is accepted in this draft.

## 21. Production Approval Draft

Known facts:

- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.
- Handbook workflow requires no uncontrolled production changes.

Unknown / needs verification:

- Exact production approval process.
- Exact approval authority.
- Exact approval record.
- Exact approval scope.
- Exact emergency approval behavior.

No exact production approval workflow is accepted in this draft.

## 22. Auditability / Release Logging Draft

Known facts:

- Handbook workflow requires review before commit and git status checks.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact release logging behavior.
- Exact auditability model.
- Exact release notes behavior.
- Exact approval evidence behavior.
- Exact relationship between release logging and handbook status/backlog.

No release logging or auditability behavior is accepted in this draft.

## 23. Cross-Surface Consistency Requirements

### Mobile

Unknown / needs verification:

- Exact mobile release consistency requirements.
- Exact relationship between mobile release and dashboard, Web/Public, and Supabase backend compatibility.

### Dashboard

Unknown / needs verification:

- Exact dashboard release consistency requirements.
- Exact relationship between dashboard release and mobile, Web/Public, and Supabase backend compatibility.

### Web/Public

Unknown / needs verification:

- Exact Web/Public release consistency requirements.
- Exact relationship between Web/Public release and mobile, dashboard, and Supabase backend compatibility.

### Supabase Backend

Unknown / needs verification:

- Exact Supabase backend release consistency requirements.
- Exact relationship between backend/database/storage/auth changes and all frontend surfaces.

## 24. Security Risks

Security risks to verify:

- Frontend build success being treated as backend/RLS/storage/auth correctness.
- Production SQL, migrations, functions, RLS, storage, or auth changes without explicit approval.
- Release gates missing security review.
- Deployment target or environment assumptions being treated as canonical before verification.
- Uncontrolled production changes.

No security risk in this section is a confirmed release defect unless later verified.

## 25. Privacy Risks

Privacy risks to verify:

- Release changes exposing private/protected/non-public data due to unverified RLS, storage, auth, feed, media, messaging, or public sharing behavior.
- Production changes affecting privacy-sensitive behavior without explicit approval.
- Cross-surface release mismatch exposing protected data.

No privacy risk in this section is a confirmed release defect unless later verified.

## 26. Determinism Risks

Determinism risks to verify:

- Different surfaces released with incompatible assumptions.
- Local checks differing from CI/CD or production behavior.
- Build commands or release gates interpreted differently across teams.
- Backend, RLS, storage, auth, or database changes diverging from frontend assumptions.
- Release status diverging from handbook specs, audits, patch plans, ADRs, status, or backlog.

## 27. Operational Risks

Operational risks to verify:

- Missing rollback or reversal process.
- Missing production approval evidence.
- Missing release logging.
- Unverified app store, TestFlight, Play Store, Cloudflare, web hosting, or Supabase production process.
- Unclear branch strategy or release ownership.

## 28. Maintainability Risks

Maintainability risks to verify:

- Build/release process scattered across surfaces without clear ownership.
- Commands, branches, environments, deployment targets, CI jobs, release gates, or production processes treated as canonical before verification.
- BuildAndRelease.md duplicating or contradicting product, architecture, database, security, module, audit, patch plan, ADR, or status/backlog documents.
- Release process updated without preserving scoped diffs and review-before-commit workflow.

## 29. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has multiple implementation surfaces: mobile app, dashboard, Web/Public where applicable, and Supabase backend/storage/database/auth concepts.
- JoinFolk uses or may use React Native / Expo concepts for mobile.
- JoinFolk uses or may use Vite / React concepts for dashboard or web surfaces.
- JoinFolk uses or may use Supabase or Supabase-like backend concepts.
- JoinFolk uses or may use RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.
- Prior context mentioned build commands, build-like checks, release surfaces, and deployment surfaces, but none are accepted canonical release process, CI/CD process, deployment target, command, or environment contracts.
- Handbook workflow requires no production SQL/migrations/functions/RLS/storage/auth changes without explicit approval, scoped diffs, git status checks, review before commit, and no uncontrolled production changes.

Unknown / needs verification:

- Exact accepted build and release implementation across mobile, dashboard, Web/Public, Supabase backend, database, storage, auth, CI/CD, production approval, and rollback.

## 30. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact CI/CD process.
- Exact branch strategy.
- Exact release gates.
- Exact build commands.
- Exact deployment process.
- Exact rollback process.
- Exact production approval process.
- Exact app store/TestFlight/Play Store process.
- Exact Cloudflare/web deployment process.
- Exact Supabase migration/release process.
- Exact local verification process.
- Exact dashboard build process.
- Exact mobile build process.
- Exact Web/Public build process.
- Exact storage/RLS/auth release behavior.
- Exact auditability/release logging behavior.
- Exact cross-surface consistency requirements.

## 31. Acceptance Criteria for v1.0

Build and Release v1.0 can be accepted only after verification establishes:

- Accepted build/release vocabulary.
- Accepted implementation surfaces and ownership.
- Accepted local verification process.
- Accepted dashboard build process.
- Accepted mobile build process.
- Accepted Web/Public build process where applicable.
- Accepted Supabase/database/storage/auth release process.
- Accepted CI/CD process.
- Accepted branch strategy.
- Accepted release gates.
- Accepted deployment process.
- Accepted production approval process.
- Accepted rollback/reversal process.
- Accepted TestFlight/App Store/Play Store process where applicable.
- Accepted Cloudflare/web hosting process where applicable.
- Accepted auditability and release logging behavior.
- Accepted cross-surface consistency requirements.
- Accepted relationship to product, architecture, database, security, module, audit, patch plan, ADR, and status/backlog documents.

Until these criteria are met, this document remains non-canonical.

## 32. Open Questions

- What CI/CD process is accepted?
- What branch strategy is accepted?
- What release gates are accepted?
- What build commands are accepted?
- What local verification process is accepted?
- What dashboard build process is accepted?
- What mobile build process is accepted?
- What Web/Public build process is accepted?
- What deployment process is accepted?
- What rollback or reversal process is accepted?
- What production approval process is accepted?
- What app store, TestFlight, or Play Store process is accepted?
- What Cloudflare or web hosting process is accepted?
- What Supabase migration/release process is accepted?
- What storage, RLS, auth, RPC, or SECURITY DEFINER release behavior is accepted?
- What release logging or auditability behavior is accepted?
- How should release operations prove product, security, database, module, and cross-surface consistency?
- Which surfaces are part of the accepted release process today: mobile, dashboard, Web/Public, and Supabase backend?
