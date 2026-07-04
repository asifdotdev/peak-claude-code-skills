---
name: api-integration-wiring
description: "Wire a completed backend API into an existing frontend app without assuming a framework or template. Use when discovering API contracts from docs, server code, generated clients, tests, or user-provided specs, then adding typed API calls, state/cache integration, auth handling, and UI data wiring."
---

# API Integration Wiring

Use this skill to connect an existing backend API to an existing frontend while preserving the app's UI and architecture.

## Scope Rules

- Treat backend code as read-only unless the user explicitly asks for backend changes.
- Preserve visual layout, navigation, accessibility, test IDs, and component structure unless data requirements force a focused change.
- Follow the frontend's existing network, state, caching, error, and environment patterns.
- Work by feature/domain when the API is large.
- In TypeScript projects, keep request and response types explicit; do not introduce `any` to avoid understanding the contract.

## Contract Discovery

Use the best available source of truth. Do not assume a specific documentation tool.

Possible sources:

- user-provided API contract.
- official API docs.
- machine-readable API schema if present.
- generated client/types if present.
- backend route, handler, resolver, or equivalent server code.
- request validators, serializers, schemas, entities, or resource transformers.
- API tests.
- existing frontend calls.
- runtime network traces when the app already talks to the API.

For each endpoint or operation, identify:

- method or operation name.
- path or function target.
- auth requirements.
- path params, query params, body fields, headers, and upload format.
- response shape and pagination shape.
- error envelope and status/error codes.
- special behavior such as refresh tokens, idempotency keys, retries, or rate limits.

If the contract is unknowable from available sources, ask for the missing API details instead of guessing.

## Frontend Audit

Read existing frontend patterns before adding code:

- package manager and build/test commands.
- HTTP or RPC client setup.
- env/config conventions.
- type organization.
- state management or server-state cache.
- auth/session storage.
- error and toast/snackbar patterns.
- loading and empty-state patterns.
- form handling.
- routing and route params.
- existing feature modules that already call APIs.

Match local style. Do not create a parallel network layer if the app already has one.

## Shared API Layer

Only add shared infrastructure when it fits the repo:

- base URL or client config through existing env conventions.
- request helper or API client extension.
- typed success/error envelopes.
- pagination helpers.
- auth header injection.
- refresh-token or session-expiry handling.
- upload helpers.

Preserve existing config keys and environment files. Do not rename app-wide network primitives casually.

## Per-Feature Wiring

For each feature/domain:

1. Determine which screens/components actually need the API.
2. Define request and response types from the contract.
3. Add endpoint/client functions following the existing network pattern.
4. Add state/cache/store logic only when the app pattern requires it.
5. Add hooks/composables/helpers only when they remove real duplication or match local conventions.
6. Replace mocks/static data with real calls.
7. Wire forms, detail pages, lists, filters, pagination, uploads, and refresh behavior.

Do not add state slices, hooks, or abstractions by default. Let the existing frontend architecture decide.

## UI Wiring Rules

For every affected screen or component:

- Fetch at the right lifecycle point.
- Show the app's standard loading state.
- Show the app's standard error state.
- Show empty states for empty responses.
- Keep user-entered form data safe during failed submissions.
- Disable duplicate submissions where needed.
- Map server validation errors to the right form fields when possible.
- Preserve route params and navigation behavior.
- Preserve accessibility labels and test selectors.
- Avoid layout/style churn unrelated to API wiring.

## Auth and Sessions

When wiring auth:

- Match the backend's real token/session contract.
- Store credentials only in the app's established secure storage location.
- Handle login, logout, registration, profile fetch, refresh, and expired sessions consistently.
- Clear sensitive state on logout or token invalidation.
- Avoid leaking tokens in logs, URLs, analytics, or error messages.

## Verification

Before completion:

- Typecheck or compile if the project supports it.
- Run relevant tests.
- Run lint/format checks if the repo expects them.
- Exercise at least one happy path and one failure path for each wired feature when practical.
- Confirm mocks/static data were removed only for the integrated feature.
- Confirm backend files were not modified.
- Confirm all request/response fields match the discovered contract.
- Confirm loading, error, empty, and auth-expired states behave correctly.
