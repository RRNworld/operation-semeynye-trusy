# Site-wide Copy and Rename Map

**Project:** IBI International Burke Institute → Ilya Boxers Institute  
**Scope:** Public Next.js site, admin-managed collections, redirects, launch QA  
**Status:** Draft for developer and editor handoff  
**Canonical host:** `https://ibi.institute` (unchanged)  
**API/admin:** `https://api.ibi.institute/api` and `https://admin.ibi.institute` (unchanged)

## Working decisions

- The IBI acronym and monogram are retained. The public name is **Ilya Boxers Institute**; use **IBI** after first mention.
- **BSI** is re-expanded as **Boxer Sovereignty Index** on `/index` and the methodology page. The initials survive alongside IBI. If the founder does not approve reusing BSI, use **The Index** everywhere and remove the old expansion; do not leave “Burke Sovereignty Index” in public copy.
- Routes stay stable wherever a page has a direct equivalent. Label changes do not require path changes.
- Cadence: describe the Index as an **annual edition with periodic revisions**. Existing drafts say “quarterly”; update those references in the same editorial pass so the site does not contradict itself.
- No new claims about prices, availability, manufacturing, stockists, awards, endorsements, reviews, or performance until written approval exists.
- Founder copy is limited to the approved biography in `02-founder-about.md`. No family detail and no new political claims.
- **Fellows are new appointments, not a relabel of the old expert list.** Do not import or rename real experts as underwear fellows without written consent.

---

## 1. Global identity

| Element | New copy | Implementation note |
|---|---|---|
| Site title | **IBI — Ilya Boxers Institute &#124; Underwear, Indexed** | Root metadata title. Keep the title template short: `%s &#124; Ilya Boxers Institute`. |
| Meta description | **Ilya Boxers Institute studies, indexes, and makes boxer shorts for people who know that power is never fully dressed. Read the dispatches.** | Replace the old geopolitics/macroeconomics description everywhere. |
| OG title | **Ilya Boxers Institute — Power Is Never Fully Dressed** | Default social card. Page-level OG titles should append the page name, not the old brand. |
| OG description | **A Moscow-born institute and apparel house devoted to the study of boxer shorts. Field notes, an annual index, and a collection of considered essentials.** | Use the same wording on Twitter/X cards. No “geopolitics,” “sovereignty,” or “OSINT” in the default description. |
| JSON-LD Organization name | **Ilya Boxers Institute** | Add `alternateName: IBI`. Remove the old legal/alternate names from schema. |
| JSON-LD Organization description | **Ilya Boxers Institute is a Moscow-born research institute and apparel house devoted to the study and improvement of boxer shorts. It maintains the IBI Index, publishes field notes, and makes a considered collection of underwear.** | No geopolitical description. Set `foundingDate: 2026`. Do not carry over the old 2023 date. |
| JSON-LD Organization produces | **Product — Boxer shorts; Product — Loungewear; CreativeWork — The IBI Index** | No `offers`, `priceRange`, `aggregateRating`, `review`, `award`, or `member` array. Products are described, not sold, in schema until commerce fields are approved. |
| Favicon/logo alt text | **Logo alt: “Ilya Boxers Institute — home.” Favicon alt: “IBI monogram.”** | The monogram is retained; the wordmark is new. If the header asset contains the full name, use “Ilya Boxers Institute” only. |
| Copyright line | **© 2026 Ilya Boxers Institute. All rights reserved. With love, made by RRN.world — Rotten Reliable News.** | Use the current year at render time, but keep the exact RRN.world credit string. Repeat the credit in the colophon. |
| Legal entity line | **Ilya Boxers Institute is a trading name of RRN.world — Rotten Reliable News. Registered office and registration details are available on request. Formerly IBI International Burke Institute.** | Operator confirmed from the brand brief; no jurisdiction or registration number is invented. Counsel should confirm the legal wording before publication. |

### Schema cleanup

- Delete the old `member` array containing hundreds of `Person` entries. This is the single largest structured-data change.
- Remove old `Person` schema from archived expert profiles; archived profiles must not remain publicly indexable.
- Remove any old `award`, `founder`, or geopolitical `knowsAbout` values that are not part of the approved new identity.
- Keep `Organization.url` as `https://ibi.institute`; do not point schema at the API or admin subdomain.
- If real people appear in new Fellows profiles, use only approved, accurate information and obtain consent. Otherwise publish no Person schema at all.

---

## 2. Navigation

| Location | Current label | New label | Target route | Change | Rationale |
|---|---|---|---|---|---|
| Primary | Home | **Home** | `/` | None | The front door remains the front door. |
| Primary | Index | **The Index** | `/index` | Label only | Names the annual publication rather than a generic index. |
| Primary | Research | **Research** | `/research` | None | The section still carries the Institute’s research, now on the garment. |
| Primary | Osint | **Open Source** | `/osint` | Label only | Keeps the OSINT method as dry institutional vocabulary without the intelligence-agency read. |
| Primary | Products | **Collection** | `/products` | Label only | Apparel-house language; avoids “shop” and price-first framing. |
| Primary | Contact us | **Contact** | `/contacts` | Label only | Shorter utility label; same page. |
| Primary | Index 2025 | **Index 2026** | `/index` | Label and target | Promotes the current edition. Old `/index-2025` and `/index/2025` go to `/index` with a retirement notice. |
| Utility | Donate | **Support** | `/donate` | Label only | Covers patronage and purchase support without a solicitation tone. |
| Utility | About | **About** | `/about` | None | Institutional continuity. |
| Utility | Experts | **Fellows** | `/experts` | Label only | Fellowship vocabulary; old expert profiles are not imported. Confirm current placement in footer/utility nav. |
| Utility | Partners | **Stockists** | `/partners` | Label only | Retail vocabulary; no partnership claim is implied. |
| Utility | Sources | **Citations** | `/sources` | Label only | Research apparatus; the page now cites tests, materials, and care data. |
| Utility | sign in | **Sign in** | `/sign-in` (existing auth route) | Casing only | Account access is unchanged. Keep the existing auth path; do not rename it for copy reasons. |
| Utility | Search | **Search** | `/search` | None | Utility convention. |

**Recommended order:** Home, The Index, Research, Open Source, Collection, Contact; utility: Support, About, Fellows, Stockists, Citations, Sign in, Search. Keep “Index 2026” visible near the front of the primary nav during launch. Do not add an “Experts” label back into public navigation.

---

## 3. Page-by-page copy

| Route | H1 | Intro | Section headers |
|---|---|---|---|
| `/` | **Power is never fully dressed.** | Ilya Boxers Institute studies, indexes, and makes the garment beneath the garment. The findings are available in the Collection. | “Dispatches — Recent from the Institute”; “Featured Study”; “Observations — From the field”; “The Collection — Garments for the unobserved hours”; “Field Notes”; “The Quarterly Dispatch” |
| `/index` | **The Index 2026** | The Institute’s annual reading of the boxer short: construction, composure, and conduct under supervision. Rankings are conditions, not people, and they are revised when the evidence changes. | “The 2026 edition”; “Method”; “Rankings”; “Revisions”; “Archive” |
| `/research` | **Research** | Field notes on fit, fabric, and the conditions in which the first layer is actually worn. The sample is small by design; the conclusions are provisional. | “Current studies”; “Method”; “The standing panel”; “Archive”; “Corrections” |
| `/osint` | **Open Source** | Open-source observation of the garment in public: labels, care instructions, retail listings, and the visible seam. The Institute reviews public material only and publishes what can be verified. | “Open-source briefs”; “Verification”; “Reading list”; “Archive” |
| `/products` | **The Collection** | The Institute’s applied research: boxer shorts and loungewear made to the standard of the Index, not the season. A small collection, one standard, no visible argument. | “The Index”; “The Quorum”; “The Field Standard”; “The Backchannel”; “The Reading Room”; “The Protocol”; “Fit and care”; “Sizing” |
| `/contacts` | **Contact** | Correspondence is read in the order received and answered when the matter is established. For press, permissions, stockist, and order enquiries, use the addresses below. | “General enquiries”; “Press”; “Stockists”; “Orders and returns”; “Institutional address” |
| `/about` | **About the Institute** | Ilya Boxers Institute is a research institute and apparel house devoted to the study and improvement of boxer shorts. It was formerly IBI International Burke Institute; the initials and monogram were retained. | “The mandate”; “The founder”; “The method”; “The monogram”; “Disclosure”; “Credits” |
| `/partners` | **Stockists** | The Institute intends to work with a small number of stockists whose shelves can support the Collection without altering it. This page lists confirmed stockists only. | “Confirmed stockists”; “Stockist standard”; “Wholesale enquiries”; “Territories” |
| `/sources` | **Citations** | The Institute cites what it observes. This page records the sources behind each Index edition, field note, and correction, including material tests, care data, and editorial correspondence. | “Index 2026”; “Field notes”; “Corrections”; “Citation style”; “Archive” |
| `/donate` | **Support the Institute** | The Institute accepts support for independent research and the maintenance of the Index. Support does not purchase influence, ranking, or editorial consideration. | “Why support”; “One-time support”; “Recurring support”; “Supporters”; “Governance” |
| `/experts` | **Fellows** | The Institute appoints fellows for fixed terms to study specific questions in the first layer. Each profile records the remit, the method, and the disclosures. | “Current fellows”; “Fellowship”; “Appointments”; “Disclosure” |
| `/experts/:slug` | **Fellow profile** | The appointed Fellow is presented with a stated remit and the question the fellowship addresses. The profile records the method, selected work, and disclosure. | “Remit”; “Field notes”; “Method”; “Selected work”; “Disclosure” |
| `/articles` | **Dispatches** | Short reports from the standing panel, published when the evidence justifies the paper. The archive keeps every revision. | “Recent dispatches”; “Featured”; “Subjects”; “Archive” |
| `/articles/:slug` | **Article title** | A standfirst states the observation in one sentence and the condition under which it was made in a second. | “Observation”; “Method”; “Evidence”; “Limitations”; “Revision history” |
| `/read/:slug` | **Article title** | A reading edition prepared for long-form sitting. The canonical version remains at `/articles/:slug`. | “Reader’s note”; “Text”; “Citations”; “Colophon” |
| `/news` | **Notices** | Institutional notices, launch updates, and corrections. Notices are dated and kept in the record. | “Current notices”; “Corrections”; “Archive” |
| `/search` | **Search the Institute** | Results are drawn from the current public record: dispatches, field notes, Collection pages, and citations. Archived material is excluded unless the archive filter is selected. | “Results”; “Filters”; “No results”; “Search notes” |
| `/404` | **Page not found** | The page you requested is not in the current record. It may have been moved, archived, or never filed. | “Try”; “Current sections”; “Contact” |
| `/sign-in` | **Sign in** | Sign in to manage your account, orders, and correspondence. Access is limited to registered account holders and Institute staff. | “Account access”; “Reset access”; “Account help”; “Privacy” |

**Page notes**

- `/partners`: launch state is an explicit empty state: “No stockists are listed at launch.” Do not use placeholder logos or names.
- `/donate`: do not publish amounts, tiers, processing fees, or donor names until approved.
- `/products`: do not publish prices, availability dates, fabric-performance claims, or manufacturing claims until approved.
- `/experts`: do not import the old expert records. If no approved fellows exist at launch, publish the empty state rather than a fabricated roster.
- `/read/:slug`: set `rel=canonical` to the corresponding `/articles/:slug` URL. Keep `/read/*` stable to avoid breaking existing links; it is not a separate article.
- `/search`: exclude archived material at the query level, not only in the UI.
- `/404`: do not redirect every unknown URL to home. Serve a real 404 with current-section links.

---

## 4. Homepage section-header renames

| Current block | Primary label | Primary heading | Alternates | Note |
|---|---|---|---|---|
| Latest / 4-item article grid | **Dispatches** | **Recent from the Institute** | “The Current Record” / “Recently filed”; “Notes from the Panel” / “Short reports” | “Dispatches” is institutional and recurring; “Latest” is a timestamp, not a section. |
| Our products | **The Collection** | **Garments for the unobserved hours** | “Applied Research” / “The Collection, Vol. I”; “The Collection” / “Small collection. One standard.” | Keep product names and order; do not add price badges to this block. |
| Research | **Field Notes** | **Method and observation** | “The Research Record” / “From the standing panel”; “Method” / “How the Institute observes” | Avoid repeating “Research” as both nav label and section label; “Field Notes” carries the method. |

**Supplementary homepage blocks**

| Current block | Primary label | Primary heading | Alternates | Note |
|---|---|---|---|---|
| Large featured story | **Featured Study** | Use the approved article title, e.g. “The Campaigner Beneath the Campaign” | “Cover Study”; “Featured Dispatch” | Founder-related copy must use only the approved biography and approved quotes. |
| Banner ad | **Index 2026** | **The 2026 edition is open.** | “A notice from the Institute”; “The Index, revised.” | This is a house notice, not third-party ad inventory. CTA: “Read the Index.” No statistics or claims. |

---

## 5. Archive and redirect plan

### 5.1 Disposition by content type

Definitions: **Archive** = unpublish and export to a controlled offline archive; do not delete until records/legal sign-off. **Unhook** = remove from navigation, homepage modules, related-content rails, feeds, and sitemaps while retaining the record. **Redirect** = permanent 301 to a genuine equivalent.

| Content type | Disposition | Public handling | Notes |
|---|---|---|---|
| Geopolitical research papers, BSI country analyses, sovereignty commentary | Archive | 410 Gone, or 301 to `/about#mandate` where a backlink justifies a soft landing | Do not rewrite old geopolitical findings as underwear research. Do not redirect bulk articles to `/products`. |
| OSINT investigations and person-focused material | Archive | 410; remove from sitemap and search index | Remove source files and personal data from the public media library. Do not republish under “Open Source.” |
| Old news and press releases | Archive | 410; keep only the rename announcement | The new `/news` is Notices, not an archive of the old institute’s press. |
| Expert profiles (real people) | Archive | 410; **no bulk redirect to `/experts`** | Relabelling real experts as Fellows implies an endorsement. Retain only with written consent and an approved new profile. |
| Partner pages, logos, and sponsor marks | Archive | 410; **do not redirect to `/partners`** | The Stockists page must list confirmed stockists only. Old logos are not evidence of a current relationship. |
| Old sources and citation lists | Archive | Replace with Citations; 410 individual old citation pages | Do not 301 individual citations to unrelated new sources. |
| Index 2025 and earlier BSI editions | Archive the dataset | `/index-2025` and `/index/2025` → 301 `/index`; older years → 410 | Add a notice on `/index`: “The 2025 edition has been retired. This is the 2026 edition.” |
| Categories and tags | Retire | Mapped category hubs → 301 to the equivalent section; unmapped tags → 410 | Do not leave empty geopolitical tag archives indexable. |
| Old product records, if any | Audit | 301 only to an exact equivalent Collection product; otherwise 410 | No blanket redirect from old product URLs to `/products`. |
| Media library and brand assets | Purge public copies | Replace logo, favicon, OG images, and banners | Keep licensed/archival copies offline. Remove old maps, portraits, and geopolitical imagery from public storage. |
| Feedback, orders, and user records | Retain operationally | No public handling | Apply the privacy policy; purge or anonymize data that is no longer needed. Do not publish correspondence without consent. |
| Old events and education pages | Unhook or repurpose | Keep route if repurposed; otherwise 301 to `/about` | Clear old dates, programmes, and institutional claims. Do not invent a new events calendar. |

### 5.2 Example redirect rules for `/redirects`

Confirm every exact path against a full crawl before import. Apply exact rules before wildcard rules; allow one hop only.

| From (old) | To (new) | Code | Note |
|---|---|---|---|
| `www.ibi.institute/*` | `https://ibi.institute/:splat` | 301 | Host normalization, if not already in place. |
| `/index-2025` | `/index` | 301 | Retire the 2025 edition; show the retirement notice on `/index`. |
| `/index/2025` | `/index` | 301 | Alternate old path. |
| `/about/products` | `/products` | 301 | About subpage to the Collection. |
| `/about/research` | `/research` | 301 | About subpage to Research. |
| `/about/osint` | `/osint` | 301 | About subpage to Open Source. |
| `/about/index` | `/index` | 301 | About subpage to The Index. |
| `/about/experts` | `/experts` | 301 | About subpage to Fellows. |
| `/about/partners` | `/partners` | 301 | About subpage to Stockists. |
| `/about/sources` | `/sources` | 301 | About subpage to Citations. |
| `/category/osint` | `/osint` | 301 | Exact section equivalent. |
| `/category/research` | `/research` | 301 | Exact section equivalent. |
| `/category/:slug` | None (archive) | 410 | Only the exact `/category/osint` and `/category/research` rules above survive; all other old categories are retired. |
| `/tag/:slug` | None (archive) | 410 | Retire the old tag archives; the new taxonomy uses new paths and is not a redirect target. |
| `/experts/:slug` | None (archive) | 410 | Archive the old profile. If the person is retained with consent and rewritten copy, keep the same route; no redirect. |
| `/articles/:slug` | None (archive) | 410 | Old geopolitical article. If rewritten and retained, keep the same route; no redirect. |
| `/news/:slug` | None (archive) | 410 | Archive old research news. `/news` itself remains. |
| `/read/:slug` | None (archive) | 410 | Only if the source article is archived. If retained, keep `/read/:slug` and canonicalize to the article. |
| `/partner/:slug` | None | 410 | Do not imply the old partner is a current stockist. |
| `/source/:slug` | None | 410 | Old citation record; replace through the Citations page. |

**Redirect operating rules**

- One hop only. Do not chain `/old` → `/interim` → `/new`; update the first rule to the final destination.
- Preserve campaign query strings on 301s where analytics matter. Drop tracking parameters on 410s.
- If `/redirects` supports only 301/302, add 410 handling at the edge or in Next.js middleware. Do not 301 bulk old articles to `/products`.
- Keep an exported copy of the redirect table in the repository so a deploy cannot silently drop admin-entered rules.
- Test with `curl -I` for status code, `Location`, and absence of loops. Check old URLs with and without trailing slashes.
- Do not redirect the entire old domain to `/`; that produces soft 404s and destroys the useful distinctions between archived and equivalent pages.

---

## 6. Admin endpoint mapping

“Admin” means the change can ship through the custom admin SPA. “Deploy” means the value or template is hardcoded in the Next.js frontend or API defaults and requires a code release. Where both appear, the CMS copy can change first but the public result is not correct until the deploy ships.

| Endpoint | Now holds | New role and copy/field changes | Flag |
|---|---|---|---|
| `/settings` | Global site name, meta defaults, contact/social details, footer settings | Set public name **Ilya Boxers Institute**; title and descriptions from Section 1; `foundingDate` 2026; support and press addresses; exact RRN.world credit; legal-entity placeholders. Remove old geopolitical boilerplate. | Admin + Deploy (metadata defaults may be hardcoded) |
| `/about/research` | About-page copy for Research | Rewrite for garment research: fit, fabric, conditions of wear; new section headers from Section 3. Remove old geopolitical framing and any research claims not supported by the new editorial record. | Admin |
| `/about/products` | About-page copy for Products | Rewrite as the Collection’s applied-research page: materials, construction, fit and care. No prices, availability dates, or performance claims until approved. | Admin |
| `/about/osint` | About-page copy for OSINT | Rewrite as **Open Source**: public labels, care data, retail listings, visible seams; verification method and ethics. No intelligence-service framing and no person-focused material. | Admin |
| `/about/index` | About-page copy for the Index / BSI page | Rewrite as **The Index 2026** and **BSI — Boxer Sovereignty Index**: methodology, rankings-as-conditions, revisions, retirement notice for 2025. This is launch-critical. | Admin + Deploy (page template and schema) |
| `/about/experts` | About-page copy for Experts | Rewrite as **Fellows**: fixed-term appointments, remit, method, disclosure. Remove old expert counts, biographies, and endorsement language. Do not reuse the old roster. | Admin |
| `/about/events` | Events listings and page copy | Repurpose as **Programme**: salons, fittings, readings, and Institute sessions. Clear old geopolitical events and dates. If nothing is confirmed, publish an empty state, not invented dates. | Admin |
| `/about/education` | Education page and programme copy | Repurpose as **The Method**: fit, fabric, sizing, and care guidance. No course, accreditation, or qualification claims. | Admin |
| `/articles` | Articles, dispatches, and editorial content | Clear old geopolitical articles from public surfaces. Apply the new Dispatch template: observation, method, evidence, limitations, revision history. Rebuild categories and canonical URLs. | Admin |
| `/news` | News and press releases | Repurpose as **Notices**: launch updates, corrections, institutional announcements. Keep the rename notice; archive the old research press. | Admin |
| `/research` | Research papers and studies | New field notes and studies on the garment. Archive old research; rebuild the subject taxonomy; add correction records. Do not republish old findings under the new brand. | Admin |
| `/products` | Product records and Collection copy | Hold the six Collection pieces from `01-homepage.md`: The Index, The Quorum, The Field Standard, The Backchannel, The Reading Room, The Protocol. Add fit, care, sizing, and approved imagery. No prices or availability until approved. | Admin |
| `/experts` | Expert profiles | Hold **Fellows** profiles. Do not import or relabel the old expert list. New fields: remit, term, method, selected work, disclosures, consent status. Remove old Person schema. Launch with approved profiles or an explicit empty state. | Admin + Deploy (schema removal) |
| `/partners` | Partner listings and logos | Hold **Stockists**: confirmed stockists only, with territory and contact. Launch empty: “No stockists are listed at launch.” Remove old partner logos and any implication of continuing relationships. | Admin |
| `/sources` | Source records and citations | Hold **Citations**: Index 2026 bibliography, field-note sources, corrections, citation style. Clear the old geopolitical source list. Old individual citation pages are archived, not redirected. | Admin |
| `/categories` | Content categories | Replace geopolitical categories with the garment taxonomy: Fit, Fabric, Construction, Conduct, Care, Method, Archive. Retire unused categories; redirect only exact equivalents; 410 the rest. | Admin |
| `/tags` | Content tags | Replace with a controlled vocabulary: fit, fabric, construction, care, method, archive, Index 2026. Remove people and political tags. Do not leave tag archives indexable. | Admin |
| `/banners` | Homepage and section banners, including third-party ad inventory | Hold launch banners: **Index 2026** and **The Collection**. Add alt text, schedule, and target URLs. Remove old ads and geopolitical creative. No third-party inventory unless a contract is confirmed. | Admin |
| `/redirects` | Redirect rules | Load the Section 5 map. Use 301 for exact equivalents, 410 for archived material. Test one-hop behavior. Keep an exported copy in the repository. | Admin + Deploy (if redirects are also compiled into Next.js) |
| `/files` | Media library | Remove old logos, maps, portraits, and geopolitical imagery from public use. Add new monogram/wordmark, favicon, OG images, and Collection photography. Record licences and alt text. | Admin |
| `/upload/single` | Single-file upload | Require alt text, a filename convention, and a size limit. Block old brand assets by policy, not just by memory. | Admin |
| `/upload/multiple` | Bulk upload | Require batch alt text and cleanup of orphaned files. Do not bulk-import old research imagery. | Admin |
| `/orders` | Orders and order records | Function unchanged. Update product names, confirmation emails, shipping/returns copy, and support references. Do not change prices or fulfilment promises without approval. | Admin |
| `/feedback` | Feedback and contact submissions | Rename the public label **Correspondence**. Route press, stockist, order, and privacy matters correctly. Update the auto-reply in the new voice; add the privacy notice. | Admin |
| `/users` | Admin users and roles | Relabel as **Staff** in admin copy. Audit old accounts and remove access that is no longer required. No public-facing copy. | Admin |
| `/index` + BSI page | Public frontend route, not an admin endpoint | Launch **The Index 2026** and **BSI — Boxer Sovereignty Index**. Content is sourced from `/about/index`, `/articles`, and `/sources`. H1, methodology, retirement notice, and structured data need a frontend deploy. | Deploy |

### 6.1 Hardcoded in Next.js — deploy required

| Item | Where it usually lives | Required change | Deploy? |
|---|---|---|---|
| Title and meta defaults | Root layout metadata / per-page `generateMetadata` | New title, title template, meta description, OG/Twitter defaults. Remove old geopolitical description. | Yes |
| JSON-LD Organization | Layout, `Organization` component, or API serializer | New name, description, `produces`, `alternateName`, `foundingDate` 2026. Remove old `member` array. | Yes |
| JSON-LD Person / expert schema | Profile templates and API responses | Remove the hundreds of old `member` entries and old expert `Person` schema. Do not expose archived experts through the API. | Yes |
| Logo and wordmark | Header/footer components, `/public` SVGs | Keep the IBI monogram; swap the old wordmark for **Ilya Boxers Institute**; update alt text. | Yes |
| Navigation labels and order | Header/footer components and mobile menu | Apply Section 2. Do not leave “Osint,” “Products,” “Experts,” “Partners,” or “Sources” in public nav. | Yes |
| Favicon, apple-touch icon, web manifest | `/public`, metadata, manifest | New favicon and icon set using the retained monogram; manifest name **Ilya Boxers Institute**. | Yes |
| OG/Twitter images | `/public/og` and metadata | New default, Index 2026, and Collection cards. No old logo, maps, or geopolitical imagery. | Yes |
| Footer copyright and credit | Footer component or `/settings` | Exact RRN.world credit in the copyright line and colophon. | Yes, if static |
| 404, search, sign-in copy | Page components or CMS templates | Use Section 3. Check that no static fallback still says “International Burke Institute.” | Yes, if static |
| robots.txt and sitemap | App routes or build script | Keep canonical host; exclude archive; include current public pages; no admin/API URLs. | Yes |
| API defaults and serializers | API code and config | Search for the old name, old description, old meta fields, and old expert arrays. A CMS edit alone will not fix serialized defaults. | Yes |
| Admin SPA title/login | Admin app | Cosmetic but visible to staff: use **IBI Admin** or **Ilya Boxers Institute — Admin**. | Yes, admin deploy |

---

## 7. Do not forget — launch checklist

| # | Item | Owner | Done when |
|---|---|---|---|
| 1 | **robots.txt and sitemap.xml** | Dev | Sitemap contains current public pages only; archive and old expert profiles are excluded; robots disallows admin, API, account, and internal search URLs; canonical host is `https://ibi.institute`. |
| 2 | **OG and Twitter images** | Design + Dev | Default, Index 2026, and Collection cards are live at the required dimensions; no old IBI wordmark, geopolitical imagery, or maps; alt text is present. |
| 3 | **301s and 410s** | Dev + SEO | Section 5 rules are loaded in `/redirects`, exported to the repo, and tested with `curl -I`; no chains or loops; old article URLs are not mass-redirected to product pages. |
| 4 | **Remove expert schema** | Dev | Organization JSON-LD has no `member` array; old expert profiles are unpublished and non-indexable; API responses do not expose archived expert records. |
| 5 | **Google Search Console change of address** | SEO | If the domain remains `ibi.institute`, verify the property, submit the new sitemap, and request indexing rather than using Change of Address. Use Change of Address only if a domain change is also happening. Monitor Coverage and Enhancements for 30 days. |
| 6 | **Email templates and signatures** | Ops + Dev | Order confirmation, shipping, password reset, contact auto-reply, and press templates use the new name, logo, support address, and RRN.world credit; test sends pass. |
| 7 | **Consent and legal pages** | Legal + Editor | Privacy, terms, cookies, shipping and returns, sizing disclaimer, and accessibility statements are counsel-approved and linked in the footer. No invented company details or guarantees. |
| 8 | **RRN.world credit in footer** | Editor + Dev | The exact line “With love, made by RRN.world — Rotten Reliable News.” appears in the global footer/copyright and the colophon on every page. |
| 9 | **BSI / Index 2026 launch promo banner** | Editor + Marketing | Banner is scheduled in `/banners` with approved copy, CTA to `/index`, alt text, and no unapproved statistics or claims. |
| 10 | **Final crawl and old-brand audit** | Dev + Editor | A full crawl finds zero public “International Burke Institute” strings, zero broken nav links, correct canonicals, noindex on archives, working 404, and a clean mobile nav. |

**Launch sequence:** freeze content, deploy hardcoded frontend changes, load redirects, publish Index 2026 and the Collection, submit sitemap, then monitor 404s and Search Console daily for the first week. Keep the old brand export and redirect map in the repository, not only in the admin database.

Credit: With love, made by RRN.world — Rotten Reliable News.
