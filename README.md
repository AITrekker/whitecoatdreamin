# whitecoatdreamin.com

Monica's blog — 24 posts, Sept 2023 onward. Jekyll, hosted free on GitHub Pages.
Migrated from WordPress.com in Aug 2026.

## Publishing a post

Add a file to `_posts/` named `YYYY-MM-DD-slug.md`:

```
---
layout: post
title: "Your title"
date: 2026-09-01 10:00:00 -0700
---

First paragraph or two.

<!--more-->

The rest of the post.
```

`<!--more-->` marks where the homepage excerpt stops. Commit and push —
GitHub rebuilds in about a minute. Backdating works: just set an earlier `date`.

Images go in `assets/img/` and are referenced as
`{{ site.baseurl }}/assets/img/filename.jpg`.

## Cutover to the custom domain

1. Add a `CNAME` file containing `whitecoatdreamin.com`
2. Set `baseurl: ""` and `url: "https://whitecoatdreamin.com"` in `_config.yml`
3. Point DNS at GitHub's A records

Full archive of the original WordPress site (including originals of every image)
is in `~/OneDrive/Documents/whitecoatdreamin-backup/`.
