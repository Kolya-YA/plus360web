# AGENTS.md

This file contains project-level instructions for coding agents working in this repository.

## Project

This is a multilingual static website built with Hugo.

Primary technologies:

- Hugo
- Go templates
- HTML
- SCSS / CSS
- JavaScript
- Hugo Pipes
- Dart Sass

The site has German and Russian language versions.

The project should remain lightweight, fast, maintainable, and based primarily on native web technologies.

---

## Plus360-specific conventions

### Repository and documentation

- This file applies to the main repository and its theme directory when working from this checkout. The theme is an independent Git submodule at `themes/theme-plus360`; inspect its working tree separately.
- Content, `data/`, `i18n/`, and `config/_default/` belong to the main repository. Shared layouts, SCSS, JavaScript, and theme assets belong to the theme repository.
- When asked to commit changes in both repositories, commit the theme first, then commit the updated submodule pointer together with the corresponding main-repository changes. Do not push or deploy without a user request.
- Read `README.md` for setup, `ARCHITECTURE.md` for the architecture, `DESIGN_SYSTEM.md` for visual rules, and `A11Y_AUDIT_HOME.md` for accessibility findings and their resolution. Historical findings may describe code that has since changed; inspect the current implementation.
- Continue on the user's working branch. Create a new branch when requested; do not switch or discard unrelated work.
- Project editor settings are in `.zed/settings.json`: HTML format-on-save uses the external `gotmplfmt` command. Preserve formatting compatible with Hugo Go templates; use this configured formatter for HTML rather than introducing another formatter. Do not reformat unrelated files.
- Use the Hugo version supported by the current checkout. For new Hugo APIs or template changes, consult current official Hugo documentation and avoid deprecated patterns. Do not upgrade tools as part of an unrelated task.
- Hosting is Cloudflare Pages. Contact-form delivery and deployment configuration still need separate work; do not assume they are production-ready.

### Content and localization

- German is the default language at `/`; Russian is at `/ru/`. Russian content translations use `.ru.md`.
- Homepage service cards and hero shortcuts must derive from the corresponding service Markdown pages, not a duplicated homepage list such as `.Params.services.cards`.
- Featured services define `homepage: true`, `service_id`, `linkTitle`, `summary`, `icon`, and `weight`. Keep `service_id` stable across translations because it defines homepage anchors.
- Hero shortcuts link to the larger service cards on the same homepage. Card detail links lead to the localized service pages.
- Shared UI text belongs in `i18n/de.toml` and `i18n/ru.toml`. Use localized home links rather than `.Site.BaseURL` when navigation should preserve the current language.
- Always render Impressum, AGB, and Datenschutz links in the footer. If a translation is missing, link to the German page and retain its language metadata. Do not hide required legal links.
- Preserve the positioning of Plus360 as a webatelier providing both new websites and repair, maintenance, and improvement of existing websites. Retain the meaningful 360-Grad reference.
- Pricing amounts and delivery times are provisional. Support individual offers, hourly work, and subscriptions; do not invent final commercial terms.

### Visual system

- Preserve the calm minimalist Bauhaus direction, the curved bottom of the header, and the image-free homepage hero unless the user requests a change.
- Colors come from semantic CSS custom properties in `themes/theme-plus360/assets/scss/var/_tokens.scss`. Palette values use OKLCH; components must not introduce their own literal colors or separate light/dark palettes.
- Light and dark modes override the same semantic roles. Keep system, light, and dark preferences and the pre-render theme initialization.
- Typography comes from `assets/scss/var/_typo.scss`; spacing, control sizes, radii, and motion use the existing design tokens. Structural layout values and Sass media-query breakpoints are allowed.
- Rounded components use `corner-shape: var(--corner-shape)` with `squircle`. Actual geometric circles and the header arc retain `round`.
- Heading/accent font Jost is hosted locally in the theme. Preserve the bundled SIL OFL license and copyright notices. New font assets must have a suitable license; add legal-page attribution if that license requires it.
- Service icons in homepage cards and hero shortcuts use the same action color. Decorative icons remain hidden from assistive technology.
- Keep the theme picker as an icon button in the footer's lower menu row, aligned right. It opens a native popover with CSS Anchor Positioning and a positioning fallback. Reserve space for the fixed back-to-top control.

### Accessibility constraints

- Preserve the keyboard-visible skip link and the focusable main-content target, plus the back-to-top button with reduced-motion handling.
- An open mobile menu locks page scrolling. If keyboard focus leaves the navigation, close the disclosure, unlock scrolling, and reveal the focused element. Escape closes it and restores focus to the trigger. Do not leave focus in locked offscreen content.
- Theme radios must remain usable with arrow keys. Changing a preference must not close the panel or force focus back to the trigger. Escape closes the panel and returns focus; outside clicks use native popover dismissal.
- Long translated labels must wrap at narrow widths and enlarged text sizes. Popovers need bounded dimensions and vertical scrolling when necessary; do not clip labels or create horizontal overflow.
- Keep semantic headings and landmarks, meaningful accessible names, `aria-current` for current navigation, and visible focus indicators.
- When verification is requested, include both languages and themes, 320 CSS px, text enlargement to 200%, increased text spacing, and keyboard interaction for affected components. Distinguish text enlargement from real browser zoom.
- Lighthouse scores are supplemental evidence. Do not claim WCAG conformance from a score alone or imply that an accessibility-tree inspection is a screen-reader test.
- Do not add or run automated tests unless the user requests testing or verification. Browser inspection and relevant Hugo/Sass builds follow the workflow below; documentation-only changes do not require a site build.

---

## General principles

- Inspect the existing implementation before making changes.
- Follow existing project structure and conventions.
- Prefer the smallest coherent change that fully solves the task.
- Do not refactor unrelated code.
- Do not introduce abstractions without a concrete benefit.
- Do not add dependencies for functionality that can reasonably be implemented with Hugo, CSS, or browser APIs.
- Do not introduce frontend frameworks unless explicitly requested.
- Do not add Tailwind.
- Preserve existing behavior unless the task explicitly requires changing it.
- Prefer simple and readable solutions over clever ones.
- Remove code only when it is clearly obsolete or replaced by the current change.

Before creating something new, check whether the project already contains a suitable:

- partial
- shortcode
- layout
- CSS component
- JavaScript utility
- data structure
- configuration value

Reuse existing mechanisms where appropriate.

---

## Hugo

Use Hugo features before introducing client-side or external tooling for functionality that can be handled at build time.

Prefer:

- layouts
- partials
- shortcodes
- data files
- page resources
- Hugo Pipes
- Hugo configuration
- Hugo localization mechanisms

Avoid duplicating substantial template markup.

Extract a partial when it genuinely improves reuse or readability, but do not turn very small pieces of markup into unnecessary abstractions.

Respect Hugo template lookup rules and the existing repository structure.

Do not modify generated files in `public/`.

Do not treat generated output as source code.

When changing shared templates, consider the effect on all pages that use them.

Avoid unnecessary client-side rendering when Hugo can generate the final HTML during the build.

---

## Multilingual behavior

The site has German and Russian versions.

Shared changes must preserve both language versions.

When changing:

- navigation
- shared templates
- reusable components
- metadata
- structured data
- forms
- page layout
- localized content handling

check both German and Russian output where relevant.

Do not hard-code user-facing text into shared templates when it belongs in Hugo's localization or content system.

Preserve existing localized URLs unless the task explicitly requires changing them.

Do not create separate language-specific templates merely to avoid proper localization unless the markup genuinely needs to differ.

Do not silently add text in only one language when the corresponding translation is required.

---

## HTML

Prefer semantic HTML.

Use native HTML elements before recreating their behavior with JavaScript.

Maintain accessible document structure.

Use appropriate:

- headings
- landmarks
- labels
- buttons
- links
- form elements

Do not use clickable `div` or `span` elements where a native button or link is appropriate.

Do not add ARIA when native HTML already provides the correct semantics.

Preserve useful metadata, structured data, canonical URLs, and SEO-related markup when modifying templates.

---

## Styling

The project currently uses SCSS compiled through Hugo Pipes with Dart Sass.

SCSS is an existing build and source-organization mechanism, not a requirement for new Sass-specific architecture.

Do not migrate the existing SCSS codebase to plain CSS as part of unrelated work.

At the same time, do not deepen the project's dependency on Sass without a concrete reason.

### Native CSS first

For new code, prefer modern native CSS features when they provide a clear solution.

Prefer:

- CSS custom properties
- native CSS nesting
- Grid
- Flexbox
- logical properties
- `clamp()`
- `min()`
- `max()`
- modern selectors
- media queries
- container queries where appropriate
- cascade layers where they improve the existing architecture

Avoid introducing new Sass-specific constructs merely for convenience.

In particular, avoid unnecessary:

- mixins
- Sass functions
- loops
- generated utility classes
- Sass maps
- `@extend`
- complex compile-time abstractions

Existing Sass-specific code does not need to be rewritten unless the current task directly benefits from doing so.

### CSS architecture

Keep styles modular and component-oriented.

Prefer shallow selectors.

Avoid deep nesting and strong coupling to DOM structure.

Prefer patterns such as:

```css
.component {}
.component-title {}
.component.is-active {}
```

over selectors such as:

```css
.page .section .wrapper .component > div:first-child {}
```

Reuse existing custom properties and design values before introducing new ones.

Use CSS custom properties for values that benefit from:

- inheritance
- theming
- component-level overrides
- responsive changes
- runtime changes

Keep specificity low unless there is a concrete reason otherwise.

Do not use `!important` as a routine solution.

Do not introduce a CSS framework or utility framework.

Do not add Tailwind.

---

## Sass build

The project uses Dart Sass through Hugo Pipes.

Preserve the existing build pipeline unless the task specifically concerns it.

The current architecture includes development and production behavior such as:

- expanded CSS during development
- compressed CSS in production
- source maps during development
- production fingerprinting
- Hugo-provided Sass variables

Do not replace this pipeline casually.

If editing the Sass pipeline, ensure both development and production builds still work.

Prefer Dart Sass-compatible syntax.

Do not introduce deprecated LibSass-specific behavior.

---

## JavaScript

Keep JavaScript minimal.

Before adding JavaScript, determine whether the requirement can be solved with:

1. HTML
2. CSS
3. Hugo
4. JavaScript

in that order when practical.

Prefer native browser APIs.

Do not add npm packages for trivial functionality.

Avoid introducing client-side state or framework code where simple DOM behavior is sufficient.

Do not duplicate functionality already provided by the browser.

Keep JavaScript modules focused and understandable.

Avoid global state unless the existing architecture requires it.

Progressive enhancement is preferred where practical.

Pages should remain functional without unnecessary JavaScript dependencies.

---

## Responsive design

Changes affecting layout must be checked at multiple viewport sizes.

At minimum, consider approximately:

- 390px mobile
- 768px tablet
- 1440px desktop

Do not optimize only for the viewport shown in the original task.

Check for:

- horizontal overflow
- clipped content
- broken navigation
- overlapping elements
- unreasonable spacing
- unreadable line lengths
- unexpectedly large layout shifts

Prefer fluid layouts over unnecessary collections of device-specific breakpoints.

---

## Browser verification

Chrome DevTools MCP is available and should be used for user-facing changes when browser verification provides meaningful value.

For changes affecting:

- HTML
- CSS / SCSS
- JavaScript
- responsive behavior
- navigation
- interactive elements
- forms
- asset loading
- rendered Hugo templates

verify the affected page in Chrome.

Check as appropriate:

- the affected URL
- visual rendering
- browser console
- failed network requests
- responsive layout
- interactions
- relevant German and Russian variants

Fix regressions caused by the current change before finishing.

Do not perform unnecessary browser verification for trivial source-only changes where rendered behavior cannot reasonably be affected.

---

## Performance

Keep the site lightweight.

Avoid adding large dependencies for small features.

Prefer Hugo-generated static output over client-side processing.

Avoid unnecessary JavaScript execution.

Do not add large libraries when a small native implementation is sufficient.

Preserve image optimization and asset-pipeline behavior where present.

When modifying page structure or assets, avoid introducing obvious:

- render-blocking resources
- unnecessary network requests
- oversized assets
- layout shifts
- duplicate CSS or JavaScript

Do not prematurely optimize code that is already simple and fast.

---

## Accessibility

Do not knowingly introduce accessibility regressions.

For interactive changes, check:

- keyboard usability
- visible focus states
- appropriate semantics
- form labels
- meaningful link/button text
- basic contrast
- reasonable heading structure

Prefer native browser controls where possible.

---

## Content and templates

Keep content separate from presentation where the existing Hugo architecture allows it.

Do not move ordinary editable content into templates without a good reason.

Do not duplicate localized content in template logic when it belongs in:

- content files
- data files
- configuration
- localization resources

Preserve front matter and existing content conventions.

When changing front matter structures, inspect how those values are consumed elsewhere before modifying them.

---

## Dependencies

Do not add a dependency without first determining whether the project actually needs it.

Before adding one, consider whether the same result can reasonably be achieved using:

- Hugo
- Go templates
- native CSS
- native JavaScript
- existing project code

If a dependency is necessary:

- choose a maintained and appropriately scoped package
- avoid unnecessary transitive complexity
- use the project's existing package manager
- update the relevant lockfile
- do not modify unrelated dependencies

Do not perform broad dependency upgrades as part of an unrelated task.

---

## Generated files and build artifacts

Do not manually edit generated files.

Typical generated or temporary output should not be treated as source.

Respect the repository's `.gitignore`.

Do not commit:

- temporary files
- editor state
- build caches
- local development artifacts
- secrets

unless the repository explicitly expects them.

---

## Secrets and configuration

Never hard-code secrets, credentials, API keys, passwords, or private tokens.

Do not expose values from local environment files.

Do not commit `.env` or equivalent secret-containing files unless the repository explicitly uses a safe example file.

Use existing configuration mechanisms.

When an example configuration is needed, use placeholder values.

---

## Git

Inspect the working tree before making substantial changes.

Useful commands include:

```bash
git status
git diff
git log
```

Do not overwrite unrelated uncommitted work.

Do not revert unrelated changes.

Keep the final diff focused on the requested task.

Before finishing, inspect the final diff for:

- accidental changes
- generated files
- debug output
- temporary code
- unrelated formatting churn

Do not force-push or perform destructive Git operations unless explicitly requested.

---

## Verification

After code changes, perform the checks appropriate to the task.

At minimum, changes affecting Hugo rendering should pass a Hugo build.

Run:

```bash
hugo
```

unless the repository documents a more specific build command.

If the task affects the Sass pipeline or styles, verify that Sass compilation succeeds.

For user-facing changes:

1. build the site
2. start or use the Hugo development server as needed
3. inspect the affected page in Chrome
4. check console errors
5. check failed network requests
6. inspect relevant viewport sizes
7. check relevant language variants
8. review the final Git diff

Do not claim that something was tested if it was not actually tested.

If a check cannot be performed, state exactly which check was not performed and why.

---

## Debugging

When the cause of a bug is unclear, investigate before editing.

Prefer:

1. reproduce the problem
2. inspect the relevant implementation
3. identify the root cause
4. make the smallest appropriate fix
5. verify the fix
6. check for regressions

Do not make speculative changes across many files hoping that one of them fixes the issue.

Use browser console and network information when debugging frontend behavior.

---

## Refactoring

Do not perform unrelated cleanup while solving another task.

Refactor when:

- it is required for the requested change
- it removes duplication directly encountered by the task
- it substantially simplifies the implementation
- the user explicitly asks for it

Otherwise preserve the existing architecture.

Large architectural changes should be separated from functional changes where practical.

---

## Comments and documentation

Prefer self-explanatory code.

Add comments when they explain:

- a non-obvious constraint
- Hugo-specific behavior
- a browser workaround
- an architectural decision
- why something must be done in a particular way

Do not add comments that merely restate the code.

Update project documentation when a change alters:

- setup
- build commands
- project architecture
- deployment requirements
- required configuration

---

## Scope discipline

Do not turn a small task into a repository-wide rewrite.

Examples:

If asked to fix one component, do not redesign the entire CSS architecture.

If asked to change one template, do not reorganize all Hugo layouts.

If asked to add one interaction, do not introduce a JavaScript framework.

If asked to change styling, do not migrate SCSS to CSS unless migration is explicitly part of the task.

If an unrelated problem is discovered, mention it separately rather than silently expanding the task.

---

## Definition of done

A task is complete when:

- the requested behavior is implemented
- existing project conventions are respected
- unrelated code was not changed unnecessarily
- the Hugo build succeeds where applicable
- Sass compilation succeeds where applicable
- user-facing changes were verified in the browser where appropriate
- relevant German and Russian versions were checked
- no new console errors or failed requests were introduced
- responsive behavior remains sound
- the final Git diff contains only intentional changes
- temporary debugging code has been removed
