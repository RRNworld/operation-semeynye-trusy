# Settings payload notes — 22 October 2026

**Payload file:** `content/15-settings-payloads.json`  
**Scope:** the 14 requested `/settings` fields, prepared as `{ "value": "<string>" }` values.  
**Method:** built from the approved content files and the current settings export. No network calls and no API calls were made. Existing content files were not modified.

The payload is a single top-level object. Every value is a string. Structured values (`sourcesData`, `metaTags`, `banners`, `aboutData`, `partnersData`, `pagesVisibility`, `table`) are JSON objects serialized with `json.dumps(..., ensure_ascii=False)`. Editor.js values (`about`, `aboutIndex`, `about-index`, `about-research`) are serialized envelopes with `time`, `blocks`, and `version`.

## 1. Field-by-field sources

### `sourcesData`
- **Source:** `06-secondary-pages.md` §3 (Press, Permissions, Institutional address) and §5 (Citations, Corrections).
- **Shape:** `{text, email, phone, hours}`, matching the empty object in the current backup.
- **Value:** press and permissions routed through Mikhail Biyun, Head of Special Operations; citation corrections to `boxers@rrn.world` with the subject line “Citation correction.” Phone is `By appointment`; hours are `Monday–Friday, 10:00–18:00 MSK`.
- **Not included:** any old source list, old mailbox, or telephone number.

### `metaTags`
- **Source:** `07-settings-and-banners.md` §4.7 and the task instruction.
- **Value:** `[]`.
- **Reason:** clears the leftover `{"name":"script","content":"test"}` and empty entry. No verification or script tag is invented.

### `banners`
- **Source:** task-provided upload filenames and links; key names and object shape from the current backup.
- **Shape:** `{"Main page": {...}, "Footer": {...}, "Article 1": {...}, "Article 2": null}`. Each non-null record has `loading: false`, `serverPath`, and `link`. The current serializer shape has no `active`, `alt`, or schedule field, so none was added.
- **Paths:** `serverPath` values are filenames only, with no `uploads/` prefix, as requested.

Banner mapping:

| Placement | `serverPath` | Target link | Intended content |
|---|---|---|---|
| Main page | `38b42f82-b5f7-4d45-bdcb-252d0c2ffbfb.webp` | `https://ibi.institute/read/boxer-sovereignty-index-2026?type=index` | 2026 Boxer Sovereignty Index |
| Footer | `c61f10a4-f37f-4b2e-89ac-8755c90f0c3a.webp` | `https://ibi.institute/read/research-method-2026?type=research` | 2026 research method |
| Article 1 | `e5a7e63a-908c-42fd-86f6-b33bc159e2cc.webp` | `https://ibi.institute/read/portugal?type=index` | Portugal index page |
| Article 2 | — | — | No record; value is `null` |

### `aboutData`
- **Source:** `02-founder-about.md` “The Operations Desk” and `06-secondary-pages.md` §3.
- **Shape:** array of `{text, email, hours}`, matching the current backup.
- **Rows:**
  1. General enquiries — `boxers@rrn.world` — `Monday–Friday, 10:00–18:00 MSK`.
  2. Press and operations — Mikhail Biyun, Head of Special Operations, Monogram House, Moscow — `boxers@rrn.world` — same hours.
- **Not included:** the old Tel Aviv address, `info@ibi.institute`, the old telephone number, or a `phone` key (the current row shape does not have one; the global phone is `phoneNumber`).

### `partnersData`
- **Source:** `06-secondary-pages.md` §4 (Stockists, Wholesale enquiries).
- **Shape:** `{text, email, phone, hours}`, matching the backup.
- **Value:** stockist-enquiry copy; confirmed stockists only; an enquiry implies no partnership; subject line “Stockist enquiry.” Email `boxers@rrn.world`; phone `By appointment`; hours `Monday–Friday, 10:00–18:00 MSK`.
- **Not included:** stockist names, logos, territories, or partnership claims.

### `phoneNumber`
- **Source:** task instruction; `06-secondary-pages.md` §3 (“Telephone: by appointment”).
- **Value:** `By appointment`.
- **Implementation note:** do not render this as a `tel:` link. The old `+972502901775` number is removed.

### `footerDonateText`
- **Source:** `06-secondary-pages.md` §6, especially the standfirst.
- **Value:** two deadpan sentences: support the Institute; support does not purchase influence, ranking, or editorial consideration.
- **Not included:** amounts, tiers, bank details, or donor names.

### `donateText`
- **Source:** `06-secondary-pages.md` §6 (Why support, One-time support, Recurring support, Supporters, Governance).
- **Value:** four short plain-text paragraphs. It covers independent research and Index maintenance; the archive and correction record; one-time and recurring support; supporter consent and the empty supporter list; governance and the conflict-of-interest review. It includes `boxers@rrn.world`.
- **Not included:** amounts, tiers, bank details, payment controls, processing fees, or donor names.

### `pagesVisibility`
- **Source:** task instruction; exact key names confirmed against `backup/settings-2026-10-22.json`.
- **Values:** `donate`, `about`, `partners`, `index`, `research`, `osint`, `products`, `sources` are `true`. `educations`, `events`, `expertLogin`, `experts`, `result25` are `false`.
- **Note:** the backup had `expertLogin: true` and `result25: true`; both are changed here. The backup confirms the full key is `result25`, not a truncated name.

### `table`
- **Source:** `05-boxer-sovereignty-index.md`, “The 2026 Ranking.”
- **Shape:** `{"header": ["Rank","Entrant","Origin","Composite","Tier","Note"], "data": [[...], ...]}`.
- **Types:** `Rank` is an integer, `Composite` is a number; all other cells are strings. Markdown bold markers were removed from entrant names.
- **Rows:** exactly the ten ranked garments. The unranked Unbranded Multipack and the Reading Room annex are excluded, as is footnote 1.
- **Replacement:** this replaces the old country table, including its `Place`, `Country`, `Macroregion`, `Total/700`, and `% Total` columns.

### `about`
- **Source:** `02-founder-about.md`.
- **Included:** subtitle; the founder account; “The Operations Desk”; the professional timeline; the Ilya Method; the FAQ; Credits and Colophon.
- **Excluded:** the H1 (`# The First Layer`), the entire `## Pull Quotes` section, the `## Page Metadata` section, and every `[draft quote — for founder approval]` line.
- **Top block:** image `uploads/f5e310c5-b3ca-421b-908e-c4f606ad560f.webp` (image-02).
- **Privacy note:** the founder account is reproduced from the approved 02 copy. No family details were added. The approved source does contain birth details and the 2009 kidnapping episode; if the client wants a stricter privacy cut, those passages should be redacted before applying, not silently rewritten here.

### `aboutIndex` and `about-index`
- **Source:** `05-boxer-sovereignty-index.md`, from the top through the end of `## Methodology`, cut before `## The 2026 Ranking` (source lines 1–64).
- **Included:** breadcrumb, H1, standfirst, body, disclosure, “What the Index Measures” and its seven dimensions, the full Methodology (Sample, Panel, Scoring, Audit cadence, Tier bands, tier-bands table), and the former Burke Sovereignty Index line.
- **Excluded:** “The 2026 Ranking” and all later sections. The ranking lives in the `table` field, not in this intro.
- **Top block:** the same image-02 file.
- **Duplicate keys:** both fields receive the **exact same string**, including block IDs, because the settings export shows both keys carrying the old Index copy and the consumer is still unknown. Do not delete either until the frontend/API code audit identifies the canonical key.

### `about-research`
- **Source:** `06-secondary-pages.md` §1 (`/research`), lines 15–63.
- **Included:** H1, standfirst, body, Current studies (Study 01–03), Method, The standing panel, Archive, and Corrections.
- **Excluded:** the Empty state, CTA, and SEO blocks.
- **Header levels:** the deck uses `###` for these sections under its `## 1. /research` divider. In the payload they are promoted to `header` level 2 so the page has H1 → H2 structure.
- **Top block:** the same image-02 file.

## 2. Shape assumptions

- **JSON-in-string:** every structured field is double-encoded exactly as the settings API expects: the outer value is a string, and parsing that string yields the object or array.
- **Editor.js envelope:** `{"time": 1792627200000, "blocks": [...], "version": "2.31.0"}`. The timestamp is `2026-10-22T00:00:00Z`, used only as a stable build timestamp.
- **Allowed block types:** `header`, `paragraph`, `table`, `image`. No `list` or `delimiter` blocks were needed. The block types used are all within the permitted set.
- **Header/paragraph tunes:** `{"textAlignment": {"alignment": "left"}}`, matching the current export.
- **Image block:** `data.file.url` is the relative path `uploads/f5e310c5-b3ca-421b-908e-c4f606ad560f.webp`; `caption`, `withBorder`, `stretched`, and `withBackground` are present with default values. If the custom renderer expects `data.url` or an absolute `/uploads/...` path, transform the URL only; do not rebuild the block.
- **Table block:** `{"withHeadings": true, "content": [[...], ...]}`. The tier-bands table is the only table block in the Editor.js fields.
- **`table` setting:** `header` plus `data`; no wrapper object beyond that, matching the current backup shape.
- **`banners`:** the current backup shape is `loading`, `serverPath`, `link`, so no `active` or scheduling fields were added. If the live serializer requires them, add them only after checking the `/banners` handler.

## 3. Risks and checks

1. **The `table` frontend may still expect the old 12-column country table.** The current table component may read positions such as `row[10]` (Total/700) or `row[11]` (% Total), or may assume headers `Place`, `Country`, `Macroregion`, and the seven dimension columns. The new six-column garment table will then render blank cells, misalign, or break. Test the `/index` table render before applying this field, or wait for the frontend table component to be updated. Do not restore the old country data as a fallback.
2. **`aboutIndex` vs `about-index`.** Both keys are set to the same string, but the consuming key is still unconfirmed. Audit the frontend/API before removing either. Duplicate keys are safer than a blank Index intro.
3. **Banner serializer.** The payload matches the current export shape only. If the live handler expects `active`, `alt`, a schedule, or different placement keys, the records may save but not render. `Article 2` is `null`; verify the frontend has a null guard.
4. **Banner paths.** `serverPath` is a filename only, as instructed. If the renderer does not prepend the uploads base path, the images will 404. Check one banner first.
5. **Editor.js image shape.** `data.file.url` is the standard Editor.js image-tool shape, but custom renderers sometimes store `data.url`. Verify one page before applying all four.
6. **`pagesVisibility` and `expertLogin`.** Setting `expertLogin` to `false` could hide a shared sign-in surface if the flag also gates `/sign-in`. Confirm with engineering. `result25` is confirmed against the backup and set to `false`; verify the old results route is archived.
7. **`about` length.** The `about` value is about 17 KB because it carries the full founder account, timeline, method, FAQ, and credits. If the settings API, database column, or admin form has a smaller limit, the write may fail or truncate. Test this field on its own.
8. **Founder privacy.** The 02 copy includes birth details and the kidnapping passage. It is approved source copy, but the task also says no personal/family details. No family details were added. If the stricter reading applies, redact those two passages before applying.
9. **RRN.world credit.** The credit is retained in `about` (Credits and Colophon) and in the global footer copy. The `aboutIndex` and `about-research` excerpts stop before the full-page colophons, so they do not duplicate the credit. Confirm the live page footer/colophon still renders the exact RRN.world line.
10. **Historical strings.** `aboutIndex` retains the approved historical line “The former Burke Sovereignty Index ranked approximately 160 countries…” and `about` retains “formerly IBI International Burke Institute.” These are approved historical references, not current branding. The old email, phone, Tel Aviv address, `Macroregion`, `Total/700`, and `info@ibi.institute` strings are absent from the payload.

## 4. Recommended apply order

1. **Test one field first.** Apply `phoneNumber` (short plain string) to a staging or test setting, then read it back and verify the public contact/footer surfaces. If the admin/API path is healthy, apply `footerDonateText`. Keep the 22 October settings export as the rollback.
2. **Apply the structured fields.** Order: `metaTags` → `aboutData` → `partnersData` → `sourcesData` → `table` → `banners`. Apply `table` separately and inspect `/index` before continuing; it is the highest-risk field because of the old 12-column component. Apply `banners` one placement at a time and check the image path and null `Article 2`.
3. **Apply the Editor.js pages.** Order: `about-research` (smallest) → `aboutIndex` and `about-index` (same value, apply to both) → `about` (largest). Render each page after its write and check for lost paragraph breaks, duplicate blocks, the image, and leftover draft text.
4. **Apply `pagesVisibility` last.** It gates routes and navigation, so apply it only after the destinations render correctly. Then run the final crawl: `/index`, `/research`, `/about`, `/contacts`, `/partners`, `/sources`, `/donate`, banner targets, and the old-brand/old-table sweep.

If `table` or `banners` fails the render check, stop and leave the previous value in place rather than mixing old and new data.

## 5. Verification summary

- Outer JSON parses; keys are exactly the 14 requested names in the requested order.
- Every top-level value is a string.
- Inner JSON parses for all structured and Editor.js fields.
- `table` has 10 rows and the new 6-column header; Rank/Composite are numeric.
- Editor.js envelopes use `version: "2.31.0"` and `time: 1792627200000`; block counts: `about` 51, `aboutIndex`/`about-index` 31, `about-research` 25.
- `about` contains no `Pull Quotes`, `Page Metadata`, or `[draft quote` text.
- `pagesVisibility` matches the requested true/false set.
- No API, network, or existing-content-file changes were made.

Credit: With love, made by RRN.world — Rotten Reliable News.
