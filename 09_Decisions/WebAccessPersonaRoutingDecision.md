# Web Access Persona Routing Decision

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Source confidence: product-owner decision with current Cloudflare/deployment evidence
- Decision type: Web access IA / persona routing / account identity
- Applies to: marketing web, public web app access, personal persona, host persona, dashboard access, login routing
- Last evidence date: 2026-07-08

## 2. Decision

JoinFolk uses one authentication identity per user account.

An email address represents the account identity, not a single persona.

A single authenticated account may access both personal persona and host persona surfaces when the account has the required tier, role, or host entitlement.

Host users must not be forced to maintain a separate email account for dashboard access.

Personal persona and host persona must be separated by authorization, routing, and active persona context, not by duplicate accounts.

## 3. Surface Model

| Surface | Primary purpose | Access model |
| --- | --- | --- |
| Marketing web | Public product/brand entry | Guest/public |
| Public event/provider web | Discover events, providers, tickets, reservations | Guest/authenticated |
| Personal app/profile | Personal identity, tickets, reservations, participation | Authenticated account |
| Host dashboard | Event/business/provider management | Authorized host persona only |
| Staff tools | Scanner/manager workflows | Assigned staff role only |

## 4. Domain Direction

| Domain | Intended role |
| --- | --- |
| `join-folk.com` | Marketing/root public website |
| `www.join-folk.com` | Marketing/root public website |
| `app.join-folk.com` | Application surface for public app, personal persona, host dashboard, and authenticated flows |

The production Cloudflare project ownership for `app.join-folk.com` is governed by `09_Decisions/CloudflareProductionSurfaceRoutingDecision.md`.

## 5. Persona Routing Rule

After login, routing must be determined by user intent, available persona entitlements, and the requested route.

| User intent / route | Required behavior |
| --- | --- |
| Guest opens public event/provider page | Show public page with login/register/buy/reserve CTA where allowed |
| Normal user logs in from user flow | Land in personal persona area |
| Host logs in from host/dashboard flow | Land in host persona dashboard if authorized |
| Host logs in from user flow | Land in personal persona area unless host context was explicitly requested |
| User without host entitlement opens dashboard | Block dashboard access and route to personal/user area or upgrade/request flow |
| Staff opens staff tool | Require assigned staff role |
| Unknown/ambiguous login intent | Use safe default: personal persona area |

## 6. Account and Persona Law

- Email belongs to the account identity.
- Personal persona belongs to the account.
- Host persona belongs to the account when the user has host entitlement.
- Dashboard access is not granted by email alone.
- Dashboard access requires host persona entitlement, tier eligibility, or assigned role.
- Normal users must not see or access business dashboard management surfaces.
- Host users may still use JoinFolk as normal participants from the same account.
- Persona switching must preserve authorization boundaries.
- Persona switching must not mutate ownership, tickets, reservations, or staff assignments.

## 7. Product Direction

Marketing web should branch into two clear user journeys:

| Journey | Target |
| --- | --- |
| Explore / Join events | Public/user app flow |
| Host / Manage events | Host dashboard flow |

The branch must be product-level IA, not a separate-account requirement.

## 8. Blocked Decisions

The following decisions are explicitly blocked:

| Blocked decision | Reason |
| --- | --- |
| Requiring separate emails for personal and host persona | Breaks account identity and creates support/product friction |
| Giving dashboard access to all authenticated users | Exposes business management surfaces to normal users |
| Treating host persona as a separate user account | Conflicts with persona-based model |
| Routing every host login to dashboard by default | Host users also need personal participant flows |
| Routing every login to personal area without preserving dashboard intent | Breaks host management workflows |
| Using Cloudflare project separation as the security boundary | Security must be enforced by app authorization, DB/RLS/RPC, and persona context |

## 9. Required Future Gates

| Gate | Required before | Status |
| --- | --- | --- |
| Web Access IA inventory | Any web access routing patch | Required |
| Persona login routing design | Any login routing patch | Required |
| Dashboard authorization matrix | Any dashboard access patch | Required |
| Public event/provider route decision | Any SEO/public web patch | Required |
| Persona switch UX decision | Any persona switch implementation | Required |
| DB/RLS/RPC authorization review | Any backend authorization mutation | Required |

## 10. No-Modification Confirmation

- No application code was modified by this handbook task.
- No dashboard/mobile/web code was modified by this handbook task.
- No Supabase tree was modified by this handbook task.
- No SQL was executed by this handbook task.
- No production mutation was executed by this handbook task.
- No Cloudflare setting was changed by this handbook task.
- No Supabase CLI was run by this handbook task.
- No files were staged or committed by this handbook task.
- Only `09_Decisions/WebAccessPersonaRoutingDecision.md` was created.
