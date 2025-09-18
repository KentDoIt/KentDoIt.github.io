# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Hexo static blog** using the **NexT theme**, deployed on **GitHub Pages**. The blog is written in Traditional Chinese (zh-TW) and focuses on JavaScript and web development topics.

## Common Commands

```bash
# Development server
npm run server

# Build the site
npm run build

# Clean generated files
npm run clean

# Deploy to GitHub Pages
npm run deploy

# Security audit
npm run audit
npm run audit-fix

# Create new post
hexo new post "Title"

# Create new page
hexo new page "page-name"
```

## Architecture

### Core Configuration
- **Main config**: `_config.yml` - Hexo core settings, plugins, and deployment
- **Theme config**: `themes/hexo-theme-next/_config.yml` - NexT theme customization
- **Posts**: `source/_posts/` - Blog articles in Markdown format
- **Generated**: `public/` - Built static files (excluded from git)

### Key Features Enabled
- **Permalink structure**: `:category/:abbrlink/` using CRC32 algorithm
- **Reading time estimation**: via `hexo-symbols-count-time`
- **Search functionality**: via `hexo-generator-searchdb`
- **SEO optimization**: sitemap, feed generation, meta tags
- **Syntax highlighting**: Hexo built-in highlighter (not Prism)

### Plugin Dependencies
The project uses these key Hexo plugins:
- `hexo-abbrlink` - Short URL generation
- `hexo-deployer-git` - GitHub Pages deployment
- `hexo-generator-*` - Various content generators (archive, category, feed, sitemap, etc.)
- `hexo-renderer-*` - Template engines (EJS, Marked, Stylus)
- `hexo-symbols-count-time` - Reading time calculation

### Security Considerations
- Package versions are **pinned** (no `^` or `~`) for supply chain security
- `hexo-generator-baidu-sitemap` was **removed** due to security vulnerabilities
- Regular `npm audit` checks are recommended

### Deployment
- **Target**: GitHub Pages (`git@github.com:KentDoIt/KentDoIt.github.io.git`)
- **Branch**: master
- **Domain**: https://kentdoit.github.io/
- **Process**: `npm run deploy` builds and pushes to GitHub

## Content Structure

### Post Front Matter
Posts use this typical structure:
```yaml
---
title: Post Title
categories: JavaScript
tags:
  - JavaScript
  - Hexo
description: Brief description
abbrlink: 1234567890
date: YYYY-MM-DD HH:mm:ss
---
```

### Category Mapping
- `Hexo` → `hexo`
- `JavaScript` → `javascript`

### Tag Mapping
- `Hexo 部落格` → `hexo blog`

## Backup Recommendations

**Critical files to backup regularly:**
- `_config.yml` - Main configuration
- `source/` directory - All posts and pages
- `themes/hexo-theme-next/_config.yml` - Theme settings
- `package.json` - Dependencies with pinned versions

Create backups before major changes or plugin updates.

## Development Notes

- **Language**: Traditional Chinese (zh-TW)
- **Post naming**: `YYYY-MM-DD-title.md` format
- **URL structure**: Uses abbrlink for SEO-friendly URLs
- **Theme**: NexT theme with custom configurations
- **Build output**: Files in `public/` are auto-generated, don't edit directly