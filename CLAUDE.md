# CHRiSTiEN NEW — Site Notes

Static multi-page site (no build step). Pages are individual `.html` files in `christien-new-site/`. Shared logic lives in `js/main.js`; shared styles in `css/style.css`.

## Page transitions / preloader

Two animations run when navigating to most pages:

1. **`.mil-preloader`** — the "Change / Your / AURA" → "Change Your LIFE" intro animation in each page's `<head>`-adjacent markup, driven by the GSAP timeline at the top of `js/main.js`.
2. **`.mil-curtain` + swup** — soft page-to-page transition via `js/plugins/swup.min.js`, configured in `js/main.js` (containers `#swupMain`, `#swupMenu`).

### Skipping the preloader on internal navigation

Some internal links bypass the intro animation. The convention:

- On the **source** link, add `data-no-swup` and `onclick="sessionStorage.setItem('skipPreloader','1')"` so swup hands off to a normal navigation that carries the flag.
- On the **destination** page, a small script in `<head>` reads the flag, removes it, and adds `html.skip-preloader`, which a companion `<style>` block uses to hide `.mil-preloader` and `.mil-curtain` and force `.mil-up` content visible immediately.

Reference implementation: [order.html](order.html) lines ~131–141. The same block was added to [oursolution.html](oursolution.html) so the "Yes" button on [index.html](index.html) (line ~639) lands with no transition.

**If you add a new page that should be reachable without the intro animation, copy that script + style block into its `<head>`.** Setting `skipPreloader` from the source alone is not enough — the destination must consume it.

## Layout conventions

- Header/menu markup is duplicated per page (no templating). Changes to nav links generally need to be applied across all `*.html` files.
- `data-no-swup` on a link opts that link out of swup so a full page reload happens (used together with `skipPreloader` for the no-animation path, and standalone for cases where swup's container diff would break the target page).
