---
name: GitHub Pages Markdown Audit
description: A skill to inspect and organize specified Markdown files or folders for this Obsidian plus GitHub Pages repo, with privacy stop rules, draft-only recategorization, frontmatter repair, local image handling, and index-aware cleanup.
---

# GitHub Pages Markdown Audit

Use this skill when the user wants to inspect or organize specific Markdown targets in this repo.

This repo is both an `Obsidian` vault and a `GitHub Pages` knowledge base, so the workflow must preserve both structures.

Follow `CONTENT_CHARTER.md` as the highest repo rule, then `AI_MARKDOWN_WORKFLOW.md` as the short execution spec.

Do not proactively process the whole repo unless the user explicitly asks for a full-project re-check.

## Default Operating Mode

Default to a two-stage workflow:

1. Inspect the specified file or folder and produce findings plus proposed changes.
2. Wait for confirmation before editing, moving, converting images, or updating indexes.

If the user explicitly asks to directly organize or directly apply fixes, you may skip the pause after inspection.

## Supported Targets

This skill should operate only on targets explicitly named by the user:

- one new Markdown file
- one existing Markdown file
- one specified folder

If the target is a folder, recursively inspect all `.md` files within that folder and its subfolders.

## Step-by-Step Instructions

0. **Resolve Scope First**
   - Confirm which file or folder the user wants to process.
   - Never widen the scope on your own from one target to the whole repo.
   - If the user names a folder, recursively gather `.md` files only from that folder tree.

1. **Run Privacy Scan Before Anything Else**
   - Scan the specified Markdown targets for:
     - private email addresses
     - API tokens
     - API keys
     - personal phone numbers
   - If any target matches, stop the entire batch immediately.
   - When stopped for privacy, do NOT:
     - edit files
     - move drafts
     - update frontmatter
     - update indexes
     - perform external research
     - convert or move images
   - Only report which file and roughly where the sensitive content was found, then wait for user confirmation.

2. **Inspect Before Modifying**
   - Default behavior is inspect first, then propose changes.
   - The inspection report should cover:
     - missing frontmatter fields
     - weak or missing `source`
     - missing or low-quality `tags`
     - whether the target is in `draft/`
     - whether a draft appears to belong in a deeper existing category
     - local image assets that need renaming, moving, or conversion
     - links or sections that appear incomplete but have supporting URLs

3. **Repair Frontmatter**
   - Ensure a YAML frontmatter block exists.
   - Required minimum fields:
     - `title`
     - `tags`
     - `desc`
     - `author`
     - `date`
     - `source`
     - `status`
   - `source` should contain one primary source URL only.
   - If no usable source exists, set `source: local`.
   - Put additional references in the article body, not in the `source` field.
   - Existing formal articles may receive repaired frontmatter, `source`, and `tags`, but do not get a `draft` tag automatically.

4. **Handle Draft-Only Recategorization**
   - Only files that currently live in `draft/` should be automatically reclassified and moved.
   - Existing formal articles should stay in place unless the user explicitly asks for recategorization.
   - For draft recategorization:
     - prefer the deepest already-existing category that clearly fits the content
     - do not invent overly deep new folder structures for a single file
     - rename the file to a descriptive English kebab-case slug if needed
     - add or preserve the `draft` tag after moving, so the user can manually verify later

5. **Generate or Improve Tags**
   - Derive tags from the file content and destination path.
   - Keep tags practical for later search and reuse.
   - If the article came from `draft/` and was moved into a formal category, ensure `draft` remains in the tag list until the user later removes it manually.

6. **Handle Local Images Only**
   - Only process local images already referenced in the Markdown.
   - Move images to the destination category's `image/` directory when the article is moved or reorganized.
   - Rename images using descriptive English kebab-case filenames.
   - Convert `.png` files to `.jpg`.
   - Use JPEG quality `60%`.
   - Update Markdown image paths after moving or converting files.
   - Do not automatically download or convert remote image URLs.

7. **Use External Research Sparingly**
   - Only do this when content is clearly incomplete or uncertain and the article already includes relevant URLs.
   - Prefer:
     - official websites
     - official documentation
     - official GitHub repositories
     - trustworthy technical articles
   - Use research only to:
     - confirm positioning
     - fill small factual gaps
     - improve structure and clarity
   - Do not turn the note into a broad encyclopedia rewrite.
   - Do not add speculative claims.

8. **Keep Indexes Coherent**
   - When a draft is moved into a formal category, update the relevant root index page and, when needed, the local category index.
   - If a draft leaves `draft/`, update `draft.md` as needed.
   - Do not create unrelated navigation or structure changes outside the specified task scope.

9. **Final Verification**
   - Ensure links are valid after any rename or move.
   - Ensure image references point to the new local path.
   - Ensure frontmatter, tags, and source remain consistent with the final location.
   - Summarize what was inspected, what was changed, and any follow-up review items for the user.
