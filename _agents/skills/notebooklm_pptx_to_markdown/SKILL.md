---
name: Process NotebookLM PPTX into Markdown
description: A skill to instruct the agent on how to process NotebookLM-generated PPTX files, insert them into Markdown, and convert images to 70% JPEG quality.
---

# Process NotebookLM PPTX into Markdown

When asked to process a presentation that was generated from NotebookLM (or another source) and insert it into a Markdown document, follow this workflow:

## Step-by-Step Instructions

1. **Verify PPTX Format**
   - Ensure the user provides a `.pptx` file. If they provide a PDF, remind them that PPTX is significantly more convenient for automatic extraction of structure, text, and images.

2. **Extract Content**
   - Parse the PPTX to extract the slide titles, bullet points, and speaker notes.
   - Extract all embedded images from the slides.

3. **Image Optimization (70% JPEG)**
   - All extracted images MUST be converted to **JPEG** format.
   - Apply a **70% quality compression** to all JPEGs to optimize for web performance and file size constraints while retaining visual clarity.
   - Save these optimized images to an appropriate `assets` or `images` directory according to the user's project structure.

4. **Markdown Insertion**
   - Seamlessly insert the extracted and formatted content directly into the target `.md` file.
   - Ensure the optimized JPEG images are properly linked using standard markdown syntax. (e.g. `![Slide N](../assets/slide_N.jpg)`)
   - Structure the markdown logically, combining the existing written article content with the newly inserted slide presentation visuals.
