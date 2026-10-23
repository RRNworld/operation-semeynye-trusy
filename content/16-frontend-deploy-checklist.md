# Frontend Deploy Checklist — Ilya Boxers Institute

**Audit date:** 22 October 2026  
**Live host:** `https://ibi.institute`  
**Method:** Read-only GET requests to public pages. No login, no POST/PATCH/DELETE, no admin/API writes, no repository changes.  
**Status:** Audit only. No fixes are claimed. Settings are being updated separately; this document records what the frontend actually served during the audit.

**Scope note:** The Next.js repository is not in this workspace. Component and file locations below are inferred from the rendered output and are marked as such where relevant.

---

## 0. Approved replacement strings used below

These are the draft values from `01-homepage.md` and `03-site-copy-map.md`. Use them consistently; do not paraphrase per page.

- **Site title:** `IBI — Ilya Boxers Institute | Underwear, Indexed`
- **Title template:** `%s | Ilya Boxers Institute`
- **Meta description:** `Ilya Boxers Institute studies, indexes, and makes boxer shorts for people who know that power is never fully dressed. Read the dispatches.`
- **OG title:** `Ilya Boxers Institute — Power Is Never Fully Dressed`
- **OG description:** `A Moscow-born institute and apparel house devoted to the study of boxer shorts. Field notes, an annual index, and a collection of considered essentials.`
- **Organization JSON-LD name:** `Ilya Boxers Institute`
- **Organization JSON-LD alternate name:** `IBI`
- **Organization JSON-LD description:** `Ilya Boxers Institute is a Moscow-born research institute and apparel house devoted to the study and improvement of boxer shorts. It maintains the IBI Index, publishes field notes, and makes a considered collection of underwear.`
- **Organization founding date:** `2026`
- **Primary nav order:** Home, The Index, Research, Open Source, Collection, Contact
- **Utility nav order:** Support, About, Fellows, Stockists, Citations, Sign in, Search
- **Footer copyright:** `© 2026 Ilya Boxers Institute. All rights reserved. With love, made by RRN.world — Rotten Reliable News.`
- **Contact details:** `boxers@rrn.world`; telephone `By appointment`; hours `Monday–Friday, 10:00–18:00 MSK`
- **Exact credit:** `With love, made by RRN.world — Rotten Reliable News.`

---

## 1. Status snapshot

| Route | HTTP | Main observed issue |
|---|---:|---|
| `/` | 200 | Old brand title/meta/OG; old logo; no hero or H1; legacy module labels; old banner to `/index/2025` |
| `/index` | 200 | Legacy sovereignty formula, `Burke Index`, seven sovereignty dimensions |
| `/about` | 200 | Legacy `The Burke Institute Statement` and old geopolitical copy |
| `/contacts` | 200 | Old brand H1, old `info@ibi.institute`, old routing copy |
| `/donate` | 200 | Old donation form with `$50/$100/$250`; no Support intro |
| `/partners` | 200 | Old partner solicitation, old phone `+972502901775`, no stockist empty state |
| `/sources` | 200 | Empty main content |
| `/products` | 200 | Old `Products` title/label; no H1; all six product links lead to HTTP 500 |
| `/research` | 200 | Thin legacy page, `IBI Library`, one article |
| `/osint` | 200 | Thin legacy page, `Osint` label, one article |
| `/read/boxer-sovereignty-index-2026?type=index` | 200 | Old header/footer; `Burke Index` print header; no canonical/H1 |
| `/read/portugal?type=index` | 200 | Old header/footer; `Burke Index` print header; no canonical/H1 |
| `/read/collection-the-index?type=products` | 500 | Product template server error; content present in body |
| `/read/research-method-2026?type=research` | 200 | Old header/footer; `Burke Index` print header; no canonical/H1 |
| `/definitely-not-a-real-page-xyz` | 404 | Correct status, minimal old-brand 404 body |
| `/search` | 200 | Legacy labels `All section`, `By relevation`; no H1/empty state |
| `/sign-in` | 404 | Route absent; header sign-in control is a JS-only `div` |

Additional route checks: all six `collection-*` product read pages return 500; `/index/2025` returns 200; `/index-2025` returns 404; `/index/2026` and `/index/2026/portugal` return 404; `/read/this-slug-does-not-exist-12345` returns 200.

---

## 2. P0 blockers

### P0-01 — Sitewide metadata, OG, and Twitter defaults are still the old brand

**Pages:** `/`, `/index`, `/about`, `/contacts`, `/donate`, `/partners`, `/sources`, `/products`, `/research`, `/osint`, `/search`, `/404`, `/experts`, `/read/*`

**Observed:**
- `/` serves `<title>IBI International Burke Institute</title>`.
- `/` serves `<meta name="description" content="IBI International Burke Institute"/>`.
- `/` serves `og:title`, `og:site_name`, and `twitter:title` as `IBI International Burke Institute`.
- `/` serves `og:image` and `twitter:image` as `https://ibi.institute/_next/static/media/ibi-logo.2002f510.webp`.
- Inner pages use titles such as `Index | IBI International Burke Institute`, `Products | IBI International Burke Institute`, `Osint | IBI International Burke Institute`, and `About | IBI International Burke Institute`.
- `/contacts` is inconsistent: `Contact Us - International Burke Institute`.
- `/donate` has no page-specific title; it serves the bare old brand.
- Read pages inherit `og:site_name` `IBI International Burke Institute` even when the article OG title is correct.

**Expected:**
- Root title: `IBI — Ilya Boxers Institute | Underwear, Indexed`.
- Title template: `%s | Ilya Boxers Institute`.
- Global description: `Ilya Boxers Institute studies, indexes, and makes boxer shorts for people who know that power is never fully dressed. Read the dispatches.`
- OG title: `Ilya Boxers Institute — Power Is Never Fully Dressed`.
- OG description: `A Moscow-born institute and apparel house devoted to the study of boxer shorts. Field notes, an annual index, and a collection of considered essentials.`
- Twitter/X cards mirror the OG title and description.
- Replace the old logo social image with the new default card; page-level OG titles append the page name.

**Deploy:** Root layout metadata and per-page `generateMetadata`; a CMS settings update alone will not fix hardcoded defaults.

---

### P0-02 — JSON-LD Organization is entirely the old entity and contains the old member array

**Pages:** All pages that emit Organization schema, including `/`, `/index`, `/about`, `/contacts`, `/products`, and read pages.

**Observed:**
```
"name":"IBI International Burke Institute"
"description":"IBI International Burke Institute is an independent research institute specializing in geopolitics, macroeconomics, sovereignty and international relations, and the publisher of the Burke Sovereignty Index (BSI)."
"foundingDate":"2023"
"produces":[{"@type":"Dataset","name":"Burke Sovereignty Index (BSI)","url":"https://ibi.institute/index","description":"Composite index measuring national sovereignty across ~160 countries."},{"@type":"Dataset","name":"Research Units","url":"https://ibi.institute/research"}]
"subOrganization":[{"@type":"Organization","name":"BSI Team"},{"@type":"Organization","name":"Research Units"},{"@type":"Organization","name":"Departments"}]
"member":[{"@type":"Person","name":"Najah M. ADAM","url":"https://ibi.institute/experts/najah-m-adam"}, ...]
```
The `member` array contains 50 `Person` entries in the rendered homepage JSON-LD. Old expert pages also emit `Person` schema.

**Expected:**
- `name`: `Ilya Boxers Institute`.
- `alternateName`: `IBI`.
- New description from Section 0.
- `foundingDate`: `2026`.
- `produces`: `Product — Boxer shorts`, `Product — Loungewear`, `CreativeWork — The IBI Index`.
- Remove `member`, `subOrganization`, old `Dataset` produces, and old `award`, `founder`, and geopolitical `knowsAbout` values.
- No `offers`, `priceRange`, `aggregateRating`, `review`, or `award` until commerce fields are approved.
- Keep `Organization.url` as `https://ibi.institute`; do not point schema at the API or admin host.

**Deploy:** Organization component or API serializer, plus the layout that injects it.

---

### P0-03 — The old expert roster is still public, indexable, and in the sitemap

**Pages:** `/experts`, `/experts/najah-m-adam`, and 347 other `/experts/:slug` URLs.

**Observed:**
- `/experts` returns 200 and lists the old expert roster.
- `/experts/najah-m-adam` returns 200 with title `Najah M. ADAM | IBI International Burke Institute`, description `Development Specialist, Researcher, Foreign Policy Analyst, Horn of Africa Observer`, canonical `https://ibi.institute/experts/najah-m-adam`, and `robots: index, follow`.
- The profile emits `Person` schema.
- `sitemap-pages.xml` contains 348 `/experts/` URLs.
- The homepage Organization `member` array still links to 50 of these profiles.

**Expected:**
- Archive old expert profiles; do not bulk redirect them to `/experts`.
- Serve 410 or a genuine noindex archive response.
- Remove all `/experts/:slug` URLs from the sitemap.
- Remove the Organization `member` array and Person schema.
- If no approved Fellows exist at launch, `/experts` publishes the explicit empty state, not the old roster. Do not relabel real experts as Fellows without written consent.

**Deploy:** Frontend profile template, API serializers, sitemap generator, plus content unpublishing.

---

### P0-04 — Header and footer still use the old logo and wordmark

**Pages:** Every audited page.

**Observed:**
- Header and footer image: `alt="IBI International Burke Institute logo"`.
- Header asset: `/_next/static/media/logo.c1bd0495.png`, rendered at 936 × 216.
- Footer uses the same old asset and alt text.
- No new wordmark is present.

**Expected:**
- Retain the IBI monogram.
- Swap the old wordmark for `Ilya Boxers Institute`.
- Header logo alt: `Ilya Boxers Institute — home.`
- Favicon alt: `IBI monogram.`
- If the header asset contains the full name, use `Ilya Boxers Institute` only.

**Deploy:** Header/footer components and `/public` assets.

---

### P0-05 — Favicon and icon metadata are incomplete and not verifiably new

**Pages:** Every audited page.

**Observed:**
- Only one icon declaration is present:
  `<link rel="icon" href="/favicon.ico" type="image/x-icon" sizes="32x32"/>`
- `/favicon.ico` returns 200, 32 × 32 ICO, one image.
- No `apple-touch-icon` and no web manifest link are present in the rendered HTML.

**Expected:**
- Retained IBI monogram favicon and icon set.
- Apple touch icon.
- Web manifest with name `Ilya Boxers Institute`.
- Verify visually that the favicon is the retained monogram, not the old wordmark or old institutional mark.

**Deploy:** `/public`, root metadata, manifest route.

---

### P0-06 — Primary navigation labels and order are still legacy

**Pages:** Every audited page.

**Observed header order:**
`Donate`, `About`, `Partners`, `Sources`, `sign in`, then primary: `Home`, `Index`, `Research`, `Osint`, `Products`, `Contact us`, `Index 2025` (the last is highlighted with class `hl`).

Missing labels include `The Index`, `Open Source`, `Collection`, `Support`, `Fellows`, `Stockists`, `Citations`.

**Expected primary order:** Home, The Index, Research, Open Source, Collection, Contact.  
**Expected utility order:** Support, About, Fellows, Stockists, Citations, Sign in, Search.  
`Index 2026` should be visible near the front of the primary nav during launch; it should point to `/index`, not `/index/2025`. Do not add an `Experts` label back into public navigation.

**Deploy:** Header component and mobile menu.

---

### P0-07 — Footer copyright is old, misspelled, and the RRN.world credit is absent

**Observed on `/`, `/index`, `/about`, `/products`, `/read/*`, and every audited page:**
`© 2025 The Internationalal Burke Institute. All rights reserved.`

The typo `Internationalal` is live. `RRN.world` appears zero times on `/`, `/about`, and `/products`; the only occurrences observed were inside article bodies.

**Expected:**
`© 2026 Ilya Boxers Institute. All rights reserved. With love, made by RRN.world — Rotten Reliable News.`

Repeat the exact credit in the colophon. Use the current year at render time.

**Deploy:** Footer component or settings-backed footer, but the current output is hardcoded enough that a settings-only edit is not sufficient.

---

### P0-08 — Footer `Index` link still points to an old country page

**Observed:**
- Footer `Index` points to `/index/germany`.
- `https://ibi.institute/index/germany` returns 200 with title `Germany Sovereignty Index | IBI International Burke Institute` and H1 `GERMANY SOVEREIGNTY INDEX`.
- Footer also links `Index 2025` to `/index/2025`.

**Expected:**
- Footer `Index` points to `/index`.
- Footer label is `The Index` or `Index 2026`, not `Index 2025`.
- Retire old `/index/:country` pages. If an exact new equivalent exists, 301 to `/read/:country?type=index`; otherwise 410. Do not leave the old country template live under the new navigation.

**Deploy:** Footer component, redirects, and content retirement.

---

### P0-09 — Every Collection product template returns HTTP 500

**Pages:**
- `/read/collection-the-index?type=products`
- `/read/collection-the-quorum?type=products`
- `/read/collection-the-field-standard?type=products`
- `/read/collection-the-backchannel?type=products`
- `/read/collection-the-reading-room?type=products`
- `/read/collection-the-protocol?type=products`

**Observed:**
- All six return `HTTP/2 500`, `content-type: text/html; charset=utf-8`, `x-powered-by: Next.js`.
- The response body still contains the product article copy, so the record exists and the failure is in rendering or a page-level dependency.
- The product page also renders the old print header text `Burke Index`.
- `/products` itself returns 200, so the Collection is browsable but no product can be opened.

**Expected:**
- All six product pages return 200.
- They carry the correct product title, meta description, canonical, and new header/footer.
- No `Burke Index` print header.
- No price, availability, or performance claim until approved.

**Deploy:** Product route/template fix. This is launch-critical and cannot be fixed through settings.

---

### P0-10 — `/index` is still the old sovereignty formula page

**URL:** `https://ibi.institute/index`

**Observed:**
- Title: `Index | IBI International Burke Institute`.
- Body includes `About Index`, `Formula and Data Sources for Generating. The Sovereignty Index`, `The Sovereignty Index (Burke Index)`, seven sovereignty dimensions, and a `100-700` cumulative score.
- No H1.
- The page is linked from the primary nav and is the target of the `Index` label.

**Expected:**
- H1: `The Index 2026`.
- Intro: `The Institute’s annual reading of the boxer short: construction, composure, and conduct under supervision. Rankings are conditions, not people, and they are revised when the evidence changes.`
- Sections: `The 2026 edition`, `Method`, `Rankings`, `Revisions`, `Archive`.
- BSI expands as `Boxer Sovereignty Index`.
- Retirement notice: `The 2025 edition has been retired. This is the 2026 edition.`

**Deploy:** Frontend template and the settings field it reads. If the page reads `aboutIndex`, confirm which duplicate key is wired before clearing `about-index`.

---

### P0-11 — `/index/2025` is live and unredirected

**URLs:** `https://ibi.institute/index/2025`, `https://ibi.institute/index-2025`

**Observed:**
- `/index/2025` returns 200, title `Index | IBI International Burke Institute`, and the body shows only `INDEX 2025`.
- `/index-2025` returns 404, not 301.
- Header, footer, and homepage banner link to `/index/2025`.

**Expected:**
- `/index/2025` and `/index-2025` both 301 to `/index`.
- Remove the `Index 2025` nav item and all `/index/2025` links.
- Show the retirement notice on `/index`.
- Remove `/index/2025` from the sitemap.

**Deploy:** Redirects, nav/footer, sitemap.

---

### P0-12 — Old banner links and old banner creatives are still live

**Observed:**
- Homepage main banner: `https://ibi.institute/index/2025?utm_source=site&utm_medium=banner&utm_content=mainpage`.
- Footer banner: `https://ibi.institute/index?utm_source=site&utm_medium=banner&utm_content=footer`.
- Every read page sidebar banner: `https://ibi.institute/read/the-paradox-of-economic-sovereignty-why-is-rich-norway-on-a-dubious-track-but-tiny-liechtenstein-is-not?utm_source=site&utm_medium=banner&utm_content=footer`.
- The old article URL returns 200 but renders generic homepage modules, not the article.
- Banner images use `alt=""`.

**Expected:**
- Remove the old main-page banner, footer banner, and old article banner.
- Replace with the approved launch records from `07-settings-and-banners.md`:
  - `index-2026-launch` → `/index`; alt `Boxer Sovereignty Index 2026 — Ilya Boxers Institute.`
  - `collection-launch` → `/products`; alt `The Collection — boxer shorts and loungewear by Ilya Boxers Institute.`
  - `quarterly-dispatch` → `/#quarterly-dispatch`; alt `The Quarterly Dispatch — the Ilya Boxers Institute newsletter.`
- Keep banners inactive until image, schedule, target, and alt text are approved.

**Deploy:** Settings `/banners`, banner serializer, and removal of hardcoded old banner components.

---

### P0-13 — Unknown and archived `/read/:slug` URLs return HTTP 200 with fallback content

**URLs:**
- `/read/the-paradox-of-economic-sovereignty-why-is-rich-norway-on-a-dubious-track-but-tiny-liechtenstein-is-not`
- `/read/this-slug-does-not-exist-12345`

**Observed:**
- Both return 200.
- Both serve the site title `IBI International Burke Institute`.
- The body renders generic homepage modules (`Latest`, product tiles, `Our products`, `Research`) instead of an article or a 404.
- No canonical. `robots` remains `index, follow`.

**Expected:**
- Archived old article: 410.
- Unknown slug: real 404 with the approved 404 copy.
- No 200 fallback for missing article records.
- Remove the old banner pointing to the archived article.

**Why P0:** every stale `/read/*` backlink can return a 200 soft 404, which is an indexing and trust problem, and the current old banner actively sends readers there.

---

### P0-14 — Route mismatch: `/read/:slug` vs planned `/index/2026/:slug`

**Observed:**
- `/read/portugal?type=index` returns 200.
- `/read/boxer-sovereignty-index-2026?type=index` returns 200.
- `/index/2026` returns 404.
- `/index/2026/portugal` returns 404.
- `/index/2026/boxer-sovereignty-index-2026` returns 404.
- Read pages have no canonical link.
- `03-site-copy-map.md` also describes `/read/:slug` as a reading edition whose canonical is `/articles/:slug`, but `/articles` returns 404.

**Expected:** Choose one canonical route before launch and apply it consistently:
- If the planned route is `/index/2026/:slug`, implement it, 301 the corresponding `/read/:slug` URLs to it, set self-canonicals, update all internal links, and put the new URLs in the sitemap.
- If `/read/:slug` remains, update the copy map and set canonicals to the chosen canonical article route. Do not run both routes without redirects and canonicals.

**Deploy:** Route/template decision, redirects, canonical tags, sitemap, internal links. This needs an explicit owner decision before redirect work.

---

### P0-15 — Sitemap and robots still expose the archive and omit new product pages

**Observed robots.txt:**
```
User-agent: *
Content-Signal: search=yes,ai-train=yes
Allow: /
```
No `Sitemap:` directive. No disallow for internal search, account, admin, or API paths.

**Observed sitemap:**
- `/sitemap.xml` returns a sitemap index with three children.
- `sitemap-pages.xml`: 358 URLs, including 348 `/experts/` URLs, `/index/2025`, and old pages.
- `sitemap-indexes.xml`: 23 URLs, including `/index/2025`, 20 country profiles, and the BSI.
- `sitemap-posts.xml`: 1 URL, only `/read/research-method-2026`.
- The 29 new published items are BSI 2026, 20 country profiles, 6 Collection products, Research, and Open Source. The sitemap covers BSI + 20 countries + Research = 22. The six product pages and the Open Source page are missing.

**Expected:**
- `Sitemap: https://ibi.institute/sitemap.xml` in robots.txt.
- Disallow internal search, account, admin, and API paths where applicable.
- Sitemap contains current public pages only: all 29 new items, no `/index/2025`, no old expert profiles, no archived articles, no old country template pages.
- Canonical host remains `https://ibi.institute`.

**Deploy:** Robots route, sitemap generators, and content visibility flags.

---

### P0-16 — Homepage lacks the approved hero and still uses legacy module labels

**URL:** `https://ibi.institute/`

**Observed:**
- No `<h1>` on the page.
- No `Power is never fully dressed.`, no hero eyebrow, no hero subhead, no `Read the latest Index` or `View the Collection` CTAs.
- First major block is `Latest`.
- Sections include `Our products` and `Research`, not `Dispatches`, `The Collection`, and `Field Notes`.
- No `The Quarterly Dispatch` newsletter block.
- The page still includes UN/UNICEF/World Bank/UNESCO/SIPRI source logos.

**Expected:**
- Hero eyebrow: `The Ilya Boxers Institute — Moscow`.
- H1: `Power is never fully dressed.`
- Subhead: `Ilya Boxers Institute studies, indexes, and makes the garment beneath the garment. The findings are available in the Collection.`
- Primary CTA: `Read the latest Index`; secondary CTA: `View the Collection`.
- Sections: `Dispatches — Recent from the Institute`; `Featured Study`; `Observations — From the field`; `The Collection — Garments for the unobserved hours`; `Field Notes`; `The Quarterly Dispatch`.
- Remove old source-logo block or replace it with current citations only.

**Deploy:** Homepage template and section components. This is the primary brand surface and should not launch with only metadata fixes.

---

### P0-17 — Old contact data is still public

**Pages:** `/contacts`, `/partners`

**Observed:**
- `/contacts` H1: `Contact the International Burke Institute`.
- `/contacts` meta description: `Get in touch with the International Burke Institute. Fellowship applications, partnership proposals, media inquiries and general questions.`
- `/contacts` `mailto:info@ibi.institute`.
- `/partners` `mailto:info@ibi.institute` and `tel:+972502901775`.
- `/partners` hours: `Monday–Friday, 11:00–19:00`.

**Expected:**
- `boxers@rrn.world` for general, press, stockist, order, and privacy correspondence.
- Telephone: `By appointment`; do not render a `tel:` link for the old number.
- Hours: `Monday–Friday, 10:00–18:00 MSK`.
- Institutional address: `Ilya Boxers Institute, Monogram House, Moscow`.
- Remove old `info@ibi.institute` and old phone from settings, schema, and page templates; confirm any old institutional address is also gone.

**Why P0:** the old phone number is still publicly linked and the old mailbox is still the primary contact route. Confirm removal before launch.

---

### P0-18 — Read pages still print `Burke Index` in the article header

**Pages:** All `/read/:slug` pages, including `/read/boxer-sovereignty-index-2026?type=index`, `/read/portugal?type=index`, and `/read/research-method-2026?type=research`.

**Observed:**
- The print/header block contains the literal `Burke Index`.
- The same block uses the old logo SVG and `alt="IBI"`.
- The page title/body may be new, but the article chrome is old.

**Expected:**
- Use `Ilya Boxers Institute` or `IBI` in the print header.
- No `Burke Index`, `Burke Sovereignty Index`, or old institutional wordmark in article chrome.
- Keep the monogram if it is the retained mark.

**Deploy:** Read/article template.

---

## 3. P1 copy and UX

### P1-01 — `/about` is still the old Burke statement

**Observed:** `The Burke Institute Statement`, an Edmund Burke quote, `The Burke Institute is a community of leading experts...`, old events, publishing, and sovereignty language. The page has no H1.

**Expected:** H1 `About the Institute`; intro from `03-site-copy-map.md`: `Ilya Boxers Institute is a research institute and apparel house devoted to the study and improvement of boxer shorts. It was formerly IBI International Burke Institute; the initials and monogram were retained.` Sections: `The mandate`, `The founder`, `The method`, `The monogram`, `Disclosure`, `Credits`. Founder copy must follow `02-founder-about.md`; remove the old quote and old programme claims.

---

### P1-02 — `/contacts` still uses old copy and old routing categories

**Observed:** H1 and description are old; the page lists one generic email and office hours, with no press, stockist, order/return, privacy, or institutional-address sections. No contact form sections from the draft.

**Expected:** H1 `Contact`; standfirst and sections from `06-secondary-pages.md` §3: General enquiries, Press, Stockists, Orders and returns, Institutional address, Send a message. Use `boxers@rrn.world` and the routing labels from the draft.

---

### P1-03 — `/partners` is still a partner solicitation page

**Observed:** `If you would like to become a partner, please contact us for more information`, old email, old phone, old hours. No H1. No empty state.

**Expected:** H1 `Stockists`; intro from `06-secondary-pages.md` §4; explicit launch empty state `No stockists are listed at launch.`; sections `Confirmed stockists`, `Stockist standard`, `Wholesale enquiries`, `Territories`. No placeholder logos or implied partnerships.

---

### P1-04 — `/sources` is effectively empty

**Observed:** Title `Sources | IBI International Burke Institute`; main content is blank apart from global chrome.

**Expected:** H1 `Citations`; intro from `06-secondary-pages.md` §5; sections `Index 2026`, `Field notes`, `Corrections`, `Citation style`, `Archive`. Replace the old source list; do not carry forward old geopolitical citations.

---

### P1-05 — `/donate` still shows old donation amounts and form

**Observed:** `Make a one-time gift`, `Make a monthly gift`, `$50`, `$100`, `$250`, `$`, `Your information`, `Make a gift`. No H1, no Support intro.

**Expected:** H1 `Support the Institute`; standfirst and body from `06-secondary-pages.md` §6; no amounts, tiers, processing fees, or donor names until approved. Use `Support` as the public label, not `Donate`.

---

### P1-06 — `/research` and `/osint` are thin legacy pages

**Observed:**
- `/research`: title `Research | IBI International Burke Institute`; body shows `Research`, `IBI`, `Library`, and one article. No new intro or section structure.
- `/osint`: title `Osint | IBI International Burke Institute`; body shows `Osint` and one article. No `Open Source` H1 or intro.

**Expected:**
- `/research`: H1 `Research`; intro from `03-site-copy-map.md`; sections `Current studies`, `Method`, `The standing panel`, `Archive`, `Corrections`.
- `/osint`: H1 `Open Source`; intro from `03-site-copy-map.md`; sections `Open-source briefs`, `Verification`, `Reading list`, `Archive`. Remove intelligence-service framing.

---

### P1-07 — `/products` lacks the Collection page copy

**Observed:** Title `Products | IBI International Burke Institute`; no H1; label `Products`; six product cards; a `Latest` sidebar. Product links go to HTTP 500 pages.

**Expected:** H1 `The Collection`; intro: `The Institute’s applied research: boxer shorts and loungewear made to the standard of the Index, not the season. A small collection, one standard, no visible argument.` Sections for the six products, `Fit and care`, `Sizing`. No prices or availability until approved.

---

### P1-08 — The 404 page is minimal and still wrapped in old chrome

**Observed:** `/definitely-not-a-real-page-xyz` returns 404 (correct), title `Page not found`, body `404 | Page not found - Go to home page`. No standfirst, current-section links, search, or contact route. Header/footer still old brand.

**Expected:** H1 `Page not found`; standfirst and body from `06-secondary-pages.md` §12; current-section links (The Index 2026, Research, Open Source, The Collection, Dispatches, Notices, Fellows, Stockists, Citations, Contact); Search and Contact CTAs. Keep the real 404 status and `noindex`.

---

### P1-09 — `/sign-in` is absent

**Observed:** `/sign-in` returns 404. The header `sign in` control is a `div.login-btn` with no href, not a link to the route.

**Expected:** `/sign-in` page with H1 `Sign in`, standfirst, account access, reset access, account help, and privacy copy from `06-secondary-pages.md` §13. Casing: `Sign in`. Do not rename the auth path.

---

### P1-10 — `/search` still uses legacy labels and has no approved empty state

**Observed:** Title `Search | IBI International Burke Institute`; labels `All section`, `All time`, `By date`, `By relevation`; results area shows `Found ...`. No H1, no empty-query copy, no no-results copy. The page is client-rendered; static HTML alone does not prove search behavior.

**Expected:** H1 `Search the Institute`; standfirst and field copy from `06-secondary-pages.md` §11; filters `All, Research, Open Source, Collection, Dispatches, Notices, Citations`; empty-query heading `Nothing searched yet.`; no-results heading `No results.`; fix `By relevation` to `By relevance`; exclude archive material at query level.

---

### P1-11 — Footer newsletter is only an input and a button

**Observed:** Footer HTML contains `placeholder="Subscribe to email updates"` and a `Subscribe` button. No `The Quarterly Dispatch` heading, no newsletter copy, no fine print.

**Expected:** Heading `The Quarterly Dispatch`; copy `One letter each quarter: new field notes, index revisions, and occasional notices from the Collection. No campaign literature.`; field label `Email address`; button `Subscribe`; fine print `By subscribing, you agree to receive the Quarterly Dispatch. Unsubscribe at any time.` Use the exact copy from `01-homepage.md` and `07-settings-and-banners.md`.

---

### P1-12 — Banner and product-card alt text is missing or generic

**Observed:**
- Homepage main banner `<img ... alt="">`.
- Footer banner `<img ... alt="">`.
- Read-page sidebar banner `<img ... alt="">`.
- Product-card images use `alt="title"` (literal string).

**Expected:**
- Index banner alt: `Boxer Sovereignty Index 2026 — Ilya Boxers Institute.`
- Collection banner alt: `The Collection — boxer shorts and loungewear by Ilya Boxers Institute.`
- Newsletter banner alt: `The Quarterly Dispatch — the Ilya Boxers Institute newsletter.`
- Product images use the product name, e.g. `The Index — boxer short`.
- Decorative images may use empty alt only when genuinely decorative; meaningful banners must not.

---

### P1-13 — Footer legal and utility links are absent

**Observed:** No footer links to `/privacy`, `/terms`, `/cookies`, `/shipping-returns`, `/accessibility`, or `/legal`.

**Expected:** Add the legal links from `09-legal-and-emails.md`: Privacy Policy, Terms of Service, Cookie Policy, Shipping & Returns, Accessibility, Legal Notice. Keep the RRN.world credit in the same footer block.

---

### P1-14 — Read-page breadcrumbs point to a 404 and use `Article` as a section

**Observed:**
- Read-page breadcrumb second item is `Article` and its URL is `https://ibi.institute/read`, which returns 404.
- The JSON-LD `BreadcrumbList` uses the same `Article` item and `/read` URL.
- The visible print header says `Burke Index`.

**Expected:** Breadcrumb section label should match the content type: `The Index`, `Research`, `Open Source`, or `The Collection`, with a valid section URL. JSON-LD breadcrumb items must resolve. Print header per P0-18.

---

### P1-15 — BSI article still contains the old `Burke Sovereignty Index` expansion

**Observed:** `/read/boxer-sovereignty-index-2026?type=index` contains: `The former Burke Sovereignty Index ranked approximately 160 countries across seven dimensions. The initials, the dimensions, and the committee structure were retained.`

**Expected:** Use the approved rename narrative without leaving the old expansion in public copy. If the historical sentence is intentionally retained, obtain editorial approval and ensure it cannot be read as the current name. Otherwise replace with `The former index` or `Boxer Sovereignty Index`.

---

### P1-16 — Most public pages have no H1

**Observed:** Zero `<h1>` on `/`, `/index`, `/about`, `/products`, `/research`, `/osint`, `/search`, `/404`, and all audited `/read/*` pages. `/contacts` and `/experts/najah-m-adam` each have one.

**Expected:** Exactly one H1 per page, using the approved page H1s in `03-site-copy-map.md`. Convert the current visual title `div`s or add a semantic H1 without changing layout.

---

### P1-17 — Footer support label and line are still the old solicitation

**Observed:** Footer button text `DONATE`; line `We’d be grateful for your donation — it helps us grow, improve the service and keep making it better for you!`

**Expected:** Public label `Support`; line from `07-settings-and-banners.md` §3.5: `Support the Institute. The Index is published either way.` Keep the field key if the CMS uses one; change the public label.

---

### P1-18 — RRN.world credit is not in the footer or colophon

**Observed:** The exact credit appears inside product article bodies, but not in the global footer, About page credits, or read-page colophon.

**Expected:** Exact line `With love, made by RRN.world — Rotten Reliable News.` in the global footer/copyright and in the colophon on every page. About credits should also carry `Brand strategy, identity, and copy: RRN.world — Rotten Reliable News.`

---

## 4. P2 polish

### P2-01 — Public HTML is uncached

**Observed:** `/`, `/index`, `/products`, and `/read/boxer-sovereignty-index-2026` return `cache-control: private, no-cache, no-store, max-age=0, must-revalidate` and `cf-cache-status: DYNAMIC`.

**Expected:** Public editorial pages should use a deliberate ISR/cache policy with revalidation, not blanket `no-store`. Keep account, search, and form pages private. Purge Cloudflare after deploy.

---

### P2-02 — Canonical URLs are missing on almost every page

**Observed:** `/contacts` has `https://ibi.institute/contacts`. `/`, `/index`, `/about`, `/products`, `/research`, `/osint`, `/search`, and all read pages have no canonical link. Read-page `og:url` includes the query string, e.g. `https://ibi.institute/read/portugal?type=index`.

**Expected:** Self-canonical on every indexable page; one canonical article URL; strip tracking/query variants where they do not change content. Resolve together with P0-14.

---

### P2-03 — OG image strategy is still the old logo

**Observed:** Default OG/Twitter image is `ibi-logo.2002f510.webp`, dimensions 2532 × 1334. No dedicated Index 2026 or Collection card. Article pages use content images as OG images.

**Expected:** Default 1200 × 630 card; Index 2026 card; Collection card; article cards at the required dimensions. No old wordmark, maps, or geopolitical imagery. Preserve alt text in the asset metadata where supported.

---

### P2-04 — Redirect table is not implemented

**Observed:**
- `/index/2025/` returns 308 to `/index/2025`; `/index/2025` is 200.
- `/index-2025` returns 404.
- `/about/products`, `/about/research`, `/about/osint`, `/about/index`, `/about/experts`, `/about/partners`, `/about/sources` all return 404.
- `/category/osint` and `/category/research` return 404.
- `http://ibi.institute/` and `https://www.ibi.institute/` correctly 301 to `https://ibi.institute/`.

**Expected:** Implement the Section 5 redirect map: 301 exact equivalents, 410 archived material, one hop only. Verify trailing-slash behavior after the final rules are loaded. Do not mass-redirect old articles to `/products`.

---

### P2-05 — Structured data is incomplete on read pages

**Observed:** Read pages emit only `BreadcrumbList`. The breadcrumb references `https://ibi.institute/read`, which returns 404. No `Article` schema; no Product schema on product pages (and product pages are 500).

**Expected:** Valid `BreadcrumbList` with resolving URLs; `Article` schema for dispatches/field notes; Product schema only for approved Collection records, with no offers/ratings until commerce fields are approved. Validate with a schema validator after deploy.

---

### P2-06 — Image dimensions and loading behavior need review

**Observed:** Banner images are raw `<img>` elements without width/height or lazy loading. Product and card images use Next.js `fill`; some product-card alts are `title`. The logo has explicit 936 × 216 dimensions.

**Expected:** Reserve dimensions for banners to avoid CLS; lazy-load below-fold imagery; prioritize the hero/LCP image; use meaningful alt text; confirm no layout shift at 390 px, 768 px, and 1440 px.

---

### P2-07 — Analytics and consent need verification

**Observed:** Google Analytics `G-WLRFXXL39W` is present in the rendered RSC payload. A Google site verification meta tag is also present.

**Expected:** Confirm GA only loads under the approved consent configuration in `09-legal-and-emails.md`; no duplicate tags; no analytics on account/private routes without a basis. Keep the Search Console verification token property-specific; do not invent or rotate it.

---

### P2-08 — Sitemap coverage and lastmod need a final check

**Observed:** 22 of the 29 new items are represented; six product pages and the Open Source page are absent. `sitemap-pages.xml` includes 348 old expert URLs. `sitemap-indexes.xml` includes `/index/2025`.

**Expected:** 29 of 29 new items; no archived experts, old articles, old country pages, or `/index/2025`; accurate `lastmod`; no admin/API URLs. Confirm all sitemap URLs return 200 after redirects are live.

---

### P2-09 — Planned `/articles` and `/news` routes are missing

**Observed:** `/articles` and `/news` return 404. The copy map expects Dispatches and Notices. `/read` also returns 404.

**Expected:** If these routes are in launch scope, add them with the approved copy and navigation. If they are post-launch, document the omission and ensure no nav or sitemap link points to them. `/read` should not be a public index unless it is intentionally built.

---

## 5. Deploy order

### Step 1 — API settings first

1. Apply the approved `/settings` values: site name, meta defaults, contact fields, footer fields, `about`, `aboutIndex`, `about-research`, `donateText`, `footerDonateText`, `aboutData`, `partnersData`, `sourcesData`, `banners`, `metaTags`, and visibility flags.
2. Update API defaults and serializers for public name, description, contact data, and Organization JSON-LD. Remove the Organization `member` array and old `Person` serialization.
3. Keep new banners inactive until image, schedule, target, and alt text are approved.
4. Verify the API GET responses contain the new strings and no old expert array. Do not assume a settings edit fixes hardcoded metadata, nav, or templates.

### Step 2 — Frontend deploy

1. Root metadata: title, template, description, OG, Twitter, canonical policy.
2. Logo, wordmark, favicon, apple-touch icon, and manifest.
3. Header, footer, desktop nav, mobile nav, footer legal links, newsletter block.
4. Homepage hero and section labels.
5. `/index`, `/about`, `/contacts`, `/partners`, `/sources`, `/donate`, `/products`, `/research`, `/osint`, `/search`, `/404`, `/sign-in`.
6. Fix the product template 500 and the `/read/:slug` fallback/404 behavior.
7. Remove the old `Burke Index` print header and all old banner components.
8. Implement the chosen `/read/:slug` vs `/index/2026/:slug` canonical route.

Deploy target routes before loading redirects to them. Do not activate old banners as a fallback.

### Step 3 — Redirects, sitemap, robots

1. 301 `/index/2025`, `/index/2025/`, and `/index-2025` to `/index`.
2. 301 the `/about/*` subpages to their current equivalents (`/products`, `/research`, `/osint`, `/index`, `/experts`, `/partners`, `/sources`) where the equivalent exists.
3. 301 old `/index/:country` pages to `/read/:country?type=index` only where the new profile is the exact equivalent; otherwise 410.
4. 410 archived old articles and old expert profiles; do not mass-redirect them to `/products` or `/experts`.
5. Load the chosen `/read/:slug` → `/index/2026/:slug` redirects if that route is selected; one hop only.
6. Rebuild the sitemap with all 29 new items and no archive/old-expert URLs.
7. Update robots.txt with the sitemap directive and appropriate disallows.
8. Purge the Cloudflare cache for HTML and metadata routes.

### Step 4 — Search Console

1. Verify the `https://ibi.institute` property if not already verified.
2. Submit `https://ibi.institute/sitemap.xml`.
3. Inspect `/`, `/index`, `/products`, one product page, one country profile, and the BSI article; request indexing for the corrected URLs.
4. Do not use Change of Address; the domain is unchanged.
5. Monitor Coverage, Page Indexing, and Enhancements daily for the first week.

### Step 5 — QA

1. Run the route matrix and redirect checks in Section 6.
2. Grep the rendered pages for old-brand strings; allow only approved historical/legal mentions.
3. Validate JSON-LD: no `member`, no old Person schema, correct name/description/foundingDate/produces.
4. Test the mobile nav at 390 px, 768 px, and 1440 px.
5. Check the 404, unknown `/read` slug, `/index/2025`, and product pages.
6. Check OG/Twitter cards and image dimensions.
7. Test the newsletter and contact forms in staging only; do not POST to production during this audit.

---

## 6. Verification commands and checks

### 6.1 Route status matrix

```bash
for u in / /index /about /contacts /donate /partners /sources /products /research /osint \
  '/read/boxer-sovereignty-index-2026?type=index' \
  '/read/portugal?type=index' \
  '/read/collection-the-index?type=products' \
  '/read/research-method-2026?type=research' \
  /definitely-not-a-real-page-xyz /search /sign-in; do
  printf '%-72s ' "$u"
  curl -sS -o /dev/null -w '%{http_code}\n' "https://ibi.institute$u"
done
```

Expected after fixes: collection product URL 200; `/sign-in` 200; the fake path 404; `/index/2025` not 200.

### 6.2 Product pages

```bash
for p in index quorum field-standard backchannel reading-room protocol; do
  printf '%-20s ' "$p"
  curl -sS -o /dev/null -w '%{http_code}\n' \
    "https://ibi.institute/read/collection-the-$p?type=products"
done
```

Expected: six 200s. Also confirm the response body does not contain `Burke Index`.

### 6.3 Redirects and soft 404s

```bash
for u in /index/2025 /index/2025/ /index-2025 /index/2026 /index/2026/portugal \
  /read/this-slug-does-not-exist-12345 \
  /read/the-paradox-of-economic-sovereignty-why-is-rich-norway-on-a-dubious-track-but-tiny-liechtenstein-is-not \
  /about/products /category/osint; do
  curl -sS -o /dev/null -w "$u -> %{http_code} %{redirect_url}\n" \
    "https://ibi.institute$u"
done
```

Expected: `/index/2025` and `/index-2025` 301 to `/index`; unknown `/read` slug 404 or 410; archived old article 410; old `/about/*` 301 where an equivalent exists.

### 6.4 Old-brand strings

```bash
for u in / /index /about /contacts /products /research /osint /search; do
  echo "== $u"
  curl -sS "https://ibi.institute$u" | grep -Eo \
    'IBI International Burke Institute|International Burke Institute|Internationalal Burke Institute|Burke Sovereignty Index|/index/2025|>Index 2025<|>Osint<|>Products<|>Experts<|>Partners<|>Sources<|>Contact us<|>Donate<' \
    | sort -u
done
```

Expected: no output in metadata/nav/footer. An approved historical mention on About or a legal page should be explicitly allowlisted, not silently ignored.

Also check the print header directly:

```bash
curl -sS 'https://ibi.institute/read/portugal?type=index' | grep -o 'Burke Index' | wc -l
```

Expected: 0.

### 6.5 JSON-LD

```bash
curl -sS https://ibi.institute/ | python3 -c '
import sys, re, json
h = sys.stdin.read()
for block in re.findall(r"<script type=\"application/ld\+json\"[^>]*>(.*?)</script>", h, re.S):
    d = json.loads(block)
    if d.get("@type") == "Organization":
        print("name:", d.get("name"))
        print("alternateName:", d.get("alternateName"))
        print("foundingDate:", d.get("foundingDate"))
        print("members:", len(d.get("member", [])))
        print("produces:", [x.get("name") for x in d.get("produces", [])])
'
```

Expected: `Ilya Boxers Institute`, `IBI`, `2026`, `0`, and the new Product/CreativeWork produces list. Also check an old expert URL returns 410 and no `Person` schema remains.

### 6.6 Sitemap and robots

```bash
curl -sS https://ibi.institute/robots.txt
curl -sS https://ibi.institute/sitemap.xml

for s in sitemap-pages.xml sitemap-posts.xml sitemap-indexes.xml; do
  echo "== $s"
  curl -sS "https://ibi.institute/$s" \
    | grep -o '<loc>[^<]*' | sed 's/<loc>//' \
    | grep -E 'experts|index/2025|collection-|open-source' || true
done
```

Expected: robots has `Sitemap: https://ibi.institute/sitemap.xml`; sitemap has no `experts/` or `index/2025`; sitemap contains all six `collection-` product URLs and the Open Source URL.

### 6.7 Metadata and social cards

```bash
curl -sS https://ibi.institute/ | grep -E \
  '<title>|meta name="description"|og:title|og:description|og:site_name|twitter:title|twitter:description'
```

Expected: new title, description, OG title/description, site name, and Twitter equivalents. Repeat for `/index`, `/products`, and one read page. Confirm the OG image is the new card, not `ibi-logo.2002f510.webp`.

### 6.8 Mobile navigation

Manual check at 390 × 844, 768 × 1024, and 1440 × 900:

- Open the menu; confirm the primary order Home, The Index, Research, Open Source, Collection, Contact.
- Confirm utility order Support, About, Fellows, Stockists, Citations, Sign in, Search.
- Confirm no `Index 2025`, `Osint`, `Products`, `Experts`, `Partners`, `Sources`, `Contact us`, or `Donate` labels.
- Confirm focus states, Escape close, scroll lock, and no horizontal overflow.
- Confirm the logo alt reads `Ilya Boxers Institute — home.`

### 6.9 Search Console and index checks

- Submit the sitemap; inspect `/`, `/index`, `/products`, one product, one country, BSI, Research, Open Source.
- Confirm `/index/2025`, old expert profiles, and archived articles are excluded or 410.
- Check Page Indexing for soft 404s; the `/read` fallback must not appear as an indexed 200.
- Confirm no Change of Address is needed.

### 6.10 Forms and newsletter

Test only in staging, or with an approved test address after launch:

- Newsletter shows the Quarterly Dispatch copy, accepts an address, and returns the confirmation state.
- Contact form routes General/Press/Stockist/Order/Privacy correctly.
- Do not POST to production during the audit.

---

## 7. Known limitations

- The Next.js repository is not in this workspace. Exact component, layout, serializer, and route-handler paths are inferred from the live output; they must be confirmed in the repo.
- All checks were read-only GETs. No login, no authenticated admin/API inspection, no POST/PATCH/DELETE, and no code or content changes were made.
- JavaScript was not executed. Client-side search results, the mobile menu, form submission, analytics consent, and modal sign-in behavior were not fully exercised.
- Settings are being updated separately. The live strings recorded here may change without a frontend deploy; conversely, some P0 items are hardcoded and will not be fixed by settings alone.
- The old article and old expert observations may change when API caches or visibility flags are refreshed. Recheck after the settings/API step.
- Favicon content was not visually verified; only its metadata and dimensions were checked. Confirm it is the retained monogram, not an old mark.
- CDN/edge caching can serve stale HTML after deploy. Purge and recheck with cache-busting query strings where necessary.
- No legal review, accessibility audit, performance lab test, or cross-browser matrix was performed. The legal-link and consent items require counsel/owner confirmation.

Credit: With love, made by RRN.world — Rotten Reliable News.
