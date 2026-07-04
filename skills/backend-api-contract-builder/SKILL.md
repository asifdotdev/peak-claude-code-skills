---
name: backend-api-contract-builder
description: "Build or extend backend API modules from API contracts, product rules, permissions, and existing codebase conventions. Use when implementing endpoints, request/response validation, data models, services, integrations, tests, and backend quality gates without assuming a specific framework or project template."
---

# Backend API Contract Builder

Use this skill for contract-driven backend work. The API contract and product rules define behavior; the existing codebase defines implementation style.

## Source Priority

1. API contract or user-provided endpoint specification.
2. Product rules, permissions, and edge cases.
3. Existing backend architecture and local conventions.
4. Existing frontend code only to verify user-facing fields, enum values, and payload names.

Do not impose a framework, folder layout, ORM, auth package, documentation generator, or state model. Discover what the repo already uses and match it.

## Preflight

- Identify the backend stack, package manager, test runner, validation approach, data layer, auth approach, error format, response format, and environment config pattern.
- Read nearby modules that already solve similar problems.
- Identify how the repo separates routing/handlers, business logic, validation, data access, background jobs, and integrations.
- Confirm the intended scope: new endpoint, new domain/module, extension of existing behavior, or bug fix.
- Keep changes inside the requested backend surface unless the user asks for frontend or infrastructure changes too.

## Implementation Plan

Before writing code, record a short plan:

- endpoints or operations to implement.
- request fields, response fields, params, query filters, auth requirements, and validation rules.
- data model changes, migrations, indexes, constraints, and relationships.
- business rules, permissions, state transitions, and error cases.
- integrations, credentials, retries, timeouts, and failure behavior.
- fields or concepts that are frontend-only and must not become persisted backend data.
- tests and verification commands.

If the contract is incomplete, make conservative assumptions only for low-risk behavior. Ask before deciding ownership, money movement, irreversible state transitions, or external side effects.

## Frontend-Only Leak Prevention

Never create persisted fields, request fields, endpoints, or response fields for frontend-only concepts:

- `confirmPassword`
- local validation state
- theme/UI state unless server persistence is explicitly required
- navigation, animation, tab, or scroll state
- computed display values
- secrets, password hashes, internal tokens, or provider-only metadata in public responses

For user-facing fields, verify actual names against existing frontend code when available. Match the frontend payload names unless the API contract explicitly defines a different mapping.

## Implementation Rules

- Follow the repo's naming conventions over generic preferences.
- Use the existing validation approach for every inbound field.
- Keep transport handlers thin when the codebase separates handlers from business logic.
- Route all database access through the repo's established abstraction, if one exists.
- Reuse existing auth, email, notification, upload, payment, queue, logging, and config utilities.
- Do not install duplicate packages for capabilities the repo already has.
- Do not add extra endpoints or fields just because they are common in other apps.
- Make external integrations real when the contract requires them; no fake SDK calls or hardcoded success.
- Handle external failures intentionally: critical side effects should fail visibly, non-critical side effects can log and continue when product behavior allows it.

## No-Stub Rule

The following are failures:

- TODO/FIXME/HACK comments in implemented paths.
- empty handlers or service methods.
- `throw new Error("Not implemented")`.
- placeholder console logs.
- hardcoded success responses that skip required work.
- imported SDKs that are never called.
- fake persistence, fake queues, fake emails, fake payments, or fake uploads.

If something cannot be implemented safely, stop with a blocker naming the missing decision, credential, service, or contract detail.

## Business Logic Checklist

For each operation, handle applicable cases:

- authenticated identity and role.
- ownership and tenant isolation.
- entity existence before read/update/delete.
- duplicate/conflict checks before create.
- referenced entity validation before storing relationships.
- state transition validation for status/phase fields.
- limits, quotas, rate/frequency rules, and boundary values.
- soft-delete, hard-delete, cascade, detach, or archive behavior.
- idempotency for creates with side effects.
- clear error type/status and actionable message.
- logs/audit events for significant business actions.
- retry/timeout/rollback behavior for integrations when needed.

## Error Semantics

Use the repo's existing error classes and response envelope. When choosing HTTP semantics:

- `400`: malformed input, validation failure, invalid OTP/code, wrong password.
- `401`: missing, expired, or invalid authentication.
- `403`: authenticated but not allowed.
- `404`: requested resource does not exist or is hidden by access policy.
- `409`: duplicate, conflict, or incompatible current state.

Do not use auth errors for normal validation failures.

## Tests

Add tests at the level the repo uses: unit, integration, request, resolver, service, or end-to-end.

Cover:

- happy path.
- validation failure.
- unauthenticated and unauthorized access.
- not found.
- duplicate/conflict.
- invalid state transition.
- ownership or tenant boundary.
- business rule enforcement.
- delete/cascade behavior.
- integration failure behavior when applicable.

Do not mock away the rule being tested.

## Verification

Run the strongest existing backend gates:

- formatter or linter.
- typecheck or compile.
- unit/integration tests.
- migration checks if data model changed.
- app/server boot or request smoke test when practical.

Then inspect the diff:

- every requested operation is implemented.
- no unrelated endpoints, fields, packages, or refactors were added.
- no frontend-only fields leaked.
- data access and validation match local patterns.
- security and ownership checks are present.
- generated files or migrations are included only when expected.

Do not mark complete until mechanical gates and behavior checks pass.
