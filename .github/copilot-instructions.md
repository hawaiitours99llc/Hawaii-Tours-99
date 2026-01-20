# Copilot instructions — Hawaii Tours 99 (static site)

Summary
- Single-page static website. Primary entry: `index.html` at repo root. Images live in `Photos/` and some image files at repo root.
- No build toolchain: styling via Tailwind CDN, Font Awesome CDN, Google Fonts. JS is inline in `index.html`.

What to know (big picture)
- This is a client-only site (no backend). Navigation is implemented by toggling sections in `index.html`: each page is a `<div id="page-<name>" class="page-content">`.
- Core JS functions (all in `index.html`):
  - `navigateTo(pageId)` — activates `#page-<pageId>` and adds `nav-link.active-page` to `#nav-<pageId>`.
  - `toggleMobileMenu()` — toggles `#mobile-menu.hidden` for mobile nav.
  - `setLanguage(lang)` — sets `body.lang-<xx>` class, toggles `.lang-btn.active`, and stores language under `localStorage['hi99_lang']`.
  - `window.onload` calls `setLanguage(saved)` and `navigateTo('home')`.

Language / localization pattern
- Content is duplicated inline for each language using `data-lang` attributes (e.g., `<span data-lang="en">...` and `<span data-lang="jp">...`).
- The visible language is controlled by the `body` class: `lang-en`, `lang-jp`, `lang-es`, `lang-zh`.
- When adding translations: add matching `data-lang` spans (block and inline), and ensure a language button `id="btn-<code>"` exists in the `#language-switcher`.

Styling and layout patterns
- Tailwind utility classes are used everywhere. A small set of custom CSS is defined in `<style>` in the head (CSS variables `--gold`, `--navy`, etc. and helpers like `.hero-gradient`, `.fleet-card`).
- Keep custom style names consistent: `page-content.active`, `nav-link.active-page`, `lang-btn.active`, `fleet-card`, `service-card`.

Images and assets
- Some images are remote (Unsplash, chromedata) and some local (e.g., `5bc57dc2-...jpg`, `/Photos/A9.jpg`). Note the hero uses `background: url('/Photos/A9.jpg')` — leading slash means server-root path; when previewing locally ensure the server serves repo root as web root.
- Many images include `onerror` handlers that hide the `<img>` and reveal a fallback sibling — preserve that markup when editing.

External integrations
- Booking form: external link `https://customer.moovs.app/...` (opens in new tab).
- Tel links use `tel:`. No API keys or server-side integrations in repo.

Developer workflows (how to preview/debug)
- No build step. To preview locally, serve the repo root with a static server. Examples:
  - `python -m http.server 8000` (Python 3)
  - `npx serve .`
  - Use VS Code Live Server extension
- Debugging hints:
  - Check console for JS errors; `navigateTo` and `setLanguage` are central.
  - If images fail to load, verify path (remove leading `/` if serving from a nested root) and check `onerror` fallbacks.
  - Inspect `localStorage['hi99_lang']` to see current language state.

Editing rules and conventions (concrete)
- When adding a new “page” section: create `<div id="page-<name>" class="page-content">` and match nav link `id="nav-<name>"` and any mobile menu entries.
- When adding or editing language text:
  - Use `data-lang` spans/blocks for each supported language.
  - Update `#language-switcher` with a `button` having `id="btn-<code>"` that calls `setLanguage('<code>')`.
- Preserve CSS variables and utility-class approach. Prefer adding minimal custom CSS to head rather than large global overrides.
- If extracting inline JS to a separate file, ensure it runs after DOM load and maintain `window.onload` behavior or migrate to `DOMContentLoaded`.

Common pitfalls observed
- Broken image/background paths due to leading `/` when previewing from a non-root server — check server root.
- Tailwind CDN upgrades may change utilities; when updating Tailwind, test layout and utility class names.

Where to look for examples in this repo
- Navigation & pages: `index.html` sections `#page-home`, `#page-services`, `#page-fleet`, `#page-terms`, `#page-contact`.
- Language usage: many inline examples of `data-lang` spans throughout `index.html`.
- Image fallbacks: logo `<img>` tags with `onerror` handlers in header/footer of `index.html`.

If something is unclear or you want this file expanded (tests, CI, or deployment instructions), tell me which area to expand.
