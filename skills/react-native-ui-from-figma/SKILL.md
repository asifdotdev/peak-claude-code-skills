---
name: react-native-ui-from-figma
description: "Build React Native UI from Figma designs. Use when implementing mobile screens, route groups, tabs/stacks, reusable components, asset extraction, navigation wiring, accessibility IDs, and visual QA from Figma design data."
---

# React Native UI from Figma

Use this skill to turn Figma screens into production React Native implementation. Treat structural Figma design data as a contract, not inspiration.

## Operating Rules

- Work on one screen or one route slice at a time unless the user explicitly asks for a broader pass.
- Read existing project patterns before changing code: routing, component library, theme tokens, responsive helpers, assets, and navigation.
- If the project already has a screen list, task list, or design handoff order, respect it without skipping ahead.
- Use the available prototype, navigation map, or user-provided route requirements as the source of truth for navigation.
- Keep route files and shared source code in the locations already used by the app.
- Preserve user changes and existing app architecture. Avoid unrelated refactors.

## Route Structure

- Follow the app's existing router and folder conventions.
- Do not create a second navigation system when the app already has one.
- Keep tab roots and detail/flow screens separated so the tab bar appears only where intended.
- Never hide a detail screen inside a tab navigator in a way that still renders tab chrome.
- Build shared screens/components once when the same design appears in multiple flows.
- Use replace/reset navigation when switching flows where the previous stack should not remain in back history.

## Figma Extraction

- Prefer structural Figma data for implementation.
- Use screenshot or rendered previews for comparison, not as the primary data source.
- Use screenshots for QA comparison only. Do not infer element hierarchy, colors, spacing, or text from screenshots when structural data is available.
- If a Figma frame is taller than the device viewport, implement all offscreen content with the correct scroll container.
- Calibrate responsive utilities from the Figma frame size before translating dimensions.

## Assets

- Download every visual asset shown in Figma: icons, photos, illustrations, avatars, backgrounds, decorative graphics.
- Use PNG for image fills, illustrations, multi-layer art, gradients, effects, and uncertain assets.
- SVG is acceptable only for simple monochrome vector icons.
- Inspect SVGs before committing. Reject SVGs with `var(--*)`, embedded images, base64 payloads, external clip refs, or large generated markup.
- Route assets through the app's existing asset convention. If no convention exists, create a small one instead of scattering imports through screen files.
- Do not substitute emoji, color blocks, gradient views, or fake placeholders for real Figma assets.

## Component Work

- Use existing components when they match the required role, but restyle them when the Figma design differs.
- Create typed reusable components when a pattern appears for the first time and will recur.
- Keep screen files composition-focused; move reusable primitives and compound UI to the app's shared component location.
- Add variants through props instead of copying similar component styles into screens.
- Use project color tokens and responsive utilities. Avoid raw hex values and hardcoded sizing in screen files when the project has token utilities.

## Navigation Wiring

- Wire only transitions defined by the prototype, product requirement, or user instruction.
- Handle normal navigation, overlays/modals, back actions, and tab changes deliberately.
- If a visible CTA has no known destination, implement the UI and report the missing navigation decision.
- If a destination route does not exist yet, use a temporary no-op handler only during incremental screen build; remove all placeholders in the final navigation pass.
- Never leave a tappable element without an `onPress` handler.

## Testability

- Add stable `testID` and `accessibilityLabel` values for screens, primary CTAs, inputs, tabs, list items, and scroll containers.
- Do not use array index as the stable id for list/card test IDs.
- If a stable ID is impossible, use the best fallback and note the reason.

## Interactive UI

When Figma shows charts, maps, calendars, sliders, video, date pickers, camera, QR scanning, or other real interactions, use a proven library or native integration. Do not build static view mockups that only look right in screenshots.

## Verification

Run the strongest available gates for the project:

- TypeScript check.
- build/export command used by the repo.
- Screen-level visual comparison against the Figma screenshot.
- Navigation test for every edge touched.
- Audit for missing assets, raw colors, dead tappables, incomplete scroll content, tab bar leakage, and removed test IDs.

Do not mark work complete while a screen is partially implemented, visually mismatched, missing real assets, or wired against guessed navigation.
