# Security Definer Function Grant Phase 1 Smoke Check Report

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Source confidence: operator-provided manual smoke check result
- Production Phase 1 status: Executed and verified
- Smoke check status: Passed with uncovered negative web-access checks

## 2. Passed Smoke Checks

| Check | Result |
| --- | --- |
| Publish plain event | PASS |
| Publish with share group | PASS |
| Open check-in | PASS |
| End event | PASS |
| Cancel event | PASS |
| List reminders | PASS |
| Upsert reminder | PASS |
| Delete reminder | PASS |
| Errors/logs | None reported |

## 3. Not Covered

| Check | Result | Reason |
| --- | --- | --- |
| Non-host negative check | NOT TESTED | Web/profile/dashboard access path is currently unclear |
| Anon negative check | NOT TESTED | Public web/persona access path is currently unclear |

## 4. Interpretation

- Phase 1 search_path hardening did not break the tested publish, host control, or reminder flows.
- Negative access behavior was not fully tested through web because public/profile/dashboard entry routing remains an open product gap.
- This does not claim broad grant hardening.
- This does not claim full platform security hardening.
- This does not claim launch-ready.

## 5. New Product Gap Identified

Product gap:
Public web, profile access, host dashboard access, and persona-based login routing need a dedicated Web Access IA pass.

Required direction:

- Public event/provider pages should be reachable from web.
- Normal users should land in personal profile/persona area.
- Host/business users should land in dashboard/host persona area.
- Non-app users should be able to discover providers/events and start ticket/reservation flows from web.
- Business dashboard access must not be exposed to normal users.

## 6. Required Next Gates

| Next gate | Required before | Status |
| --- | --- | --- |
| Web Access IA inventory | Any web access routing patch | Required |
| Persona login routing decision | Any login routing patch | Required |
| Public event/provider route decision | Any SEO/public web patch | Required |
| Phase 2 grant hardening matrix | Any grant/revoke production change | Required |

## 7. No-Modification Confirmation

- No application code was modified by this handbook task.
- No dashboard/mobile/web code was modified by this handbook task.
- No Supabase tree was modified by this handbook task.
- No SQL was executed by this handbook task.
- No production mutation was executed by this handbook task.
- No Supabase CLI was run by this handbook task.
- No private rows were inspected.
- No storage objects were listed.
- No files were staged or committed by this handbook task.
- Only `00_Status/SecurityDefinerFunctionGrantPhase1SmokeCheckReport.md` was created/modified.
