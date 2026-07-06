# JoinFolk Engineering Handbook

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This handbook defines platform-level engineering specifications, governance rules, audit records, patch plans, and accepted technical decisions for JoinFolk.

The handbook exists to make engineering changes traceable. It separates what is known, what is proposed, what has been verified, and what has been accepted.

## Repository Relationship

This handbook is platform-level documentation.

It is separate from the JoinFolk dashboard, mobile, web, and Supabase repositories. Those repositories may provide evidence during audits, but they are not edited from this handbook unless a separate approved patch process explicitly allows it.

Application code is evidence. It is not automatically truth.

## Core Workflow

All non-trivial engineering work should follow this sequence:

1. Specification
2. Audit
3. Patch Plan
4. Patch
5. Verification
6. Accepted Change

Work must not skip from assumption directly to patch.

## Authority Model

Accepted specs are authoritative.

Draft specs are not authoritative. They are working material and must not be treated as final implementation rules.

Code is evidence, not automatically truth. Existing code may be incomplete, accidental, stale, or inconsistent with the intended platform model.

If code and an Accepted spec conflict, mark the issue as a platform inconsistency. Do not silently choose one source without recording the conflict.

If code and a Draft spec conflict, record the conflict as an open question until the intended behavior is reviewed.

## Agent Usage Rules

Agents must read relevant Accepted specs before auditing or patching.

Agents must not patch based only on assumptions.

Agents must not modify do-not-touch areas without explicit approval.

Agents must cite the specs, audit notes, patch plans, and verification evidence used to justify a change.

## Current Phase

Current phase: Handbook Bootstrap.

No product specs are accepted yet.

All product, architecture, database, security, operations, audit, patch plan, decision, and status documents should be treated as draft placeholders unless explicitly marked Accepted in a future version.

## Open questions

- Which documents should become the first Accepted platform specs?
- What review process will Mustafa / JoinFolk use to accept a spec?
- Which repositories will be audited first against this handbook?
