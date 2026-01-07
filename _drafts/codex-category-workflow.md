# CODEX Workflow: Add a Jekyll Category Page

Use this workflow to add a new category landing page for the blog. Keep it idempotent and avoid touching posts unless explicitly requested.

## Task
Add a new Jekyll category page.

## Inputs
- Category slug: `<CATEGORY_SLUG>` (lowercase, hyphens)
- Category title: `<CATEGORY_TITLE>` (Title Case)

## Explanation
This workflow is intended to be copy-paste friendly for CODEX runs. Replace the placeholders wrapped in angle brackets (`<>`) with the real category values before executing the steps. Keep the result consistent with the repo conventions and avoid modifying posts unless explicitly asked.

## Glossary (Placeholders to Replace)
- `<CATEGORY_SLUG>`: The URL-friendly category identifier used in paths and front matter (lowercase, hyphen-separated).
- `<CATEGORY_TITLE>`: The human-readable category name shown in the page title (Title Case).

## Repo Conventions
- Category pages live at: `category/<slug>/index.md`
- Category page front matter must set:
  - `layout: default`
  - `category: <slug>`
  - `title: <title>`
- Category template include is: `{% include category_list.html %}`

## Steps
1) Check whether `category/<CATEGORY_SLUG>/index.md` already exists.
2) If it exists, do nothing and report that the page is already present.
3) If it does not exist:
   - Create the directory `category/<CATEGORY_SLUG>/` if needed.
   - Create `index.md` with the required front matter and include.
4) Do not modify posts unless explicitly asked.
5) Make a single commit with message: `Add category page: <CATEGORY_SLUG>`.

## File Template
Use this exact structure when creating the file:

```md
---
layout: default
category: <CATEGORY_SLUG>
title: <CATEGORY_TITLE>
---

{% include category_list.html %}
```

## Output
- Show the created/edited file content.
- Mention whether the operation was a no-op due to an existing file.
