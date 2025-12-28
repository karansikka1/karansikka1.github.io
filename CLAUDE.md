# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a personal academic website for Dr. Karan Sikka, Research Scientist at Meta, built using the **al-folio** Jekyll theme. The site is deployed to GitHub Pages and features a project portfolio, publications page, news announcements, and more.

- **Base theme**: al-folio (a Jekyll theme for academics)
- **Static site generator**: Jekyll
- **Deployment**: GitHub Pages (automated via GitHub Actions)
- **Main branch**: `master`
- **Owner**: Dr. Karan Sikka (karan.sikka1@gmail.com)
- **Google Scholar**: Kn-2t0oAAAAJ

## Development Commands

### Local Development (Docker - Recommended)

```bash
# Start local development server
docker-compose up

# Access site at http://localhost:8080
# Live reload is enabled on port 35729
```

### Local Development (Native)

```bash
# Install dependencies
bundle install

# Serve site locally with live reload
bundle exec jekyll serve --livereload

# Build site for production
export JEKYLL_ENV=production
bundle exec jekyll build --lsi
```

### Code Quality

```bash
# Format code with Prettier
npx prettier --write .

# Check formatting
npx prettier --check .
```

### Testing

The site uses GitHub Actions for automated testing:
- Prettier formatting checks
- Broken link detection (lychee)
- Lighthouse performance testing
- Accessibility testing (Axe - manual)

## Architecture

### Directory Structure

- **`_config.yml`**: Main Jekyll configuration. Contains site metadata, theme settings, plugin configuration, and deployment settings.
- **`_layouts/`**: Page templates (about, post, page, distill, bib, profiles, etc.)
- **`_includes/`**: Reusable HTML/Liquid components included in layouts
- **`_pages/`**: Static pages (about, publications, misc, patents, dropdown menu)
- **`_posts/`**: Blog posts in Markdown format with YAML front matter (currently empty - blog functionality not in use)
- **`_projects/`**: Project pages organized as a Jekyll collection (3 active projects)
- **`_news/`**: News/announcements (displayed on homepage - currently enabled)
- **`_bibliography/`**: BibTeX files for publications (managed by jekyll-scholar)
- **`_sass/`**: SCSS stylesheets for theming
- **`_plugins/`**: Custom Jekyll plugins (Ruby)
- **`assets/`**: Static assets (images, PDFs, JSON, CSS, JS)
- **`_site/`**: Generated static site (gitignored, created during build)

### Key Jekyll Plugins

- **jekyll-scholar**: Manages publications from BibTeX files
- **jekyll-paginate-v2**: Pagination for blog posts
- **jekyll-archives**: Creates archive pages by year, tag, category
- **jekyll-jupyter-notebook**: Embeds Jupyter notebooks in posts
- **jekyll-imagemagick**: Responsive image generation
- **jekyll-minifier**: Minifies HTML/CSS/JS for production
- **jekyll-toc**: Table of contents generation
- **jemoji**: GitHub-style emoji support

### Custom Ruby Plugins (`_plugins/`)

- **cache-bust.rb**: Adds cache-busting query strings to assets
- **external-posts.rb**: Fetches external blog posts from RSS feeds
- **google-scholar-citations.rb**: Fetches citation counts from Google Scholar
- **download-3rd-party.rb**: Downloads third-party libraries for offline use

### Collections

Jekyll collections are defined in `_config.yml`:
- **news**: Announcements displayed on homepage (enabled in `_pages/about.md`)
- **projects**: Portfolio projects with categories (accessible via dropdown menu at /projects/)

### Content Front Matter

**News announcements** (`_news/`):
```yaml
---
layout: post
date: 2024-01-15 12:00:00-0000
inline: true
related_posts: false
---

Your announcement text here.
```

**Projects** (`_projects/`):
```yaml
---
layout: page
title: Project Name
description: Short description
img: assets/img/project.jpg  # Thumbnail
importance: 1  # Display order
category: work  # or 'fun'
related_publications: true  # Show related papers
---
```

### Theming

- Theme colors are defined in `_sass/_themes.scss` and `_sass/_variables.scss`
- The site supports light/dark mode (configured in `_config.yml`)
- Current theme color is customizable via the `--global-theme-color` CSS variable

### Third-Party Libraries

Third-party library versions and integrity hashes are centrally managed in `_config.yml` under `third_party_libraries`. This includes:
- MathJax (math typesetting)
- Mermaid (diagrams)
- Chart.js (charts)
- Vega/Vega-Lite (visualizations)
- Bootstrap, jQuery, etc.

## Deployment

### Automated Deployment (GitHub Actions)

The site automatically deploys to GitHub Pages when changes are pushed to `master`. The workflow:

1. Checkout code
2. Setup Ruby 3.2.2
3. Install dependencies (including Jupyter for notebook support)
4. Build site with `bundle exec jekyll build --lsi`
5. Run PurgeCSS to remove unused styles
6. Deploy to `gh-pages` branch

See `.github/workflows/deploy.yml` for details.

### Build Process

The production build uses:
- `JEKYLL_ENV=production` environment variable
- `--lsi` flag forLatentSemanticIndexing (better related posts)
- PurgeCSS for CSS optimization

### GitHub Pages Configuration

- **Source**: Deploy from `gh-pages` branch
- **URL**: Configured in `_config.yml` (url and baseurl fields)
- **Workflow permissions**: Requires "Read and write permissions" in repo settings

## Working with Content

### Current Site Structure

The site currently features:
- **Homepage**: About section with news announcements and selected publications
- **Navigation Dropdown Menu** (submenus):
  - Publications (/publications/)
  - Projects (/projects/)
  - Misc (/other/)
  - Patents (/patents/)

**Active Projects** (3 total):
1. **LLM Consistency & Hallucination Mitigation** (`llm_consistency.md`) - importance: 1
2. **Multimodal Learning** (`multimodal_learning.md`) - importance: 2
3. **Robotics with Large Language Models** (`robotics_llms.md`) - importance: 3

**Selected Publications** (3 papers marked with `selected={true}`, ordered by recency):
1. DRESS (CVPR 2024) - Vision-language model alignment
2. SayNav (PNAS 2024) - LLMs for robot navigation
3. Measuring Chain-of-Thought Reasoning (NAACL 2024) - LLM consistency

### Adding a News Announcement

1. Create a new file in `_news/` with format: `announcement_name.md`
2. Add YAML front matter with layout: post, date, inline: true
3. Write brief announcement text in Markdown
4. Announcements appear on homepage (limited to 5 most recent by default)

### Adding a Project

1. Create a new file in `_projects/` (e.g., `my_project.md`)
2. Add YAML front matter with layout, title, description, img, importance, category
3. Write project description in Markdown
4. Add related publications by setting `related_publications: true` and referencing them in your BibTeX file

### Managing Publications

1. Edit `_bibliography/papers.bib` to add BibTeX entries
2. Customize fields: add `pdf`, `code`, `website`, `slides`, `poster`, `video`, `blog`, `abstract`, `abbr`, etc.
3. Mark important papers with `selected={true}` to display them on the homepage
4. Publications are automatically rendered on the publications page
5. Configuration is in `_config.yml` under `scholar` section (currently set to: last_name: Sikka, first_name: Karan)

**BibTeX Field Reference**:
- `selected={true}`: Display on homepage as selected publication
- `abbr`: Venue abbreviation badge (e.g., CVPR, ICCV, NAACL)
- `abstract`: Paper abstract (displayed on click)
- `pdf`: Link to PDF file
- `code`: Link to code repository
- `website`: Project website
- `video`: Video presentation
- `award`: Award received (e.g., "Best Paper Award")
- `honor`: Alternative field for awards

## Configuration Files

- **`_config.yml`**: Main site configuration (metadata, plugins, collections, scholar settings)
- **`package.json`**: NPM dependencies (Prettier for code formatting)
- **`Gemfile`**: Ruby gem dependencies
- **`.prettierrc`**: Prettier formatting rules (liquid plugin, 150 char line width)
- **`purgecss.config.js`**: PurgeCSS configuration for removing unused CSS
- **`docker-compose.yml`**: Docker setup for local development

## Site-Specific Configuration

### Contact Information
- Email: `karan.sikka1@gmail.com` (configured in `_config.yml`)
- GitHub: `karansikka1`
- LinkedIn: `ksikka`
- Google Scholar: `Kn-2t0oAAAAJ`

### Homepage Settings (`_pages/about.md`)
- `news: true` - News announcements enabled
- `selected_papers: true` - Selected publications displayed
- `social: true` - Social media icons enabled

### Navigation Structure
The site uses a dropdown menu system defined in `_pages/dropdown.md`:
- Menu title: "submenus"
- Menu order: 8
- Contains: Publications, Projects, Misc, Patents

## Important Notes

- The site uses Liquid templating language (Jekyll's template engine)
- Images are automatically converted to responsive WebP format via jekyll-imagemagick
- Math is supported via MathJax (wrap in `$$...$$` or `\\[...\\]`)
- Code syntax highlighting uses Rouge (supports 100+ languages)
- The theme supports distill.pub-style posts (use `layout: distill`)
- Blog functionality is available but not currently in use (no posts in `_posts/`)
- CV page has been removed - CV PDF is linked from homepage bio
