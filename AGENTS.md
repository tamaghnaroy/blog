# AGENTS.md

Guidance for LLM coding agents working in this repository.

## Goal

Help authors create high-quality blog posts in `_posts/` for a Jekyll + GitHub Pages site.

## Required post format

When creating a new post, always:

1. Create a file under `_posts/`.
2. Name it `YYYY-MM-DD-slug.md`.
3. Add valid front matter:

```yaml
---
layout: post
title: "Clear, specific title"
categories: [math]
---
```

4. Write content in Markdown with clear section headings.

## Content quality rules

- Use concise, technically correct language.
- If formulas are used, ensure LaTeX syntax is valid for MathJax.
- Prefer one idea per section.
- Include at least one practical example (code, derivation, or worked numeric example).
- End with a short takeaway list.

## Math and notation

- Inline equations: `$...$`
- Display equations: `$$...$$`
- Use standard symbols and define variables before using them.

## Tables and charts

- Use Markdown tables for simple comparisons.
- Prefer static image charts in `assets/` for reliability.
- If using interactive charts (e.g., Chart.js), include script tags directly in the post and keep dependencies minimal.

## Editing constraints

- Do not rename existing posts unless asked.
- Do not remove existing categories unless asked.
- Keep changes scoped to the user request.

## Suggested output checklist for agents

Before finishing a blog-post task:

- [ ] New post filename follows Jekyll date format.
- [ ] Front matter is valid YAML.
- [ ] Markdown renders cleanly.
- [ ] Math expressions are syntactically correct.
- [ ] Tables/charts render or gracefully degrade.
