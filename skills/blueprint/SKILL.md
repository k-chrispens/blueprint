---
name: blueprint
description: Start a new plan-writing session. Starts from goal, constraints, context and evaluation, then narrows.
---

## Blueprint — Start plan session

Start a new plan-writing session. Help the user write a plan through multi-round Q&A, starting at the level of the whole problem and narrowing only as it becomes clear.

### Step 1: Parse arguments, select template and format

Parse the user's message for the feature description.

Read [references/templates.json](references/templates.json) to get the available templates. Ask the user which template they'd like to use — one per template, using the template's `name` as the label and `description` as the description. Always include an "Other" option so the user can describe a custom template.

In the same prompt, ask which format the plan should be written in: **HTML** (a self-contained page to open in a browser) or **Markdown**. Skip either question if the user's message already answers it.

This is the ONLY time you should ask about the template or the format during the session.

### Step 2: Read the plan template

Read the selected template's `prompt` field to understand the structure and level of detail expected. If the user selected "Other", use their custom description as the template prompt.

Your questions should match the template's level and perspective.

### How to think in this session

Work the way a rigorous, curious physicist would at a whiteboard with a colleague: stay at the level of the whole problem until it is understood, and only then go down into it.

- Say the problem back in your own plain words before asking about it. If you can't state it simply, that is the first thing to sort out.
- Ask the question whose answer would change the most. One good question beats five careful ones.
- Make guesses sharp enough to be wrong ("I think what you really want is X, because Y — is that right?") rather than listing neutral options.
- When the user tells you something new, let it change your picture. Say what you believed before, what you believe now, and drop questions that no longer matter.
- Keep what you know apart from what you are guessing, and say "I don't know" when you don't.
- Be curious about the problem itself, and speak plainly. No jargon the user didn't use first, no quoting or imitating anyone.

### Step 3: Orient

Take a light first look: enough of the project structure and the files nearest the request to ask informed high-level questions. Do not read deeply yet. Deep reading comes later, pointed at whatever the user says matters.

There may be no relevant codebase at all, for example when the user wants to evaluate an approach or learn something by building it. Then orient on the topic instead: look up what the thing is and what is already known about it.

### Step 4: Ask the first questions

The early rounds are about four things, in this order. Do not move down into details until these are understood.

1. **Goal** — what the user is trying to achieve or find out, and why. Is this building something, evaluating an approach, or learning by building?
2. **Constraints** — time, compute, data, compatibility, what must not change.
3. **Context** — what you should read or know: relevant files, prior attempts, papers, docs, datasets, other people's opinions. Ask about context in every round, not only the first, and go and read what the user points to before the next round.
4. **Evaluation** — how we will know it worked. Any answer is acceptable: "I'll build it and look", a fuzzy criterion, or a concrete benchmark, dataset or test. Get it stated before the work starts.

Ask 1-5 questions following the format in [references/questions.md](references/questions.md). Fewer, broader questions are better early on.

Guidelines:
- Keep text between questions brief — a sentence or two of context at most, not a full analysis
- Gather facts before asking — if something can be determined by reading code, searching docs, or looking up external references, find the answer yourself. Do not make subjective decisions on behalf of the user
- Do NOT ask questions whose answers are already obvious from what you have read
- Users generally expect to continue existing patterns — only question existing patterns when the user's change clearly conflicts with them
- Do NOT ask questions about the plan template itself

#### Edge cases wait

Do NOT ask about edge cases, error handling or implementation details in the first two rounds. When one comes up, from you or from the user, add it to a short **Parked for later** list and move on. Raise parked items only when the user asks for them, or once goal, constraints, context and evaluation are settled (usually round three or four). Anything still parked when the plan is generated goes into its open questions, and is worked through during refinement.

#### Reading

When the task is to evaluate an approach or to learn something, suggest 2-4 things to read: papers, articles or docs. Search for them — do not cite from memory — and give each a link and one line on why it is worth the user's time. In the next round, ask what the user made of them, and let the answers change the direction.

Before the questions, add this hint:

```
> Answer with shorthand like `1a, 2b, 3e, 4a, 5b` or write freely.
```

After the questions, add:

```
Once you're done answering, I'll follow up with more questions. When you're ready, invoke the blueprint-generate skill to end the Q&A and generate the plan.
```

### Step 5: Continue Q&A

Wait for the user to respond. Accept answers in any format:
- Shorthand: `1a, 2b, 3e` or `1a 2b 3e`
- Prose: natural language answers
- Mixed: `1a, 2b, 3. I think we should...`

When they answer:
- If they answered a question with a follow-up question of their own, answer it (or finish the discussion with them) before moving to the next round
- If they revise an earlier answer or the goal itself, go back: update the refined prompt, and re-ask whatever the change invalidates instead of pressing on
- Acknowledge briefly
- Show the updated refined prompt — take the original feature description and add bullet points (using `*` syntax) incorporating all clarifications so far. Follow the rules in [references/refine-prompt.md](references/refine-prompt.md). Display it in a blockquote so the user can see how their answers are shaping the plan. If anything is parked, show the **Parked for later** list under it.
- ALWAYS ask 1-5 more questions. These may be follow-ups to their answers or topics that still need discussing. Use the same question format as Step 4 (including the shorthand hint before the questions and the blueprint-generate reminder after).
- Keep asking rounds of follow-up questions until the user invokes the blueprint-generate skill

IMPORTANT: Do NOT stop asking questions on your own. Only the user decides when Q&A is done by invoking blueprint-generate. Do NOT generate the plan. Do NOT write or modify any code files — you are only gathering information.

### Progress indicator

Once a feature description has been provided and a template selected, append a single progress line at the end of **every** message. Do NOT show the progress line before then (e.g. when asking for a missing feature description or during template selection).

The line shows all four workflow phases using `✓` (completed), `●` (active), `○` (pending). Explore completes before the first user-visible question, so always show:

```
✓ Explore  ● Plan  ○ Write  ○ Refine
```

Place the line after all other content, separated by a blank line.
