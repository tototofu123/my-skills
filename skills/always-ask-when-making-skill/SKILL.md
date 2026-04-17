---
name: always-ask-when-making-skill
description: Enforce a disciplined skill-authoring workflow that prioritizes clarity and substance over speed. Trigger this skill whenever the user asks to create a new skill, edit or optimize an existing skill, or mentions SKILL.md in any context. Always require clarification questions first, present a plan before drafting, obtain approval before writing SKILL.md content, then generate test prompts and set up iteration loops. Never jump to draft without understanding intent. Avoid filler content by asking for more clarification when substance is insufficient. This skill ensures every new skill starts with solid foundations.
version: 1.0.0
---

# Always Ask When Making Skill

## Purpose
Prevent incomplete or misaligned skills by enforcing a structured workflow: deep clarification, explicit planning, approval gates, and substance-first content generation. Every skill should start with strong intent capture and high-quality documentation.

## Mandatory Activation
Trigger this skill whenever the user:
1. Asks to create a new skill
2. Asks to edit, improve, or optimize an existing skill
3. Mentions SKILL.md, skill creation, skill modification, or skill-building in any context
4. References skill framework, skill authoring, or skill development

Do not assume the user's intent is clear. Always activate this workflow.

## Core Principle: Never Skip to Draft
The number one failure mode is jumping to writing SKILL.md content before understanding the actual user problem and design constraints. This workflow prevents that.

## Required Workflow Phases

### Phase 1: Adaptive Clarification Questions
**Goal:** Capture intent, constraints, scope, and success criteria.

Ask questions until ambiguity is low. Use adaptive depth — do not predetermine a fixed number. Cover these areas:
1. **Intent:** What should this skill enable? What user problem does it solve?
2. **Triggering:** When should this skill activate? What user phrases or contexts?
3. **Scope:** What is in scope? What is explicitly out of scope?
4. **Output Format:** What should the skill produce or do?
5. **Examples:** Can the user provide 1-2 real usage examples?
6. **Dependencies:** Does this rely on specific tools, files, or prior context?
7. **Edge Cases:** What happens when constraints conflict or inputs are malformed?
8. **Success Criteria:** How will the user know the skill worked?

**Stopping Rule:** Stop questioning when you can write a 1-paragraph summary of the skill's purpose and triggering conditions without ambiguity.

**User Control:** If the user asks to skip questions and jump to drafting, respect that request UNLESS ambiguity remains high. If ambiguity is high:
- Ask 1-2 critical clarification questions on the highest-uncertainty areas only.
- Explicitly state which questions are blockers and why.
- Proceed only after the user answers or explicitly acknowledges the risk.

### Phase 2: Understanding Summary
**Goal:** Confirm mutual agreement on what the skill will do.

Write a 2-4 sentence summary that covers:
1. Skill name
2. What it does (one clear sentence)
3. When it triggers (context or user phrases)
4. Expected output or behavior

Ask the user: "Is this understanding correct, or should I adjust?"

Do not proceed without confirmation.

### Phase 3: Plan Presentation
**Goal:** Outline the skill's structure and behavior before writing content.

Present a structured plan with:
1. **Skill Metadata:** Name, description line, version
2. **Core Workflow:** Numbered list of steps or phases the skill enforces
3. **Content Sections:** Titles and purpose of each section in the SKILL.md
4. **Examples:** 1-2 concrete examples of expected behavior
5. **Failure Modes:** What happens if constraints are violated
6. **Success Criteria:** How to verify the skill works as intended

Ask the user: "Does this plan align with your intent? Any changes before I write the draft?"

Do not proceed without approval.

### Phase 4: Approval Gate
**Goal:** Obtain explicit permission to draft.

The user must confirm the plan by saying something like "yes," "looks good," "proceed," or "approved."

If the user proposes changes, update the plan and re-ask for approval.

Proceed only after explicit approval.

### Phase 5: Draft SKILL.md Content
**Goal:** Write the full SKILL.md file based on the approved plan.

**Content Requirements:**
1. Draft output must be at least 100 meaningful lines of markdown.
2. No filler, no padding, no repetition for line count.
3. Each section must add substance: rules, examples, workflows, or constraints.
4. Use clear headers, numbered lists, and concrete examples.
5. Frontmatter must include name, description, and version.

**Substance Rules:**
If the content falls below 100 lines and feels complete, that's a signal the scope may be too narrow. Ask clarifying questions:
- "Is there more depth to this workflow you want captured?"
- "Are there edge cases or exceptions to document?"
- "Should there be examples or failure modes explained?"

Do not pad content with useless repetition. Instead, expand scope or ask the user if the narrower scope is intentional.

**Draft Presentation:**
Present the draft and ask: "Does this draft match the plan? Any adjustments before I finalize and create the skill file?"

### Phase 6: Test Prompts
**Goal:** Provide realistic usage examples and expected behavior.

After draft approval, generate 2-3 realistic test prompts that demonstrate the skill in action. Format:

```
Test Prompt 1: [User's natural request phrasing]
Expected behavior: [How the skill should respond]

Test Prompt 2: [Another realistic usage]
Expected behavior: [What should happen]
```

Explain: "Here are test cases to verify the skill works as intended. Shall we try them, or refine the skill first?"

### Phase 7: Iterate and Finalize
**Goal:** Optional improvement loop.

If the user wants to test and refine, run test prompts and gather feedback. Update the skill based on results.

Once satisfied, finalize by:
1. Creating the skill folder in `.claude/skills/<skill-name>/`
2. Writing the final SKILL.md file
3. Confirming the skill is ready to use

If no iteration is needed, proceed directly to file creation.

## Output Format Standard
Use structured headers and numbered steps for every phase:

```
### Phase X: [Phase Name]

**Objective:** [One-line goal]

[Content with numbered lists, clear examples, explicit next action]

**User Confirmation Required:** "Do you approve this phase? [specific approval phrasing]"
```

## No-Filler Rule
**Enforce strictly.** If content is thin or repetitive:
- Do not pad with generic filler text.
- Ask the user: "Is this scope complete, or should we add more depth?"
- Expand scope or adjust based on the user's answer.

Example of filler to avoid:
- "This skill is important because it helps users."
- "The workflow is useful and effective."
- "Always follow best practices."

Example of substance to include:
- "When X happens, do Y because [specific reason]. Here's an example: [concrete case]."
- "Error Z blocks the workflow. To unblock, try [specific fix]."

## Skip-Question Exception
If the user says "skip questions" or "jump to draft":
1. Respect the request and proceed.
2. Exception: If ambiguity is still high on critical intent, triggering, or scope:
   - Ask 1-2 critical questions only on the highest-uncertainty area.
   - Clearly state: "I need clarity on [specific area] before drafting. [Question]?"
   - Proceed only after the user answers.

Do not ask more than 1-2 critical questions in this exception case.

## Skill Metadata Conventions
All new skills should include:
```
---
name: skill-name-with-dashes
description: Comprehensive description covering when to trigger and what the skill does.
version: 1.0.0
---
```

The description field is critical for triggering. Make it explicit and comprehensive so Claude knows when to use the skill.

## Common Pitfalls to Avoid
1. **Jumping to implementation:** Asking questions without a plan first.
2. **Ambiguous triggering:** Leaving the trigger condition unclear in the description.
3. **Vague workflows:** Not specifying step order or decision gates.
4. **No examples:** Providing rules without concrete examples.
5. **Scope creep:** Adding features during drafting that weren't in the plan.
6. **Under-triggering:** Writing skill descriptions that are too passive or narrow.

## Success Checklist
Before marking a skill as complete, verify:
- [ ] Clarification questions were asked and answered
- [ ] Understanding summary was confirmed by the user
- [ ] Plan was presented and approved
- [ ] Draft is 100+ meaningful lines with no filler
- [ ] Description text is explicit about triggering conditions
- [ ] Workflow phases or steps are clearly numbered
- [ ] Examples and edge cases are documented
- [ ] Test prompts were generated and reviewed (optional iteration done)
- [ ] Skill folder created and SKILL.md written to disk

## When to Re-Enter This Workflow
If the user asks to improve an existing skill:
1. Treat it as editing, not creating from scratch.
2. Start at Phase 1 (clarification) with focused questions on what to change.
3. Apply the same approval gates and draft-quality rules.
4. Create a new version if changing triggering or core behavior.
