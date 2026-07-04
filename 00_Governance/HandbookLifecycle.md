# Handbook Lifecycle

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Draft

## Purpose

This document defines how handbook documents move from draft material to accepted platform authority.

## Document Lifecycle

Handbook documents move through this lifecycle:

1. Draft
2. Reviewed
3. Accepted
4. Superseded
5. Deprecated

## Status Meanings

Draft means the document is incomplete, unverified, or not yet approved. Draft content may guide discussion, but it is not authoritative.

Reviewed means the document has been checked by the relevant owner or reviewer, but it has not yet been accepted as canonical platform guidance.

Accepted means the document is approved as authoritative for the scope it covers. Agents and engineers must treat Accepted specs as binding unless a newer Accepted version supersedes them.

Superseded means a newer Accepted document or version has replaced this document. Superseded documents remain useful for history, but they do not control new work.

Deprecated means the document should no longer be used for new work. Deprecated documents may describe removed, abandoned, or intentionally retired behavior.

## Acceptance Criteria

A spec may become Accepted only when:

- Its scope is clear.
- Its owner is identified.
- Its status, version, review date, and source confidence are present.
- Its rules are operational enough to audit or implement.
- Its open questions are resolved, explicitly deferred, or moved to a tracked status document.
- Its relationship to related specs is clear.
- Any code-observed claims have supporting evidence.
- Mustafa / JoinFolk has approved it for the stated scope.

## Canonical Specs

A spec is canonical when it is marked Accepted and explicitly controls the platform behavior or engineering rule for its scope.

Only canonical specs are authoritative. Draft, Reviewed, Superseded, and Deprecated documents are not canonical unless a specific section says otherwise and has been accepted.

## Future Versions

Future versions must be created when a material rule, scope, contract, or platform behavior changes.

Minor wording corrections may be made inside the same version only when they do not change meaning.

Material changes require a version bump and review trail.

## Editing Accepted Specs

Accepted specs are not edited casually.

Changes to Accepted specs require versioned updates. The update must identify what changed, why it changed, what evidence supports the change, and whether related specs, audits, patch plans, or ADRs must also change.

## Open questions

- Who besides Mustafa / JoinFolk may mark a document Reviewed?
- What exact approval record is required before a document becomes Accepted?
- Should Accepted specs require a changelog section in every future version?
