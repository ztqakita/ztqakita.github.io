# Theme Customization Guide

This site uses the al-folio Jekyll theme. Most content changes are data or Markdown edits; you usually do not need to touch layout files.

## Common Commands

Use Docker for local preview if your local Ruby environment is not ready:

```bash
docker compose -f docker-compose-slim.yml up
```

Then open `http://localhost:8080`.

Automatic deployment is already configured. After editing content:

```bash
git add <changed-files>
git commit -m "Update site content"
git push origin source
```

GitHub Actions builds the site and publishes the generated files to the `gh-pages` branch. GitHub Pages is configured to serve from `gh-pages /`.

## Main Identity

Edit `_config.yml` for site-wide settings:

- `title`: browser/site title
- `first_name`, `middle_name`, `last_name`: displayed name
- `description`: SEO description
- `keywords`: SEO keywords
- `url`: should stay `https://ztqakita.github.io`
- `baseurl`: should stay blank for this personal GitHub Pages site
- `footer_text`: footer credit text

## Home Page

Edit `_pages/about.md`.

Useful fields:

- `subtitle`: short line under the title
- `profile.image`: profile image filename from `assets/img/`
- `profile.more_info`: short HTML lines under the profile image
- `announcements.enabled`: show or hide news
- `latest_posts.enabled`: show or hide latest blog posts
- page body: the homepage biography text

Profile images live in `assets/img/`. The current image is `assets/img/ztq.png`.

## CV Page

Edit `_data/cv.yml` for the rendered CV page at `/cv/`.

The PDF download button is configured in `_pages/cv.md`:

```yaml
cv_pdf: /assets/pdf/resume.pdf
cv_format: rendercv
```

To update the PDF itself, replace `assets/pdf/resume.pdf`.

The CV data file supports sections such as:

- `Research Interests`
- `Education`
- `Publications`
- `Projects`
- `Academic Services`
- `Books`
- `Honors and Awards`
- `Highlighted Conferences and Summer Schools`
- `Teaching`
- `Skills`
- `Languages`

For sections that are not specially handled by the theme, use simple bullet entries:

```yaml
Some Section:
  - bullet: "Your item here."
```

## Navigation

Each page in `_pages/` controls whether it appears in the top navigation:

```yaml
nav: true
nav_order: 3
```

Set `nav: false` to hide a page from the navbar. The following have already been hidden:

- `_pages/repositories.md`
- `_pages/teaching.md`
- `_pages/profiles.md` (`/people/`)

The pages can stay in the repository; hidden pages do not clutter the navbar.

## Blog Posts

Blog posts live in `_posts/`.

File names follow:

```text
YYYY-MM-DD-title.md
```

Front matter usually looks like:

```yaml
---
layout: post
title: "Post title"
date: 2026-05-07
description: "Short summary"
tags: research notes
categories: blog
---
```

Images used in posts should go under `assets/img/` or a clear subfolder such as `assets/img/posts/`.

## Projects

Project cards live in `_projects/`.

To add a new project, copy one existing `_projects/*.md` file and edit its front matter. To hide the projects page entirely, set `nav: false` in `_pages/projects.md`.

## News

News items live in `_news/`.

The home page controls whether they appear:

```yaml
announcements:
  enabled: true
```

Set it to `false` in `_pages/about.md` if you do not want a news block on the homepage.

## Publications Page

The standalone publications page uses `_bibliography/papers.bib`. The CV page uses `_data/cv.yml`.

If you want one source of truth later, keep `_data/cv.yml` as the readable CV source and update `_bibliography/papers.bib` only when you want the dedicated `/publications/` page to show full BibTeX-style publication cards.

## Preserved Old Site

The previous Hugo site is archived in `_legacy_hugo_site/`.

Useful old content:

- `_legacy_hugo_site/content/`: old posts
- `_legacy_hugo_site/static/`: old files and images
- `_legacy_hugo_site/data/`: old profile, project, and experience data

The archive is excluded from Jekyll builds by `_config.yml`, so it will not be published as part of the new site.
