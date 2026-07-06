# Patch Policy

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines patch categories, approval rules, verification expectations, and rollback requirements.

## Patch Categories

Documentation-only changes modify handbook or documentation content without changing application behavior.

Safe cleanup changes remove obvious dead code, unused documentation clutter, formatting problems, or non-behavioral duplication.

Low-risk refactors change internal structure while preserving behavior.

Business logic changes alter user-visible rules, product flows, ownership, eligibility, event behavior, commerce behavior, notifications, messaging, or platform decisions.

Backend/security changes affect backend behavior, permissions, authentication, authorization, RLS, RPCs, storage policies, secrets, jobs, APIs, or security boundaries.

Data migrations alter database schema, persistent data, migration history, storage layout, or production data shape.

## Approval Rules

Documentation-only changes require handbook-scope approval when they alter governance, accepted specs, or platform meaning. Draft placeholder edits may proceed within the approved documentation scope.

Safe cleanup changes require confirmation that behavior is not changing and that no do-not-touch area is affected.

Low-risk refactors require a patch plan, relevant spec references, build/test verification, and rollback notes.

Business logic changes require explicit approval, relevant specs, a patch plan, verification evidence, and review of platform consistency impact.

Backend/security changes require explicit approval before patching, security-aware review, verification evidence, and rollback planning.

Data migrations require explicit approval before patching, migration review, backup or recovery consideration, verification evidence, and rollback or forward-fix planning.

## Build And Test Requirement

Patches must run the relevant build and test checks for the affected repositories or systems.

If a build or test cannot be run, the patch record must say why and identify the remaining risk.

Passing tests do not replace required product, security, or manual verification.

## Manual Verification Requirement

Manual verification is required when a change affects user-visible behavior, permissions, data ownership, commerce, scanning, event lifecycle, or operational workflows.

Manual verification must state what was checked and what result was observed.

## Rollback Requirement

Every patch plan must include a rollback approach.

The rollback approach may be a direct revert, a forward fix, a migration rollback, a feature flag, or an operational mitigation.

For backend, security, and data migration patches, rollback planning must happen before patching.

## Absolute Rule

Never use "fix everything" as a patch scope.

Patch scopes must be specific, bounded, reviewable, and verifiable.

## Open questions

- Which build and test commands are canonical for each JoinFolk repository?
- What approval record is required for backend/security and data migration patches?
- Which manual verification checklist should be used for each high-risk area?
