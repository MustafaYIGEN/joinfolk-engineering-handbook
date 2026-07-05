# Storage Model

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level storage and media specification draft for JoinFolk.

It records known storage/media concepts, authority boundaries, verification requirements, and unresolved storage questions.

This document is not canonical. No bucket, path, policy, upload rule, visibility rule, file rule, database relationship, RPC relationship, RLS relationship, auth relationship, delete behavior, or moderation behavior is accepted by this document alone.

## Storage Model Definition

JoinFolk uses Supabase storage or storage-related behavior.

Supabase provides database, RPC, storage, and auth responsibilities.

Storage/media behavior is security-sensitive where protected media or protected files are involved.

Exact storage model is not accepted yet.

## Storage Authority Model

### What Storage Policies May Enforce

Storage policies may enforce access to buckets, paths, files, uploads, reads, deletes, and protected media where applicable.

Exact storage policies are Unknown / Needs verification.

### What Backend/RPC/RLS May Enforce

Backend, RPC, RLS, and auth may enforce security-sensitive storage/media behavior where applicable.

Backend authority may depend on database records, viewer roles, event lifecycle, personas and tiers, ticketing/check-in, reservations, venue/business tools, staff scanner, ops/admin, host identity transfer, and public sharing.

Exact backend/RPC/RLS/auth behavior is Unknown / Needs verification.

### What Frontend Must Not Enforce Alone

Frontend upload/display logic is UX only where security-sensitive.

Frontend must not enforce protected media access, upload authority, delete authority, moderation authority, public/private visibility, file rules, or ownership-sensitive storage behavior alone.

## Known Storage / Media Concepts Draft

Prior project context mentioned storage buckets or concepts such as:

- `posters`
- `venue-posters`
- `venue-media`
- event videos

These names are known concepts only.

They must not be treated as accepted canonical storage model until verified.

## Non-Accepted Storage Areas

The following are not accepted:

- exact storage buckets
- exact storage policies
- exact media upload pipeline
- exact public/private media visibility rules
- exact file size/type rules
- exact storage relationship to database records
- exact storage relationship to RLS/RPC/auth
- exact media/gallery permissions
- exact deletion behavior
- exact moderation behavior
- exact public sharing media behavior

No storage behavior is canonical in this draft.

## Storage Verification Requirements

### Buckets

Verify exact storage buckets before acceptance.

No bucket name is canonical in this draft.

### Paths

Verify exact path structure and path authority before acceptance.

### File types

Verify allowed file types before acceptance.

Exact file type rules are Unknown / Needs verification.

### File size limits

Verify file size limits before acceptance.

Exact file size rules are Unknown / Needs verification.

### Read access

Verify read access rules for public and protected media before acceptance.

### Write/upload access

Verify upload authority and write rules before acceptance.

### Delete access

Verify delete authority before acceptance.

### Moderation access

Verify moderation authority before acceptance.

### Public/private visibility

Verify public/private media visibility rules before acceptance.

### Database record relationship

Verify how storage files relate to database records before acceptance.

### RPC/RLS/auth relationship

Verify how storage behavior relates to RPC, RLS, and auth before acceptance.

## Relationship to Product Domains

### Media/gallery

Storage/media behavior may support media/gallery.

Exact media/gallery storage behavior is Unknown / Needs verification.

### Event lifecycle

Storage/media behavior may interact with event lifecycle.

Exact lifecycle-related storage behavior is Unknown / Needs verification.

### Viewer roles

Storage/media behavior may interact with viewer roles.

Exact viewer-role-related storage behavior is Unknown / Needs verification.

### Personas and tiers

Storage/media behavior may interact with personas and tiers.

Exact persona/tier-related storage behavior is Unknown / Needs verification.

### Ticketing/check-in

Storage/media behavior may interact with ticketing and check-in.

Exact ticketing/check-in-related storage behavior is Unknown / Needs verification.

### Reservations

Storage/media behavior may interact with reservations.

Exact reservation-related storage behavior is Unknown / Needs verification.

### Venue/business tools

Storage/media behavior may interact with venue/business tools.

Exact venue/business storage behavior is Unknown / Needs verification.

### Staff scanner

Storage/media behavior may interact with staff scanner.

Exact scanner-related storage behavior is Unknown / Needs verification.

### Ops/admin

Storage/media behavior may interact with ops/admin.

Exact ops/admin storage behavior is Unknown / Needs verification.

### Host identity transfer

Storage/media behavior may interact with host identity transfer.

Exact host identity transfer storage behavior is Unknown / Needs verification.

### Public sharing

Storage/media behavior may interact with public sharing.

Exact public sharing media behavior is Unknown / Needs verification.

## Public Media / Protected Media Draft

Public/private media visibility rules are not accepted.

Protected media access is security-sensitive.

Public media behavior must not be assumed from frontend display logic alone.

Exact public media and protected media rules are Unknown / Needs verification.

## Upload / Delete / Moderation Draft

Exact media upload pipeline is not accepted.

Exact deletion behavior is not accepted.

Exact moderation behavior is not accepted.

Upload, delete, and moderation authority must be backend-enforced where security-sensitive.

Exact upload, delete, and moderation behavior is Unknown / Needs verification.

## Storage / Database Relationship Draft

Exact storage relationship to database records is not accepted.

Storage files may require database records or metadata where applicable, but exact relationships are Unknown / Needs verification.

Storage and database relationships must be verified before media/gallery, venue/business, public sharing, or protected media behavior is accepted.

## Security Risks

Unverified storage policies may expose protected media or files.

Frontend-only upload/display logic may allow unauthorized access or mutation.

Unverified public/private visibility rules may expose private event, venue, user, or media data.

Unverified database relationships may leave orphaned files, incorrect ownership, or inconsistent visibility.

Unverified delete or moderation behavior may remove or expose files incorrectly.

Unverified RPC/RLS/auth relationships may create gaps between database permissions and storage access.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may interpret storage/media rules differently.

Frontend display may diverge from storage policy behavior.

Database records may diverge from stored files.

Public sharing media behavior may diverge from public/private visibility rules.

Upload, delete, and moderation behavior may differ across surfaces.

## Current Known Implementation

Known implementation facts:

- JoinFolk uses Supabase storage or storage-related behavior.
- Supabase provides database, RPC, storage, and auth responsibilities.
- Storage/media behavior is security-sensitive where protected media or protected files are involved.
- Prior project context mentioned `posters`, `venue-posters`, `venue-media`, and event videos.
- Current dashboard audit did not complete backend/RLS/storage verification.

These facts are not accepted as canonical storage model rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact storage buckets.
- Exact storage policies.
- Exact media upload pipeline.
- Exact public/private media visibility rules.
- Exact file size/type rules.
- Exact storage relationship to database records.
- Exact storage relationship to RLS/RPC/auth.
- Exact media/gallery permissions.
- Exact deletion behavior.
- Exact moderation behavior.
- Exact public sharing media behavior.
- Cross-surface storage/media consistency across Mobile, Dashboard, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Storage buckets are verified or explicitly deferred.
- Storage paths are documented or explicitly deferred.
- File type rules are documented or explicitly deferred.
- File size limits are documented or explicitly deferred.
- Read access rules are documented or explicitly deferred.
- Upload/write access rules are documented or explicitly deferred.
- Delete access rules are documented or explicitly deferred.
- Moderation access rules are documented or explicitly deferred.
- Public/private visibility rules are documented or explicitly deferred.
- Database record relationships are documented or explicitly deferred.
- RPC/RLS/auth relationships are documented or explicitly deferred.
- Known storage concepts are verified, rejected, or explicitly deferred.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- Which storage buckets exist?
- What paths and path rules are accepted?
- What file types and file sizes are allowed?
- What read, upload, delete, and moderation rules exist?
- What media is public, and what media is protected?
- How do storage files relate to database records?
- How do storage policies interact with RPC, RLS, and auth?
