# kaylairish.github.io

Kayla Irish's personal academic site, built on the [al-folio](https://github.com/alshedivat/al-folio) Jekyll theme. Deployed as a GitHub Pages user site.

## Local preview

```bash
bin/preview
```

Then open http://localhost:4000. Live-reloads on file changes; Ctrl+C to stop.

That script sets `PATH`, `GEM_HOME`, `LANG`, and `LC_ALL` before calling `bundle exec jekyll serve` — see "Environment gotchas" below for why. If you're not using the script (e.g. running `bundle exec jekyll build` directly), export those same vars first or the build will fail.

If `bin/preview` fails with a missing-gem error, run `bundle install` first (same env vars needed — just run `bin/preview`, or prefix any bundle command with the exports above).

## Environment gotchas

- **System Ruby is too old.** macOS ships Ruby 2.6, which can't resolve al-folio's gem dependencies cleanly. A modern Ruby is installed via Homebrew at `/usr/local/opt/ruby/bin` — this is what `bin/preview` puts first on `PATH`. It's also set up in `~/.bash_profile`, so a plain `bundle exec jekyll serve` works in any new terminal without extra setup.
- **`GEM_HOME` must be recomputed after changing `PATH`.** `~/.bashrc` sets `GEM_HOME` by shelling out to `ruby -e 'puts Gem.user_dir'` — but it does this _before_ the Homebrew Ruby is put on `PATH` (bash_profile sources bashrc first), so it locks in the system Ruby's gem path otherwise. Both `~/.bash_profile` and `bin/preview` re-run that same command after the `PATH` change to fix this.
- **Locale must be UTF-8.** Without `LANG`/`LC_ALL` set, Ruby defaults to US-ASCII and `bibtex-ruby` (used to render `_bibliography/papers.bib`) throws `invalid byte sequence in US-ASCII` during build. This is also set globally in `~/.bash_profile`.

## Deployment

Push to `main` → `.github/workflows/deploy.yml` builds the site with Ruby 3.3.5 in CI and force-pushes the built `_site/` to the `gh-pages` branch, which GitHub Pages serves at kaylairish.github.io. No manual deploy step. Don't commit a `Gemfile.lock` produced by a local build unless you're sure the resolved gem versions still work under CI's Ruby 3.3.5 — the local Homebrew Ruby version may resolve differently than CI's.

## Where content lives

| Page                                   | Source                                                                                                                |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| About / bio                            | `_pages/about.md`                                                                                                     |
| Publications                           | `_bibliography/papers.bib` (BibTeX; rendered automatically via jekyll-scholar)                                        |
| Projects                               | `_projects/*.md` (one file per project card)                                                                          |
| Teaching                               | `_pages/teaching.md`                                                                                                  |
| CV                                     | `_pages/cv.md` (points `cv_pdf:` at a file in `assets/pdf/`) — download-only by design, no `_data/cv.yml` (see below) |
| Site title/bio one-liner, social links | `_config.yml`, `_data/socials.yml`                                                                                    |

## Content provenance

Content was migrated from an old Google Sites page, then corrected/filled in against Kayla's actual LaTeX CV (source kept out of the built site — see below) once she supplied it. The CV source is the more authoritative of the two where they conflict (e.g. the Google Site had a stale advisor affiliation).

- `assets/img/prof_pic.png` — real photo (note: `.png`, not `.jpg` — `_pages/about.md`'s `profile.image` must match).
- `assets/pdf/Kayla_Irish_CV.pdf` — real CV PDF, referenced from `_pages/cv.md`'s `cv_pdf:` field.
- `me.tex` at the repo root is the LaTeX source for the CV PDF (kept for her reference, excluded from the Jekyll build via `_config.yml`'s `exclude:` list — it's not meant to be a page on the site). If bio details on `_pages/about.md` are ever out of sync, `me.tex` is the source of truth to re-sync against.
- The `/cv/` page deliberately has no itemized `_data/cv.yml` — Kayla chose to keep the CV as a PDF download only (al-folio supports an itemized web view via `_data/cv.yml`, but that file was intentionally removed here). Don't re-add it without asking.
- al-folio also supports a JSON-Resume-format CV (`assets/json/resume.json` + the `jekyll_get_json`/`jsonresume` config in `_config.yml`) as an alternative to `_data/cv.yml` — it's commented out in `_config.yml` and should stay that way, since when active it silently takes over the `/cv/` page instead of `_data/cv.yml` (that's what caused a debugging detour during the initial content migration).
