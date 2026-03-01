# SKILL.md — Blog Post Creation Skill

A reusable skill guide for AI assistants (Claude, Cursor, and Windsurf) to create blog posts in this repository.

## Applies to

- Claude
- Cursor
- Windsurf

## Objective

Create publication-ready posts for a Jekyll blog with correct front matter, clean Markdown, valid LaTeX, and optional tables/charts.

## Workflow

1. **Clarify topic and audience**
   - Identify domain (math, theory, AI, quant finance, etc.).
   - Determine depth (introductory, intermediate, advanced).

2. **Create file**
   - Path: `_posts/YYYY-MM-DD-slug.md`
   - Slug: lowercase, hyphen-separated.

3. **Insert front matter**

```yaml
---
layout: post
title: "Specific and informative title"
categories: [math]
---
```

4. **Draft structure**
   - Problem statement
   - Intuition
   - Formal model or derivation
   - Example / implementation snippet
   - Key takeaways

5. **Add technical elements as needed**
   - **LaTeX**: `$...$` for inline, `$$...$$` for display.
   - **Tables**: Markdown table syntax.
   - **Charts**:
     - Preferred: static image under `assets/`.
     - Optional: interactive Chart.js embed.

6. **Quality pass**
   - Ensure no broken Markdown fences.
   - Verify YAML front matter syntax.
   - Keep tone concise and analytical.
   - Ensure every symbol in equations is defined.

## Templates

### Minimal post template

```markdown
---
layout: post
title: "Your Title"
categories: [theory]
---

## Problem

State the problem.

## Intuition

Explain why this matters.

## Model

$$
\text{equation}
$$

## Example

| Metric | Value |
|------:|:------|
| A     | B     |

## Takeaways

- Point 1
- Point 2
```

### Chart.js snippet template

```html
<canvas id="chart"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
  new Chart(document.getElementById('chart'), {
    type: 'line',
    data: {
      labels: ['T1', 'T2', 'T3'],
      datasets: [{ label: 'Series', data: [1, 3, 2] }]
    }
  });
</script>
```

## Tool-specific notes

### Claude
- Ask for the intended reader level if unclear.
- Prefer clearer prose and explicit assumptions.

### Cursor
- Generate the post directly in `_posts/`.
- Keep incremental edits small and verifiable.

### Windsurf
- Use a scaffold-first approach: front matter + headings, then fill sections.
- Run a final syntax check for Markdown and YAML correctness.
