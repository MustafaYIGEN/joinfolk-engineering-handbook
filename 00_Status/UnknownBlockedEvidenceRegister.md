# Unknown Blocked Evidence Register

## 1. Metadata

- Status: Draft
- Version: 0.3
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-14
- Source confidence: Production evidence status pack + operator-confirmed result summaries through `10g` + handbook synthesis
- canonical: false

## 2. Purpose

This register records the exact evidence gaps that still block final DB-to-surface decisions.

## 3. Evidence Gaps

| Gap | State | Blocking effect |
| --- | --- | --- |
| `01a_schemas.csv` canonical raw export missing | OPEN | exact live schema inventory cannot yet be revalidated from canonical raw evidence |
| `01b_relations.csv` canonical raw export missing | OPEN | exact live relation inventory remains partially indirect |
| `03c_sequence_ownership.csv` canonical raw export missing | OPEN | sequence ownership reachability is not yet canonically raw-backed |
| `05a_foreign_keys.csv` canonical raw export missing | OPEN | foreign-key dependency closure remains incomplete |
| `06a_buckets.csv` canonical raw export missing | OPEN | production bucket state is result-confirmed, but canonical raw bucket export remains missing |
| `07a_extensions.csv` result confirmed, canonical raw export missing | OPEN | extension confirmation is summarized but not raw-export-backed canonically |
| `07b_publications.csv` result confirmed, canonical raw export missing | OPEN | publication facts are confirmed but not raw-export-backed canonically |
| `07c_publication_tables.csv` result confirmed, canonical raw export missing | OPEN | publication membership facts are confirmed but not raw-export-backed canonically |
| `07d_cron_jobs.csv` result confirmed, canonical raw export status still open | OPEN | cron linkage is confirmed; remaining gap is evidence hygiene, not execution |
| `08a_availability.csv` result confirmed, canonical raw export missing | OPEN | runtime capability facts are usable, but not canonically raw-backed |
| `08b_database_reset.csv` result confirmed, canonical raw export missing | OPEN | stats reset facts are usable, but not canonically raw-backed |
| `08c_table_stats.csv` raw CSV exists in Downloads, canonical export missing | OPEN | table runtime evidence exists, but canonical evidence normalization is incomplete |
| `08d_function_stats.csv` result confirmed zero rows | RESULT_CONFIRMED_ZERO_ROWS | zero rows are expected because `track_functions = none`; this is not non-use proof |
| `08e_pg_stat_statements_note.csv` result confirmed, canonical raw export missing | OPEN | conditional collection authorization is confirmed, but not canonically raw-backed |
| `08f_pg_stat_statements_rpc_runtime.csv` raw CSV exists in Downloads, canonical export missing | OPEN | function-name runtime evidence exists, but canonical evidence normalization is incomplete |
| `09_versioned_rpc_families.csv` raw CSV exists in Downloads, canonical export missing | OPEN | exact family evidence exists, but canonical evidence normalization is incomplete |
| `10a_search_functions.csv` result confirmed, canonical raw export missing | OPEN | search priority contract is materially confirmed, but not canonically raw-backed |
| `10b_payment_order_functions.csv` result confirmed, canonical raw export missing | OPEN | payment-order priority contract is materially confirmed, but not canonically raw-backed |
| `10c_reservation_ticket_checkin_functions.csv` result confirmed, canonical raw export missing | OPEN | reservation and check-in priority contract is materially confirmed, but not canonically raw-backed |
| `10h` through `10j` priority contract exports | PENDING_EXECUTION | push exact contract closure remains incomplete |
| `11a_dm_p0_function_bodies.csv` canonical raw export | COMPLETE_VALIDATED | exact live DM production-body evidence is now canonical, eight signatures are present exactly once, and source/canonical hashes match |
| DM function-body authorization and caller-body parity | CLOSED | `11a` is `COMPLETE_VALIDATED`; all eight exact live DM bodies were reviewed, no active caller-body mismatch was confirmed, and no exact function remains `UNKNOWN_PRODUCTION_BODY_MISSING` |
| storage bucket raw export hygiene | OPEN | no storage mutation is authorized; remaining gap is canonical raw bucket evidence, not missing production-state evidence |
| realtime product decision | DECIDED: `POLLING_FIRST_V1` | no longer a decision gap; realtime remains deferred post-launch |

## 4. Binding Outcome

Any object whose decision depends on one of the gaps above must remain `UNKNOWN_BLOCKED` until the gap is closed or the owner explicitly accepts the uncertainty.

Completed execution must not be relabeled `PENDING_EXECUTION` merely because canonical raw export normalization is still open.
