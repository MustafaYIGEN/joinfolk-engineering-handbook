# Change Control

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the required control process for changing specs, code, database rules, infrastructure, or operational behavior.

## No Direct Random Changes

Direct random changes are not allowed.

Changes must have a stated reason, a defined scope, and a record of what evidence supports them.

Agents and engineers must not make opportunistic changes outside the approved scope.

## Change Proposal Requirement

Before material work begins, there must be a change proposal or equivalent written intent.

The proposal must state:

- The problem or inconsistency.
- The affected area.
- The expected outcome.
- The relevant specs or known lack of specs.
- The risk level.
- Whether any do-not-touch area may be affected.

## Patch Plan Requirement

Material changes require a patch plan before implementation.

The patch plan must define:

- Files, modules, schemas, policies, or systems expected to change.
- The category of patch.
- The required approvals.
- The tests, builds, audits, or manual checks to run.
- The rollback approach.
- Any open questions that must be resolved before patching.

## Required Spec References

Every material patch must reference relevant specs.

If no Accepted spec exists, the patch plan must say so explicitly.

If a Draft spec is used, the patch must not claim that the Draft spec is authoritative.

If code conflicts with an Accepted spec, the conflict must be recorded as a platform inconsistency before or during the patch plan.

## Verification Evidence

Accepted changes require verification evidence.

Verification may include builds, automated tests, database checks, policy checks, manual QA, screenshots, logs, or production-safe operational checks.

The required evidence depends on the patch category and risk level.

Unverified changes must not be recorded as Accepted.

## Recording Accepted Changes

Accepted changes must be recorded in the relevant spec, audit, patch plan, ADR, or status document.

The record must include:

- What changed.
- Why it changed.
- Which specs were used.
- What verification was completed.
- What risks or follow-up items remain.

## ADR Usage

Architecture Decision Records are required for major architecture decisions.

Use an ADR when a change affects platform boundaries, data ownership, security authority, deployment strategy, storage model, major integrations, or long-term technical direction.

ADRs must reference related specs and must not replace detailed implementation verification.

## Open questions

- What exact template should change proposals use?
- What exact template should patch plans use?
- Who may approve each change category?
