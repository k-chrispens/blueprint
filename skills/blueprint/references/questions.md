# Questions

Output a numbered question list in the conversation (NOT in the plan file):

```
> Answer with shorthand like `1a, 2b, 3e` or write freely.

---

**Q1. [Question text]**

_Context: [1-2 lines on what prompted this question]_

a) Option A
b) Option B
c) Option C
d) Other (describe)


**Q2. [Question text]** (select all that apply)

_Context: [1-2 lines on what prompted this question]_

a) Option A
b) Option B
c) Other (describe)


**Q3. [Open-ended question text]**

_Context: [1-2 lines on what prompted this question]_
```

Leave two blank lines between questions. Leave a blank line between the question text, context line, and options. For questions with clear enumerable options, provide lettered choices. Always include a final "Other (describe)" option so the user can provide their own answer. If a question is better answered with free text, omit choices. Focus on decisions that change the direction of the work — not trivial or obvious choices. In early rounds, prefer open-ended questions; offer lettered choices only when the options really are enumerable.
