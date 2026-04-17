---
name: visual-layout-debugger
description: Instantly diagnose layout, flexbox, grid, spacing, and alignment issues without a browser. Trigger this skill whenever the user reports layout problems, mentions spacing is broken, shows alignment issues, or asks for help debugging visual structure. Automatically wrap problematic components with high-contrast debug borders (red/blue/green hierarchy) to visualize the DOM structure, explain what each border reveals, diagnose the likely cause, and suggest concrete fixes. This skill is essential when the user cannot easily see the rendered output and needs code-based visual debugging.
version: 1.0.0
---

# Visual Layout Debugger

## Purpose
Layout and spacing bugs are notoriously hard to debug without seeing the rendered output. This skill provides code-based visual debugging by adding temporary high-contrast borders that force the DOM structure to become visible so the user can diagnose alignment, flexbox, grid, and spacing issues.

## Mandatory Activation
Trigger this skill when:
1. User reports "layout is broken" or "alignment is wrong"
2. User mentions "flexbox" or "grid" with a problem context
3. User says "spacing doesn't match" or "elements are overlapping"
4. User asks to debug visual structure without a browser
5. User shows CSS/JSX code and the layout looks potentially problematic
6. User asks "why are my elements not aligned?" or similar
7. User shows a component screenshot or code with visible positioning issues

Do not wait for explicit "add borders" request. Proactively use this skill when layout issues are described.

## Core Principle: Make Structure Visible
The fundamental truth: if you cannot see the box model, you cannot diagnose the problem. This skill makes the invisible visible by applying debug borders that show:
- Where each element actually starts and ends
- How padding and margin are applied
- Whether flex containers are actually flex
- Whether grid columns are sized correctly
- What is overflowing or hidden
- Which elements are inline vs block

## Workflow

### Step 1: Receive Code and Issue Description
The user provides:
1. Code snippet (JSX, HTML, CSS — any format)
2. Description of what's wrong (elements misaligned, spacing broken, flex not working, etc.)

Immediately analyze the structure without asking clarifying questions about the code itself.

### Step 2: Identify Problem Area and Add Debug Borders
Do NOT change the functional code. Only add visualization borders.

**Border Color Hierarchy:**
1. **Red (border-2 border-red-500 or outline-2 outline-red-500)** — Target component with the problem
2. **Blue (border-2 border-blue-500 or outline-2 outline-blue-500)** — Direct parent container
3. **Green (border-2 border-green-500 or outline-2 outline-green-500)** — Grandparent or sibling context

**Application Rules:**
- Add borders to the problematic component (red)
- Add borders to its immediate parent (blue)
- If relevant, add to siblings or grandparent (green)
- Do NOT change any functional styles, margins, padding, or positioning
- Use Tailwind classes by default if code is Tailwind-based; use CSS otherwise

### Step 3: Show Before/After Code Comparison
Format:

```
## Before (original code)
[Original code block]

## After (with debug borders)
[Modified code with red/blue/green borders added]
```

The "After" version should be copy-paste ready so the user can test it immediately.

### Step 4: Explain What Each Border Reveals
Write a concise explanation:

```
## Debug Border Meanings

- **Red border on [ComponentName]:** Shows the exact size and position of this component. 
  - If red border is missing or smaller than expected, the component is being sized by its parent.
  - If red border extends beyond siblings, there's overflow or negative margin.

- **Blue border on [ParentName]:** Shows the parent container's actual size.
  - If blue border is smaller than red, the parent is overflowing and likely has overflow: hidden or is constrained.
  - If blue border is much larger than red, check parent's padding or width constraints.

- **Green border on [SiblingContext]:** Shows how siblings are laid out relative to the target.
  - If green doesn't align with red/blue, there's a positioning context or z-index issue.
```

Explain specifically what the borders reveal about the structure in this case.

### Step 5: Diagnose the Likely Cause
Based on the border visualization, identify the root cause. Common patterns:

**Flexbox Issues:**
- Missing `display: flex` on parent → elements stack as block instead of flex
- Wrong `flex-direction` (row vs column) → elements flow in unexpected direction
- Missing `gap` → elements touching with no spacing between
- `justify-content: space-between` but not enough space → elements cluster
- `align-items: center` on row flex but text baseline differs → slight misalignment
- Child width is too wide for flex container → overflow or shrink behavior unexpected

**Grid Issues:**
- Grid columns defined but items are wider than columns → overflow or stretch behavior
- Missing `gap` property → grid items directly adjacent with no spacing
- Auto-placement not matching visual design → items flow unexpectedly
- Explicit column/row spanning conflicts with other content

**Spacing/Margin Issues:**
- Margin collapse (block elements) → expected margin gap doesn't appear
- Padding on parent conflicts with child margin → double spacing or no spacing
- Negative margin applied to child → overlaps with siblings or parent edge
- Text inside container has default line-height → more space than expected

**Positioning Issues:**
- `position: relative` missing on parent, child `position: absolute` is relative to wrong ancestor
- `overflow: hidden` on parent clips child content unexpectedly
- Z-index wars with siblings → element appears behind when should be in front
- Width/height set too small → content overflows and hidden by parent

Present the most likely cause based on what the borders show.

### Step 6: Suggest Concrete Fix
Provide exact code changes:

```
## Suggested Fix

**Problem:** [One-line diagnosis]

**Fix:**
[Code change with before/after if necessary]

**Why this works:** [One sentence explaining the fix]
```

Example:
```
**Problem:** Flex container is missing display: flex, so children are stacking as block.

**Fix:**
Change: `<div className="flex-container">` 
To: `<div className="flex flex-row gap-4">`

**Why this works:** `display: flex` enables flex layout, `flex-row` ensures horizontal direction, `gap-4` adds spacing between children.
```

### Step 7: Offer Verification
Ask the user: "Apply this fix and remove the debug borders (red/blue/green). Does the layout now match your design?"

If the fix doesn't work, repeat the workflow with a new hypothesis.

## Supported CSS Frameworks

### Tailwind CSS
Use Tailwind utility classes for borders:
- `border-2 border-red-500` (2px red border)
- `outline-2 outline-blue-500` (2px blue outline, does not affect layout)
- `border-dashed`, `border-dotted` for variation

Prefer `outline` over `border` to avoid affecting layout calculations.

### CSS Modules or Scoped CSS
Create a temporary debug style:
```css
/* In component's style file */
.debug-target {
  border: 2px solid red;
}
.debug-parent {
  border: 2px solid blue;
}
```

Then apply class names in JSX/HTML.

### Inline Styles (React)
Apply directly in the component:
```jsx
<div style={{ border: '2px solid red' }}>
  {children}
</div>
```

### Plain CSS
Add directly to the style block:
```css
.problematic-component {
  border: 2px solid red;
}
.parent {
  border: 2px solid blue;
}
```

### CSS-in-JS (styled-components, Emotion, etc.)
Use template literals:
```javascript
const DebugDiv = styled.div`
  border: 2px solid red;
`;
```

## Common Layout Patterns

### Pattern 1: Flex Row Not Aligning
**Symptom:** Items are next to each other but not vertically centered.
**Debug:** Add blue border to flex container, red to items. Check if parent has `align-items: center`. Check if children have conflicting `align-self` or different heights.

### Pattern 2: Grid Overflow
**Symptom:** Grid items break out of their cells or overlap.
**Debug:** Add blue border to grid container, red to grid items. Check if item width exceeds column width. Check if `overflow: hidden` is clipping content.

### Pattern 3: Margin Collapse
**Symptom:** Margin between block elements is smaller than expected.
**Debug:** Add blue border to parent, red to children. If space looks smaller than the margin values, margin collapse is happening. Fix: add `overflow: hidden` to parent or use `gap` instead.

### Pattern 4: Absolute Positioning Off
**Symptom:** Absolutely positioned element is in the wrong place.
**Debug:** Add green border to expected parent (that should have `position: relative`), red to the absolutely positioned child. If green border is not around red, the position context is wrong.

### Pattern 5: Overflow Hidden Clipping Content
**Symptom:** Child content is cut off unexpectedly.
**Debug:** Add blue border to parent with `overflow: hidden`, red to child. Compare red border to blue border edges. Content is clipping if red extends beyond blue.

## Example Walkthroughs

### Walkthrough 1: Missing Flex Direction

**User Code:**
```jsx
<div className="flex">
  <div className="w-32">Box 1</div>
  <div className="w-32">Box 2</div>
  <div className="w-32">Box 3</div>
</div>
```

**User Report:** "Boxes are stacking vertically instead of horizontally."

**Debug Version:**
```jsx
<div className="flex outline-2 outline-blue-500">
  <div className="w-32 outline-2 outline-red-500">Box 1</div>
  <div className="w-32 outline-2 outline-red-500">Box 2</div>
  <div className="w-32 outline-2 outline-red-500">Box 3</div>
</div>
```

**Diagnosis:** Flex container has `display: flex` but no `flex-direction: row`. Flexbox defaults to `flex-direction: column` in some contexts.

**Fix:** Add `flex-row` to the parent:
```jsx
<div className="flex flex-row">
```

---

### Walkthrough 2: Missing Gap

**User Code:**
```jsx
<div className="flex flex-row">
  <button>Save</button>
  <button>Cancel</button>
</div>
```

**User Report:** "Buttons are touching each other, no space between them."

**Debug Version:**
```jsx
<div className="flex flex-row outline-2 outline-blue-500">
  <button className="outline-2 outline-red-500">Save</button>
  <button className="outline-2 outline-red-500">Cancel</button>
</div>
```

**Diagnosis:** Red outlines show buttons are directly adjacent. No `gap` is defined.

**Fix:** Add `gap-2` (or desired spacing) to the parent:
```jsx
<div className="flex flex-row gap-2">
```

---

### Walkthrough 3: Grid Column Mismatch

**User Code:**
```jsx
<div className="grid grid-cols-3 gap-4">
  <div>Item 1</div>
  <div>Item 2</div>
  <div style={{ width: '500px' }}>Item 3</div>
</div>
```

**User Report:** "Item 3 is breaking out of the grid and overlapping other items."

**Debug Version:**
```jsx
<div className="grid grid-cols-3 gap-4 outline-2 outline-blue-500">
  <div className="outline-2 outline-red-500">Item 1</div>
  <div className="outline-2 outline-red-500">Item 2</div>
  <div style={{ width: '500px' }} className="outline-2 outline-red-500">Item 3</div>
</div>
```

**Diagnosis:** Blue outline shows grid container is narrower than 500px (Item 3's forced width). Red outline shows Item 3 extends beyond blue outline, causing overflow.

**Fix:** Remove fixed width or constrain it:
```jsx
<div style={{ width: '100%' }}>Item 3</div>
```

Or add `overflow: hidden` to grid container to contain the overflow.

## Cleanup
After diagnosis and fix:
1. Copy the fixed code without debug borders.
2. Apply the fix to your actual component.
3. Remove all red/blue/green outline/border additions.
4. Test in the browser or development environment.

The skill does not automatically remove borders — the user is responsible for cleanup after diagnosis is complete.

## Forbidden Behaviors
1. Do not modify functional code (just visualize).
2. Do not assume the issue without showing debug borders first.
3. Do not suggest fixes without explaining what the borders revealed.
4. Do not skip the "what each border means" explanation.
5. Do not assume the fix without a clear diagnosis.

## Success Criteria
Diagnosis is successful when:
- User can see the exact DOM structure with borders
- User understands what each border reveals
- Likely cause is identified and explained
- Suggested fix is concrete and copy-paste ready
- User can apply fix and verify in their actual code
