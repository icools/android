---
name: GitHub Pages Markdown Audit
description: A skill to audit and ensure markdown files have the required frontmatter properties and are correctly structured for automatic indexing on GitHub Pages.
---

# GitHub Pages Markdown Audit

When a new markdown file is added or an existing one is modified, use this skill to ensure it displays correctly on the GitHub Pages site.

## Step-by-Step Instructions

1. **Check for Frontmatter Properties**
   - Ensure the markdown file begins with a YAML block (enclosed in `---`).
   - The block MUST contain at least:
     - `title`: A descriptive page title.
     - `desc`: A short summary of the content (used for cards and SEO).
     - `tags`: A list of relevant tags.
     - `author`: The author name (e.g., `icools`).
     - `date`: Completion/Update date (`YYYY-MM-DD`).

2. **Auto-Generate Missing Properties**
   - If properties are missing, summarize the file content to generate a fitting `title` and `desc`.
   - Recommend a set of `tags` based on the directory path and content.

3. **Verify Indexing Status**
   - Check if the file is located in a directory that has an `index.md`.
   - If the directory does NOT have an `index.md`, create one with basic category properties. This ensures the files in that folder are "indexed" and appear as card components on the parent page (per the `_layouts/default.html` logic).

4. **Update Parent Index (Optional)**
   - If the file is a primary entry point, suggest adding a manual link to it in the parent `index.md` file's main list for better visibility.

5. **Final Clean-up**
   - Ensure the layout is using `default` or the standard repo layout.
   - Verify that all relative links within the document are correct.
