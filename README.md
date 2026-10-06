# Personal site

A Jekyll site for GitHub Pages.

## Structure

```
_config.yml          site settings, nav, social links
_layouts/            default, page, post templates
_includes/           header and footer
_posts/              blog posts (YYYY-MM-DD-title.md)
_projects/           one Markdown file per project
assets/css/style.scss
assets/images/       avatar and other images
index.html           home page
about.md, blog.html, projects.html, 404.html
```

## Publish

1. Push these files to the `main` branch.
2. On GitHub, open **Settings → Pages** and set the source to **Deploy from a branch → main / (root)**.
3. The site goes live at `https://steveneugenehager.github.io` within a minute or two.

## Run locally

Requires Ruby (on Windows, use RubyInstaller with the DevKit).

```bash
bundle install
bundle exec jekyll serve --livereload
```

Then open http://localhost:4000.

## Customize

- **Avatar:** replace `assets/images/avatar.svg` (and update the path in `index.html` if you use a `.jpg` or `.png`).
- **Colors:** edit the CSS variables at the top of `assets/css/style.scss`. Dark mode follows the visitor's system setting.
- **Nav:** edit the `nav` list in `_config.yml`.
