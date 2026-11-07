---
name: reusable-prototype
description: Build a portable, open-and-run HTML UI prototype from an implementation doc with locally vendored frameworks, optionally modeled on an existing reference page.
argument-hint: "<implementation-doc> <css-framework> [--like <page-path>]"
disable-model-invocation: true
---

Build radically different variants of a **user-facing** page to answer one design question, as a stakeholder demo that developers later port with `match-prototype`. The bar is **open-and-run**: anyone with a browser opens a page file and it works, with nothing to install, build, serve, or download, and no internet connection needed.

Method adapted from the UI branch of mattpocock/skills `prototype`.

## Method

- **Question first.** Every run answers one stated design question.
- **3 variants by default, 5 at most** (`A`, `B`, `C`, …).
- **Structurally different.** Variants differ in layout, hierarchy, and primary interaction, never only in color or copy. Not several card grids.
- **Reference as baseline.** With a reference page, variant `A` follows it and the other variants stay structurally different within its design language (step 4).
- **Stubs, not mutations.** Actions change mock state only.
- **Hand over links.** Share a `?variant=` link per variant.
- **Skip code ceremony** (tests, build steps, abstractions), never visual craft.

## 1. Take one implementation doc

Each run covers one implementation doc (e.g. `implementation-docs/spec-s01.md`). When none is given, or the request names two or more, stop and ask which one to build now.

Read the implementation doc and the source story and ACs it links to. Derive the design question from them (e.g. "Does pause/resume on the timer feel right?"); when they imply no clear question, ask.

**Reference page (optional).** The user may name an existing page whose functionality is similar, via `--like <page-path>` or plainly ("like history.html"). It can be a prototype page (`prototype/history.html`) or an app page or component (`app/src/pages/History.vue`). When the path does not resolve to a file, or the file is not a user-facing page (a test, utility module, or backend file), stop and ask for a valid page. Read a valid reference page in full, plus the components, styles, and scripts it pulls in, and note its CSS framework, layout skeleton, and interaction patterns.

Done when you have one implementation doc, its story ID, its question, and either a read reference page or none.

## 2. Settle the frameworks

- **JS framework**: read the app's `package.json`. Ask only when there is no app yet or the dependencies are ambiguous.
- **CSS / UI framework**: use the one the user specified, or the one in `package.json`. If neither settles it, ask which to use and wait for the answer.
- **Reference conflict**: when the reference page uses a different CSS framework from the one settled above, ask which to build with, naming both (e.g. "history.html uses Vuetify; you asked for Bootstrap. Build with which?"), and wait for the answer. When they match, continue.

Done when both frameworks are known and match the reference page or the user has picked one.

## 3. Place the story on a page

Prototype files are one per **app page**, not per story; a page collects stories across runs.

- When the story happens on a page that already has a prototype file, extend that file.
- Create a new file only for a page the app does not have yet, named after the page in kebab-case (`timer.html`). When it is unclear which page the story belongs to, ask.
- A page holds **one open story at a time**. When the page's header already has a story marked `→ open`, stop and ask the user to settle its verdict (step 6) first.
- Every page file starts with a `Stories:` header, one story per line with its question and status. Add this story as `→ open`:

  ```html
  <!-- Stories:
    S01: Does starting the timer feel right? → B (single big button), approved by Jane 2026-11-03
    S02: Does pause/resume feel right? → open (modeled on prototype/history.html)
  -->
  ```

  With a reference page, append `(modeled on <page-path>)` to the story line so `match-prototype` knows where its patterns came from.

- Link between pages with plain relative links (`<a href="history.html">`).
- Regenerate `index.html` (step 4).

Done when the story has its page file and is in the file's header as `→ open`.

## 4. Build the page

**Frameworks vendored locally.** `prototype/vendor/` holds only **reachable** files: each page's `<script src>` and `<link href>` targets, everything those files load in turn (`url()` fonts and images, runtime assets), plus each library's `LICENSE`. Pin every library to an exact version (`bootstrap@5.3.3`, never `@latest`; `.min` builds are fine) and unpack `npm pack <pkg>@<version>` into a temporary folder outside `prototype/`. Pick the one build the page loads, copy its reachable files into `prototype/vendor/<lib>/`, then delete the temporary folder. When the download fails, stop and ask the user to place the files there. CDN URLs are not used: corporate proxies can redirect them and the page renders blank. Fonts a stylesheet loads (e.g. MDI) go in `vendor/<lib>/fonts/` with the stylesheet in `vendor/<lib>/css/` so relative `url()` paths resolve.

- **Lazy-loading libraries** (Stencil-based ones like Ionic, or any loader that picks chunks at runtime): vendor the package's single-file build (UMD, IIFE, `*.bundle.*`). When it has none, bundle once with a pinned esbuild (`bundle: true, format: 'iife', platform: 'browser'`) into one classic script, and vendor that script plus its entry file. The chunked `dist/` folder stays in the temporary folder: its loader fails over `file://` and its chunks are never reachable.
- **Icon sets**: for SVG icons, vendor only the icons the pages use, or register them inside the bundle. An icon font stays whole (stylesheet plus the fonts it names).
- **`prototype/vendor/VERSIONS.md`**: one row per library with its exact version and exact vendored file paths; for a bundle, also its entry file and esbuild version so it can be rebuilt.

- Use the UI framework's own components and JS for interactions it provides (modals, toggles, menus) so behavior matches production.
- Write page logic in the app's JS framework through its no-build global build: Vue's `vue.global.js`, or React/Preact with `htm`. Put page markup in the HTML as in-DOM templates, and component templates as template strings in `js/*.js` via `app.component('name', { template: ... })` rather than `.vue` files. In-DOM templates need kebab-case component tags and explicit closing tags (`<task-card></task-card>`, never `<TaskCard />`).
- When the JS framework has no no-build option (Angular, Svelte), write page logic in vanilla JS.
- Put pure page logic in `js/<page>-logic.js` to drive the UI.

**User-facing UI.** Render only the product UI the end user sees, plus the floating switcher bar. Express state through that UI itself. Write natural consumer copy in English; keep story IDs, spec codes, model/field names (`FocusSession`, `RemainingTime`), and variant rationales out of the canvas (use the `Stories:` header instead).

**Reference page.** With a reference page, reuse everything that transfers across all variants: its framework components, colors and tokens, typography, spacing and density, interaction patterns (how lists, forms, confirmations, and empty/loading/error states behave), and copy tone and labelling. Mock data comes from the implementation doc; borrow the reference's data shape only for fields the doc leaves open. Variant `A` also follows the reference's layout skeleton (regions, columns, where actions sit) when it fits the new story; when it does not fit, give `A` a layout suited to the story and keep everything else. A prototype reference's files can be copied directly; an app reference is translated into the no-build, `file://` form below.

**Visual craft.** Follow the reference page's design language when there is one, then the UI framework's own design language and tokens first. Where it leaves a choice open, apply Apple HIG / Google Material 3 principles:

- **Neutral surfaces, focused accent.** Default to a clean light palette unless dark mode is specified. Keep page and card surfaces neutral (for glass themes, use neutral translucent surfaces with a subtle `1px` specular border) and reserve brand color strictly for the primary CTA, active selection, and key data fills.
- **Single-layer containment.** Use at most one card layer on the canvas; separate groups inside a card with whitespace or `1px` hairline dividers instead of nested bordered boxes. Match column count to container width (single column below `480px`) and keep buttons, chips, and nav labels on one line (`white-space: nowrap`).
- **Typographic hierarchy.** Give each view one clear focal point, clean control labels, and muted secondary text differentiated by weight and opacity, with `font-variant-numeric: tabular-nums` on all timers, counters, and metrics.

**Floating switcher bar.** A compact, single-row translucent pill (`<= 40px` tall, `bottom: 16px`, subtle `1px` border) defined once in `js/common.js` and `css/styles.css`: `←` | segmented `A · Name` pills | `→`. Read the variant with `URLSearchParams` and switch by setting `location.search` (a reload, reliable over `file://`). Left/right arrow keys switch too, except while focus is in an input. When a story depends on time passing or external events, append compact simulation chips (e.g. `+1m`, `Reset`) after a `1px` vertical divider in this same bar so the app canvas stays pure product UI.

**App frame.** Rebuild the app's own chrome (header, nav, realistic user data) once as a shared component in `js/common.js`. Every page renders inside it; UI variants swap only the content area.

**Demo state.** Keep state in memory by default. When a story's flow crosses pages, persist it through a single `demoStore` in `js/common.js` backed by `sessionStorage` and seeded from the mock data in `js/`.

**Cover sheet.** Keep `prototype/index.html` outside the app frame: each page with its stories and their status, a link per variant of any open story, and a **Reset demo** link that clears `demoStore`.

**Works over `file://`.** Pages open from disk, so:

- Load local scripts as classic `<script src>`; they share globals instead of `import`/`export`.
- Keep mock data as JS objects in `js/` files; read nothing with `fetch()`.

**Readable local files.** Put your own CSS and JS in separate files referenced by relative paths: `js/<page>.js` per page, `js/<page>-logic.js` for pure page logic, shared code in `js/common.js`. Keep them unminified. Keep images as files in `images/`.

**Other scripts** (e.g. a design-capture script) are vendored the same way, only when a task needs them.

```
prototype/
├── index.html
├── timer.html
├── history.html
├── css/
│   └── styles.css
├── vendor/
│   ├── VERSIONS.md
│   ├── vue/
│   │   ├── vue.global.prod.js
│   │   └── LICENSE
│   ├── vuetify/
│   │   ├── vuetify.min.js
│   │   ├── vuetify.min.css
│   │   └── LICENSE
│   └── mdi/
│       ├── css/materialdesignicons.min.css
│       ├── fonts/        (only the files that stylesheet's url() names)
│       └── LICENSE
├── js/
│   ├── common.js
│   ├── timer.js
│   ├── timer-logic.js
│   └── history.js
└── images/
```

Done when the page renders only user-facing product UI (plus the variant switcher) inside the shared app frame.

## 5. Check open-and-run

Scan the prototype folder and fix any hit:

- `type="module"` on a local script, or `fetch(`
- inline `<style>` or `<script>` blocks with content
- `data:image` (base64 images)
- any `http://` or `https://` URL in a page or local script (CDN or otherwise)
- a vendored library missing from `vendor/VERSIONS.md`, without an exact version, or whose listed files differ from what is in `vendor/<lib>/`
- a file in `vendor/` that no page or other vendored file reaches, other than `LICENSE` and `VERSIONS.md` (fix: delete it, including leftovers from earlier runs)

With whatever browser tool is available, open the page file and cycle through every variant at a desktop width and a phone width (`<= 400px`), fixing any nested box clutter, low-contrast text, or mid-phrase line wrapping. When no browser tool is available, report "visual check: pending (manual)" with the `?variant=` link for each variant.

Report the page file, the story it now covers, the vendored files with their versions and total file count, and the visual check status. With a reference page, also report whether variant `A` reused its layout skeleton or only its design language, and why. Done when the scan is clean and every variant is visually verified or reported as pending.

## 6. Capture the verdict

Write the verdict only after the user explicitly picks the winning variant. Ask who approved it, then replace the story's `→ open` in the header with the winner, approver, and date (e.g. `S02: Does pause/resume feel right? → B (single toggle button), approved by Jane 2026-11-03`), keeping any `(modeled on <page-path>)` suffix.

Fold the winning variant into the page, then delete the losing variants and the whole bar, simulation chips included; git history keeps them. Regenerate `index.html`.

Done when the verdict is in the header and the page shows only the winner.
