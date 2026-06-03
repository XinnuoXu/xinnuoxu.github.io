# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal academic website (Xinnuo Xu) built on the **academicpages** Jekyll template, itself a fork of the Minimal Mistakes theme. It is served via GitHub Pages from `https://xinnuoxu.github.io`. There is no application code to compile — content lives in markdown collections and the layout/styling comes from the vendored theme.

## Local development

```bash
bundle install                      # install Ruby deps (delete Gemfile.lock first if it errors)
bundle exec jekyll liveserve        # build + serve at localhost:4000 with live reload (hawkins gem)
bundle exec jekyll serve            # plain serve, no live reload
```

- `_config.dev.yml` overrides config for local runs (sets `url: localhost:4000`, disables analytics). Jekyll's `liveserve`/`serve` does **not** pick it up automatically — pass it explicitly if needed: `bundle exec jekyll serve --config _config.yml,_config.dev.yml`.
- **`_config.yml` is not hot-reloaded.** Restart the server after editing it.
- GitHub Pages builds the site automatically on push to `master`; no manual deploy step.

## Content model

Content is Jekyll **collections**, one directory per type, each file a markdown doc with YAML front matter. Output URLs and default layouts are wired in `_config.yml` under `collections:` and `defaults:`.

- `_publications/` — papers. Front matter drives `_pages/publications.md`. See `_publications/xu2019unsupervised.md` for the expected fields (`title`, `venue`, `citation`, `paper_link`, `doi`, `collection: publications`).
- `_talks/`, `_teaching/`, `_portfolio/` — same pattern, other collections.
- `_posts/` — blog posts, named `YYYY-MM-DD-title.md`.
- `_pages/` — standalone pages (about, cv, 404, archive views). The `.html` files here (e.g. `publications.html`, `archive` views) are list/index templates that loop over collections.
- `_drafts/` — unpublished drafts.

Filenames in publications/talks conventionally encode the citation key / date (e.g. `xu2019unsupervised.md`, `2014-03-01-talk-3.md`).

## Navigation and config

- `_data/navigation.yml` — the top menu. Most links are commented out; only Publications and CV are active. Uncomment entries here to expose a collection in the nav.
- `_config.yml` — site-wide settings: author profile/sidebar (`author:`), social links, analytics, collection definitions, and per-collection `defaults` (layout, author_profile, share, comments). Edit `author:` to change the sidebar bio/links.
- `_data/ui-text.yml`, `_data/authors.yml` — theme UI strings and multi-author metadata.

## Theme internals (rarely edited)

- `_layouts/` — page templates (`single.html`, `talk.html`, `archive.html`, etc.). Front matter `layout:` selects one.
- `_includes/` — partials composing the layouts.
- `_sass/` + `assets/css/` — styles. JS lives in `assets/js/`; rebuild the bundle with `npm run build:js` (uglifies into `assets/js/main.min.js`). Node is only needed for this.

## Bulk-generating content from spreadsheets

`markdown_generator/` converts TSV files into collection markdown. Run the python scripts (or the equivalent `.ipynb` notebooks) from inside that directory:

```bash
cd markdown_generator
python publications.py    # reads publications.tsv -> writes ../_publications/*.md
python talks.py           # reads talks.tsv        -> writes ../_talks/*.md
```

`pubsFromBib.py` generates publication markdown from a BibTeX file instead of TSV.

## Talk map

`talkmap.py` / `talkmap.ipynb` geocode talk locations and build an interactive map into `talkmap/`, surfaced by `_pages/talkmap.html`. The talk map nav link is gated by `talkmap_link` in `_config.yml` (currently false).

## Notes

- `_site/`, `vendor/`, and `.bundle` are git-ignored build/dependency output — do not edit or commit them.
- If a security/dependency warning appears, the template's guidance is to delete `Gemfile.lock` and reinstall.
