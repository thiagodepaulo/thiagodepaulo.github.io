# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Personal academic website of Thiago de Paulo Faleiros (Dept. of Computer Science, University of Brasília), published at https://thiagodepaulo.github.io via GitHub Pages. It is a Jekyll site built from the [Academic Pages](https://github.com/academicpages/academicpages.github.io) template (itself derived from the Minimal Mistakes theme). The theme is vendored into the repo — there is no remote theme gem.

Deployment is automatic: pushing to `master` triggers the GitHub Pages build. There are no tests or CI beyond that.

## Commands

Run everything inside the `mysite` conda environment (`conda activate mysite`, or prefix commands with `conda run -n mysite`). Gems are installed locally in `vendor/bundle` (set via `.bundle/config`; both are gitignored).

```bash
bundle config set --local path vendor/bundle   # once, before the first install
bundle install                      # Ruby deps (github-pages gem); delete Gemfile.lock if it errors
bundle exec jekyll build            # build into _site/
bundle exec jekyll serve -l -H localhost   # preview at http://localhost:4000 with live reload
npm install && npm run build:js     # only needed after editing assets/js/_main.js or plugins
```

`_config.yml` is not reloaded by `jekyll serve`; restart the server after editing it.

## Structure

- `_config.yml` — site metadata, author sidebar (avatar, links, email), collections, per-collection front-matter defaults, plugins. Locale is `pt-BR`.
- `_data/navigation.yml` — top nav menu links.
- `_pages/` — standalone pages. The ones with real content:
  - `about.md` — the homepage (`permalink: /`), includes a Google Calendar booking button.
  - `publications.md` — a hand-maintained Markdown list (Journal Articles / Book Chapters / Conference Papers, newest first) generated from the Lattes CV XML export. It does not use the `_publications/` collection.
- Collections (each item is a Markdown file with front matter; output at `/:collection/:path/`):
  - `_publications/`, `_talks/` and `_teaching/` (currently absent; recreate to add items), `_portfolio/`, `_posts/` (blog).
  - Talks use the `talk` layout; everything else defaults to `single` (see `defaults:` in `_config.yml`).
  - Collection listing pages live in `_pages/` (e.g. `talks.html`, `teaching.html`, `portfolio.html`, `year-archive.html`).
- `_layouts/`, `_includes/`, `_sass/`, `assets/` — theme code. `assets/js/main.min.js` is a build artifact of `npm run build:js`.
- `files/` — downloadable files (PDFs etc.), served at `/files/<name>`. `images/` — images, including the avatar `thiago.png`.
- `markdown_generator/` — Python scripts/notebooks that generate collection Markdown from `publications.tsv`, `talks.tsv`, or a BibTeX file.
- `talkmap.py` / `talkmap.ipynb` / `talkmap/` — builds a Leaflet map of talk locations (run from `_talks/`; needs `geopy`, `getorg`). Disabled via `talkmap_link: false`.

## State of the content

Much of the content is still the template's placeholder material: all items in `_publications/`, `_portfolio/`, `_posts/`, plus demo pages like `_pages/markdown.md` (no longer linked in the nav) and `_data/comments/`. The customized parts are `_config.yml` (author info), `_pages/about.md`, `_pages/cv.md` (written from the Lattes XML), `_pages/publications.md`, `_pages/talks.html` (YouTube playlist embeds, listed in a Liquid `playlists` string with hard-coded video counts), `_pages/projects.md` (research projects from the Lattes XML plus SIGAA research/extension pages), `_pages/teaching.html` (static HTML tables of UnB courses copied from SIGAA, siape 1821656), and `images/`. Never commit the Lattes export (`*.xml`, `CV_*.zip`) — it contains personal data (CPF, ID number, parents' names). Don't assume placeholder entries are real data.

## Conventions for new content

Collection items follow the template's front-matter shape, e.g. for a talk:

```yaml
---
title: "..."
collection: talks
type: "Talk"
permalink: /talks/YYYY-MM-DD-slug
venue: "..."
date: YYYY-MM-DD
location: "City, Country"
---
```

Publications additionally use `excerpt`, `venue`, `paperurl`, `citation`; teaching uses `type`, `venue`, `location`. Filenames are prefixed with the date (`YYYY-MM-DD-slug.md`). Use `{% include base_path %}` and `{{ base_path }}` for internal links in Liquid pages.
