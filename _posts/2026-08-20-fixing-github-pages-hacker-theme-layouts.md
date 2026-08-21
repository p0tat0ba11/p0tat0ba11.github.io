---
layout: post
title: "Fixing Missing Layout Errors in GitHub Pages Hacker Theme"
date: 2026-08-20
tags: [Jekyll, GitHub Pages, Web Development]
---

If you are setting up your website using GitHub Pages and try switching to the Hacker theme by setting `remote_theme: pages-themes/hacker@v0.2.0`in your `_config.yml`, you might encounter missing layout warnings during build and end up with a blank site.

During the build process, you will likely see output similar to this:
```text
Generating... 
    Jekyll Feed: Generating feed for posts
Build Warning: Layout 'page' requested in 404.html does not exist.
Build Warning: Layout 'page' requested in about.md does not exist.
Build Warning: Layout 'home' requested in index.md does not exist.
    done in 0.265 seconds.
```
Your pages fail to render because the Jekyll engine cannot find the `home` or `page` layouts referenced in the front matter of `index.md`, `about.md`, or `404.html`.

If we dive into the source code of [the official Hacker theme on GitHub](https://github.com/pages-themes/hacker), we can see that the `_layouts/` directory only contains two files:
- `default.html`
- `post.html`
Because the theme does not ship with `home.html` or `page.html`, Jekyll falls back to rendering nothing for pages requesting those specific layouts.

To fix this, you can create the missing layout files in your local repository's `_layouts/` folder to override the missing layouts and inherit from `default.html`.
1. Create a folder named `_layouts` in your project root if it doesn't already exist.
2. Inside `_layouts/`, create two files: `home.html` and `page.html`.
3. Add the following base code to both files:
```html
layout: default

{{ content }}
```
By wrapping your custom `page` and `home` layouts inside the theme's built-in default layout, Jekyll will properly wrap your Markdown content with the Hacker theme's global styles and structure.

From here, you can customize `_layouts/page.html` and `_layouts/home.html` further to add sidebars, post lists, or metadata as needed.

Hope this post helps you solve the problem and gets your site up and running!