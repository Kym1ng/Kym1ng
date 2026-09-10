# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A personal portfolio/resume site for Haoming Guo (Kimi G), served via GitHub Pages at
https://kym1ng.github.io/Kym1ng/index.html. It is plain static HTML — there is no build system,
package manager, linter, or test suite in this repo.

## Working with the site

- **Preview locally**: just open `index.html` in a browser, or run a static server from the repo
  root, e.g. `python3 -m http.server` then visit `http://localhost:8000/index.html`.
- **No build/lint/test commands exist** — don't invent `npm run` / lint / test steps for this repo.
- **Deployment**: GitHub Pages serves directly from the `main` branch root per repository settings.
  There is no deploy workflow in `.github/workflows/` — pushing/merging to `main` is what publishes
  the live site.
- **Styling**: hand-written CSS in a single `<style>` block in `index.html` — no framework, no CDN
  dependency (Tailwind/Font Awesome/Google Fonts were removed in the single-page redesign). Keep it
  that way; don't reintroduce a CDN script/link unless asked.

## Structure

- `index.html` — the entire site: one scrolling page with hero, education, skills, experience, and
  a fully enriched projects section (image, description, bullet list, GitHub link per project), plus
  contact. There is no separate `projects.html` — everything lives in this one file.
- Scroll behavior: sections fade/slide in via `IntersectionObserver` (`.reveal`, `.reveal-side`,
  `.reveal-scale` classes), the experience timeline has a rail that fills based on scroll position,
  and there's a top progress bar plus a right-edge dot-nav (desktop only, hidden below 820px) that
  jumps to sections. All motion is disabled under `prefers-reduced-motion: reduce`. This logic lives
  inline in a `<script>` at the bottom of `index.html` — no build step, no external JS files.
- `README.md` — doubles as the GitHub profile README **and** a parallel plain-text copy of the same
  resume content (education/experience/projects/skills) shown in `index.html`. These two are not
  templated from a single source — when updating experience, education, or skills, update both
  `index.html` and `README.md` by hand to keep them in sync.
- `self1-web.jpg`, `coupon-chat-web.jpg`, `amz.png`, `LSTM.png` — image assets referenced by relative
  path from `<img src="...">` tags. The `-web` images are resized/compressed copies (via `sips`) of
  originals that were multiple MB; keep new images web-sized (roughly under 150KB, longest edge
  ~500–900px) rather than committing camera-original files.
- `google19c131cb6998e7ce.html` — Google Search Console site-verification file; must stay at the repo
  root with its exact filename, don't move or delete it.

## GitHub Actions (Claude Code integration)

Two workflows call `anthropics/claude-code-action@v1` using the `CLAUDE_CODE_OAUTH_TOKEN` secret:

- `.github/workflows/claude.yml` — responds when `@claude` is mentioned in an issue comment, PR
  review comment, PR review, or issue body/title.
- `.github/workflows/claude-code-review.yml` — auto-runs `/code-review:code-review --comment` on
  every PR open/sync/reopen.
