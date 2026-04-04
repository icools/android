---
name: GitHub Pages Markdown Audit
description: A skill to audit and ensure markdown files have the required frontmatter properties and are correctly structured for automatic indexing on GitHub Pages.
---

# GitHub Pages Markdown Audit

When a new markdown file is added or an existing one is modified, use this skill to ensure it displays correctly on the GitHub Pages site.

## Step-by-Step Instructions

0. **Draft Management & Growth Check (Pre-processing)**
   - If the file is located in the `draft/` folder:
     - **Check length**: Count the lines in the file. If it has **fewer than 5 lines**, ignore it (do not process or move).
     - **Auto-Categorize**: If it has 5 or more lines, analyze its content to determine the best destination folder:
       - `ai/openai/`: ChatGPT, GPT-4, Codex, etc.
       - `ai/google/`: Gemini, Gemma, Android Studio AI, etc.
       - `android/api/`: Android framework, Hilt, Worker, Compose, etc.
       - `tools/`: Testing, Decompile, CLI tools, etc.
       - `design/`: UI/UX, Architecture, Diagrams, etc.
     - **Move file**: Rename the file to a descriptive slug (e.g., `nerd.md` -> `nereid-plugin-architecture.md`) and move it to the target directory.

1. **Check for Frontmatter Properties**
   - Ensure the markdown file begins with a YAML block (enclosed in `---`).
   - The block MUST contain at least:
     - `title`: A descriptive page title.
     - `desc`: A short summary of the content (used for cards and SEO).
     - `tags`: A list of relevant tags.
     - `author`: The author name (e.g., `icools`).
     - `date`: Completion/Update date (`YYYY-MM-DD`).
     - `source`: The primary source URL if applicable (e.g., official docs, external articles).

2. **Auto-Generate Missing Properties**
   - If properties are missing, summarize the file content to generate a fitting `title` and `desc`.
   - Recommend a set of `tags` based on the directory path and content.
   - **Check for Source URLs**: If the content contains a primary reference URL (e.g., Android Developers link, news source), automatically add it to the `source` property.

3. **Verify Indexing Status**
   - Check if the file is located in a directory that has an `index.md`.
   - If the directory does NOT have an `index.md`, create one with basic category properties. This ensures the files in that folder are "indexed" and appear as card components on the parent page (per the `_layouts/default.html` logic).

4. **Update Parent Index (Optional)**
   - If the file is a primary entry point, suggest adding a manual link to it in the parent `index.md` file's main list for better visibility.

5. **Final Clean-up**
   - Ensure the layout is using `default` or the standard repo layout.
   - Verify that all relative links within the document are correct.

6. **Image Optimization (PNG to WebP/JPEG)**
   - Identify all `.png` image links in the markdown.
   - **Convert**: Use the `sips` tool (on macOS) to convert them to `.webp` format for better performance on GitHub Pages:
     `sips -s format webp input.png --out output.webp`
   - **Link Update**: Update the markdown code to point to the new `.webp` file extension.
   - **Cleanup**: Delete the original `.png` if it is no longer needed.
