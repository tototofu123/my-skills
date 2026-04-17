---
name: design-intake-guard
description: Companion verifier for frontend design tasks. Use this skill alongside primary design skills whenever the user asks for UI/frontend design, redesign, styling, or layout changes. This skill does not replace implementation skills. It verifies that aesthetic intake happened, checks for generic/cliche styling, and flags unjustified gradients or heavy shadows before finalizing output.
version: 1.0.0
---

# Design Intake Guard

## Purpose

This is a companion verification skill.

It is intentionally secondary to primary design skills that do planning and implementation.
Use it to review and constrain visual decisions so outputs align with user intent.

## Priority and Role Boundary

1. Do not take over the main design workflow.
2. Let the primary design skill do ideation, structure, and implementation.
3. Use this skill to verify quality and issue flags.
4. Return concise corrections when problems are found.

Primary skills may include:
- frontend-design
- impeccable
- layout
- typeset
- colorize
- polish
- audit

If no primary design skill is active, still behave as a verifier and keep guidance lightweight.

## Mandatory Verification Scope

Always verify the following before finalizing major frontend output:

1. Aesthetic Intake Present
- Confirm target aesthetic is explicit.
- Example values: Minimalist Portfolio, High-Density SaaS, Brutalist Landing Page, Editorial, Playful Consumer App.

2. Design Intent Artifact Present
- Confirm there is a quick checklist.
- Confirm there is a short design brief.
- Confirm there are initial color and type tokens.

3. Generic Styling Guard
- Flag default or cliche gradients that are not requested.
- Flag excessive drop shadows used as generic polish.
- Flag boilerplate visual patterns that ignore the requested aesthetic.

4. Justification Check
- If gradients or shadows are used, ensure they are subtle and justified by the stated aesthetic.
- If justification is weak, flag and suggest alternatives.

## Intake Protocol

Before major design generation, ensure one direct aesthetic question was asked.

Required question pattern:
- "What target aesthetic should this design follow?"

Recommended optional follow-ups:
- "What density do you want: airy, balanced, or compact?"
- "What tone do you want: formal, playful, premium, or utilitarian?"

If the user already provided clear style direction, do not repeat intake.

## Missing Aesthetic Fallback

If no aesthetic is given:

1. Ask once for aesthetic.
2. If still not provided, allow temporary fallback:
- Use neutral non-gradient styling.
- Avoid heavy shadows.
- Mark output as temporary style pending aesthetic confirmation.

This fallback is allowed to keep progress moving.
It is not the final desired state.

## Strict Visual Constraints

Default behavior unless user intent says otherwise:

1. Do not introduce generic hero gradients.
2. Do not stack multiple decorative shadows by default.
3. Do not add visual effects only to make UI look "fancy".
4. Prefer structure, spacing, typography, and hierarchy over effects.

Allowed only when justified:

1. Subtle gradient with clear aesthetic reason.
2. Subtle elevation shadows that support usability.
3. One intentional visual accent aligned with the selected aesthetic.

## Companion Workflow

Use this workflow with primary design skills.

### Step 1: Observe
Read the user request and the primary skill output.

### Step 2: Verify Intake
Check whether the aesthetic and intent artifacts exist.

### Step 3: Verify Style Compliance
Check gradients, shadows, and boilerplate patterns against aesthetic intent.

### Step 4: Report
Return pass or flags with exact corrective actions.

### Step 5: Re-check
After corrections, run a brief second pass and confirm status.

## Output Format

Always produce this structure in verifier mode:

### Verification Status
- Overall: PASS or FLAGGED

### Checks
- Intake: PASS or FLAGGED
- Brief and Tokens: PASS or FLAGGED
- Generic Styling Guard: PASS or FLAGGED
- Effect Justification: PASS or FLAGGED

### Findings
- List each issue in one line.
- Include why it violates intent.

### Corrections
- Provide concrete fixes.
- Prefer minimal changes needed.

### Final Note
- If fallback was used, state: "Temporary neutral style used pending aesthetic confirmation."

## Interoperability Examples

Example A: With a primary builder skill
- Primary skill builds a landing page.
- This skill checks intake and style compliance.
- If generic gradients appear without justification, flag and request a revision.

Example B: With a polish skill
- Polish skill applies finishing touches.
- This skill ensures polish does not introduce cliche effects.
- Approve only if visual effects align with target aesthetic.

Example C: Existing design system
- Primary skill follows existing tokens.
- This skill checks that any visual additions remain system-consistent.
- If system conflicts with requested aesthetic, recommend a compromise and note tradeoff.

## Failure Modes and Handling

1. No aesthetic specified
- Ask once.
- If still missing, allow temporary neutral fallback and label it.

2. Conflicting aesthetic requests
- Ask user to prioritize one direction.
- Provide two concise options if helpful.

3. Heavy effects already in existing codebase
- Do not rewrite everything.
- Suggest incremental corrections aligned with requested aesthetic.

4. Tight timeline request
- Keep verification concise.
- Flag only high-impact style mismatches.

## Success Criteria

The skill is successful when:

1. Primary design skill remains in control of implementation.
2. Aesthetic intake is captured or fallback is clearly labeled.
3. Generic gradients and excessive shadows are reduced unless justified.
4. Output includes clear pass or flag status with actionable fixes.

## Test Prompts

Test Prompt 1:
"Build a landing page for my portfolio."
Expected behavior:
- Ask direct aesthetic question before major styling.
- If user does not answer, use temporary neutral non-gradient fallback and label it.

Test Prompt 2:
"Make a high-density SaaS dashboard with subtle steel-blue gradients."
Expected behavior:
- Accept gradient usage only if subtle and justified by the stated aesthetic.
- Verify brief and tokens are present.

Test Prompt 3:
"Polish this pricing page and make it modern."
Expected behavior:
- Verify polish output does not add generic cliche effects.
- Return pass or flags and exact corrections.

## Short Checklist for Fast Passes

- Aesthetic explicitly known or temporary fallback labeled
- Brief plus tokens present
- No default cliche gradient usage
- No excessive drop-shadow stacking
- Corrections are concrete and minimal
