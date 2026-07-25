# JoinFolk Engineering Handbook Operating Model

## 1. Purpose & Constitution

The **JoinFolk Engineering Handbook** (`C:\dev\joinfolk-engineering-handbook`) serves as the single binding engineering constitution for the JoinFolk platform.

All architectural decisions, security boundaries, database containment gates, deployment runbooks, and launch blockers are governed by this handbook. No production change, migration apply, or security claim is valid without an explicit gate record in this handbook.

---

## 2. Directory Structure & Folder Roles

| Folder Path | Primary Role & Responsibilities |
| :--- | :--- |
| `00_Governance` | Constitutions, operating models, change control policies, repository topology, and do-not-touch rules. |
| `00_Status` | Master status index (`StatusIndex.md`), evidence registers (`EvidenceRegistry.md`), launch blocker registers, and production verification execution reports. |
| `01_Architecture` / `02_Architecture` | System topology, domain boundaries, multi-surface contracts (mobile, web, dashboard), and state sync patterns. |
| `03_Database` | Database schemas, RPC surface contracts, privilege grant models, and migration execution policies. |
| `04_Modules` | Domain engine specifications (e.g., Event Module Engine, Ticketing, Polls, Venues, Host Workstation). |
| `05_Security` | Security architecture, RLS policy models, role boundaries, and vulnerability mitigations. |
| `06_Operations` | Operational runbooks, release procedures, deployment access protocols, and verifier roles. |
| `07_Audits` | Production catalog audit results, signature reconciliation reports, and baseline comparison evidence. |
| `08_PatchPlans` | Step-by-step patch specifications, migration repair plans, and dry-run execution specifications. |
| `09_Decisions` | Binding architectural, security, and product decisions (`PriorityRpcDecisionRegister.md`). |
| `10_Status` | Historical status records retained for audit trail compliance. |

---

## 3. Strict Gate Lifecycle (9-Step Pipeline)

Every database or platform containment wave must strictly follow this 9-step lifecycle:

```text
1. AUDIT                     -> Catalog inspection & signature reconciliation
2. CLASSIFICATION            -> Risk grading & candidate target set selection
3. PATCH PLAN                -> Technical specification & static assertion definitions
4. STATIC REVIEW             -> Pre-execution file verification & hash assertion
5. DRY-RUN / VERIFICATION    -> In-transaction rollback dry-run execution
6. COMMIT / PUSH             -> Commit and push approved migration code to source repo
7. PRODUCTION APPLY          -> Single-target atomic apply (or normal migration flow if aligned)
8. POST-APPLY VERIFICATION   -> Production schema & privilege state verification
9. HANDBOOK CLOSEOUT         -> Document completion, update Status Index & Evidence Registry, push docs
```

---

## 4. Evidence Linking Rules

1. **Explicit Identity**: Every production claim must state exact migration versions, file basenames, git commit hashes, and file SHA256 hashes.
2. **Local Evidence Artifacts**: Executed evidence files under `C:\dev\joinfolk-evidence\` (e.g., `target_only_apply_final_evidence.json`) must be referenced by absolute path.
3. **No Unbacked Claims**: A gate may be marked `CLOSED / PASSED` only when backed by empirical production verification evidence.

---

## 5. Document Management: Update vs. Create

- **Create a New File**:
  - Production execution & verification reports (e.g., `00_Status/P0AnonRpcContainmentProductionExecutionAndVerificationReport.md`).
  - Major audit reports or workstream-specific patch plans.
- **Update Existing Files**:
  - `00_Status/StatusIndex.md` (Master index and current gate state).
  - `00_Status/EvidenceRegistry.md` (Central evidence registry table).
  - `00_Status/LaunchBlockerRegister.md` (Active launch blockers).
- **Superseded Documents**:
  - Mark superseded docs with a clear header block: `> [!IMPORTANT]\n> Status: SUPERSEDED by [New Document Path]`. Never delete historical audit trail files.

---

## 6. Explicitly Forbidden Status Claims

- **Never** claim "Production Safe" or "Fully Security Hardened" globally based on a single workstream closure.
- **Never** claim "Launch Ready" while any `BLOCKER` remains open in `LaunchBlockerRegister.md`.
- **Never** claim production applied based only on local dry-run evidence.
