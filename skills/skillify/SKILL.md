---
name: skillify
description: Turn a repeatable workflow from the current conversation into a reusable standard skill. Use when the user wants to capture what you just did as a skill, turn a successful session into a `SKILL.md`, preserve a reusable process, or says things like "make this a skill", "turn this into a skill", "capture this workflow", or "productize this process."
---

# Skillify

Turn the current session into a reusable, portable skill. Recover what actually happened, where the user steered or corrected you, which inputs mattered, and what counts as success. Then confirm the missing details with the user and write a clean skill folder with a `SKILL.md`.

## Start with the session

Before asking questions:

- Review the current conversation and any available session summary or memory.
- If the user supplied a short description alongside the invocation, treat it as a hint rather than ground truth.
- Extract the repeatable process, inputs or parameters, ordered steps, success artifacts or criteria, user corrections, tool usage, agent usage, and hard preferences.

If session memory is unavailable, rely on the conversation history and ask only for the gaps that matter.

## Interview style

- Prefer `AskUserQuestion` for all clarifying questions when that tool is available.
- If `AskUserQuestion` is unavailable, ask concise plain-text questions instead.
- Ask in rounds instead of dumping every question at once.
- Do not add fake options like "Other", "Needs tweaking", or "I'll edit it" when the UI already supports freeform follow-up.
- Stop asking once the workflow is clear enough to write confidently. Simple workflows should stay simple.
- Pay extra attention to moments where the user corrected your earlier approach. Those corrections often define the most important rules for the generated skill.

## Round 1: Confirm the shape

- Propose a skill name and a one-line description based on your analysis.
- Confirm the core goal and what "done" looks like.
- If the observed process is too broad, propose a tighter scope instead of writing a vague mega-skill.

## Round 2: Inputs and operating mode

- Present the major steps you inferred as a numbered list.
- Suggest arguments only if a future invocation would truly need caller-provided inputs.
- Decide whether the generated skill should run inline or as a self-contained forked flow:
  - inline: the user will probably steer or review mid-process
  - forked: the workflow is self-contained and can run with minimal interaction
- Ask where the generated skill should be saved. Good defaults:
  - repo-specific workflow -> `.pi/skills/<name>/SKILL.md`
  - cross-repo personal workflow -> `~/.pi/agent/skills/<name>/SKILL.md`

## Round 3: Break down each step

For each major step that is not already obvious, clarify:

- what artifacts, data, or IDs it produces for later steps
- what proves the step succeeded
- whether there should be a human checkpoint before irreversible actions
- whether some steps can run in parallel or through subagents
- which hard constraints or strong preferences must survive into the new skill

Split this into multiple question rounds if there are many steps. Do not over-interview a simple two-step workflow.

## Round 4: Triggering and gotchas

- Confirm when Claude should invoke the generated skill automatically.
- Include trigger phrases and realistic example requests.
- Ask about gotchas, anti-patterns, or edge cases only if they are still unclear.

## Write the generated skill

Create a portable standard skill folder with a `SKILL.md`. Prefer the simple Agent Skills format unless the target runtime explicitly needs richer frontmatter.

Use this shape as the default:

```markdown
---
name: skill-name
description: Clear one-line description that says what the skill does and when to use it.
---

# Skill Title

## Purpose
What this workflow is trying to achieve.

## When to use it
How to recognize the situations where the skill should trigger.

## Inputs
- Required arguments, files, or context.

## Workflow

### 1. Step name
Concrete instructions.

**Success signals**
- What proves this step is done.
```

When writing the generated skill:

- Put trigger guidance in the `description`, not scattered vaguely through the body.
- Use imperative instructions.
- Explain why important steps or rules matter.
- Include clear success signals so the workflow knows when to move on.
- Add arguments, extra frontmatter fields, or execution-mode details only when they materially improve reuse.
- Keep simple skills short.
- If the workflow depends on scripts, templates, or references, create those files alongside `SKILL.md` and point to them clearly.

## Review before saving

- Show the complete generated `SKILL.md` to the user in a fenced code block before writing it.
- Prefer a `yaml` fenced block if the frontmatter is the main thing under review; otherwise use `markdown`.
- Ask for final confirmation with `AskUserQuestion` when available.
- Only write the file after the user confirms.

## After saving

Tell the user:

- where the skill was saved
- how to invoke it
- that they can edit `SKILL.md` directly to refine behavior

## Quality bar

- Capture the workflow that actually happened, not an idealized fantasy version.
- Preserve user steering, naming preferences, and do-or-don't-do-this corrections.
- Optimize for reuse across future sessions instead of overfitting to one example.
- If the observed workflow is still too broad, narrow it before writing.
