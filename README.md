# InboxReady — turn CMS exports into sendable HTML

*A tiny, client-side tool that polishes CMS-exported email HTML so it’s safe for inboxes—without changing your layout.*

**Live:** _(add your GitHub Pages URL once the repo is published)_

---

## Why InboxReady?

CRM managers can now self-serve the final “post-CMS” tidy-up that email developers usually do by hand. Drop your HTML in, click **Run fix**, and copy/download the cleaned result.

- **Zero server** — everything runs in your browser (privacy-friendly).
- **Structure-safe** — it only edits inside `<body>` text nodes. No `<tbody>` insertion, no table surgery, no layout changes.
- **Link-safe** — never touches attribute values like `href` or `src`.
- **Adobe Campaign-safe** — leaves `<% … %>` directives (and includes) alone.

---

## What gets fixed (and nothing else)

- **NBSP ties (body only):** configurable spam trigger words are tied to the **next** word with `&nbsp;`.
  - Default list (case-insensitive): `Win, Free, Click, Cash, Discount, Price, Bargain`
  - Example: `Win prizes` → `Win&nbsp;prizes`
- **Special character encodes (body text nodes only):**
  - e.g. `£` → `&pound;`, `€` → `&euro;`, smart quotes → HTML entities, em/en dashes, ellipses, bullets, etc.
  - Existing entities are preserved (no double-encoding).
- **ALT text GBP rule (body only):**
  - Any `£N` in an `<img alt="…">` becomes `NGBP` (e.g., `alt="£129.99"` → `alt="129.99GBP"`).
- **File size meter:**
  - ≤ **90 KB** (green), ≥ **97 KB** (amber), ≥ **100 KB** (red). Quick sanity check for provider limits.
- **Change report:**
  - Summary counters (**NBSP ties / ALT fixes / Special encodings**).
  - Collapsible table listing each special-character encoding with **line:col**, **before → after**, and **context**.

---

## What it will **never** do

- **Never** encode inside attribute values (e.g., `href`, `src`, `style`, `data-*`).
- **Never** modify Adobe Campaign directives like:
  - `<%@ include view='MirrorPageUrl' %>`
  - `<%@ include view='dsgOptOutLinkCPCWB' %>`
  - `<% … %>`
- **Never** add structural markup (`<tbody>`, wrappers, etc.).
- **Never** touch outside `<body>` (your `<head>`, CSS, etc. remain intact).
