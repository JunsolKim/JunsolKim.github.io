# Editing this website

This site is built with [Jekyll](https://jekyllrb.com/). The idea is simple:
**content lives in YAML files**, **layout lives in HTML templates**. For most
day-to-day edits (adding a news item, publishing a new paper) you only need
to touch one YAML file — you never need to look at HTML.

---

## Quick reference: where does each thing live?

| I want to change…                              | Edit this file |
|------------------------------------------------|----------------|
| Add/remove/reorder a news item                 | `_data/news.yml` |
| Add a new paper, or update a paper's venue     | `_data/publications.yml` |
| Which papers appear in the homepage shelf      | `_data/featured.yml` |
| Which papers appear in the homepage list       | `_data/homepage_publications.yml` |
| Research theme text, or which papers appear under a theme | `_data/themes.yml` |
| My name, email, CV link, portrait photo        | `_config.yml` → `author` |
| Hero text on the homepage                      | `_config.yml` → `hero` |
| Nav bar links (Home, Research, …)              | `_config.yml` → `nav` |
| Footer social links                            | `_config.yml` → `footer.social` |
| Publications page intro + section grouping     | `_config.yml` → `publications_page` |
| Research page intro                            | `_config.yml` → `research_page` |
| Visual design (fonts, colors, spacing)         | `assets/css/style.css` |

> When you edit `_config.yml`, you must **restart** your local `jekyll serve`
> (Ctrl-C then rerun it). Data files in `_data/` hot-reload automatically.

---

## Common tasks

### Add a news item

Open `_data/news.yml` and add a new block at the top (newest first):

```yaml
- date: "2026.09"
  text: >
    Presenting <em>My paper title</em> at
    <a href="https://example.com">Venue 2026</a>, City.
```

HTML works inside `text`: `<em>…</em>` for italics, `<a href="…">…</a>` for
links, `&middot;` for a bullet dot. Save — done.

### Add a new publication

1. Open `_data/publications.yml` and add a new block:

    ```yaml
    - id: my-new-paper                    # unique slug; used in other files
      title: "My new paper title"
      url: "https://arxiv.org/abs/…"     # optional — omit if not available
      authors: "<em>Junsol Kim</em>, Co-Author"
      venue: "Full venue name (ICLR)"     # shown on the Publications page
      venue_short: "ICLR"                 # optional; shown on home + research
      badge: "Oral"                       # optional small pill after venue
      year: "2026"
      category: peer_reviewed             # peer_reviewed | under_review | working_paper
    ```

2. (Optional) To put the paper in the homepage shelf, add a `shelf:` block to it:

    ```yaml
      shelf:
        image: "assets/img/papers/my_new_paper.jpg"
        venue_short: "ICLR"
        year_short: "2026"
    ```

    Then add `- my-new-paper` to `_data/featured.yml` in the position you want.

3. (Optional) To put it in the homepage Publications list, add `- my-new-paper`
   to `_data/homepage_publications.yml`.

4. (Optional) To put it under a research theme, add `- my-new-paper` to the
   relevant theme's `papers:` list in `_data/themes.yml`.

### Remove a paper from the homepage shelf (but keep it on the Publications page)

Just delete its id from `_data/featured.yml`. The paper's entry in
`_data/publications.yml` stays untouched.

### Change my name / email / CV link

Open `_config.yml` and edit the `author:` block:

```yaml
author:
  name: "Your Name"
  email: "you@example.com"
  cv_pdf: "junsol_kim_cv.pdf"
  portrait: "assets/img/junsol.jpg"
  scholar: "https://scholar.google.com/..."
  github: "https://github.com/..."
  linkedin: "https://www.linkedin.com/..."
```

### Add a new page (e.g. a Teaching page)

1. Create `teaching.html` at the root with this front matter:

    ```yaml
    ---
    layout: default
    page_id: teaching
    page_title: "Teaching · Junsol Kim"
    ---

    <section class="section">
      <div class="container">
        <h1>Teaching</h1>
        <p>…</p>
      </div>
    </section>
    ```

2. Add it to the nav in `_config.yml`:

    ```yaml
    nav:
      - text: "Teaching"
        url: "teaching.html"
        id: "teaching"     # must match the page_id above to highlight it
    ```

---

## Running the site locally (optional)

You only need this if you want to preview changes before pushing to GitHub.
Otherwise, push to GitHub and GitHub Pages builds the site automatically.

### One-time setup

You need Ruby (≥ 2.7) and Bundler. On macOS:

```bash
# Ruby is pre-installed on macOS, but using rbenv/homebrew is cleaner.
brew install rbenv
rbenv install 3.2.4
rbenv global 3.2.4

# Bundler (gem-level package manager)
gem install bundler
```

On Ubuntu/Debian:

```bash
sudo apt install ruby-full build-essential zlib1g-dev
gem install --user-install bundler
```

### Install the site's dependencies

From this folder:

```bash
bundle config set --local path vendor/bundle
bundle install
```

This installs Jekyll + plugins into `vendor/bundle/` (already gitignored).

### Preview

```bash
bundle exec jekyll serve --livereload
```

Then open <http://localhost:4000>. The site rebuilds automatically when you
edit data files. (For `_config.yml` changes, Ctrl-C and rerun.)

### Deploy

Just push to GitHub. `JunsolKim.github.io` is a GitHub Pages user site, so
GitHub builds and serves it automatically on every push to `main`.

```bash
git add .
git commit -m "Update news / publications / ..."
git push
```

---

## Project structure

```
JunsolKim.github.io/
├── _config.yml               ← site-wide settings, author, hero, nav, footer
├── _data/                    ← content you edit most often
│   ├── news.yml
│   ├── publications.yml      ← single source of truth for all papers
│   ├── featured.yml          ← homepage shelf (list of paper ids)
│   ├── homepage_publications.yml  ← homepage Publications list
│   └── themes.yml            ← research themes
├── _includes/                ← reusable HTML fragments (edit rarely)
│   ├── head.html
│   ├── header.html
│   ├── footer.html
│   └── publication.html      ← renders one paper; used everywhere
├── _layouts/
│   └── default.html          ← page skeleton (head + header + content + footer)
├── index.html                ← homepage (uses layout + data)
├── research.html             ← research page
├── publications.html         ← publications page
├── assets/                   ← CSS, images, fonts (unchanged by Jekyll setup)
├── junsol_kim_cv.pdf
├── Gemfile                   ← Jekyll dependency declaration
└── EDITING.md                ← this guide
```

Files you'll likely **never** touch: `_layouts/`, `_includes/`, `Gemfile`,
and the HTML pages themselves (unless adding new pages).

---

## Troubleshooting

**The site builds but a paper doesn't show up.**
- Double-check its `category:` value matches one of the categories in
  `_config.yml → publications_page.sections`.
- If it's supposed to be on the shelf, make sure it has a `shelf:` block
  *and* its id is listed in `_data/featured.yml`.

**YAML error when building.**
Usually it's a quoting issue. Wrap strings containing `:` or `#` in quotes:

```yaml
title: "Title with: a colon"
```

**A paper's title appears twice.**
Jekyll's `where` filter matches by exact category string. If you renamed a
category but kept the old value somewhere, you might get overlap. Search
`_data/publications.yml` for category values.

**GitHub Pages build fails after a push.**
GitHub will email you. Most common cause: a YAML syntax error. Pull the
email, fix the file, push again.
