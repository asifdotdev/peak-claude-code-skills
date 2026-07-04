---
name: figma-page-builder
description: "Build production web pages from Figma designs. Use when converting Figma page nodes into target-aware HTML, CSS, React, or CMS-ready output with exact content, real assets, responsive behavior, design tokens, and visual QA."
---

# Figma Page Builder

Use this skill to build production web pages from Figma. The target can be a static HTML file, a CMS/embed snippet, or an existing web app. Match the target environment instead of forcing a template.

## Target Contract

Before building, identify the output target:

- existing app component or route.
- static HTML page.
- CMS/embed fragment.
- landing page section.
- email or other constrained renderer.

Then follow the target's constraints:

- For existing apps, use the app's component, styling, asset, routing, and build conventions.
- For static pages, choose a simple structure that can run locally.
- For CMS/embed snippets, keep markup, styles, and scripts self-contained only when the platform requires it.
- Do not add external dependencies unless the target permits them and they solve a real problem.

## Source of Truth

- Use structural Figma data for implementation.
- Use screenshots only for visual QA and ambiguous text confirmation.
- Do not decide hierarchy, colors, spacing, or content from screenshot-only inspection when Figma data is available.
- Every heading, paragraph, button label, stat number, testimonial, and list item must come from Figma or explicit user-provided copy.
- Never ship lorem ipsum, generic names, or placeholder content.

## Page Workflow

1. Read the target page, section, or page list.
2. Fetch structural Figma data for the current target.
3. Fetch design context if available for code hints.
4. Fetch screenshot/render preview for QA comparison.
5. Detect whether Figma dimensions need scaling for the target viewport.
6. Extract design tokens and reusable patterns.
7. Inventory every section top-to-bottom.
8. Build one section at a time.
9. Add responsive behavior as each section is built.
10. Run visual QA before moving to another page.

Never skip a section. If the Figma data response is partial, fetch child nodes or narrower section nodes.

## Scale And Measurements

Figma pages are sometimes designed at 2x or another canvas scale. Detect this before translating measurements:

- compare frame width to intended content width.
- compute a scale factor when needed.
- translate font sizes, spacing, radii, borders, icon sizes, and dimensions for the actual target.
- round to clean values while preserving visual fidelity.

Do not paste raw oversized Figma coordinates into CSS or layout code without checking scale.

## Design System

Extract:

- colors into the target's token system or CSS variables.
- typography styles with exact size, weight, line-height, and family.
- layout max widths and section spacing.
- button, card, nav, pill, badge, form, and icon patterns.
- border radii, shadows, image ratios, and interaction states.

Use reusable classes/components/tokens instead of duplicating styles.

## Assets

- Download every image, icon, illustration, logo, avatar, and decorative graphic shown in Figma.
- Use raster assets for photos, illustrations, composed graphics, effects, gradients, and uncertain exports.
- Use SVG only for clean vector assets that render reliably in the target environment.
- Inspect SVGs before using them; reject broken exports with unresolved variables, embedded images, oversized markup, or distorted aspect behavior.
- Do not use emoji, HTML entities, colored boxes, gradients, or CSS drawings as substitutes for real assets.
- Set explicit dimensions or responsive constraints so images do not shift layout.
- Use descriptive asset filenames and the target's asset convention.

## Section Implementation

For each section:

- build semantic markup/components matching the content.
- match spacing, type, color, radii, shadows, and image sizes.
- include desktop, tablet, and mobile behavior before moving on.
- keep decorative absolute elements responsive; do not blindly copy Figma `x/y` into `left/top`.
- preserve stacking order, crop behavior, and interaction states.

Use grid/flex/layout primitives based on the actual structure.

## Responsive Rules

- Establish the desktop baseline from the design.
- Add tablet and mobile behavior where the layout needs it.
- Ensure text fits and buttons do not overflow.
- Do not hide required content unless the design or user request says so.
- Use target-appropriate breakpoints instead of hardcoding arbitrary ones.

## Verification

Before completion:

- render the page locally if possible.
- compare against the Figma screenshot/render preview.
- test desktop, tablet, and mobile widths.
- check no section is missing.
- check no placeholder content or placeholder assets remain.
- check images load from committed or target-valid asset paths.
- run the target's lint/build checks when available.

If the page cannot be built from available Figma data, stop with a clear blocker instead of inventing the missing design.
