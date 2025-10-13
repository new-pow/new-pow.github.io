# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Handmade Blog is a lightweight static blog generator built with TypeScript. It converts Markdown files to HTML using templates and publishes to GitHub Pages.

## Key Commands

### Development
```bash
npm install              # Install dependencies
npm start                # Start local dev server at http://localhost:8080/
npm run watch            # Watch for changes in templates, styles, and articles
```

### Content Publishing
```bash
npm run publish article          # Publish all articles
npm run publish article [id]     # Publish specific article by ID
npm run publish work             # Publish all works (portfolio items)
npm run publish page             # Publish all pages (index, about)
```

### Build & Deploy
```bash
npm run build            # Build all files to dist/ directory
npm run deploy           # Build and deploy to GitHub Pages (gh-pages branch)
```

## Architecture

### Content Processing Pipeline
1. **Markdown → HTML**: `ArticlePublisher` and `WorkPublisher` convert markdown files with front matter to HTML using MarkdownIt
2. **Template Rendering**: EJS templates in `app/templates/` are rendered with article/work data
3. **Build Process**: `tools/build.sh` orchestrates publishing, file copying, and minification
4. **Deployment**: `tools/deploy.sh` uses git subtree to push `dist/` to `gh-pages` branch

### Directory Structure
- `_articles/` - Markdown blog posts with front matter (id, title, date, tags)
- `_works/` - Markdown portfolio items
- `app/templates/` - EJS templates for pages
- `app/styles/` - CSS source files
- `app/public/` - Generated HTML (intermediate output, do not edit directly)
- `dist/` - Final build output (minified, do not edit directly)
- `services/` - Core publishing logic (ArticlePublisher, WorkPublisher, PagePublisher)
- `tools/` - Build scripts and utilities

### Core Services
- **ArticlePublisher**: Reads markdown from `_articles/`, converts to HTML using MarkdownIt with plugins (KaTeX, footnotes, code highlighting, mermaid), and renders with article template
- **WorkPublisher**: Similar to ArticlePublisher but for portfolio works
- **PagePublisher**: Generates index, about, articles list, and works list pages

### Markdown Features
The MarkdownIt configuration supports:
- Code syntax highlighting (highlight.js)
- KaTeX math equations
- Footnotes
- Table of contents (auto-generated)
- Mermaid diagrams
- Custom toggle containers: `::: toggle(Title)\ncontent\n:::`
- Lazy image loading
- Anchors for headings

### Front Matter Format
Articles require front matter:
```yaml
---
id: 0
title: "Article Title"
subtitle: "Optional subtitle"
date: 2023-01-01
tags: tag1, tag2
---
```

## Development Workflow

1. **Live Development**: Run `npm run watch` in one terminal and `npm start` in another. Changes to templates, styles, and articles auto-rebuild.
2. **Writing Content**: Create markdown files in `_articles/` or `_works/`, then run `npm run publish article|work`
3. **Template Changes**: Edit EJS files in `app/templates/`, then run `npm run build`
4. **Deployment**: Ensure clean working directory, then `npm run deploy` pushes to GitHub Pages

## Important Notes

- Never edit files in `app/public/` or `dist/` directly - they are auto-generated
- The build script expects `ts-node` to run TypeScript files
- Deploy script requires clean git working directory
- Custom domain support: Create `CNAME` file in root (build script copies it to dist/)
