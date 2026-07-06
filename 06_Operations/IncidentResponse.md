# Incident Response

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is the platform-level incident response operations draft for JoinFolk.

This is a handbook draft. It is not a code audit and is not an accepted incident response contract. This document describes draft incident response boundaries and verification needs.

No incident severity model, escalation process, owner, response time, rollback path, communication process, alert rule, monitoring/logging process, or production response process is accepted until verified. Prior implementation notes must not be treated as canonical.

## 3. Incident Response Definition

Incident response covers the draft boundaries for detecting, classifying, triaging, containing, mitigating, communicating, evidencing, analyzing, and learning from incidents across JoinFolk implementation surfaces.

Known facts:

- JoinFolk has multiple implementation surfaces: mobile app, dashboard, Web/Public where applicable, and Supabase backend/storage/database/auth concepts.
- Incident response must preserve product, architecture, database, security, module, audit, monitoring/logging, build/release, testing, and cross-surface consistency.
- Frontend symptoms alone do not prove backend, RLS, storage, auth, data, security, or production root cause.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact incident response strategy.
- Exact incident severity model.
- Exact owner/escalation model.
- Exact response times, rollback, hotfix, communication, audit, RCA, and postmortem process.

## 4. Relationship to BuildAndRelease.md

Incident response may require build/release coordination, but exact rollback, hotfix, release, and production response behavior is not accepted yet.

Unknown / needs verification:

- Exact relationship between incident response and build/release operations.
- Exact emergency release behavior.
- Exact rollback or reversal relationship.

## 5. Relationship to TestingStrategy.md

Incident response may require testing or verification before, during, or after mitigation, but exact incident testing behavior is not accepted yet.

Unknown / needs verification:

- Exact incident verification process.
- Exact regression or smoke testing relationship.
- Exact test evidence requirements during incidents.

## 6. Relationship to MonitoringAndLogging.md

Monitoring/logging may support incident response, but exact incident response integration is not accepted yet.

Unknown / needs verification:

- Exact monitoring/logging integration.
- Exact alert-to-incident relationship.
- Exact evidence or timeline logging behavior.

## 7. Relationship to Handbook Governance

Handbook workflow requires:

- Scoped diffs.
- Git status checks.
- Review before commit.
- No uncontrolled production changes.
- No production SQL/migrations/functions/RLS/storage/auth changes without explicit approval.

Incident response may interact with:

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
- Testing strategy.
- Monitoring and logging.

Unknown / needs verification:

- Exact governance workflow for incident response.
- Exact relationship between incidents, audits, patch plans, ADRs, and status/backlog.

## 8. Authority Model

### What frontend symptoms may indicate

Frontend symptoms may indicate an issue affecting a user-facing surface.

Examples of affected surfaces:

- Mobile app.
- Dashboard.
- Web/Public where applicable.

Unknown / needs verification:

- Exact relationship between frontend symptoms and incident classification.
- Exact diagnostic value of frontend symptoms.

### What frontend symptoms do not prove

Frontend symptoms do not by themselves prove:

- Backend root cause.
- RLS root cause.
- Storage policy root cause.
- Auth root cause.
- Data root cause.
- Security root cause.
- Production root cause.
- Incident severity.
- Correct rollback or mitigation path.

Unknown / needs verification:

- Exact evidence required before root cause, severity, or mitigation is accepted.

### What backend/audit/production evidence must establish where security-sensitive

Backend, audit, and production evidence must establish facts for security-sensitive incidents.

Security-sensitive areas include:

- Ops/admin behavior.
- Host identity transfer.
- Public exposure.
- Feed visibility.
- Media visibility/upload.
- Messaging where applicable.
- Ticketing.
- Reservations.
- Wallet/ownership.
- Staff scanner/check-in.
- Storage/RLS/auth behavior.
- Production changes.

Unknown / needs verification:

- Exact evidence requirements.
- Exact audit/timeline requirements.
- Exact production verification behavior.

## 9. Known Incident Surfaces Draft

### Mobile

Known facts:

- JoinFolk has a mobile app.
- JoinFolk uses or may use React Native / Expo concepts for mobile.

Unknown / needs verification:

- Exact mobile incident behavior.
- Exact mobile evidence and triage behavior.

### Dashboard

Known facts:

- JoinFolk has dashboard surfaces.
- JoinFolk uses or may use Vite / React concepts for dashboard or web surfaces.

Unknown / needs verification:

- Exact dashboard incident behavior.
- Exact Dashboard/Ops incident behavior.

### Web/Public

Known facts:

- JoinFolk may have Web/Public surfaces.
- JoinFolk uses or may use Vite / React concepts for dashboard or web surfaces.

Unknown / needs verification:

- Exact Web/Public incident behavior.
- Exact public exposure incident behavior.

### Supabase Backend / Database / Storage / Auth

Known facts:

- JoinFolk uses or may use Supabase or Supabase-like backend concepts.
- JoinFolk uses or may use RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.

Unknown / needs verification:

- Exact Supabase/backend incident behavior.
- Exact database/storage/auth incident behavior.
- Exact production incident behavior.

## 10. Known Incident Concepts Draft

Known incident response concepts:

- Detection.
- Classification.
- Severity / priority.
- Triage.
- Containment.
- Mitigation.
- Rollback / reversal.
- Hotfix.
- Production approval during incidents.
- Communication.
- Customer/user communication.
- Internal status updates.
- Evidence / audit / timeline.
- Root cause analysis.
- Postmortem.
- Security incident.
- Privacy / data incident.

These concepts are known concepts only and are not accepted canonical incident procedures until verified.

## 11. Non-Accepted Incident Response Areas

The following areas are not accepted yet:

- Exact incident response strategy.
- Exact incident severity model.
- Exact incident classification model.
- Exact incident owner/escalation model.
- Exact response time or SLA/SLO behavior.
- Exact triage process.
- Exact root cause analysis process.
- Exact rollback/reversal process.
- Exact hotfix process.
- Exact production approval process during incidents.
- Exact communication process.
- Exact customer/user communication process.
- Exact internal status update process.
- Exact monitoring/logging integration.
- Exact audit/evidence collection process.
- Exact postmortem process.
- Exact security incident process.
- Exact privacy incident process.
- Exact data incident process.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 12. Incident Detection Draft

Known facts:

- Monitoring/logging may support incident response.

Unknown / needs verification:

- Exact incident detection process.
- Exact alert rules.
- Exact monitoring/logging integration.
- Exact detection ownership.

No incident detection behavior is accepted in this draft.

## 13. Incident Classification Draft

Known facts:

- Exact incident classification model is not accepted yet.

Unknown / needs verification:

- Exact classification model.
- Exact classification criteria.
- Exact relationship between classification, severity, priority, product domains, and security/privacy/data incidents.

No incident classification behavior is accepted in this draft.

## 14. Severity / Priority Draft

Known facts:

- Exact incident severity model is not accepted yet.
- Exact response time or SLA/SLO behavior is not accepted yet.

Unknown / needs verification:

- Exact severity levels.
- Exact priority levels.
- Exact response times.
- Exact SLA/SLO behavior.
- Exact severity-to-escalation relationship.

No severity or priority behavior is accepted in this draft.

## 15. Triage Draft

Known facts:

- Exact triage process is not accepted yet.
- Frontend symptoms alone do not prove backend, RLS, storage, auth, data, security, or production root cause.

Unknown / needs verification:

- Exact triage process.
- Exact triage evidence requirements.
- Exact triage ownership.
- Exact cross-surface triage behavior.

No triage process is accepted in this draft.

## 16. Containment Draft

Known facts:

- Exact containment behavior is not accepted yet.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact containment process.
- Exact containment authority.
- Exact containment approval requirements.
- Exact containment relationship to production changes.

No containment behavior is accepted in this draft.

## 17. Mitigation Draft

Known facts:

- Exact mitigation behavior is not accepted yet.

Unknown / needs verification:

- Exact mitigation process.
- Exact mitigation approval requirements.
- Exact mitigation testing requirements.
- Exact mitigation communication requirements.

No mitigation behavior is accepted in this draft.

## 18. Rollback / Reversal Draft

Known facts:

- Exact rollback/reversal process is not accepted yet.

Unknown / needs verification:

- Exact rollback procedures.
- Exact reversal procedures.
- Exact rollback approval behavior.
- Exact rollback evidence and communication behavior.

No rollback or reversal process is accepted in this draft.

## 19. Hotfix Draft

Known facts:

- Exact hotfix process is not accepted yet.

Unknown / needs verification:

- Exact hotfix process.
- Exact hotfix approval behavior.
- Exact hotfix testing behavior.
- Exact hotfix release behavior.

No hotfix process is accepted in this draft.

## 20. Production Approval During Incidents Draft

Known facts:

- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact production approval process during incidents.
- Exact emergency approval behavior.
- Exact approval evidence requirements.
- Exact relationship between incident urgency and production change approval.

No production incident approval behavior is accepted in this draft.

## 21. Communication Draft

Known facts:

- Exact communication process is not accepted yet.

Unknown / needs verification:

- Exact communication process.
- Exact communication owners.
- Exact communication templates.
- Exact communication timing.

No communication process is accepted in this draft.

## 22. Customer / User Communication Draft

Known facts:

- Exact customer/user communication process is not accepted yet.

Unknown / needs verification:

- Exact customer/user communication process.
- Exact external communication criteria.
- Exact external communication approval behavior.
- Exact customer/user communication templates.

No customer/user communication process is accepted in this draft.

## 23. Internal Status Updates Draft

Known facts:

- Exact internal status update process is not accepted yet.

Unknown / needs verification:

- Exact internal status update process.
- Exact status update cadence.
- Exact audience.
- Exact status template.

No internal status update process is accepted in this draft.

## 24. Evidence / Audit / Timeline Draft

Known facts:

- Exact audit/evidence collection process is not accepted yet.
- Monitoring/logging may support incident response.

Unknown / needs verification:

- Exact evidence format.
- Exact audit evidence behavior.
- Exact timeline behavior.
- Exact storage, access, and retention behavior for incident evidence.

No evidence, audit, or timeline process is accepted in this draft.

## 25. Root Cause Analysis Draft

Known facts:

- Exact root cause analysis process is not accepted yet.
- Frontend symptoms alone do not prove backend, RLS, storage, auth, data, security, or production root cause.

Unknown / needs verification:

- Exact RCA process.
- Exact RCA format.
- Exact evidence required for RCA.
- Exact RCA ownership and review behavior.

No root cause analysis process is accepted in this draft.

## 26. Postmortem Draft

Known facts:

- Exact postmortem process is not accepted yet.

Unknown / needs verification:

- Exact postmortem process.
- Exact postmortem format.
- Exact postmortem ownership.
- Exact follow-up tracking behavior.

No postmortem process is accepted in this draft.

## 27. Security Incident Draft

Known facts:

- Ops/admin behavior is security-sensitive.
- Host identity transfer is security-sensitive.
- Public exposure, feed visibility, media visibility/upload, messaging, ticketing, reservation, wallet/ownership, staff scanner/check-in, storage/RLS/auth, and production changes can be security-sensitive.

Unknown / needs verification:

- Exact security incident process.
- Exact security incident classification.
- Exact security incident evidence, containment, communication, mitigation, and postmortem behavior.

No security incident process is accepted in this draft.

## 28. Privacy / Data Incident Draft

Known facts:

- Public exposure, feed visibility, media visibility/upload, messaging, ticketing, reservation, wallet/ownership, staff scanner/check-in, storage/RLS/auth, and production changes can be security-sensitive.

Unknown / needs verification:

- Exact privacy incident process.
- Exact data incident process.
- Exact privacy/data evidence, containment, communication, mitigation, and postmortem behavior.

No privacy or data incident process is accepted in this draft.

## 29. Product Domain Incident Draft

### Event lifecycle

Unknown / needs verification:

- Exact event lifecycle incident behavior.

### Viewer roles

Unknown / needs verification:

- Exact viewer role incident behavior.

### Personas and tiers

Unknown / needs verification:

- Exact persona/tier incident behavior.

### Ticketing

Unknown / needs verification:

- Exact ticketing incident behavior.

### Reservations

Unknown / needs verification:

- Exact reservation incident behavior.

### Wallet/ownership

Unknown / needs verification:

- Exact wallet/ownership incident behavior.

### Media/gallery

Unknown / needs verification:

- Exact media/gallery incident behavior.

### Feed/discovery

Unknown / needs verification:

- Exact feed/discovery incident behavior.

### Messaging

Unknown / needs verification:

- Whether messaging incident behavior applies.
- Exact messaging incident behavior.

### Notifications

Unknown / needs verification:

- Exact notification incident behavior.

### Staff scanner/check-in

Unknown / needs verification:

- Exact staff scanner/check-in incident behavior.

### Venue/business tools

Unknown / needs verification:

- Exact venue/business incident behavior.

### Host identity transfer

Known facts:

- Host identity transfer is security-sensitive.

Unknown / needs verification:

- Exact host identity transfer incident behavior.

### Ops/admin

Known facts:

- Ops/admin behavior is security-sensitive.

Unknown / needs verification:

- Exact ops/admin incident behavior.

### Public sharing

Unknown / needs verification:

- Exact public sharing incident behavior.

## 30. Cross-Surface Consistency Requirements

### Mobile

Unknown / needs verification:

- Exact mobile incident response consistency requirements.
- Exact relationship between mobile symptoms and dashboard, Web/Public, and Supabase backend evidence.

### Dashboard

Unknown / needs verification:

- Exact dashboard incident response consistency requirements.
- Exact relationship between dashboard symptoms and mobile, Web/Public, and Supabase backend evidence.

### Web/Public

Unknown / needs verification:

- Exact Web/Public incident response consistency requirements.
- Exact relationship between Web/Public symptoms and mobile, dashboard, and Supabase backend evidence.

### Supabase Backend

Unknown / needs verification:

- Exact Supabase backend incident response consistency requirements.
- Exact relationship between backend/database/storage/auth evidence and all frontend surfaces.

## 31. Security Risks

Security risks to verify:

- Frontend symptoms being treated as confirmed backend/security root cause.
- Incident response bypassing explicit production approval for SQL, migrations, functions, RLS, storage, or auth changes.
- Security-sensitive incidents lacking backend/audit/production evidence.
- Rollback, hotfix, or containment actions causing uncontrolled production changes.
- Communication or evidence processes exposing sensitive data.

No security risk in this section is a confirmed incident response defect unless later verified.

## 32. Privacy Risks

Privacy risks to verify:

- Incident evidence exposing private/protected/non-public data.
- Communications exposing sensitive user, profile, event, media, messaging, ticketing, reservation, wallet/ownership, venue/business, staff scanner/check-in, ops/admin, or public sharing data.
- Privacy/data incidents lacking accepted handling process.

No privacy risk in this section is a confirmed incident response defect unless later verified.

## 33. Determinism Risks

Determinism risks to verify:

- Incident classification differing across surfaces or responders.
- Frontend symptoms and backend evidence being interpreted inconsistently.
- Severity, priority, escalation, and response decisions differing without accepted rules.
- Rollback, mitigation, hotfix, and communication decisions diverging from accepted evidence.
- Incident timeline differing from actual event order.

## 34. Operational Risks

Operational risks to verify:

- Missing accepted incident response strategy.
- Missing accepted severity/classification model.
- Missing accepted owner/escalation model.
- Missing accepted response time or SLA/SLO behavior.
- Missing accepted communication process.
- Missing accepted rollback, hotfix, RCA, or postmortem process.
- Missing monitoring/logging integration.

## 35. Maintainability Risks

Maintainability risks to verify:

- Incident response behavior scattered across product, architecture, database, security, module, audit, monitoring/logging, build/release, testing, and status/backlog documents without clear ownership.
- IncidentResponse.md duplicating or contradicting BuildAndRelease.md, TestingStrategy.md, MonitoringAndLogging.md, security docs, audits, patch plans, ADRs, or status/backlog documents.
- Severity, priority, owner, escalation path, response time, rollback path, hotfix path, alert, evidence format, RCA format, postmortem format, or production response process treated as canonical before verification.

## 36. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has multiple implementation surfaces: mobile app, dashboard, Web/Public where applicable, and Supabase backend/storage/database/auth concepts.
- JoinFolk uses or may use React Native / Expo concepts for mobile.
- JoinFolk uses or may use Vite / React concepts for dashboard or web surfaces.
- JoinFolk uses or may use Supabase or Supabase-like backend concepts.
- JoinFolk uses or may use RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.
- JoinFolk has security-sensitive domains including event lifecycle, viewer roles, personas and tiers, ticketing, reservations, wallet/ownership, media/gallery, feed/discovery, messaging where applicable, notifications, staff scanner/check-in, venue/business tools, host identity transfer, ops/admin, and public sharing.
- Prior operations drafts define only draft boundaries for build and release, testing strategy, and monitoring and logging.
- Monitoring/logging may support incident response, but exact incident response integration is not accepted yet.
- Handbook workflow requires scoped diffs, git status checks, review before commit, no uncontrolled production changes, and no production SQL/migrations/functions/RLS/storage/auth changes without explicit approval.

Unknown / needs verification:

- Exact accepted incident response implementation across mobile, dashboard, Web/Public, Supabase backend, database, storage, auth, RLS, RPC, SECURITY DEFINER, product domains, monitoring/logging, testing, build/release, and production.

## 37. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact incident response strategy.
- Exact incident severity model.
- Exact incident classification model.
- Exact incident owner/escalation model.
- Exact response time or SLA/SLO behavior.
- Exact triage process.
- Exact root cause analysis process.
- Exact rollback/reversal process.
- Exact hotfix process.
- Exact production approval process during incidents.
- Exact communication process.
- Exact customer/user communication process.
- Exact internal status update process.
- Exact monitoring/logging integration.
- Exact audit/evidence collection process.
- Exact postmortem process.
- Exact security incident process.
- Exact privacy incident process.
- Exact data incident process.
- Exact cross-surface incident response consistency.
- Exact relationship to BuildAndRelease.md, TestingStrategy.md, and MonitoringAndLogging.md.

## 38. Acceptance Criteria for v1.0

Incident Response v1.0 can be accepted only after verification establishes:

- Accepted incident response vocabulary.
- Accepted incident response surfaces and ownership.
- Accepted detection process.
- Accepted classification model.
- Accepted severity/priority model.
- Accepted owner/escalation model.
- Accepted response time or SLA/SLO model, if applicable.
- Accepted triage process.
- Accepted containment process.
- Accepted mitigation process.
- Accepted rollback/reversal process.
- Accepted hotfix process.
- Accepted production approval behavior during incidents.
- Accepted communication process.
- Accepted customer/user communication process.
- Accepted internal status update process.
- Accepted evidence/audit/timeline process.
- Accepted root cause analysis process.
- Accepted postmortem process.
- Accepted security incident process.
- Accepted privacy/data incident process.
- Accepted monitoring/logging integration.
- Accepted cross-surface consistency requirements.
- Accepted relationship to product, architecture, database, security, module, audit, monitoring/logging, build/release, testing, ADR, and status/backlog documents.

Until these criteria are met, this document remains non-canonical.

## 39. Open Questions

- What incident response strategy is accepted?
- What incident classification model is accepted?
- What severity or priority model is accepted?
- What owner or escalation model is accepted?
- What response time or SLA/SLO behavior is accepted, if any?
- What triage process is accepted?
- What containment process is accepted?
- What mitigation process is accepted?
- What rollback or reversal process is accepted?
- What hotfix process is accepted?
- What production approval process during incidents is accepted?
- What communication process is accepted?
- What customer/user communication process is accepted?
- What internal status update process is accepted?
- What monitoring/logging integration is accepted?
- What evidence, audit, or timeline process is accepted?
- What root cause analysis process is accepted?
- What postmortem process is accepted?
- What security incident process is accepted?
- What privacy or data incident process is accepted?
- How should incident response preserve product, architecture, database, security, module, audit, monitoring/logging, build/release, testing, and cross-surface consistency?
- Which surfaces are part of accepted incident response today: mobile, dashboard, Web/Public, and Supabase backend?
