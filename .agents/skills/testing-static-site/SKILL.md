---
name: testing-static-site
description: How to serve and end-to-end test this static HTML/CSS portfolio site locally (responsive breakpoints, focus/skip-link, 404 page, asset 404 checks).
---

# Testing the portfolio static site

## Serving
No build step and no dependencies. From the repo root:

```bash
python3 -m http.server 8000
```

Then browse `http://localhost:8000/index.html`. Pages: `index.html`, `about.html`,
`projects.html`, `contact.html`, `404.html`. There is no JS on the site.

Note: `python3 -m http.server` does NOT auto-serve `404.html` for unknown paths the way
GitHub Pages does — load `/404.html` directly to test it.

## Responsive breakpoints
The desktop resolution here is 1600x1200, so a maximized Chrome gives a ~1600px viewport
(enough to prove the `--max-width: 1100px` centering). For narrow widths Chrome refuses to
resize the OS window below ~500px, so use DevTools device mode:
F12, then Ctrl+Shift+M, dock DevTools to the bottom (⋮ → dock-to-bottom) and type the width
into the "Dimensions" width field in the toolbar. Breakpoints that matter: 768px
(hero padding 3rem→5rem, projects flex→grid) and 1200px (3-column grid).

Quick overflow check via the console:
`document.documentElement.scrollWidth <= innerWidth`.

## Finding asset 404s quickly
In the Network panel enable "Preserve log" and put `-status-code:200 -status-code:304` in the
filter box, then click through all pages. The counter reads `N / total requests`; anything
non-zero (other than Chrome's own `favicon.ico` request when viewing a PDF) is a real 404.

## Keyboard / focus testing
Click on empty page background first, press Ctrl+Home, then Tab — the first Tab should reveal
the `.skip-link`. Focus outlines are the orange `--color-accent` (#f39c12) via `:focus-visible`,
so zoom into the screenshot to confirm them.

## Known pitfall
`.btn:hover` in `css/components.css` is not scoped to links, so any element carrying `.btn`
(e.g. the `<span class="btn btn--disabled">Coming soon</span>` on projects.html) picks up the
solid-blue hover + lift and looks clickable. If a "disabled" chip appears to react to hover,
that's this rule, not a browser quirk.

## Devin Secrets Needed
None — everything is local and unauthenticated.
