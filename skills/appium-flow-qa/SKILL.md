---
name: appium-flow-qa
description: "Run Appium QA for mobile apps. Use when testing screens, user journeys, navigation, UI presence, input behavior, test IDs, and pass/fail/skipped QA reports without modifying source code."
---

# Appium Flow QA

Use this skill for read-only mobile UI QA. The goal is coverage and evidence, not fixing code unless the user explicitly requests a fix phase.

## Non-Negotiables

- QA mode is read-only. Do not modify app source code.
- Test UI and navigation only. Do not validate backend business correctness in this phase.
- Use the app's routes, prototype, QA brief, or user-provided journey list as the navigation source.
- Use Appium tools for UI actions: tap, type, scroll, back, find/wait.
- Use shell only for build, launch, device/emulator inspection, and artifact handling.
- Do not use sleeps. Poll for stable destination anchors with waits/retries.
- Do not dump huge screenshots or page source into chat. Save artifacts to disk.
- Do not use page source as the primary detection method. Use element lookup first; collect source on failures.

## Inputs

Build a QA inventory from the app, design/prototype, or user-provided journeys:

- screen inventory: name, route or expected anchor when available.
- journey inventory: source screen, trigger, navigation type, destination.
- deterministic screen order based on app entry and primary user journeys.
- entry screen: splash/onboarding/login/home depending on app state.

If the requested QA cannot be started because required app access or journey information is missing, stop and report the blocker clearly.

## Environment Setup

- Select the requested device, emulator, or simulator. Prefer an already booted test target when available.
- Prepare the correct Appium automation backend for the platform under test.
- Build/install the app with the repo's documented command.
- Determine the actual app identifier from the installed app or project config instead of assuming it.
- Create the Appium session pinned to the selected test target.

If build or session setup fails, stop with a blocker that includes a short actionable summary.

## Element Strategy

Use this lookup priority:

1. accessibility id
2. platform-native selector
3. platform-native hierarchy query
4. XPath only as a fallback

Avoid coordinate taps. If a coordinate tap is unavoidable, record it as testability debt.

Every screen needs at least one stable anchor proving the screen is visible. Prefer visible labels, primary headings, route-specific controls, or stable test IDs.

## Screen Tests

For every screen in the inventory:

- Reach the screen from entry using normal user navigation where possible.
- Verify the anchor is visible.
- Verify at least two meaningful elements exist when the screen has enough UI.
- Fill inputs with realistic fake data and verify the value is set.
- Retry typing with the tool's focused-field action if normal typing fails.
- Save one source snapshot per screen when useful.
- Save screenshots only when the tool can write them to disk.

Auth and data handling:

- Never block on OTP, email, SMS, captcha, or real payments.
- For OTP screens, try a valid-shaped fake code such as `123456`.
- Use realistic fake credentials and input values.
- If navigation is blocked by server validation, network, or remote data state, mark it skipped with a clear backend-blocked reason and continue.

## Edge Tests

For every journey or navigation edge:

1. Navigate to the source screen.
2. Trigger the action.
3. Wait for the destination anchor.
4. Mark PASS, FAIL, or SKIPPED.
5. On failure, record expected destination, observed state, locator used, and artifact path.

If source navigation fails, restart from entry and retry the edge once before marking it failed or skipped.

## Report

Write a markdown report in the repo's QA/artifacts location. If none exists, use a simple `qa/` folder.

Include:

- summary counts for screens and edges.
- screen inventory table.
- edge matrix with PASS/FAIL/SKIPPED.
- per-screen anchor, presence, input, and artifact notes.
- failure details with expected vs actual.
- issues grouped by Critical, Major, Minor, and Testability Debt.
- missing test IDs, coordinate taps, unreachable screens, and backend-blocked paths.

Final output should include the report path.

## Cleanup

Terminate the app and close the Appium session when finished. Do not leave required QA sessions running after the task is complete.
