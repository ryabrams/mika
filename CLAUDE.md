# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A one-page static "coming soon" placeholder site for the custom domain in `CNAME` (`mikaabrams.com`). The entire site is `index.html` plus `favicon.png` — there is no build system, no dependencies, no test suite, and no CI.

Because there is no toolchain, there are no build/lint/test commands to run. To preview changes, open `index.html` directly in a browser, or serve the directory (`python3 -m http.server`) if you need the GTM snippet and absolute paths to behave as they do in production.

## Branch workflow

`master` is the mainline — there is no `main` branch in this repo, so read any instruction about "main" as referring to `master`. GitHub Pages deploys from it, which makes every commit that lands there a live change to the public site.

Do all development on `dev`, never directly on `master`.

Before starting work, confirm `dev` is in sync with the mainline:

```
git fetch origin master
git log --oneline dev..origin/master
```

Empty output means `dev` is current. Any commits listed mean `dev` is behind — merge `origin/master` into `dev` and resolve before writing new code.

**Ask the user before moving anything from `dev` to `master`.** Pushing to `master`, merging `dev` into it, or opening a PR that targets it all require explicit confirmation first, because they publish to the live domain. Pushing `dev` itself to `origin` does not need confirmation.

## Architecture

`index.html` is deliberately self-contained: CSS lives in an inline `<style>` block, the clock illustration is inline `<svg>` markup (not an external image), and the footer year script is inline. Adding an external stylesheet, script, or image file would break that single-file property — prefer inlining.

Two structural details are easy to get wrong:

- **Vertical centering is done with table display, not flexbox.** `html { display: table }` + `body { display: table-cell; vertical-align: middle; text-align: center }`. Everything added to `<body>` inherits centered text and participates in this layout; a new block element lands under the existing content automatically. Changing these display values will collapse the centering.
- **The Google Tag Manager container ID appears twice** — in the `<head>` loader script and again in the `<noscript>` iframe at the top of `<body>`. Both must carry the same ID.

The footer prints a hardcoded year in the markup and overwrites it via `new Date().getFullYear()` in an inline script placed after the element. The static value is the no-JavaScript fallback, so keep it current when editing rather than removing it.

## Deployment

GitHub Pages serves this repo at the domain in `CNAME`. That file is required for the custom domain to resolve — do not delete it. (The repeated "Create CNAME"/"Delete CNAME" pairs in the history are GitHub rewriting the file when the Pages custom-domain setting was toggled, not intentional edits.)

## Upstream template provenance

This site is derived from dr5hn's "Coming Soon" HTML template, and two files still belong to that upstream project rather than to this site:

- `README.md` documents the *template* (its screenshot, its icon source, the original author's social links) — it is not a description of the deployed site.
- `.github/FUNDING.yml` points at the upstream author's sponsor accounts.

Do not treat either as authoritative about this site, and don't "fix" the README to describe the placeholder page unless asked.
