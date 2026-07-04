---
name: supabase-app-backend
description: "Build Supabase backends for apps, including Postgres schema, row-level security policies, SQL functions, Edge Functions, generated client types, and frontend stitching. Use for Supabase delivery from product requirements, module lists, and existing app code."
---

# Supabase App Backend

Use this skill to build a Supabase backend and connect it to an existing app. Keep database security, type generation, and local app conventions at the center of the workflow.

## Scope Rules

- During backend implementation, do not modify app files unless the user asks for frontend stitching now.
- During stitching, modify app files only for Supabase integration.
- Work one domain/module at a time when the user provides a module list.
- Commit or report progress according to the repo's existing workflow.
- Stop for unsafe decisions involving money movement, irreversible state changes, unclear ownership, production credentials, or project creation costs.

## Required Inputs

Read:

- product requirements or implementation brief.
- module/domain list if provided.
- existing app screens, types/models, state/cache, and service stubs.
- current Supabase directory, migrations, functions, and generated types.
- existing config/env conventions.

Use requirements for business intent, but verify field and user-flow details against the built app.

## Project Setup

- Verify an existing Supabase project before using it.
- Create a project only when the user explicitly allows it.
- If creation requires cost confirmation, follow the provider confirmation flow.
- Store project URL, anon key, and project id using the repo's existing config pattern.
- Enable required extensions only when needed.
- Create shared SQL helpers when they reduce duplication, such as updated-at triggers or role helpers.
- Prefer Supabase Auth when it fits the product instead of building custom auth.

## Domain Workflow

For each domain, record a short business analysis before writing:

- user stories.
- actors and roles.
- business rules.
- error scenarios.
- edge cases.
- data entities and relationships.
- direct client queries vs Edge Functions.
- row-level security summary.
- cross-domain dependencies.
- assumptions.

Then implement:

1. migrations for tables, constraints, indexes, enums, triggers, and relationships.
2. row-level security enabled on every exposed table.
3. policies for select, insert, update, and delete per role.
4. SQL functions for atomic invariants and state transitions when needed.
5. Edge Functions for secrets, integrations, complex writes, payments, webhooks, or privileged operations.
6. generated client types after schema changes.
7. tests for user stories, business rules, edge cases, and security policies.

## Row-Level Security Rules

- Prefer deny-by-default.
- Owner-only is the conservative default when permissions are unclear.
- Public read is allowed only for clearly public content.
- Admin policies must be explicit and testable.
- Service-role-only operations belong in Edge Functions or trusted backend contexts.
- Test allowed and denied access for each role.

## Edge Functions

Use Edge Functions for:

- operations needing secrets.
- payment or webhook verification.
- multi-step writes that must enforce server-side invariants.
- admin or privileged actions.
- external SDK calls.

Match the repo's existing response envelope if one exists. Validate input, authenticate callers, check authorization, and map errors clearly.

## Type Generation

After every schema-changing migration:

- generate client types using the repo's Supabase tooling.
- write them to the repo's expected generated-types location.
- use generated types during frontend stitching.

Do not manually maintain duplicate database types when generated types are available.

## Testing

For each domain, test:

- happy path for each user story.
- each acceptance criterion.
- each business rule violation.
- validation errors.
- forbidden access.
- not found.
- conflict or duplicate state.
- row-level security policies for allowed and denied roles.
- Edge Function auth, validation, and error behavior.

Do not mark a domain complete while policies are untested.

## Frontend Stitching

Run after backend domains are complete or when explicitly requested:

- install Supabase client dependencies if missing.
- create or extend the app's Supabase client setup.
- wire public environment variables using the app's config convention.
- implement auth/session restoration if the app uses Supabase Auth.
- replace mocks/stubs with typed queries or function calls.
- wire realtime and storage only where the product needs them.
- handle loading, error, empty, forbidden, and expired-session states.
- verify typecheck/build, auth flow, and row-level security behavior from the app.

Keep UI changes minimal; this phase changes data plumbing, not design.
