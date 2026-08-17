# whitecoatdreamin.com — migration plan and site notes

Last updated 16 Aug 2026.

## Where things are

| | |
|---|---|
| Live site (still WordPress) | https://whitecoatdreamin.com |
| New site (preview) | https://aitrekker.github.io/whitecoatdreamin/ |
| Repo | https://github.com/AITrekker/whitecoatdreamin (public) |
| Full archive of the old site | `~/OneDrive/Documents/whitecoatdreamin-backup/` |
| Local working copy | `~/claude-sessions/Wordpress/whitecoatdreamin` |

## Why we're moving

WordPress.com Personal costs $48 + WA tax (~$53/yr) and is the cheapest plan that
allows a custom domain — the free plan cannot map one. The blog gets a handful of
posts a year, so managed hosting isn't earning its cost.

| | Now | After |
|---|---|---|
| Domains (2, at Squarespace) | $30 | $30 |
| Hosting | ~$53 | $0 |
| **Total** | **~$83/yr** | **$30/yr** |

Dropping `whitecoatdreaming.com` (the unused misspelling) would take it to $15/yr.

## Deadline

**15 Sep 2026** — the WordPress.com plan renews. PayPal has been removed from the
account, so it will lapse rather than auto-renew. There is no fallback: once it
lapses, the old site goes away.

Target DNS cutover is **~1 Sep**, leaving two weeks of buffer. Do not leave it to
the final week — DNS needs slack.

Separate from this: the WordPress.com *domain connection* is paid to 15 Oct, and the
two domain *registrations* run to **11 Aug 2027**. The domains are never at risk here.

## Domains

Both registered at **Squarespace Domains** (ex-Google Domains), $15/yr each,
transfer-locked, expiring 11 Aug 2027.

- `whitecoatdreamin.com` — the live one. Nameservers currently point at WordPress.com,
  so cancelling the plan takes DNS with it. This is why DNS must move first.
- `whitecoatdreaming.com` — unused misspelling, on Google Cloud DNS, parked at Squarespace.

Squarespace provides free DNS management, so **nameservers do not need to move**.
GitHub Pages serves apex domains via plain A records. (Cloudflare Pages would have
required moving nameservers — that's why GitHub Pages won.)

## The stack

Jekyll on GitHub Pages. No Actions workflow, no theme gem, no build config —
GitHub builds Jekyll natively. Nothing to maintain.

- **Layout**: top tabs for pages, left rail listing all posts, single reading column.
  On mobile the rail drops below the content and tabs scroll sideways.
- **Type**: Source Serif 4 for reading, IBM Plex Sans for tabs, dates and rail.
  Self-hosted in `assets/fonts/` (565 KB) — no external dependency.
- **Permalinks**: `/:year/:month/:day/:title/`, identical to WordPress, so old links survive.
- **Excerpts**: `<!--more-->` marks the cut, placed at the first paragraph break past
  ~65 words. Short posts show in full with no "Read more".
- Dark mode via `prefers-color-scheme`. No JavaScript anywhere.

## Content

24 posts (Sept 2023 – June 2026, 10,554 words) and 6 pages.

Kept: About Me, Stories, Videos, Emerald City Angels, Non-Profit, Life as a Page in WA.
Dropped: the stock "About" placeholder, Sample Page, and four dead WooCommerce pages
(Shop, Cart, Checkout, My account) that could never function on a Simple site anyway.

Posts use only paragraphs, headings, images, lists and separators, so they converted
mechanically. The pages use buttons, YouTube embeds and social links and needed CSS
written specifically for those WordPress block classes.

## Images

47 originals, 43 MB → 11 MB (74% smaller) by converting non-transparent PNGs to JPEG
at quality 82 and capping width at 1600px. Genuinely transparent images stayed PNG.
Originals are untouched in the OneDrive backup.

Paths are written as `{{ site.baseurl }}/assets/img/…` so they work both at the preview
URL and at the custom domain.

## Known issues

- **One image is permanently lost**: `IMG_0795.jpg`, previously on the Emerald City Angels
  and Non-Profit pages. It lived on an Azure CDN from an older host that is now offline —
  it was already broken on the live WordPress site before any of this. Those two figures
  were removed. If an original turns up, drop it in `assets/img/` and re-add it.
- **4 Pexels stock photos** were dropped — they were theme leftovers, referenced by nothing.
- **8 of 24 posts** have WordPress featured images. They're unused, since showing thumbnails
  on a third of the entries and blanks on the rest looks worse than showing none.
- WordPress.com **stats history** is not exportable and will be lost.

## Remaining steps

1. Review the preview on desktop and phone; fix whatever needs fixing.
2. Add a `CNAME` file containing `whitecoatdreamin.com`.
3. In `_config.yml`, set `baseurl: ""` and `url: "https://whitecoatdreamin.com"`.
4. At Squarespace DNS, replace the WordPress.com records with GitHub Pages' four A records
   (185.199.108–111.153) plus a `www` CNAME to `aitrekker.github.io`.
5. Enable HTTPS in the repo's Pages settings once DNS resolves.
6. Verify for a few days, then let the WordPress plan lapse on 15 Sep.

Rollback at any point before step 6 is reverting the DNS records; WordPress keeps
serving until 15 Sep regardless.

## Publishing after the move

Add a file to `_posts/` named `YYYY-MM-DD-slug.md` with `layout: post`, a title and a
date, `<!--more-->` where the excerpt should stop, then commit and push. GitHub rebuilds
in about a minute. Backdating is just an earlier `date` value.
