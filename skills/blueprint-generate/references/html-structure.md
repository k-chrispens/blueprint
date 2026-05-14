# HTML structure

The plan is a single self-contained `.html` file. These rules apply to every plan, regardless of template.

## Self-containment (strict)

- Embed all CSS in a `<style>` block in `<head>`. No `<link rel="stylesheet">`.
- Do NOT load any resource over the network. No CDN scripts, no Google Fonts, no remote images, no external icon sets.
- If you need an image or icon, inline it as SVG markup or a `data:` URI.
- The page must render fully and identically with no internet access.

## Document skeleton

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>{feature title}</title>
  <style>/* embedded styles */</style>
</head>
<body>
  <header>...</header>
  <main>
    <section>...</section>
    <section>...</section>
    ...
  </main>
</body>
</html>
```

## Required `<header>`

The `<header>` must contain, in order:

- An `<h1>` with the feature title (a short human-readable name for the plan).
- The original prompt verbatim, rendered expanded and always visible (use a `<blockquote>` or `<p>`).
- The refined `*` clarification bullets wrapped in a `<details>` element collapsed by default, with a `<summary>` like "Refined prompt clarifications". Render the bullets as `<ul><li>`.
- The generation date as a full ISO 8601 timestamp with timezone (e.g. `2026-05-14T15:32:00-07:00`) inside a `<time datetime="...">` element.

## Sections

- One `<section>` per template section, in the order listed by the template's `prompt`.
- Each section starts with an `<h2>` matching the template's section name.
- Use `<ul><li>` for bullet content. Do NOT use long paragraphs.
- Use `<details>`/`<summary>` for any content that is collapsible by intent (e.g. extended rationale, alternatives considered).
- Use `<pre><code>` for code, file paths, and shell commands when they need monospace.

## Open questions callout

- The "Open questions" section (or any section that surfaces unresolved items) must be visually distinct from the rest of the page — give its `<section>` a class and style it with a colored border, background tint, or both, so the reader sees unresolved items at a glance.

## Diagrams

- Diagrams are encouraged where they help the reader (and downstream coding agent) understand structure, flow, or relationships.
- Recommended forms, in order of preference:
  1. Inline `<svg>` markup — fully self-contained, no dependencies.
  2. ASCII / box-drawing diagrams inside `<pre>` blocks — zero dependencies, easy to author.
  3. Mermaid is allowed only if the Mermaid library is fully inlined into a `<script>` block; do not load it from a CDN.
- Diagram inclusion is at your discretion per plan.

## In-page navigation

- For long plans, you may add an in-page navigation aid (e.g. a `<nav>` with anchor links to each section, optionally sticky).
- For short plans, omit it.
- Judge per plan based on length and section count.

## Styling

- All other visual style is at your discretion: typography, colors, max content width, spacing, light vs. dark theme, `prefers-color-scheme` support.
- Aim for a calm, readable page — not a marketing landing page.
