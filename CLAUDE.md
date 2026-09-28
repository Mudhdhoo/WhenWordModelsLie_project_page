# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static academic project page forked from the [Nerfies website template](https://github.com/nerfies/nerfies.github.io). It is being repurposed for a new paper (an ICRA submission: `paper/icra_paper_arxiv.pdf`, video `static/videos/icra_video.mp4`). `index.html` has been stripped down to a title, authors/affiliations, the abstract, and the paper video. More sections will be added incrementally.

There is no build system, package manager, linter, or test suite. It is plain HTML/CSS/JS served as-is (GitHub Pages).

## Local preview

```sh
python3 -m http.server 8000   # then open http://localhost:8000
```

Use a local server rather than opening `index.html` directly, so that relative asset paths and video loading behave the way they will in deployment.

## Structure

- `index.html` is the entire site: a single page built from Bulma `section`/`hero`/`columns` blocks. Each content block is marked with `<!-- Name. -->` / `<!--/ Name. -->` comments.
- `static/js/index.js` holds the template's page behavior (not currently loaded by `index.html`; re-add the `<script>` tag along with jQuery / bulma-carousel / bulma-slider if you bring back those sections) (jQuery):
  - Navbar burger toggle.
  - `bulmaCarousel.attach('.carousel', …)` for the `#results-carousel` video carousel (3 slides shown).
  - The interpolation slider preloads `NUM_INTERP_FRAMES` (240) frames from `static/interpolation/stacked/000000.jpg…`. `#interpolation-slider` then swaps the image shown in `#interpolation-image-wrapper`. If you remove the Interpolation section from the HTML, remove or guard this code too, because it will otherwise request 240 missing images.
- `static/css/index.css` holds the site-specific styles. The other CSS/JS files are vendored minified libraries (Bulma, bulma-carousel, bulma-slider, FontAwesome); don't edit those.
- External CDNs: jQuery, Google Fonts, Academicons (for the `ai ai-arxiv` icon).

## Things to be aware of when adapting the template

- `static/images/` and `static/interpolation/` are empty, and the site uses `static/videos/icra_video.mp4`, which is `icra_acc_vid_final.mp4` (the original, kept as the source) with "Anonymous submission" on the first slide (0–10.3s) painted out and replaced by "John Cao and Somil Bansal / Stanford University" (ffmpeg overlay + drawtext, Arial 33px at x=99, y=598/644). The original Nerfies sections (teaser, carousel, interpolation, BibTeX, etc.) were removed; see git history / upstream template to copy their markup back.
- The CC BY-SA 4.0 footer asks forks to keep a link back to the Nerfies template.
- Videos use `autoplay muted loop playsinline`. All of those attributes are needed for autoplay to work on mobile Safari.
