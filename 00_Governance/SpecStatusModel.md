# Spec Status Model

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines required metadata fields and how agents must interpret them.

## Status Field

Status must describe the document's lifecycle state.

Allowed status values:

- Draft
- Reviewed
- Accepted
- Superseded
- Deprecated

Draft means the document is not authoritative.

Reviewed means the document has been checked but is not yet authoritative.

Accepted means the document is authoritative for its stated scope.

Superseded means the document has been replaced by a newer Accepted document.

Deprecated means the document should not guide new work.

## Version Field

Version must use a simple numeric document version, such as `0.2`, `1.0`, or `1.1`.

Version `0.x` means bootstrap, discovery, draft, or pre-acceptance work.

Version `1.0` or higher may be used for Accepted specs after review.

Any material change to meaning requires a version bump.

Formatting-only changes may stay in the same version only if they do not change the rule, scope, or interpretation.

## Source Confidence Values

Source confidence must use one of these values:

- Draft
- User-stated
- Code-observed
- Backend-verified
- Production-verified
- Accepted

Draft means the content is preliminary and may be incomplete or wrong.

User-stated means the content comes from Mustafa / JoinFolk or another explicit product or engineering instruction, but has not necessarily been verified in code or production.

Code-observed means the content was observed in a repository, schema, configuration, or implementation. Code-observed content is evidence, not automatically authority.

Backend-verified means the content was checked against backend behavior, database rules, RPCs, policies, migrations, or service responses.

Production-verified means the content was checked against actual production behavior or production data paths using approved methods.

Accepted means the content has been approved as authoritative for its scope.

## Canonical Field

`canonical: true` means the document or section is intended to control platform behavior for its stated scope after acceptance.

`canonical: false` means the document or section is informational, provisional, historical, or not yet controlling.

A document must not be treated as canonical unless it is both Accepted and marked or understood as controlling for its scope.

## Agent Treatment Rules

Agents must treat Draft content as unverified working material.

Agents may use User-stated content as direction, but must still verify implementation before patching.

Agents may use Code-observed content as evidence, but must not assume it represents intended behavior.

Agents may use Backend-verified content as strong implementation evidence, but must still check whether it conflicts with Accepted specs.

Agents may use Production-verified content as strong operational evidence, but must not expose sensitive data or bypass approval rules.

Agents must treat Accepted content as authoritative unless it conflicts with a newer Accepted spec.

## Open questions

- Should `canonical: true/false` be required in every document header?
- What evidence format is required for Backend-verified claims?
- What evidence format is required for Production-verified claims?
