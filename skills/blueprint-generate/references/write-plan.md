# Write plan

Write the plan to `blueprint/<slug>/plan-<slug>.html` as a single self-contained HTML file.

Use the refined prompt as your primary input — it contains both the original feature description and all clarifications from the Q&A. Before writing:
- Explore the existing codebase as needed
- Understand current patterns, architecture, conventions

Follow the template's `prompt` field for which sections to include and in what order. The `prompt` is a list of strings — join them with newlines to get the full instructions. The template defines the sections; it does NOT define HTML structure.

For HTML structure, self-containment rules, the required `<header>`, the open-questions callout, diagrams, and in-page navigation, follow [html-structure.md](html-structure.md).

CRITICAL:
- Never write implementation code during this time
- The plan file must be a single `.html` file with no external dependencies
