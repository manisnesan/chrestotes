# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## ⚠️ Important: Editing .ipynb Files

**When editing Jupyter notebook files (`.ipynb`), ALWAYS use the `edit_notebook` tool, never use `search_replace` or `write` tools.** Notebooks are JSON files with a specific structure that requires special handling. See the "Working with .ipynb Files" section below for detailed guidelines.

## Project Overview

Chrestotes is a commonplace blog (personal knowledge repository) built with a dual publishing system:
- **Legacy system:** fastpages (Jekyll-based) for backward compatibility
- **Current system:** Quarto for modern publishing workflow

The blog focuses on machine learning, information retrieval, search systems (particularly Apache Solr), and NLP topics.

## Publishing Commands

### Primary Publishing Method (Quarto)
```bash
quarto publish gh-pages --no-browser
```
This publishes to GitHub Pages using Quarto's publish command.

### Local Development (Quarto)
```bash
quarto preview
```

### Legacy Jekyll Development (Docker-based)
```bash
# Start Jekyll server with live reload
make server

# Start in detached mode
make server-detached

# Convert notebooks/Word docs to posts
make convert

# Rebuild Docker images (no cache)
make build

# Rebuild with cache
make quick-build

# Stop containers
make stop

# Access notebook converter shell
make bash-nb

# Access Jekyll server shell
make bash-jekyll

# Restart Jekyll only
make restart-jekyll
```

## Content Structure

### Dual Directory System
The repository maintains two parallel post directories:
- `_posts/`: Legacy fastpages/Jekyll posts
- `posts/`: Current Quarto posts

When creating new content, add to the `posts/` directory.

### Post Formats Supported
1. **Jupyter Notebooks** (`.ipynb`) - Primary format for technical content
2. **Markdown** (`.md`) - For text-heavy posts
3. **Quarto Markdown** (`.qmd`) - For Quarto-specific features
4. **Word Documents** (`.docx`) - Legacy support via `_word/` directory

### Naming Convention
All posts must follow Jekyll's naming convention:
```
YYYY-MM-DD-title-of-post.{ipynb,md,qmd}
```
Example: `2025-11-22-hybrid-search-solr.ipynb`

## Working with .ipynb Files

### Editing Notebooks in Cursor/Claude

**IMPORTANT:** When editing Jupyter notebooks (`.ipynb` files), always use the `edit_notebook` tool, NOT the regular `search_replace` or `write` tools. The `edit_notebook` tool properly handles the JSON structure of notebook files.

**Key considerations:**
- Notebooks are JSON files with a specific structure
- Each cell has a `cell_type` (markdown, code, raw) and `cell_language` (python, markdown, etc.)
- Always specify the correct `cell_language` when editing
- Use `is_new_cell: true` to create new cells, `false` to edit existing ones
- Provide sufficient context in `old_string` to uniquely identify the cell content

### Quarto Markdown Formatting Requirements

Quarto is stricter about Markdown formatting than some other renderers. Follow these rules:

**Spacing Requirements:**
- **Always include blank lines** between different block elements (headings, paragraphs, lists)
- After a heading, add a blank line before the next content
- After a paragraph ending with a colon (`:`), add a blank line before a list
- After bold text (`**text**`), add a blank line before lists or other content

**Heading Best Practices:**
- Use proper Markdown headings (`##`, `###`, `####`) instead of bold text (`**text**`) for section headings
- Quarto renders headings better than bold text used as headings
- Example: Use `#### Stage 1: Title` instead of `**Stage 1: Title**`

**List Formatting:**
- Ensure blank lines before and after lists
- Use consistent indentation (2 spaces for nested lists)
- Don't mix list types without blank lines between them

**Code Blocks:**
- Use triple backticks with language identifiers: ` ```python `, ` ```markdown `, etc.
- Ensure blank lines before and after code blocks

### Notebook Front Matter (Metadata)

For Quarto notebooks, include YAML front matter in the first cell (cell 0):

```yaml
---
title: "Your Post Title"
date: "YYYY-MM-DD"
categories:
  - category1
  - category2
description: "Brief description of the post"
toc: true
---
```

**Location:** First cell (index 0) should be a markdown cell with the front matter.

### Cell Organization Best Practices

1. **First cell (index 0):** Always markdown with YAML front matter
2. **Subsequent cells:** Mix of markdown and code as needed
3. **Markdown cells:** Use for explanations, headings, and narrative
4. **Code cells:** Use for executable code, examples, and demonstrations

### Testing Notebook Rendering

After editing notebooks:
1. **Preview locally:** `quarto preview` to see how Quarto renders the notebook
2. **Check spacing:** Verify that all sections render with proper spacing
3. **Validate Markdown:** Ensure lists, headings, and code blocks display correctly
4. **Test code cells:** If code cells are executable, verify they run correctly

### Common Issues and Fixes

**Issue: Lists not rendering properly**
- **Fix:** Add blank lines before and after lists
- **Fix:** Ensure proper indentation (2 spaces per level)

**Issue: Headings not displaying correctly**
- **Fix:** Use proper Markdown headings (`##`, `###`) instead of bold text
- **Fix:** Add blank lines after headings

**Issue: Paragraphs and lists running together**
- **Fix:** Add blank lines between paragraphs and lists
- **Fix:** Add blank line after colons before lists

**Issue: Code blocks not rendering**
- **Fix:** Ensure triple backticks are on separate lines
- **Fix:** Add blank lines before and after code blocks

### Legacy fastpages Considerations

For notebooks in `_notebooks/` (legacy system):
- Avoid literal `\n` characters being inserted instead of actual newlines
- Test notebook output before committing
- Preview rendered output locally before publishing

### Dependency Management
Always use `uv` for installing Python dependencies:
```bash
uv pip install <package>
```

### Dependency Management
Always use `uv` for installing Python dependencies:
```bash
uv pip install <package>
```

## Architecture

### Content Processing Pipeline

**Quarto workflow (current):**
```
posts/*.{ipynb,md,qmd} → quarto render → _site/ → gh-pages branch
```

**fastpages workflow (legacy):**
```
_notebooks/*.ipynb → nb2post.py → _posts/*.md → Jekyll → _site/
_word/*.docx → word2post.py → _posts/*.md → Jekyll → _site/
```

### Key Python Scripts (_action_files/)
- `nb2post.py`: Converts Jupyter notebooks to Jekyll-compliant posts
- `fast_template.py`: Handles Jekyll naming conventions and front matter
- `word2post.py`: Converts Word documents to posts
- `action_entrypoint.sh`: Docker entrypoint for automated conversions

### Docker Services
The `docker-compose.yml` defines three services:
- `converter`: One-time conversion of notebooks/Word docs
- `watcher`: Auto-converts notebooks on file changes
- `jekyll`: Local development server (port 4000)

## Configuration Files

- `_config.yml`: Jekyll configuration (legacy)
- `_quarto.yml`: Quarto project configuration (current)
- `_action_files/settings.ini`: fastpages settings
- `Gemfile`: Ruby dependencies for Jekyll

## GitHub Actions Workflows

- `.github/workflows/publish.yml`: Quarto publishing to gh-pages (active)
- `.github/workflows/ci.yaml`: Legacy fastpages CI (retained)
- `.github/workflows/gh-page.yaml`: GitHub Pages build status check

## Common Development Tasks

### Creating a New Post
1. Create file in `posts/` directory with proper naming: `YYYY-MM-DD-title.ipynb`
2. For notebooks, include metadata/front matter if needed
3. Test locally: `quarto preview`
4. Publish: `quarto publish gh-pages --no-browser`

### Working with Jupyter Notebooks
- Store notebooks in `posts/` (current) or `_notebooks/` (legacy)
- Notebooks in `_notebooks/` are auto-converted to `_posts/` via fastpages
- Images are automatically handled and saved to appropriate directories

### Testing Changes Locally
For Quarto: `quarto preview`
For Jekyll: `make server` (opens on http://localhost:4000)

## Site Configuration

- **Site URL:** https://manisnesan.github.io/chrestotes/
- **Base URL:** /chrestotes
- **Theme:** Minima (Jekyll), Cosmo (Quarto)
- **Math Support:** KaTeX
- **Syntax Highlighting:** Rouge (Jekyll), Dracula theme

## Notes on Symbolic Links
The `_notebooks/` directory contains symbolic links to external Python files (`config.py`, `main.py`) from the Pyserini project. These are development utilities and not part of the core blog infrastructure.
