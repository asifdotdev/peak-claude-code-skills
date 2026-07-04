---
name: product-system-design
description: "Generate product analysis and backend-ready system design from product requirements, existing frontend code, screens, data models, state, services, and navigation. Use when deriving API contracts, data models, business rules, permissions, module boundaries, and integration requirements."
---

# Product System Design

Use this skill to produce backend-ready system design from product requirements and an existing frontend. The output should let a backend engineer implement without guessing.

## Output Scope

Create or update only documentation artifacts unless the user requests more:

- product analysis: human-readable product and architecture analysis.
- backend contract: precise implementation contract for APIs, data, rules, and integrations.

Do not modify app code while running this skill.

## Required Reading

Read before writing:

- product requirements.
- available prototype or navigation map.
- relevant screen files.
- existing type definitions.
- existing state management.
- reusable components.
- existing network/API layer.
- existing service layer.
- existing backend docs if present

Do not skip sections because the requirements are thin. Use the built frontend to verify what was actually implemented.

## Analysis Pass

Extract:

- app purpose and target users.
- personas, roles, and permissions.
- screen inventory and auth/public access.
- form fields users actually input.
- data each screen displays, creates, updates, or deletes.
- navigation journeys from prototype, route code, and screen interactions.
- frontend types, stores, and service patterns.
- global/admin-managed data vs user-owned data.
- notifications, offline behavior, analytics, subscriptions, files, maps, payments, email, SMS, push, and other integrations.
- frontend-only concerns.

State assumptions explicitly when the requirements or frontend are ambiguous.

## Frontend-Verified Schema Rules

- User-facing schema fields must exist in frontend code.
- Backend-only infrastructure fields may be derived from standard backend needs: password hashes, tokens, timestamps, soft-delete flags, counters, OAuth IDs, integration IDs, admin flags, status machine fields, device tokens.
- If frontend data is nested, mirror that structure with embedded sub-schemas where appropriate.
- Do not flatten nested frontend objects just because it is simpler.
- Admin-managed selectable lists should be modeled as managed reference data with CRUD and stable relationships.
- User-entered freeform tags may remain simple values if the product does not manage them centrally.
- Third-party integrations should have clear ownership boundaries rather than provider SDK fields scattered across unrelated business entities.
- Match frontend field names exactly for user-facing fields.
- Do not split fields such as `name` into `firstName` and `lastName` unless the frontend does.

## Frontend-Only List

Explicitly list features that must not become backend fields or endpoints:

- `confirmPassword`
- local validation state
- theme toggles unless server persistence is required
- animation and gesture state
- navigation, tab, and scroll state
- client-only computed display values
- component visibility flags

## Product Analysis Shape

Include:

- one-liner.
- goals and non-goals.
- personas and roles.
- information architecture.
- ownership model.
- user journeys.
- features by module.
- notifications.
- offline and sync.
- analytics/reporting.
- subscription/entitlement model.
- MVP acceptance criteria.

Every section should contain concrete content or an explicit assumption.

## Backend Contract Shape

Include:

- one-liner.
- business rules, written testably.
- permissions and security table.
- API or operation contract.
- naming conventions.
- frontend-only section.
- data storage assumptions.
- boilerplate module disposition if using a backend template.
- operations by module/domain.
- data models by entity.
- required integrations.
- multi-step flow design.
- notifications/webhooks/background jobs.
- module implementation order.

## Business Rule Depth

For every module, think through:

- ownership and authorization.
- state machines and valid transitions.
- cascade and referential integrity.
- uniqueness and conflict rules.
- boundaries and limits.
- temporal rules and expiry.
- idempotency.
- soft-delete vs hard-delete.
- edge cases with missing, deleted, duplicated, null, empty, zero, negative, or extreme values.

Write rules so a developer can turn each one into an if-statement and a test.

## API Contract Rules

HTTP endpoint tables should include method, path, auth, request body/query, response shape, and notes. For non-HTTP APIs, use the equivalent operation name, inputs, outputs, auth, and notes.

Data model tables should include field, type, required/optional, default, uniqueness/constraints, relationship/reference, source, and notes.

- Follow the naming conventions already used by the target stack. If none exist, choose one consistent convention and document it.
- Secret/internal fields never appear in public request or response contracts.
- Use `400`, `401`, `403`, `404`, and `409` consistently with backend semantics.
- Integration tables must name exact packages and env vars.

If a section cannot be determined safely, write a clear `ASSUMPTION:` instead of a placeholder.
