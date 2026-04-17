---
name: strict-local-asset-library-utilization
description: Enforce local-first UI creation for frontend and visual requests by reusing existing components from sources/libraries and assets from sources/svg and sources/images before writing new UI from scratch. Use this skill whenever the user asks for a website, web app, landing page, dashboard, component, app shell, or any other frontend design task, especially when the workspace already contains reusable libraries or visual assets.
version: 1.0.0
---

# Strict Local Asset & Library Utilization

## Purpose

This skill keeps UI work grounded in the workspace.

The assistant should not invent a new visual system when a local one already exists.

The goal is to reduce duplicate UI, avoid generic repeated layouts, and make the assistant actively search the workspace before proposing new components or placeholder art.

## Triggering

Use this skill for any request that involves:

1. Building or redesigning a website.
2. Building or redesigning a web app.
3. Creating a landing page, portfolio, dashboard, admin screen, marketing page, or app shell.
4. Designing UI components such as cards, headers, navbars, forms, hero sections, tables, or modals.
5. Choosing imagery, icons, illustrations, backgrounds, or visual treatments for frontend work.
6. Improving a visual artifact where local assets or libraries may already solve the request.

This skill should also trigger when the user asks for something that sounds visually specific but broad, such as “make it look good,” “build a clean app,” “design a homepage,” or “create a nice portfolio.”

## Core Rule

Always search the local workspace first.

If an equivalent or clearly reusable local component or asset exists, use it.

Do not write equivalent UI from scratch just because it is easier.

Do not suggest external assets until local sources have been checked.

## Required Search Order

Follow this order every time.

1. Search `sources/libraries/` for reusable UI or component kits.
2. Search `sources/svg/` for matching icons, symbols, illustrations, logos, and decorative assets.
3. Search `sources/images/` for photos, backgrounds, textures, and other raster assets.
4. Inspect local metadata files when needed, especially `sources/images/all_images.json`, `sources/svg/icons.metadata.json`, and `sources/svg/inventory.json`.
5. Only after those checks may you propose new code, generate a new placeholder, or reach for an external asset.

## Library Reuse Rules

When searching `sources/libraries/`, prefer these outcomes in order:

1. Reuse an exact component that already matches the request.
2. Adapt an existing component from the closest local library.
3. Combine multiple local components if that produces a cleaner result than rebuilding.
4. Create new UI only if no reasonable local equivalent exists.

If multiple libraries could work, do not default to the same kit every time.

Ask the user which direction they want when the choice would meaningfully change the visual language.

## Asset Reuse Rules

When searching `sources/svg/` and `sources/images/`, prefer local assets in this order:

1. Exact thematic match.
2. Strong visual match with acceptable adaptation.
3. Supporting asset that can be cropped, recolored, or combined.
4. Only then consider generating a new placeholder.

For images, consult `sources/images/all_images.json` before inventing a visual description.

For SVGs, use the folder structure and inventory files to locate the closest asset family first.

## No-Repeat Visual Rule

Avoid reusing the same visual language by default across unrelated requests.

If the workspace contains several viable directions, pause and ask the user which one to use.

This matters especially when the request could plausibly become:

1. A polished portfolio page.
2. A marketing landing page.
3. A data-heavy web app.
4. A compact dashboard.
5. A playful or experimental interface.

Do not silently collapse these into one generic style.

## Workflow

### 1. Classify the request

Decide whether the user wants a page, app, component, or asset-driven visual.

If the request is ambiguous, treat it as a frontend task and search local sources before answering.

### 2. Search local components

Look in `sources/libraries/` for a match.

Prefer the smallest useful reuse that satisfies the request.

If a component exists, reuse it instead of rebuilding the same structure manually.

### 3. Search local assets

Look in `sources/svg/` and `sources/images/` for visual support.

Use the local catalog data when available so you can match by theme, mood, use case, composition, or content tags.

### 4. Decide whether to ask the user

If you find more than one strong local direction, ask a focused question before building.

Examples of useful questions:

1. “Do you want the cleaner shadcn-style direction or the more expressive magic-ui direction?”
2. “Should this use a photo-led hero or a more icon-driven layout?”
3. “Do you want the page to feel editorial, product-like, or playful?”

### 5. Build from local sources

Use local components and assets as the base.

Adapt them to fit the request instead of replacing them with new generic UI.

### 6. Fall back only when necessary

If no equivalent local component or asset exists, say that the local search did not produce a match and then create the missing piece.

Do not skip the search and go straight to new work.

## Validation Checklist

Before finalizing the response, verify these points:

1. A local component search was performed.
2. A local asset search was performed.
3. Metadata files were checked when imagery or icons mattered.
4. No equivalent existing UI was ignored.
5. If multiple directions existed, the user was asked to choose.
6. Any fallback to new code or new assets was justified by the local search results.

## What To Say To The User

When responding, be explicit about what was found locally.

State whether the answer is based on an existing library, an existing asset, a mixed local reuse approach, or a true fallback.

If you ask the user to choose a direction, keep the question short and concrete.

Do not overwhelm the user with a long list of internal search details.

## Examples

### Example 1: Portfolio landing page

User asks for a portfolio landing page.

The skill should search `sources/libraries/` for a suitable hero, card, and typography system first.

Then it should look for a portrait image or icon set in `sources/images/` and `sources/svg/`.

If multiple clean aesthetic directions exist, ask whether the page should feel editorial, minimal, or bold.

### Example 2: SaaS dashboard

User asks for a dashboard UI.

The skill should prefer an existing dashboard shell or data-display component from the libraries before composing one manually.

If the workspace has multiple dashboard patterns, ask which visual language should lead.

### Example 3: Marketing page

User asks for a marketing landing page with a hero image.

The skill should check image metadata first, then match a local hero asset, then assemble the page around a local component kit.

If there are two equally good image directions, ask the user to pick one instead of defaulting to the same look.

## Failure Modes

If the assistant starts writing a generic UI before checking local sources, the skill has failed.

If it proposes an external image or placeholder while a local asset already fits, the skill has failed.

If it uses the same library and same visual pattern every time without asking, the skill has failed.

If metadata is incomplete, the assistant should say what could and could not be verified rather than guessing.

## Success Criteria

The skill is working when:

1. Local libraries are reused before new UI is written.
2. Local SVG and image assets are preferred before external assets.
3. The assistant asks for direction when several local options fit.
4. The output feels tied to the workspace rather than generic.
5. The user can see why a specific local source was chosen.

## Notes On Scope

This skill is intentionally strict.

It is not trying to make every UI request identical.

It is trying to make the assistant better at choosing from the workspace instead of defaulting to the same repeatable design.

If a future request needs a looser interpretation, that should be stated explicitly by the user.
