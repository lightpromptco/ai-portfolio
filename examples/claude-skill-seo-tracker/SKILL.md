---
name: seo-keyphrase-tracker
description: Build or refresh an SEO keyphrase tracker dashboard for a website from its CMS (WordPress/Yoast or a sitemap crawl), Search Console or Semrush, and HubSpot. Use when asked for an SEO dashboard, keyphrase audit, keyword cannibalization check, or the monthly SEO tracker refresh.
---

# SEO Keyphrase Tracker

Build a published dashboard that shows how a website's content is optimized, which keyphrases it targets, where those keyphrases rank, and what to fix next. Refreshes update the same dashboard and keep history so trends build over time.

## 1. Check sources before building

List which connectors and credentials are available, then tell the user in one line what the dashboard will and won't include.

| Source | Required? | Provides |
|---|---|---|
| WordPress (REST API + Application Password) | One content source required | Pages/posts, word counts, internal links, Yoast focus keyphrase + SEO/readability scores |
| Sitemap crawl (any platform) | Use when not WordPress | Titles, meta descriptions, H1s, word counts, internal links, noindex/canonical |
| Google Search Console | Recommended (free) | Queries, average position, clicks, impressions per page |
| Semrush Position Tracking API | Optional (costs API units) | Tracked positions, search volume, keyword difficulty, competitors |
| HubSpot | Optional | Page views and form submissions, last 30 days |

**WordPress gotcha:** Yoast fields (`_yoast_wpseo_focuskw`, `_yoast_wpseo_linkdex`, `_yoast_wpseo_content_score`) are not exposed to the REST API by default. If they come back empty, give the user a `functions.php` snippet that registers those three fields with `show_in_rest` and stop that part of the pull rather than reporting zero keyphrases.

**Semrush gotcha:** if a pull fails with "unable to charge units" or similar, keep the last good data, mark it stale with its date, and say so in a banner. Never silently mix fresh and stale numbers.

## 2. Collect and save the data

Save every pull to files (CSV or JSON) and compute numbers in code. Never estimate figures in prose. Keep a dated snapshot of the headline metrics each run for the trends tab.

## 3. Metrics (definitions must match exactly)

- **Published items** — published pages + posts (show the split).
- **Have a focus keyphrase** — items with a non-empty focus keyphrase, as count and %.
- **Distinct focus keyphrases** — unique keyphrases after lowercasing and trimming.
- **SEO score "Good"** — Yoast SEO score Good (or the built-in score when not WordPress), count and %.
- **Indexable items with no inbound links** — not noindex, and zero internal links pointing to them from other published items.
- **Tracked keyphrases on page 1** — position 1–10 in the latest ranking data.
- **Page views / form submissions (30 days)** — from HubSpot, with the exact date range.

## 4. Dashboard tabs

1. **Overview** — stat cards above, source status pills (source, item count, data date, fresh/stale), Yoast SEO and readability score bars for pages vs posts, and a short written summary of the biggest issues.
2. **Trends** — headline metrics across saved snapshots (up to 6 months).
3. **Hub & spoke** — topic clusters: each hub page and the posts that link to it or should.
4. **Gaps & recommendations** — prioritized fixes, each with affected URLs:
   - **Cannibalization:** two or more items sharing a focus keyphrase. Recommend one owner page; re-target or consolidate the rest.
   - **Orphans:** indexable items with no inbound links. Suggest 2–3 source pages to link from.
   - **Missing keyphrases**, **poor scores**, and **quick wins** (ranking positions 11–20).
5. **Keyword map** — keyphrase → assigned URL(s), duplicates flagged.
6. **Keyphrases** — sortable table: keyphrase, URL, score, position, volume, difficulty.
7. **Pages & posts** — every item with all pulled fields; filters for type, score, missing keyphrase, no inbound links.
8. **Rankings** — current positions, change since last snapshot, top-10 and top-100 counts.

Every metric gets an info tooltip saying what it means and where it came from. Add a "How to use" panel for first-time viewers.

## 5. Refreshing

On a refresh: re-pull every available source, add a new snapshot, update the same dashboard (never publish a new one), and update the summary's "Updated" line with what's new and what's stale.

## 6. Privacy

The dashboard contains the client's real site data. Keep it private unless the user shares it. Never reuse one client's data in another client's dashboard or in a demo.
