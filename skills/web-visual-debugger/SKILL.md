---
name: web-visual-debugger
description: Global website debugging skill that dynamically uses Figma and Pencil workflows alongside normal code edits. Trigger this for layout, spacing, responsiveness, visual consistency, and annotation tasks where quick visual edits, comments, and actionable debugging guidance are needed.
version: 1.0.0
---

# Web Visual Debugger

## Purpose

Debug website UI quickly through a hybrid workflow:
1. Visual-first adjustments using Figma context when useful
2. Comment-first annotations using Pencil/code notes when useful
3. Direct code fixes when the issue is implementation-level

The skill should choose the path dynamically and explain the chosen path to the user.

## Mandatory Activation

Use this skill whenever the user asks to:
1. Debug layout, spacing, alignment, flexbox, grid, or overflow issues
2. Diagnose responsive behavior problems
3. Add visual comments/annotations before changing code
4. Perform quick visual iteration without immediate code rewrites
5. Review UI consistency, style drift, or accessibility visuals
6. Use Figma, Pencil, or both for design/code feedback loops

## Core Principle

Use the fastest reliable loop for the current problem.

- If the issue is visual structure or composition uncertainty, start visual.
- If the issue is known in code and needs implementation correction, start in code.
- If uncertainty remains after one pass, run hybrid (visual + code annotations).

## Decision Engine (Dynamic Path Selection)

### Path A: Figma-First

Choose when:
1. User wants quick visual edits or annotations
2. The issue is about hierarchy, composition, spacing rhythm, or overall look
3. Multiple design alternatives need comparison before code changes

Actions:
1. Pull design context/screenshots where available
2. Annotate visual issues with concrete guidance
3. Propose small targeted adjustments
4. Convert accepted visual adjustments into implementation notes

Output:
- Visual findings
- Annotated recommendations
- Hand-off instructions for code updates

### Path B: Pencil/Code-Comment-First

Choose when:
1. User wants direct code-level annotations
2. The issue is localized to specific components/files
3. Team needs actionable comments in implementation artifacts

Actions:
1. Add precise inline comments or review notes
2. Mark likely root causes (container constraints, gap misuse, width conflicts, etc.)
3. Suggest minimal, concrete code edits
4. Validate expected behavior after edits

Output:
- File-level findings
- Comment-ready fix notes
- Small patch guidance

### Path C: Hybrid (Figma + Pencil + Code)

Choose when:
1. The issue spans both visual intent and implementation
2. First pass did not fully resolve ambiguity
3. User explicitly asks for both workflows

Actions:
1. Visual diagnosis pass (Figma or screenshot-based)
2. Implementation annotation pass (Pencil/comments)
3. Optional direct code fix pass
4. Consolidated resolution checklist

Output:
- Unified issue map
- Prioritized fix plan
- Verification checklist

## Communication Contract

Before acting, state in one line:
- selected path (`Figma-first`, `Pencil-first`, or `Hybrid`)
- reason for selection

Example:
`Path selected: Hybrid — layout intent is unclear visually and the code has probable flex constraints, so both visual annotation and file-level comments are needed.`

## Debugging Workflow

### Step 1: Triage

Capture:
1. Symptom (what user sees)
2. Scope (single component, page section, full page)
3. Environment (mobile/desktop/breakpoint)
4. Priority (blocking vs cosmetic)

### Step 2: Evidence

Collect relevant evidence:
1. Screenshot/design context when available
2. Component structure and surrounding containers
3. CSS utility/class conflicts
4. Runtime or console hints (if present)

### Step 3: Diagnose

Classify issue type:
1. Layout model mismatch (flex/grid/block)
2. Constraint mismatch (width/height/min/max/overflow)
3. Spacing system drift (inconsistent tokens/gaps)
4. Responsive breakpoint mismatch
5. Accessibility visual issue (focus state, contrast, touch target)

### Step 4: Annotate

Produce comments that include:
1. What is wrong
2. Why it is happening
3. Exact expected behavior
4. Minimal change to test first

### Step 5: Fix Strategy

Use smallest-change-first ordering:
1. Container model fixes
2. Sizing/constraint fixes
3. Gap/margin/padding normalization
4. Breakpoint adjustments
5. Visual polish and state refinements

### Step 6: Verify

Verify across:
1. 375px mobile
2. 768px tablet
3. 1024px+ desktop
4. Keyboard focus visibility
5. Contrast baseline checks

## Comment Quality Standard

Every annotation should be:
1. Specific to element/component
2. Actionable with one concrete next step
3. Short enough for quick execution
4. Linked to outcome (what improves)

Bad:
- `Layout seems off here.`

Good:
- `Parent uses flex-row at 375px, causing CTA text truncation. Switch to flex-col below md and keep button width full on mobile.`

## Figma Annotation Standard

When doing visual annotations:
1. Tag issue severity (blocker/high/medium/low)
2. Mark affected area clearly
3. Include one-sentence cause hypothesis
4. Include one recommended change
5. Avoid broad, vague style comments

## Pencil/Code Annotation Standard

When annotating code:
1. Reference exact selector/component area
2. Keep one issue per comment
3. Include expected before/after behavior
4. Avoid style-only opinions without measurable impact

## Integration With Normal Code Edits

This skill does not forbid code changes.

Rule:
1. Start with annotation when uncertainty is high
2. Proceed to code edits when fix is clear
3. Report what changed and why
4. Re-verify at target breakpoints

## Common Issue Playbook

### Flex/Gird Collision
- Symptom: elements overlap or compress unpredictably
- First test: verify parent display mode and child sizing rules
- Typical fix: align container model and remove conflicting fixed widths

### Mobile Overflow
- Symptom: horizontal scroll at 375px
- First test: identify fixed-width child or large gap/padding
- Typical fix: use max-width 100 percent, clamp text, reduce gap on small breakpoints

### Weak Focus States
- Symptom: keyboard users cannot track focus
- First test: tab-through all interactives
- Typical fix: add visible focus ring with contrast-safe color and offset

### Inconsistent Spacing Rhythm
- Symptom: UI feels uneven or noisy
- First test: compare spacing to base token scale
- Typical fix: normalize to 4px/8px increments and reduce ad-hoc values

## Output Format

Use this output structure:

1. `Path selected`
2. `Findings` (ordered by severity)
3. `Annotations` (visual and/or code)
4. `Minimal fix plan`
5. `Verification checklist`
6. `Optional direct patch summary` (if code was edited)

## Example Triggers

1. `This card grid breaks on tablet — can you debug quickly and annotate first?`
2. `Use Figma and comments to show why this hero feels off before changing code.`
3. `Review this layout using Pencil comments, then apply the smallest fix.`
4. `Do a hybrid pass: visual note first, then code-level fixes.`

## Success Criteria

This skill is successful when:
1. Correct path is selected dynamically and explained
2. Findings are specific and prioritized
3. Comments are actionable and concise
4. Visual and code workflows can be interchanged without confusion
5. Fixes are verified at key breakpoints and accessibility baseline

## Test Prompts

Test Prompt 1:
`Hero section is misaligned on mobile. Annotate first, then suggest minimal fix.`
Expected behavior:
- Select Pencil-first or Hybrid, identify exact container issue, provide targeted fix sequence.

Test Prompt 2:
`Use Figma-style visual notes to explain why this dashboard spacing feels wrong.`
Expected behavior:
- Select Figma-first, provide severity-tagged visual findings and spacing normalization plan.

Test Prompt 3:
`I want both Figma and code comments before any major rewrite.`
Expected behavior:
- Select Hybrid, produce visual annotations plus implementation comment set and verification checklist.
