# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal blog built with Jekyll 4.4.1, running on Ruby 3.3. The site is a static blog featuring articles on software engineering, leadership, machine learning, and product design. The main branch is `source`, and the site is published to g9labs.com.

**Note**: The site was recently upgraded from Jekyll 3.9 to Jekyll 4.4.1, which includes a migration from LibSass to Dart Sass. Some deprecation warnings about `@import` rules and color functions are expected but don't affect functionality.

## Development Commands

### Local Development
```bash
# Install dependencies
bundle install

# Serve the site locally with live reload
bundle exec jekyll serve

# Build the site (output to _site/)
bundle exec jekyll build

# Clean generated files
bundle exec jekyll clean
```

### Creating Content
```bash
# Create a new draft post
bundle exec jekyll draft "Post Title"

# Create a new post
bundle exec jekyll post "Post Title"

# Publish a draft (moves from _drafts to _posts with date)
bundle exec jekyll publish _drafts/post-name.md

# Create a new page
bundle exec jekyll page "Page Name"
```

## Architecture

### Jekyll Configuration
- **Config**: `_config.yml` contains site metadata, plugin configuration, and build settings
- **Ruby Version**: 3.3.0 (specified in `.ruby-version`)
- **Plugins**: jekyll-feed, jekyll-archives, jekyll-paginate-v2, jekyll-compose, jekyll-admin
- **Markdown**: Kramdown with Rouge syntax highlighting
- **Theme**: Minima (with heavy customization)

### Content Structure
- **Posts**: Markdown files in `_posts/` with YAML frontmatter, named `YYYY-MM-DD-title.md`
- **Drafts**: Work-in-progress posts in `_drafts/` (no date in filename)
- **Layouts**: `_layouts/` contains page templates (default, post, page, category)
- **Includes**: `_includes/` contains reusable partial templates
- **Data**: `_data/projects.yml` contains structured data for projects page

### Styling Architecture
- **Main stylesheet entry**: `stylesheets/main.scss` imports from `_sass/main.scss`
- **Sass organization**:
  - `_sass/main.scss` - Main stylesheet, imports Bourbon and component files
  - `_sass/_variables.scss` - Design tokens (colors, fonts, spacing)
  - `_sass/_typography.scss` - Typography settings
  - `_sass/_responsive.scss` - Media query breakpoints
  - `_sass/bourbon/` - Bourbon Sass mixins library
  - `_sass/components/` - Component-specific styles (paginator, projects, newsletter)
- **Syntax highlighting**: Rouge with custom styles in `stylesheets/rouge-code-styles.css`

### Layout Hierarchy
- `default.html` - Base layout with header, footer, meta tags, and navigation
- `post.html` - Extends default, adds article include and optional newsletter signup
- `page.html` - For static pages
- `category.html` - Archive page for category listings

### Key Features
- **Pagination**: jekyll-paginate-v2 handles post pagination (20 per page)
- **Archives**: Posts organized by year/month on index, categories via jekyll-archives
- **Newsletter**: Convertkit signup form included on post pages (when `newsletter_enabled: true`)
- **Analytics**: Google Analytics tracking (UA-2330913-9)
- **Social**: Open Graph meta tags for social sharing
- **Images**: jQuery automatically centers images and adds captions from title attributes

### Important Files
- `_config.yml` - Site configuration (requires server restart when changed)
- `index.html` - Homepage with paginated post listing organized by year/month
- `atom.xml` - RSS feed template
- `404.html` - Custom 404 page

## Content Authoring Notes

### Post Frontmatter
Posts require YAML frontmatter with:
```yaml
---
layout: post
title: "Post Title"
date: YYYY-MM-DD HH:MM:SS -TIMEZONE
categories:
- Category 1
- Category 2
---
```

### Special Styling
- Use `<h2 class="intro">` for post introduction paragraphs (larger italic text)
- Images automatically get captions from their `title` attribute
- Code blocks support line numbers via Rouge configuration
