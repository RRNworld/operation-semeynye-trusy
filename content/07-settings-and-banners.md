# IBI Settings and Launch Banners

**Project:** Ilya Boxers Institute — settings and banner rebrand  
**Scope:** All 18 fields in the live `/settings` collection, plus three launch banners  
**Status:** Editorial handoff — launch defaults filled; not approved for paste  
**Canonical sources:** `01-homepage.md`, `02-founder-about.md`, `03-site-copy-map.md`, `05-boxer-sovereignty-index.md`, `06-secondary-pages.md`  
**Prepared:** 22 October 2026

> **Draft values note:** Contact, legal, and product values in this handoff are launch defaults. Product specifications in the companion PDP file are drafts pending sample approval. Confirm the institutional address, telephone posture, hours, mailboxes, banner schedule, image approval, and all product specs before publishing. No legal registration details are invented here.

> **Source note:** The Research and Support values below are drawn from `06-secondary-pages.md` (§1, §15.1, and §6) and kept within the settings-field limits. The full page copy remains in 06; these fields carry the introduction, not the whole page.

## 0. Rules for this handoff

- The contact values below are launch defaults: Monogram House, Moscow; telephone by appointment; Monday–Friday, 10:00–18:00 MSK; boxers@rrn.world. Confirm each before publishing. If a value is not confirmed by freeze, hide the row or the page element.
- Do not invent addresses, emails, phone numbers, legal entities, prices, stockists, statistics, or verification tokens.
- The master table in §1 is the coverage check. Exact paste values are in §3 (prose) and §4 (structured fields).
- Character counts refer to plain text, including spaces and punctuation, not to the Editor.js JSON envelope.
- Nothing in this file authorises a claim about price, availability, manufacturing, awards, endorsements, or performance.

### Key decisions at a glance

- `aboutIndex` and `about-index` receive the same new intro at launch. `aboutIndex` is the preferred key; consolidate only after a code audit.
- `metaTags` is cleared to `[]`. No verification or script tags are added in this pass.
- `table` is cleared and rendered from `05-boxer-sovereignty-index.md`; if the admin table must remain, import the ten rows from 05 verbatim. Old country data is not reused.
- `experts`, `expertLogin`, `events`, and `educations` are false at launch. `expertLogin` needs an engineering check before it is flipped.
- Banners launch inactive until schedule, image, target anchor, and alt text are approved.

## 1. Master replacement table

| # | Field | Current state | Action | Value / reference | Public surface |
|---:|---|---|---|---|---|
| 1 | `about` | Old “The Burke Institute Statement” Editor.js content | Replace | 600-character institutional summary, §3.1. Full About copy remains 02. | `/about` |
| 2 | `aboutIndex` | Old “Formula and Data Sources for Generating. The Sovereignty Index” content | Replace; canonical duplicate | 499-character Index intro, §3.2. | `/index` |
| 3 | `about-index` | Same old Index formula content as `aboutIndex` | Replace with the identical value; consolidate later | §3.2 and §4.8. Do not clear either field before the code audit. | `/index` |
| 4 | `about-research` | Empty paragraph | Replace | 451-character Research intro, §3.3. | `/research` |
| 5 | `aboutData` | Tel Aviv address, `info@ibi.institute`, hours | Replace; remove old rows | Structured value in §4.1. Launch defaults; confirm before publishing. | `/contacts` |
| 6 | `partnersData` | Old partner text, `info@ibi.institute`, phone, hours | Replace | Stockist-enquiry copy in §4.2. | `/partners` |
| 7 | `sourcesData` | Empty fields | Replace | Citations and press-enquiry copy in §4.3. | `/sources` |
| 8 | `phoneNumber` | `+972502901775` | Replace | `By appointment`; no number is published. The old number must not be published unless confirmed. | `/contacts`, footer |
| 9 | `donateText` | Old Burke donation text | Replace | 520-character Support copy, §3.4. No amounts or tiers. | `/donate` |
| 10 | `footerDonateText` | “We’d be grateful for your donation...” | Replace | Short deadpan support line, §3.5. | Global footer |
| 11 | `metaTags` | `{"name":"script","content":"test"}` plus an empty entry | Clear | `[]`. See §4.7 for the reason. | Global `<head>` |
| 12 | `banners` | Main page banner pointing to old `/index/2025` | Replace | Three records in §4.4. | `/`, footer |
| 13 | `table` | Old BSI country table: Place / Country / Macroregion / ... / Total / 700 | Clear or replace | Clear and render from 05; or import the ten-row table from 05. See §4.5. Do not reuse old data. | `/index` |
| 14 | `pagesVisibility` | donate/about/partners/index/research/osint/products/expertLogin true; educations/events/experts false; `result25...` truncated | Replace known flags; audit the rest | §4.6. | Route gating and navigation |
| 15 | `gaEnabled` | Enabled | Keep | `true` | All public routes |
| 16 | `gaId` | `G-WLRFXXL39W` | Keep | `G-WLRFXXL39W` | All public routes |
| 17 | `searchConsoleActivationEnabled` | Enabled | Keep | Current value | Root `<head>` |
| 18 | `searchConsoleActivationValue` | Current token | Keep; mask | Current value, not reproduced here. Do not invent or substitute a token. | Root `<head>` |

## 2. Settings-to-page map

| Field | Public surface | What the reader sees | Implementation note |
|---|---|---|---|
| `about` | `/about` | Short institutional summary | The full About page copy is in 02. The field is a summary, not the full page. |
| `aboutIndex` | `/index` | Index introduction above the table | Canonical Index intro. Confirm whether the route reads this key or `about-index`. |
| `about-index` | `/index` | Duplicate Index introduction | Keep identical to `aboutIndex` until the duplicate is removed. See §4.8. |
| `about-research` | `/research` | Research page introduction | 06 §1 is the full Research page; 06 §15.1 supplies the settings-backed intro. `/about/research` is a 301 to `/research` in 03. |
| `aboutData` | `/contacts` | Institutional address block | May also feed a footer contact block. Confirm the template before removing any row. |
| `partnersData` | `/partners` | Wholesale and stockist-enquiry block | The page itself launches with the explicit empty state: “No stockists are listed at launch.” No logos or names. |
| `sourcesData` | `/sources` | Press, citations, and corrections enquiry block | Carries enquiry copy only. The Index 2026 source record belongs on the page, not in this field. |
| `phoneNumber` | `/contacts`, footer, any `tel:` link | Telephone number | Do not render a `tel:` link for the by-appointment value. |
| `donateText` | `/donate` | Support page introduction | 06 §6 is the full Support page; this field carries the intro. Public label is **Support**. Payment controls are separate; no amounts, tiers, fees, or donor names. |
| `footerDonateText` | Global footer | One-line support link or line | Field key remains `footerDonateText`; public label is Support, not Donate. |
| `metaTags` | Global `<head>` | Nothing at launch | Empty array. See §4.7. |
| `banners` | `/` and footer | Three promotional records | Placements must be confirmed against the banner serializer. |
| `table` | `/index` | 2026 ranking table | Preferred state: clear this field and render the table from 05. |
| `pagesVisibility` | All public routes and navigation | Route availability | Not page copy. It controls whether a route and its nav entry are exposed. |
| `gaEnabled` | All public routes | Analytics | Keep enabled. |
| `gaId` | All public routes | Analytics measurement ID | Keep `G-WLRFXXL39W`. |
| `searchConsoleActivationEnabled` | Root `<head>` | Verification meta | Keep enabled. |
| `searchConsoleActivationValue` | Root `<head>` | Verification token | Keep the current token; do not reproduce or replace it in this document. |

## 3. Paste-ready prose values

The values in this section are plain text for editorial approval. See §5 before converting to Editor.js JSON.

### 3.1 `about` — 600 characters

Replace the entire old statement. This is the public summary; the full About page copy is in `02-founder-about.md`. Do not put the filename in the public value.

```text
Ilya Boxers Institute is a Moscow-born research institute and apparel house devoted to the study and improvement of boxer shorts. It was formerly IBI International Burke Institute; the initials and monogram were retained. The Institute maintains the Boxer Sovereignty Index, publishes field notes from its standing panel, and makes a small Collection to the standard the Index measures. Its method is unglamorous: observe the wearer, repeat the conditions, and revise the finding when the evidence changes. The full account of the mandate, the method, and the founder is maintained on the About page.
```

### 3.2 `aboutIndex` and `about-index` — 499 characters

Use the same value in both fields at launch. `aboutIndex` is the preferred canonical key. Do not clear `about-index` until the consuming route is identified; see §4.8.

```text
The Boxer Sovereignty Index is the Institute's annual assessment of the garment beneath the garment. Until 2025 it ranked countries; it now ranks boxer shorts. The dimensions, weights, tier bands, and committee structure were retained; only the subject was narrowed. The standing panel scores each edition; the table is reviewed quarterly and revised when the evidence or the elastic requires it. The Institute is the sole entrant, auditor, and beneficiary; the disclosure is printed with the table.
```

### 3.3 `about-research` — 451 characters

Aligned to `06-secondary-pages.md` §1 and §15.1. The full Research page remains in 06; this is the settings-field introduction.

```text
The Institute studies the first layer in the conditions in which it is worn: seated, standing, in transit, and after hours. The programme is deliberately small. A study names its question, repeats its conditions, and publishes its limits alongside its finding. Where the evidence is thin, the note says so; where a conclusion changes, the correction remains in the record next to the original claim. Current studies, method, and corrections are below.
```

### 3.4 `donateText` — 520 characters

Aligned to `06-secondary-pages.md` §6. No amounts, tiers, processing fees, or donor names. Support mechanics and payment controls remain on the page and must be approved separately.

```text
The Institute accepts support for independent research and the maintenance of the Index. Support does not purchase influence, ranking, or editorial consideration. It maintains the archive and the correction record — the parts of an institute that no one photographs. The Institute is small and intends to remain so. Support is recorded; it is not announced. Support may be one-time or recurring. The Institute does not promise an outcome, a finding, or a return. It undertakes to record the work and to state its limits.
```

### 3.5 `footerDonateText` — 57 characters

Single line. No solicitation pressure and no amount.

```text
Support the Institute. The Index is published either way.
```

## 4. Structured field values

### 4.1 `aboutData`

Replace the array. Remove the Tel Aviv address, `info@ibi.institute`, the old telephone number, and the old hours. If the serializer uses `title` and `content` instead of `label` and `value`, map the keys only; keep the labels and row order unchanged.

```json
[
  { "label": "Institutional address", "value": "Monogram House, Moscow" },
  { "label": "Telephone", "value": "By appointment" },
  { "label": "Hours", "value": "Monday–Friday, 10:00–18:00 MSK" },
  { "label": "General enquiries", "value": "boxers@rrn.world" },
  { "label": "Press and operations", "value": "Mikhail Biyun, Head of Special Operations" }
]
```

This order matches the institutional-address block in `06-secondary-pages.md` §3: the Institute name as a heading, then address, telephone, hours, general enquiries. Hide any row whose value is unconfirmed at launch. Do not add a legal-entity line to this field; that requires counsel-approved text and is outside the 18 settings fields.

### 4.2 `partnersData`

Replace the old partner block. This copy belongs in the wholesale-enquiry block; the page’s confirmed-stockist list remains empty at launch.

```text
Stockist enquiries are considered for territories where the Collection can be supported without alteration. The Institute lists confirmed stockists only; an enquiry implies no partnership. Write to boxers@rrn.world with the subject line "Stockist enquiry." Include the store name, territory, website, and current retail offer. Telephone: by appointment. Hours: Monday–Friday, 10:00–18:00 MSK.
```

If the field is an array, split the final contact details into three rows: Wholesale (`boxers@rrn.world`), Telephone (`by appointment`), Hours (`Monday–Friday, 10:00–18:00 MSK`). Keep the sentences identical. Do not add stockist names, logos, territories, or partnership claims.

06 §4 routes stockist enquiries to `boxers@rrn.world` with the subject line “Stockist enquiry.” No separate wholesale mailbox is published at launch.

### 4.3 `sourcesData`

Replace the empty fields. Press and citation enquiries share the confirmed mailbox; route them by subject line.

```text
Press and permissions: Mikhail Biyun, Head of Special Operations, boxers@rrn.world. Citation and source enquiries: boxers@rrn.world with the subject line "Citation correction." Include the edition, table, and line in question. The Institute cites what it observes and records corrections in the public record. Historical citation lists are archived; individual old citations are not carried forward.
```

This follows 06 §5, which routes citation corrections to `boxers@rrn.world`; the same address handles press and permissions under separate subject lines. If the field is structured, use the labels “Press” and “Citations”. This field carries enquiry copy only. Do not paste an old source list into it.

### 4.4 `banners` — three records

Remove the old Main page banner, its old image, and its `/index/2025` target. The proposed keys and placements below must be confirmed against the `/banners` serializer before import. Set `active` to `false` until the schedule, image, target, and alt text are approved; if the serializer has no active flag, leave the records unscheduled.

| Key | Placement | Headline | Subhead | CTA | Target | Alt text | Schedule | Image |
|---|---|---|---|---|---|---|---|---|
| `index-2026-launch` | `homepage-promo` | The 2026 Index is published. | The annual reading of the garment beneath the garment. Methodology, rankings, and revisions. | Read the Index | `/index` | Boxer Sovereignty Index 2026 — Ilya Boxers Institute. | `2026-11-01T00:00:00+03:00` to `2026-12-31T23:59:59+03:00`, `Europe/Moscow` | `/uploads/banner-index-2026.webp` |
| `collection-launch` | `homepage-secondary` | Garments for the unobserved hours. | The Institute's applied research: a small collection, one standard, no visible argument. | View the Collection | `/products` | The Collection — boxer shorts and loungewear by Ilya Boxers Institute. | `2026-11-01T00:00:00+03:00` to `2026-12-31T23:59:59+03:00`, `Europe/Moscow` | `/uploads/banner-collection.webp` |
| `quarterly-dispatch` | `footer-newsletter` | The Quarterly Dispatch. | One letter each quarter: new field notes, index revisions, and occasional notices from the Collection. No campaign literature. | Subscribe | `/#quarterly-dispatch` | The Quarterly Dispatch — the Ilya Boxers Institute newsletter. | `2026-11-01T00:00:00+03:00` to `2026-12-31T23:59:59+03:00`, `Europe/Moscow` | `/uploads/banner-dispatch.webp` |

```json
[
  {
    "key": "index-2026-launch",
    "placement": "homepage-promo",
    "headline": "The 2026 Index is published.",
    "subhead": "The annual reading of the garment beneath the garment. Methodology, rankings, and revisions.",
    "ctaLabel": "Read the Index",
    "targetUrl": "/index",
    "alt": "Boxer Sovereignty Index 2026 — Ilya Boxers Institute.",
    "startAt": "2026-11-01T00:00:00+03:00",
    "endAt": "2026-12-31T23:59:59+03:00",
    "timezone": "Europe/Moscow",
    "image": "/uploads/banner-index-2026.webp",
    "active": false
  },
  {
    "key": "collection-launch",
    "placement": "homepage-secondary",
    "headline": "Garments for the unobserved hours.",
    "subhead": "The Institute's applied research: a small collection, one standard, no visible argument.",
    "ctaLabel": "View the Collection",
    "targetUrl": "/products",
    "alt": "The Collection — boxer shorts and loungewear by Ilya Boxers Institute.",
    "startAt": "2026-11-01T00:00:00+03:00",
    "endAt": "2026-12-31T23:59:59+03:00",
    "timezone": "Europe/Moscow",
    "image": "/uploads/banner-collection.webp",
    "active": false
  },
  {
    "key": "quarterly-dispatch",
    "placement": "footer-newsletter",
    "headline": "The Quarterly Dispatch.",
    "subhead": "One letter each quarter: new field notes, index revisions, and occasional notices from the Collection. No campaign literature.",
    "ctaLabel": "Subscribe",
    "targetUrl": "/#quarterly-dispatch",
    "alt": "The Quarterly Dispatch — the Ilya Boxers Institute newsletter.",
    "startAt": "2026-11-01T00:00:00+03:00",
    "endAt": "2026-12-31T23:59:59+03:00",
    "timezone": "Europe/Moscow",
    "image": "/uploads/banner-dispatch.webp",
    "active": false
  }
]
```

Notes:

- `05-boxer-sovereignty-index.md` suggests the Index subhead “The Institute ranks first. The method is unchanged.” That sentence is omitted from the banner because this brief forbids claims in banner copy. Keep it on the Index page, beside the disclosure, where it belongs.
- The newsletter target `/#quarterly-dispatch` must be checked against the built homepage. If the anchor does not exist, target `/` and rely on the footer form. Do not invent a `/newsletter` route.
- No third-party ad inventory. No statistics. No price, availability, stockist, award, or performance claim.

### 4.5 `table`

Do not edit the old table in place. The old headers, country rows, and `Total / 700` column must not survive.

**Preferred action:** clear the field and render the 2026 ranking from `05-boxer-sovereignty-index.md`. This keeps one canonical source and prevents the settings table from drifting from the published Index.

**If the admin table must remain:** replace it with the ten rows under “The 2026 Ranking” in 05. Use the columns:

`Rank | Entrant | Origin | Composite | Tier | Note`

Paste the rows verbatim from 05. Check the following before saving:

- Ten ranked rows only.
- No old Place / Country / Macroregion columns.
- No `Total` column and no score out of 700. The 2026 composites are out of 100.
- The unranked control is not added to the ten-row table.
- The Reading Room annex is not added to the boxer-short table.
- Footnote 1 is handled separately if the template supports it; otherwise keep the disclosure paragraph on the page and omit the marker from the table.
- No invented scores, ranks, entrants, origins, or notes.

If neither the ten-row import nor the template render is ready by freeze, clear the field. Do not leave the old data visible as a fallback.

### 4.6 `pagesVisibility`

Known flags to set at launch:

| Key | Launch value | Rationale / condition |
|---|---|---|
| `donate` | `true` | The Support page carries approved institutional copy. Keep payment controls hidden until the payment route, legal copy, and any amounts are approved. No amounts or tiers in the page body. |
| `about` | `true` | Approved summary plus the full copy in 02. |
| `partners` | `true` | Launch with the explicit empty state: “No stockists are listed at launch.” No logos, names, or implied relationships. |
| `index` | `true` | Launch-critical. The 2026 edition, table, and disclosure come from 05. |
| `research` | `true` | The route is in primary navigation and has approved intro copy. Old geopolitical research must remain unpublished. If no approved studies exist, show an explicit empty state rather than old papers. |
| `osint` | `true` | The Open Source intro in 03 is approved. Old OSINT investigations must be unpublished and removed from search. If that is not complete by freeze, set `false` and remove the nav link. |
| `products` | `true` | Six Collection pieces have approved copy. No prices, availability dates, or performance claims. |
| `expertLogin` | `false` | No fellows are appointed and no expert area exists. Confirm with engineering that this flag does not gate customer sign-in at `/sign-in`. If it does, keep the shared auth surface live and hide only the expert entry point. |
| `educations` | `false` | No programme, course, accreditation, or qualification claims are approved. Old education content is archived. |
| `events` | `false` | No confirmed dates or programme. Do not invent a calendar. |
| `experts` | `false` | No approved fellows exist. This launch uses the stricter rule: no Fellows page and no nav link until an approved fellow exists. 03’s empty-state option is not used here. |
| `result25...` | `false` | The key is truncated in the audit. Identify the exact key before editing. If it is the legacy 2025 results flag, set `false` and archive the route. Do not guess the key name. |

Known keys as JSON, excluding the truncated legacy key:

```json
{
  "donate": true,
  "about": true,
  "partners": true,
  "index": true,
  "research": true,
  "osint": true,
  "products": true,
  "expertLogin": false,
  "educations": false,
  "events": false,
  "experts": false
}
```

Preserve any other keys not listed here unless they expose retired content. Merge the legacy `result25...` key into this object only after its exact name is confirmed.

### 4.7 `metaTags`

Set the field to:

```json
[]
```

Why clear it:

- `{"name":"script","content":"test"}` is a leftover test artifact. At minimum it is meaningless; if a renderer or downstream integration treats `script` as a directive, it is an unapproved code path. It does not belong in production.
- The empty entry is a no-op that can render an empty tag or confuse validation.
- Verification tokens are tied to a specific property and issued by the provider. No token has been supplied, so none should be invented.
- Script tags require security and privacy review. They are not an editorial field.
- Analytics and Search Console already have dedicated fields in this collection. Do not duplicate them in `metaTags`.

Add a tag only when an owner, purpose, provider-issued value, and removal date are documented and approved. No verification or script tags in this pass.

### 4.8 Duplicate Index fields

`aboutIndex` and `about-index` currently carry the same old Index formula content. They appear to be two editable copies of the same introduction.

- Set both to the value in §3.2 at launch, so either consumer renders correct copy.
- Treat `aboutIndex` as canonical because it matches the camelCase convention of `aboutData`, `partnersData`, `sourcesData`, and `donateText`.
- Before clearing or removing `about-index`, search the frontend and API for both keys and confirm which one the `/index` route reads.
- Do not delete the unused field until the code is deployed and the page is verified. Archive the unused value in the settings export rather than leaving two editable copies.
- Edit §3.2 once and paste to both fields. Do not let the two copies drift during review.

### 4.9 Keep-as-is fields

| Field | Action | Note |
|---|---|---|
| `gaEnabled` | Keep `true` | No change in this pass. |
| `gaId` | Keep `G-WLRFXXL39W` | Production measurement ID. Do not substitute a test property. |
| `searchConsoleActivationEnabled` | Keep current value | No change in this pass. |
| `searchConsoleActivationValue` | Keep current value; mask in handoff | The token is property-specific and is not reproduced here. Do not invent or rotate it without a provider-issued replacement. |

If the verified property changes, stop and obtain the new token. Do not reuse an old token across properties.

## 5. Editor.js note

The prose fields that store Editor.js JSON are `about`, `aboutIndex`, `about-index`, `about-research`, `donateText`, and `footerDonateText`. The values in §3 are plain text for editorial approval. JSON conversion is a separate, mechanical step and is not authorised by this file.

- Convert each paragraph to a `paragraph` block. Do not add headings unless the receiving template explicitly expects them.
- Do not paste Markdown syntax (`#`, `**`, `>`, backticks) into the editor. The Markdown in this document is for the handoff only.
- Preserve the approved punctuation and wording. Conversion must not rewrite, shorten, or “improve” the copy.
- None of the prose values contains a link. Do not add links during conversion.
- `donateText` must not acquire an amount, tier, payment button, or donor name through a template default. Those controls sit outside the field and require separate approval.
- `aboutData`, `partnersData`, `sourcesData`, `banners`, `table`, `pagesVisibility`, and `metaTags` are structured values, not Editor.js prose. Do not wrap them in paragraph blocks.
- Character counts apply to the plain text. The JSON envelope will be longer. Do not edit approved copy to make the JSON length match a limit.
- After conversion, render the page and check for duplicate paragraphs, lost paragraph breaks, smart-quote substitution, and leftover draft text.

## 6. Old-brand strings to remove from settings

| String or pattern | Field(s) | Action |
|---|---|---|
| `The Burke Institute Statement` | `about` | Replace with §3.1. |
| `Formula and Data Sources for Generating. The Sovereignty Index` | `aboutIndex`, `about-index` | Replace with §3.2. |
| `Burke Sovereignty Index` | Any settings value | Replace with `Boxer Sovereignty Index`, except approved archive or historical references outside settings. |
| `International Burke Institute` / `Burke Institute` | Any settings value | Remove as a current name. The single approved exception is the “formerly IBI International Burke Institute” line in the About copy (02 and the `about` summary) and any counsel-approved legal line. |
| Old Burke donation text | `donateText` | Replace with §3.4. |
| `We’d be grateful for your donation...` | `footerDonateText` | Replace with §3.5. Search both curly and straight apostrophes. |
| Tel Aviv address | `aboutData` | Remove. Use Monogram House, Moscow. |
| `info@ibi.institute` | `aboutData`, `partnersData` | Remove. Use `boxers@rrn.world`. |
| `+972502901775` | `phoneNumber`, `partnersData` | Remove. Use “by appointment”; do not publish a number. |
| Old business hours | `aboutData`, `partnersData` | Remove. Use Monday–Friday, 10:00–18:00 MSK. |
| Old banner image and `/index/2025` link | `banners` | Remove. Replace with the three records in §4.4. |
| `{"name":"script","content":"test"}` and the empty entry | `metaTags` | Clear to `[]`. |
| `Place`, `Country`, `Macroregion`, `Total`, `700` | `table` | Clear, or replace from 05. Do not rescale the new scores to 700. |
| `result25...` | `pagesVisibility` | Identify the exact key, set `false` if it is the 2025 results flag, and archive the route. |
| Old expert, partner, and OSINT labels in serialized defaults | Any settings value | Remove. Public labels are Fellows, Stockists, and Open Source. |
| `Donate` as a public label | `footerDonateText` context | Keep the field key; the public label is Support. |

Final settings sweep: search case-insensitively for `Burke`, `Tel Aviv`, `info@ibi.institute`, `+972`, `index/2025`, `Macroregion`, `Total`, `700`, `"name":"script"`, `"content":"test"`, `We’d`, and `We'd`. Do not publish until there are zero matches outside the approved historical and legal copy.

## 7. Values to confirm before publishing

| # | Item | Field(s) | Launch default | Confirm before publishing |
|---:|---|---|---|---|
| 1 | Approval of the 06 excerpts | `about-research`, `donateText` | Values in §3.3 and §3.4; the full page copy stays in 06. | Editorial sign-off on the excerpting. |
| 2 | Institutional address | `aboutData` | Monogram House, Moscow. | Confirm the public wording; do not reuse Tel Aviv. Hide the row if unconfirmed. |
| 3 | Telephone | `phoneNumber`, `aboutData`, `partnersData` | By appointment; no number published. | Confirm the appointment wording and keep every `tel:` link hidden. |
| 4 | Hours | `aboutData`, `partnersData` | Monday–Friday, 10:00–18:00 MSK. | Confirm hours and timezone. Hide the row if unconfirmed. |
| 5 | Mailboxes | `aboutData`, `partnersData`, `sourcesData` | boxers@rrn.world for general, stockist, press, and citation enquiries. | Confirm routing and subject lines; no separate wholesale or press mailbox is published. |
| 6 | Banner schedule | `banners` | 1 November 2026 to 31 December 2026, Europe/Moscow. | Confirm the window and set `active: true` only after approval. |
| 7 | Banner images | `banners` | `/uploads/banner-index-2026.webp`, `/uploads/banner-collection.webp`, `/uploads/banner-dispatch.webp`. | Confirm the final assets; records stay inactive until images are approved. |
| 8 | Newsletter anchor | `banners` | `/#quarterly-dispatch`. | Verify against the built homepage; otherwise target `/` and use the footer form. |
| 9 | Table rendering decision | `table` | Clear the field and render from 05. | Confirm the template render or import the ten rows from 05. |
| 10 | Duplicate field consolidation | `aboutIndex`, `about-index` | Keep identical values from §3.2. | Complete the code audit before archiving either key. |
| 11 | Any approved meta tag | `metaTags` | `[]`. | Add only a provider-issued tag with an owner, purpose, and removal date. |
| 12 | `expertLogin` dependency | `pagesVisibility` | `false`. | Confirm with engineering that it does not gate customer sign-in at `/sign-in`. |
| 13 | `result25...` exact key | `pagesVisibility` | Set `false` if it is the 2025 results flag. | Identify the truncated key before editing. |
| 14 | Search Console property and token | `searchConsoleActivationEnabled`, `searchConsoleActivationValue` | Keep current values. | Do not invent or rotate the token without a provider-issued replacement. |
| 15 | GA confirmation | `gaEnabled`, `gaId` | Keep enabled and keep `G-WLRFXXL39W`. | Confirm no change is required at freeze. |
| 16 | Legal entity, jurisdiction, registration number | Not in the 18 fields | Keep legal text out of settings. | Counsel must supply any legal line; no registration number is invented. |
| 17 | Index 2026 approval | `aboutIndex`, `table` | Prepare, do not publish. | Approve the edition and its table before publication. |
| 18 | Public launch date and flags | `banners`, `pagesVisibility` | Banners inactive; visibility flags as §4.6. | Confirm at freeze and activate banners only after image approval. |

## 8. Pre-paste QA

- All 18 fields have an action; no old copy remains in any field.
- No public page renders an unconfirmed draft value.
- Character counts match: `about` 600; Index 499; Research 451; Support 520; footer 57.
- `metaTags` is `[]`.
- `table` contains no old country data, no `Macroregion`, no `Total`, and no score out of 700.
- Banners contain no `/index/2025` link, no statistics, no third-party inventory, and no price or availability claim.
- Banner records are inactive until schedule, image, target, and alt text are approved.
- Old-brand sweep returns zero matches outside approved historical and legal copy.
- RRN.world credit is present and exact in the footer and colophon.
- Editor.js conversion has been reviewed as a separate diff.
- Search Console token is unchanged; GA settings are unchanged.
- `aboutIndex` and `about-index` are identical, or the unused field has been archived after the code audit.

Credit: With love, made by RRN.world — Rotten Reliable News.
