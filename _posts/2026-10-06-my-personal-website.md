---
title: My Personal Website for Cloud and IT activities
tags: [documentation]
---

Hoping to document my cloud and data engineering journey, I created [my own GitHub repository](https://github.com/steveneugenehager/steveneugenehager.github.io) and generated the site's structure using Claude Code with a Jekyll setup specified.  I chose to use Jekyll as an accelerator so I could contribute posts and project updates with minimal up front effort.

## What are GitHub Pages?
[GitHub Pages](https://docs.github.com/en/pages) is [GitHub's](https://github.com/) free hosting for static websites. You put your site's files in a GitHub repository, and GitHub publishes them on the web for you. How it works for your site:
1. Your new repository is named exactly `YOUR-USERNAME.github.io`, in my example: `steveneugenehager.github.io`. That special name makes it your user 
site, served at <https://steveneugenehager.github.io>.
1. In the repo's settings, visit the "Pages" section and 
    1. enable "Source" as "Deploy from a branch" and
    1. specify the branch name, probably main.
    1. Note: the repository needs to have its Jekyll files uploaded to it before these can be enabled.
1. When you push changes to main, GitHub sees the Jekyll files, runs Jekyll to build the site and publishes the result. It usually takes a minute or two.
1. You can check each build's progress or errors in the repo's Actions tab.

## What is Jekyll?
Jekyll is a static site generator. You write your content as plain text files, mostly Markdown, and Jekyll turns them into a finished website made of ordinary HTML files. There's no database or server-side code, so the result is just files that any web host can serve. How it works in your site:
1. You write content. A blog post is a Markdown file in `_posts/`, such as `2026-10-06-hello-world.md`. The block between the --- lines at the top is called front matter. It holds the post's title, tags and similar details.
1. Templates supply the look. The files in `_layouts/` and `_includes/` hold the shared HTML: the header, footer, navigation and page structure. They use a template language called Liquid. For example, {% raw %}`{{ page.title }}`{% endraw %} inserts the post's title, and {% raw %}`{% for post in site.posts %}`{% endraw %} loops over every post.
1. Settings live in `_config.yml`. This file holds your site title, URL, menu items and add-ons.
1. Jekyll builds the site. It combines your content with the templates and writes the result to the `_site/` folder. That's what `bundle exec jekyll build` did earlier.

### Why Jekyll pairs well with GitHub Pages
GitHub has Jekyll built in. When you push your files, GitHub runs Jekyll for you and publishes the result at steveneugenehager.github.io. You never upload HTML yourself.

To preview the site, change to the repo directory on your local PC and run:
```bash
bundle exec jekyll serve --livereload
```
The website will be seen in your browser at <http://localhost:4000/>.

### Some steps were necessary on my Windows PC to enable Ruby and Jekyll

1. Install Ruby **with Devkit** from <https://rubyinstaller.org>. On the last screen, let it run `ridk install` and choose the MSYS2 and MINGW development toolchain option.
2. Open a new terminal and install Bundler and Jekyll:

   ```bash
   gem install jekyll bundler
   ```
3. Go to your site's folder and install its gems:

   ```bash
   bundle install
   ```

