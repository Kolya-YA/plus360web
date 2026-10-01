# plus360

## The main plus360 website

Static multilingual website built with Hugo. German pages are served at `/`, Russian pages at `/ru/`. The theme is a Git submodule in `themes/theme-plus360`.

## Local development

Use Hugo 0.167.0 or later and Dart Sass. On macOS, install the tools with Homebrew:

```sh
brew install hugo
brew tap dart-lang/dart
brew install sass/sass/sass
```

Initialize the theme after cloning:

```sh
git submodule update --init --recursive
```

Start the development server with automatic reload:

```sh
hugo server --bind 127.0.0.1 --baseURL http://localhost:1313/ --disableFastRender
```

Open http://localhost:1313/ or http://localhost:1313/ru/.

Build the production site with `hugo --minify`. Generated files are written to `public/`. The site is hosted on Cloudflare Pages; its build settings still need to be documented.

## Project structure

- `content/`: page content and front matter; Russian translations use `.ru.md`.
- `data/`: pricing and FAQ data.
- `i18n/`: interface translations.
- `config/_default/`: site configuration.
- `themes/theme-plus360/layouts/`: page templates and partials.
- `themes/theme-plus360/assets/`: SCSS and JavaScript compiled by Hugo.
- `static/`: files copied without processing.

Theme changes belong to the theme repository; the parent repository tracks its commit.

Homepage service cards and hero anchors are generated from translated service pages. Each featured service defines `homepage: true`, a stable `service_id`, `linkTitle`, `summary`, `icon`, and `weight` in its Markdown front matter. The service catalog uses the same summary and short title. Homepage content contains only section headings and introductory text, without a second list of services.

For this project used panoram generator <https://github.com/mpetroff/pannellum/tree/master/utils/multires>
