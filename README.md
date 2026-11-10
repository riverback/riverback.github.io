# Hangzhou He — Academic Homepage

A lightweight, hand-maintained Jekyll site hosted on GitHub Pages at
[hangzhouhe.com](https://hangzhouhe.com).

## Editing

- `_pages/about.md`: all homepage content, including publications. No publication database or generation step.
- `_config.yml`: identity, contact links, canonical domain, and Jekyll settings.
- `_data/navigation.yml`: links to homepage sections.
- `assets/css/main.css`: the entire responsive stylesheet; no CSS build step.
- `_layouts/` and `_includes/`: page structure, profile, navigation, and metadata.
- `images/`: personal images and favicon.
- `files/`: the public AAAI slide deck. Preserve its filename for existing links.
- `_pages/404.md`: custom error page.
- `CNAME`: GitHub Pages custom domain. Keep this file when deploying.

Publications remain ordinary Markdown inside three `markdown="1"` containers.
Keep the `publication-authors`, `publication-venue`, and `publication-links`
spans when adding an entry. The page uses Kramdown; use two trailing spaces
for line breaks. The explicit `experience` heading ID is used by navigation.

## Local preview

Install Ruby and Bundler, then run:

```sh
bundle install
bundle exec jekyll serve --config _config.yml,_config.dev.yml
```

Open http://localhost:4000. For a production check:

```sh
bundle exec jekyll build
```

GitHub Pages builds the site from source. There is no Node dependency,
JavaScript bundle, remote font, or external stylesheet. Jekyll plugins retain
`sitemap.xml` and redirects from `/about/` and `/about.html` to the homepage.

## Credits

The site originated from Academic Pages / Minimal Mistakes. Its former demo
pages and unused theme assets have been removed. The original MIT license is
retained in `LICENSE`.
