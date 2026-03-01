---
layout: page
title: How to Write Blog Posts
permalink: /how-to-write-blog-posts/
---

This guide explains how to manually create and publish a blog post in this Jekyll site.

## 1) Create a new post file

1. Go to the `_posts/` folder.
2. Create a file named using this format:

```text
YYYY-MM-DD-your-post-slug.md
```

Example:

```text
_posts/2026-01-10-stochastic-volatility-intuition.md
```

## 2) Add front matter

At the top of the file, include front matter like this:

```yaml
---
layout: post
title: "Stochastic Volatility: Intuition and Derivation"
categories: [math, theory]
---
```

Notes:
- `layout` should usually be `post`.
- `title` is the visible headline.
- `categories` should use existing category names when possible.

## 3) Write the body in Markdown

Use normal Markdown for headings, lists, links, and code blocks.

Example:

```markdown
## Why this matters

Volatility clustering is observed across many markets.

- It improves risk forecasts.
- It impacts option pricing.
```

## 4) Write formulas with LaTeX (MathJax)

This site already loads MathJax, so LaTeX formulas can be written as:

- Inline math: `$E[X_t] = \mu$`
- Display math:

```latex
$$
dX_t = \mu X_t\,dt + \sigma X_t\,dW_t
$$
```

Tips:
- Use `\\` for line breaks in aligned equations.
- Use `\label{}` and `\tag{}` only if you really need numbered equations.

## 5) Add tables

Use standard Markdown table syntax:

```markdown
| Model | Strength | Weakness |
|------:|:---------|:---------|
| GBM   | Simple   | Constant volatility |
| Heston| Smile fit| Extra calibration |
```

## 6) Add charts

### Option A: Quick static chart image (recommended for reliability)

1. Generate a chart image locally (`.png` or `.svg`).
2. Save it under `assets/`.
3. Embed it in the post:

```markdown
![Volatility Regimes]({{ '/assets/vol-regimes.png' | relative_url }})
```

### Option B: Interactive chart with Chart.js

You can embed a chart using HTML + JavaScript:

```html
<canvas id="returnsChart"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
  const ctx = document.getElementById('returnsChart');
  new Chart(ctx, {
    type: 'line',
    data: {
      labels: ['Jan', 'Feb', 'Mar', 'Apr'],
      datasets: [{
        label: 'Strategy Return',
        data: [1.2, 0.4, 1.9, 1.1]
      }]
    }
  });
</script>
```

If you use interactive scripts, preview locally before publishing.

## 7) Preview locally

From the repository root:

```bash
bundle exec jekyll serve
```

Then open the local URL shown in the terminal and check:
- Layout
- Math rendering
- Table alignment
- Chart loading
- Links and image paths

## 8) Publish workflow

1. Commit your new post.
2. Push to the repository.
3. GitHub Pages will build and publish automatically.

---

## Post template (copy/paste)

```markdown
---
layout: post
title: "Your Post Title"
categories: [math]
---

## Problem statement

State what problem this post solves.

## Core idea

Explain the method and intuition.

$$
\text{Put your key equation here}
$$

## Example

Provide a worked example or mini experiment.

| Input | Output |
|------:|:-------|
| A     | B      |

## Takeaways

- Key point 1
- Key point 2
```
