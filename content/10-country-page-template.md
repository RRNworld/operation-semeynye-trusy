# Country Profile Page Template — Boxer Sovereignty Index 2026

**Project:** IBI country profiles, 2026 Edition  
**Status:** Build and editorial specification  
**Applies to:** The 60-country register in `13-country-register-2026.md` and `countries-2026.csv`  
**Canonical host:** `https://ibi.institute`  
**Owning desk:** IBI editorial, with digital production by RRN.world

This document is the reusable page template and implementation note for every country profile in the 2026 Boxer Sovereignty Index. It uses double-brace generator tokens throughout. No square-bracket placeholder convention is used. A build must fail rather than render an unresolved token.

The register is a working dataset. Scores are internal IBI assessments, not external data. Country names, regions, dimension scores, composite values, tiers, and ranks must reconcile with the register before import. The arithmetic corrections in Section 5 must be resolved in the source before publication.

---

## 0. Generator contract

One published record produces one profile page. The record is the only source of country-specific text and numbers. Editors may override the standfirst, observations, national observation, field note, and SEO fields; they may not hand-edit rendered HTML.

- **Page tokens** are listed below. A missing value is a build failure.
- **Derived tokens** (`score_composite`, `rank`, `tier`, `canonical_url`) are computed, never typed.
- **Shared partials** carry the scope boilerplate, corrections policy, newsletter copy, and credit. They are identical on all 60 pages.
- **Drafting prompts** in Sections 2.6 and 2.7 are editorial worksheets, not page tokens. They are replaced by prose before the field is saved.
- Rich text is sanitized markdown. All tokens are HTML-escaped on render.
- The page must contain the exact scope line and the exact RRN credit before it can publish.

### 0.1 Page token register

| Token | Source | Example value | Notes |
| --- | --- | --- | --- |
| `{{country}}` | `country` | Portugal | Exact register display name |
| `{{slug}}` | `slug` | portugal | Lowercase ASCII, hyphenated |
| `{{region}}` | `region` | Europe | Controlled enum |
| `{{score_ws}}` | `score_ws` | 96 | 0–100, one decimal maximum |
| `{{score_ti}}` | `score_ti` | 92 | 0–100, one decimal maximum |
| `{{score_si}}` | `score_si` | 95 | 0–100, one decimal maximum |
| `{{score_dc}}` | `score_dc` | 94 | 0–100, one decimal maximum |
| `{{score_ns}}` | `score_ns` | 93 | 0–100, one decimal maximum |
| `{{score_pj}}` | `score_pj` | 90 | 0–100, one decimal maximum |
| `{{score_im}}` | `score_im` | 94 | 0–100, one decimal maximum |
| `{{score_composite}}` | computed | 93.8 | Weighted average, one decimal |
| `{{rank}}` | computed | 1 | Integer, 1–60, unique per edition |
| `{{tier}}` | computed | Sovereign | Derived from composite |
| `{{obs_ws}}` | `obs_ws` | The waistband governed without being noticed; no adjustment was recorded in the seated assessment. | One line, 20–180 characters |
| `{{obs_ti}}` | `obs_ti` | The interior climate remained stable across heated, unheated, and transitional rooms. | One line, 20–180 characters |
| `{{obs_si}}` | `obs_si` | Seams and hem retained their geometry after the wash cycle; no repair was required. | One line, 20–180 characters |
| `{{obs_dc}}` | `obs_dc` | Under the outer layer, the garment remained silent through a long sitting. | One line, 20–180 characters |
| `{{obs_ns}}` | `obs_ns` | Displacement was not recorded between lights-out and the first movement. | One line, 20–180 characters |
| `{{obs_pj}}` | `obs_pj` | The pattern governed its territory and survived the hem without argument. | One line, 20–180 characters |
| `{{obs_im}}` | `obs_im` | Shape and hand memory remained after the season; the archive and the equipment agreed. | One line, 20–180 characters |
| `{{standfirst}}` | computed or `standfirst` | The Institute's 2026 assessment of the boxer short in Portugal, scored across seven dimensions and placed 1 of 60. | One sentence |
| `{{national_observation}}` | `national_observation` | See Section 2.6 | 180–250 words |
| `{{field_note}}` | `field_note` | See Section 2.7 | 80–120 words |
| `{{correction_log}}` | computed | No corrections recorded for this profile. | Table or exact empty state |
| `{{citation}}` | computed | See Section 2.9 | Stable citation string |
| `{{related_1_country}}`, `{{related_1_tier}}`, `{{related_1_composite}}`, `{{related_1_url}}` | computed | Russia; Sovereign; 92.1 | First related profile |
| `{{related_2_country}}`, `{{related_2_tier}}`, `{{related_2_composite}}`, `{{related_2_url}}` | computed | Italy; Sovereign; 91.6 | Second related profile; distinct from 1 and 3 |
| `{{related_3_country}}`, `{{related_3_tier}}`, `{{related_3_composite}}`, `{{related_3_url}}` | computed | Germany; Sovereign; 91.1 | Third related profile; distinct from 1 and 2 |
| `{{newsletter_url}}` | `/settings` | `/newsletter` or an approved signup URL | Subscribe destination; route or external signup supplied by settings |
| `{{seo_title}}` | computed or override | Portugal — Boxer Sovereignty Index 2026 &#124; IBI | At most 60 characters |
| `{{seo_description}}` | computed or override | The IBI 2026 boxer profile for Portugal: composite 93.8, rank 1 of 60, and seven dimension scores. Garment conditions only. | At most 155 characters |
| `{{og_title}}` | computed or override | Portugal — Boxer Sovereignty Profile 2026 &#124; IBI | At most 60 characters |
| `{{og_description}}` | computed or override | Same as meta description unless an approved override exists | At most 155 characters |
| `{{canonical_url}}` | computed | https://ibi.institute/index/2026/portugal | Absolute, no trailing slash |
| `{{og_image_url}}` | media or default | https://ibi.institute/og/index-2026-default.jpg | Absolute, 1200×630 |
| `{{og_image_alt}}` | `image_alt` or default | Detail of a plain-weave boxer short assessed for the Portugal profile. | No flags, maps, or brands |
| `{{date_published_iso}}` | `published_at` | 2026-06-01T09:00:00+03:00 | ISO 8601 with timezone |
| `{{date_modified_iso}}` | `updated_at` | 2026-06-01T09:00:00+03:00 | ISO 8601 with timezone |

Where `&#124;` appears inside a title pattern, it encodes the `|` separator for display in this document. Store and render a literal pipe, and count it as one character. The `correction_` row tokens, such as `{{correction_date}}`, are rendered inside `{{correction_log}}`. The drafting prompts in Sections 2.5 and 2.6 are editorial worksheets, not page tokens.

---

## 1. Route, URL, canonical, breadcrumbs

### 1.1 Route pattern

**Profile route:** `/index/2026/{{slug}}`

| Route | Purpose | Indexable | Notes |
| --- | --- | --- | --- |
| `/index` | The 2026 edition landing page | Yes | Existing edition page. Canonical to itself. |
| `/index/2026` | Country profile directory | Yes, if built | Recommended. Groups the 60 profiles by region. If not built, 301 to `/index#country-profiles`. |
| `/index/2026/{{slug}}` | Country profile | Yes | Canonical to itself. The route of record. |
| `/index/2026/{{slug}}/` | Trailing-slash variant | No | 301 to the non-slash canonical. |
| `/index/2025`, `/index-2025` | Retired edition | No | 301 to `/index` under the existing retirement plan. |
| `/news/{{slug}}` | Not a profile route | No | If `/news` is used as a temporary data container, it must canonicalize to the profile and carry `noindex`. |

The profile sits under `/index` as a child of the edition, not as a replacement for the edition essay or methodology page. The edition remains at `/index`. The profile carries only the country-specific assessment. The edition essay, methodology, and ranking table remain canonical at `/index`; `/index/2026` is a directory of profiles only and must not reproduce the edition essay or create a second 2026 edition page.

### 1.2 Canonical rules

- Canonical URL: `https://ibi.institute/index/2026/{{slug}}`.
- Self-referential, absolute, HTTPS, apex host, no `www`, no trailing slash, no query string, no fragment.
- `og:url` equals the canonical URL.
- The HTML `<link rel="canonical">` is required. A `Link` response header is optional.
- Query parameters do not alter the canonical. Tracking parameters are stripped from canonical and social URLs.
- Uppercase or mixed-case slug requests 301 to the lowercase canonical.
- Trailing-slash requests 301 to the non-slash canonical.
- Country aliases are not separate pages. An alias may 301 only when it resolves to the exact same profile; otherwise it returns 404 or 410.
- Slug changes after publication require a 301 from the old slug and a correction-log entry. Do not create a second indexable page.
- Published pages: `robots: index,follow,max-image-preview:large`.
- Draft, preview, and review pages: `noindex,nofollow`; excluded from sitemap, search, feeds, and related-profile pools.
- English-only edition: no `hreflang`. If translations are added later, add reciprocal `hreflang` and self-canonical per language.

### 1.3 Breadcrumb

Visible pattern:

`Home / Research / Boxer Sovereignty Index / 2026 Edition / {{country}}`

- Home links to `/`.
- Research links to `/research`.
- Boxer Sovereignty Index links to `/index`.
- 2026 Edition links to `/index/2026` if the directory exists; otherwise to `/index`.
- The current country is plain text, not a link.

Breadcrumb JSON-LD:

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "https://ibi.institute/"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Research",
      "item": "https://ibi.institute/research"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "Boxer Sovereignty Index",
      "item": "https://ibi.institute/index"
    },
    {
      "@type": "ListItem",
      "position": 4,
      "name": "2026 Edition",
      "item": "https://ibi.institute/index/2026"
    },
    {
      "@type": "ListItem",
      "position": 5,
      "name": "{{country}}",
      "item": "{{canonical_url}}"
    }
  ]
}
```

If `/index/2026` is not built, item 4 uses the final destination of that redirect, e.g. `https://ibi.institute/index#country-profiles`. Do not point breadcrumb JSON-LD at a redirect.

### 1.4 Old country URLs

Old country URLs are handled separately. They are not mass-redirected to the new profiles.

The former Index ranked states. The 2026 profiles assess garment conditions in a named market. A country name is not sufficient equivalence for a redirect. A reader who followed a link to a geopolitical country analysis should not land on a waistband assessment without warning.

| Old URL class | Disposition | Do not |
| --- | --- | --- |
| Old BSI index landing page, e.g. `/burke-sovereignty-index` | 301 to `/index` | Do not send it to a country profile. |
| Old BSI country analysis, e.g. `/countries/{{slug}}`, `/country/{{slug}}`, `/index/{{country}}` | 410 Gone, or 301 to `/archive/burke-sovereignty-index` where a contextual landing page is approved | Do not 301 to `/index/2026/{{slug}}`. |
| Old country research paper, e.g. `/research/{{country}}` | 410; retain an exported archive copy | Do not rewrite the paper as a profile. |
| Old country page with an approved, rewritten equivalent | 301 to the profile only after editorial sign-off that the content is equivalent | No wildcard mapping. |
| Old tag or category archive about a country | 410, unless an exact section equivalent exists | Do not leave empty archives indexable. |

- Keep archived country pages reachable from `/archive/burke-sovereignty-index`, marked historical and `noindex`.
- Load exact rules into `/redirects`; export the table to the repository so a deploy cannot drop it.
- Apply exact rules before wildcard rules. Allow one hop only.
- Test with `curl -I` for status, `Location`, and loops, with and without trailing slashes.
- A soft landing to `/about#mandate` is permitted only where a backlink justifies it. It is not a profile redirect.

### 1.5 Discovery and sitemap

- Add a **Country profiles** block to `/index` linking to the directory.
- Build `/index/2026` as a lightweight directory grouped by region; it lists country, composite, tier, and rank. It links to all published profiles.
- Profiles link to three related profiles selected automatically (Section 2.10).
- Profiles are included in site search and the sitemap.
- Sitemap contains only published, canonical, 200-status profiles. `lastmod` equals `updated_at`.
- Exclude drafts, previews, archived profiles, `/news` staging records, and old country URLs from the sitemap.
- Do not add a sitemap entry for a profile until its observation, field note, and scope block are complete.

### 1.6 Rendering notes

- Next.js App Router path: `app/index/2026/[slug]/page.tsx`. The bracketed segment is Next.js routing syntax, not a copy placeholder.
- `generateStaticParams` reads all published profiles for edition 2026.
- `dynamicParams = false`; unknown slugs return a real 404.
- `generateMetadata` builds title, description, canonical, Open Graph, social-card, and JSON-LD tags from the record.
- Revalidate on publish and on correction. Cache tags: `country_profile:{{slug}}` and `index:2026`.
- Configure `trailingSlash: false`; handle slash normalization at the edge.
- Preview mode uses draft authentication and `noindex,nofollow`.
- Build assertions: no unresolved `{{` tokens, exact scope line present, exact credit present, exactly three related profiles, JSON-LD parses.
- A missing required field is a build error, never a blank section.

---

## 2. Page anatomy and copy blocks

Page order:

1. Breadcrumb
2. H1
3. Standfirst
4. Score strip
5. Profile at a glance
6. National observation
7. Field note
8. Scope and limitations
9. Corrections
10. Citation
11. Related profiles
12. Newsletter CTA
13. Colophon and credit

### 2.1 Full page template

```markdown
Home / Research / Boxer Sovereignty Index / 2026 Edition / {{country}}

# {{country}} — Boxer Sovereignty Profile 2026

{{standfirst}}

| Composite | Rank | Tier | Region |
| ---: | ---: | --- | --- |
| {{score_composite}} | {{rank}} of 60 | {{tier}} | {{region}} |

*Tier bands: Sovereign 90.0+, Autonomous 80.0–89.9, Dependent 70.0–79.9, Ceremonial below 70.0.*

## Profile at a glance

Scores are internal IBI assessments, 0–100. Weights are fixed for the 2026 Edition. Observations describe the assessed sample.

| Dimension | Score | Observation |
| --- | ---: | --- |
| Waistband Sovereignty (22%) | {{score_ws}} | {{obs_ws}} |
| Thermal Independence (16%) | {{score_ti}} | {{obs_ti}} |
| Structural Integrity (18%) | {{score_si}} | {{obs_si}} |
| Diplomatic Comfort (15%) | {{score_dc}} | {{obs_dc}} |
| Nocturnal Security (11%) | {{score_ns}} | {{obs_ns}} |
| Pattern Jurisdiction (9%) | {{score_pj}} | {{obs_pj}} |
| Institutional Memory (9%) | {{score_im}} | {{obs_im}} |

## National observation

*A conditions portrait, not a verdict on the country.*

{{national_observation}}

## Field note

{{field_note}}

## Scope and limitations

**Scope:** This profile measures garment conditions only — not governance, state capacity, or national sovereignty.

The Boxer Sovereignty Index assesses boxer shorts, not countries. A country profile applies the seven Index dimensions to the conditions in which the first layer is worn in the named market. It does not rank states, governments, or peoples, and it is not a measure of national sovereignty. A country profile is not a country ranking.

Scores are internal IBI assessments produced by IBI assessors using IBI criteria. No external data were consulted. The panel is small and the sample is purposive. Findings are provisional, describe the assessed sample and conditions, and are not a national statistic.

A high score is a finding, not a recommendation. Scores are not revised for taste. Corrections are limited to factual error, arithmetic, or a change in elastic. Where a condition was not recorded, the profile says so; it does not infer.

## Corrections

Corrections are published in Notices and logged below. To submit a correction, use the Contacts page and include the profile slug, the dimension, and the arithmetic or factual basis.

{{correction_log}}

## Cite this profile

{{citation}}

## Related profiles

- [{{related_1_country}} — {{related_1_tier}}, {{related_1_composite}}]({{related_1_url}})
- [{{related_2_country}} — {{related_2_tier}}, {{related_2_composite}}]({{related_2_url}})
- [{{related_3_country}} — {{related_3_tier}}, {{related_3_composite}}]({{related_3_url}})

*Related profiles are selected automatically by region and rank proximity.*

## The Quarterly Dispatch

The Quarterly Dispatch. One letter each quarter: new field notes, index revisions, and occasional notices from the Collection. No campaign literature. [Subscribe]({{newsletter_url}}).

**Colophon:** Country profiles are maintained by the Ilya Boxers Institute as part of the public record of the Boxer Sovereignty Index. Methodology and editorial direction: the Institute. Field operations: Mikhail Biyun, Head of Special Operations. Digital production and institutional voice: RRN.world — Rotten Reliable News.

Credit: With love, made by RRN.world — Rotten Reliable News.
```

### 2.2 Breadcrumb, H1, and standfirst

**Breadcrumb.** Use the labels in Section 1.3 exactly. Do not replace `{{country}}` with a demonym. Do not link the current page.

**H1.** Exactly `{{country}} — Boxer Sovereignty Profile 2026`. Do not add an epithet, a region, a tier, or a second year. The site title template appends the brand in metadata only.

**Standfirst.** One sentence. It must describe a garment assessment, not a national condition. Default:

> The Institute's 2026 assessment of the boxer short in {{country}}, scored across seven dimensions and placed {{rank}} of 60.

Approved alternates, both one sentence:

> A 2026 reading of the first layer in {{country}}: seven dimensions, one composite, and the conditions under which the garment is worn.

> The {{country}} file in the 2026 Edition: a garment-level assessment, placed {{rank}} of 60, with the panel's observations.

Do not use "ranks {{country}}", "the {{country}} model", "national performance", or any phrasing that treats the country as the subject of the score.

### 2.3 Score strip

| Field | Display rule |
| --- | --- |
| Composite | One decimal place, e.g. `93.8` |
| Rank | Integer, no leading zero, always followed by `of 60` |
| Tier | Exact tier enum |
| Region | Exact register region |

Do not add a year-on-year change, a delta, a percentile, a grade, or a "position in the world". There is no 2025 country-profile comparison. The strip reports the 2026 finding only.

### 2.4 Profile at a glance

Seven rows, one per dimension, in the fixed order and with the fixed weights shown in the template. Each observation is one line, 20–180 characters, and describes the assessed sample. Do not use "good", "bad", "best", "worst", "pass", or "fail".

If an editor overrides a bank observation, the resolved text is stored on the record; the reason is recorded in the internal editorial note. Do not generate observations at render time only.

**Approved default observation bank.** The import resolves the seven `obs_` fields from this bank by dimension and score band. The bank is deliberately garment-level and country-neutral. Editors may override with an evidence-based observation, but the default is publication-ready.

| Dimension | 90–100 | 80–89.9 | 70–79.9 | 0–69.9 |
| --- | --- | --- | --- | --- |
| Waistband Sovereignty | The waistband governed without being noticed; no adjustment was recorded in the seated assessment. | The waistband held its position, with one adjustment noted late in the sitting. | The waistband required occasional negotiation but did not ask for attention. | The waistband was present; authority was intermittent. |
| Thermal Independence | The interior climate remained stable across heated, unheated, and transitional rooms. | The garment held its climate in most rooms, with a detectable shift in the coolest. | Thermal conduct depended on the room; the garment followed rather than governed. | The garment deferred to the conditions and was most itself in a warm room. |
| Structural Integrity | Seams and hem retained their geometry after the wash cycle; no repair was required. | Construction survived the wash with minor softening at the hem. | The garment remained legible after washing, though the seam recorded the cycle. | The garment disclosed its history at the seam; structural claims were modest. |
| Diplomatic Comfort | Under the outer layer, the garment remained silent through a long sitting. | The garment coexisted with the outer layer, with one brief adjustment. | Diplomacy held in company; the garment was aware of the trouser above it. | The garment requested attention under tailoring; the room noticed. |
| Nocturnal Security | Displacement was not recorded between lights-out and the first movement. | The night passed without incident; one minor displacement was noted. | The garment required one adjustment before morning. | The garment did not remain where it was placed; the night was recorded as unsettled. |
| Pattern Jurisdiction | The pattern governed its territory and survived the hem without argument. | The repeat held across the seam, with a small interruption at the leg opening. | The pattern occupied the garment without governing it; the repeat was legible. | The print made a claim the garment could not support; plainness would have been safer. |
| Institutional Memory | Shape and hand memory remained after the season; the archive and the equipment agreed. | The garment retained its shape and most of its color; the record was legible. | Memory persisted in the softness; shape required interpretation. | The garment remembered use but not form; institutional memory outran maintenance. |

### 2.5 National observation

**Length:** 180–250 words, inclusive. Count the rendered prose, not HTML tags. Target 220–240 words. The worksheet below counts short because its prompts expand into full clauses; count the finished prose.

**Purpose:** a conditions portrait of the assessed sample. It is not a national character sketch, a travel note, a climate summary, or a verdict on the country.

**Editorial worksheet.** Replace each drafting prompt with a qualitative noun phrase from the conditions brief. Do not render the prompt names. Do not paste the table observations verbatim; paraphrase them in context.

```text
{{country}} enters the 2026 Edition with a composite of {{score_composite}} and a {{tier}} classification, placed {{rank}} of 60. This profile applies the Index's seven dimensions to the conditions of the first layer in {{country}}. It assesses garments and conditions, not the country or its people.

The climate note records {{climate_conditions}}. Thermal Independence scored {{score_ti}}: {{obs_ti}} Climate is treated as indoor, outdoor, and transitional conditions, not as a national characteristic.

The laundry note records {{laundry_conditions}}. Structural Integrity scored {{score_si}}: {{obs_si}} The panel follows the garment through ordinary cycles and the intervals between them; it does not estimate household behavior.

The domestic-architecture note records {{architecture_conditions}}, and the retail note records {{retail_conditions}}. Diplomatic Comfort scored {{score_dc}} and Pattern Jurisdiction {{score_pj}}, describing how the garment sits within those arrangements, not as a claim about them.

The repair note records {{repair_conditions}}. Institutional Memory scored {{score_im}}: {{obs_im}} The profile closes on the garment's record, not the country's. Findings are provisional, based on a small panel, and subject to correction. Where a condition was not recorded, the profile says so; it does not infer.
```

In the worksheet, `{{obs_ti}}`, `{{obs_si}}`, and `{{obs_im}}` stand for a paraphrased version of the corresponding table observation, not the exact table sentence.

**Conditions brief.** Five internal fields: climate, laundry infrastructure, domestic architecture, retail, repair culture. Each is a qualitative noun phrase of 8–20 words drawn from the assessment record. They are not rendered directly. They are not external data.

- **Climate:** describe the assessment rooms and transitions, e.g. "a mix of heated interiors, unheated transitional spaces, and mild outdoor intervals". Do not cite weather data or make a climate claim about the country.
- **Laundry infrastructure:** describe the sample's laundering arrangements, e.g. "ordinary domestic laundering within the assessed sample, with drying space available". Do not estimate household ownership or behavior.
- **Domestic architecture:** describe the assessment settings, e.g. "compact assessment rooms with limited dedicated laundry space". Do not generalize about national housing.
- **Retail:** describe access in the sample, e.g. "conventional access with no specialist supplier assumed". Do not make market-size, price, or shopping-preference claims.
- **Repair culture:** describe practice in the sample, e.g. "routine repair within the assessed sample; garments were returned to service rather than replaced". Do not describe a national trait.

**Do not write:**

- "The country is known for..." or "A nation of..."
- "The locals prefer..." or "The local market demands..."
- "The climate dictates..." or "The weather makes..."
- "A strong repair culture" as a national characteristic.
- Any external statistic, price, income, population, market size, growth rate, weather record, or date of a real event.
- Any reference to government, leaders, parties, elections, policies, borders, conflicts, sanctions, or state capacity.
- Any joke at the country's expense. The joke is the method.

**Unknown conditions.** If the file does not record a condition, write "not recorded in the 2026 file" and do not infer. A shorter, honest observation is preferable to a fabricated detail.

### 2.6 Field note

**Length:** 80–120 words, inclusive. The worksheet below counts short because its prompts expand into full clauses; count the finished prose.

**Setting:** a fictional assessment setting with a city, a month, and conditions. Use a real city only as a neutral location. The setting is illustrative and does not characterize the country.

**Editorial worksheet:**

```text
Field note — {{field_city}}, {{field_month}} 2026. Conditions: {{field_conditions}}. The assessment ran from {{field_start}} to {{field_end}} and covered {{field_activities}}. The garment was checked at the waistband, the seat, and the hem before and after the seated period. {{field_observation}} Adjustments: {{field_adjustment}}. The panel's finding is limited to the conditions described and the sample assessed; the setting is illustrative and does not characterize {{country}}.
```

Rules:

- No real events, holidays, elections, weather events, or news.
- No named assessors, public figures, or personal details. Use "the assessor" or "the panel".
- No third-party brand names, logos, or retailer names.
- No national stereotypes, no cultural shorthand, no travel-brochure detail.
- Include one limitation sentence. The sample is small; the setting is not the country.
- Keep the prose dry and observational. Do not write a joke with the country as the punchline.

### 2.7 Scope and limitations

This block is a shared partial. The text in Section 2.1 is the canonical copy. Do not edit it per profile.

It must contain the exact line:

**Scope:** This profile measures garment conditions only — not governance, state capacity, or national sovereignty.

It must also state that scores are internal IBI assessments, that no external data were consulted, that the panel is small and the sample purposive, that findings are provisional, that a country profile is not a country ranking, and that a high score is a finding rather than a recommendation.

### 2.8 Corrections block

Corrections are limited to factual error, arithmetic, and a change in elastic. They are not for tone, taste, or editorial preference.

`{{correction_log}}` renders a table when entries exist:

| Date | Field | Correction | Reference |
| --- | --- | --- | --- |
| {{correction_date}} | {{correction_field}} | {{correction_summary}} | {{correction_reference}} |

When no entries exist, render exactly:

> No corrections recorded for this profile.

The reference links to the relevant notice on `/news`. Do not invent a notice URL. Corrections are submitted through `/contacts` with the profile slug, dimension, and arithmetic or factual basis.

### 2.9 Citation block

`{{citation}}` renders:

> Ilya Boxers Institute. “{{country}} — Boxer Sovereignty Profile 2026.” *Boxer Sovereignty Index, 2026 Edition*. {{canonical_url}}. Published {{date_published_iso}}; last revised {{date_modified_iso}}.

Use the canonical URL, not the API or admin URL. Do not add a DOI, an ISBN, a report number, or a publisher address. Do not use a real person as author. The author is the Organization.

### 2.10 Related profiles

Exactly three links per profile, selected automatically. No manual related-content field.

Selection order:

1. Same region, excluding the current profile. Sort by absolute rank distance ascending, then rank ascending, then country name ascending.
2. If fewer than three same-region profiles are available, fill from all other published profiles using the same proximity sort, excluding those already selected.
3. Exclude drafts, review records, archived profiles, `noindex` pages, and the current profile.
4. Always return exactly three distinct published profiles for the full 60-profile edition.

Region counts in the 2026 register: Europe 25, Americas 9, Africa 6, Southeast Asia 6, East Asia 4, Middle East 4, South Asia 3, Oceania 2, Central Asia 1. The fallback is therefore required for Central Asia, Oceania, and South Asia.

Examples:

- A European profile takes three European neighbours.
- A South Asian profile takes the two other South Asian profiles and one nearest-rank profile from another region.
- A Central Asian profile takes three nearest-rank profiles from other regions.

Recompute on publish, rank change, and archive. Cache key includes the edition, the slug, and the maximum `updated_at` of the edition roster.

### 2.11 Newsletter CTA and credit

Newsletter copy is fixed:

> The Quarterly Dispatch. One letter each quarter: new field notes, index revisions, and occasional notices from the Collection. No campaign literature. [Subscribe]({{newsletter_url}}).

One CTA only. Do not add urgency, counts, incentives, or campaign language.

The page ends with the exact credit:

Credit: With love, made by RRN.world — Rotten Reliable News.

---

## 3. SEO metadata pattern

### 3.1 Metadata fields and limits

| Field | Pattern | Limit | Notes |
| --- | --- | --- | --- |
| `<title>` | `{{country}} — Boxer Sovereignty Index 2026 &#124; IBI` | 60 characters | Longest register country produces 57. Do not truncate mid-word. |
| Meta description | `The IBI 2026 boxer profile for {{country}}: composite {{score_composite}}, rank {{rank}} of 60, and seven dimension scores. Garment conditions only.` | 155 characters | Longest register country produces 136. |
| OG title | `{{country}} — Boxer Sovereignty Profile 2026 &#124; IBI` | 60 characters | Longest register country produces 59. |
| OG description | Same as meta description unless an approved override exists | 155 characters | Keep title and description consistent. |
| Canonical | `{{canonical_url}}` | — | Absolute, self-referential, no trailing slash. |
| `og:type` | `article` | — | — |
| `og:site_name` | `Ilya Boxers Institute` | — | Exact brand name. |
| `twitter:card` | `summary_large_image` | — | Use the same title and description. |
| `article:published_time` | `{{date_published_iso}}` | — | ISO 8601. |
| `article:modified_time` | `{{date_modified_iso}}` | — | ISO 8601. |
| OG image | `{{og_image_url}}` | 1200×630 | Absolute URL. Default Index image if no profile image. |
| OG image alt | `{{og_image_alt}}` | 140 characters | No flags, maps, logos, or third-party marks. |
| Robots | Published: `index,follow,max-image-preview:large` | — | Draft and preview: `noindex,nofollow`. |

`{{seo_title}}` is the complete `<title>` value. Render it as an absolute title; do not run it through the root `%s | Ilya Boxers Institute` template. The pattern already carries the short `| IBI` suffix to stay within 60 characters.

No meta keywords tag. Keywords belong in JSON-LD only. No author meta tag for a Person; the author is the Organization.

SEO copy must not contain political terms, governance claims, economic claims, or invented statistics. The only numbers permitted are IBI scores, rank, weights, dimension counts, the year, and 60.

### 3.2 JSON-LD: Article

Use `Article` for the public profile page. It carries the editorial context and the BSI membership.

```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "@id": "{{canonical_url}}#profile",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "{{canonical_url}}"
  },
  "headline": "{{country}} — Boxer Sovereignty Profile 2026",
  "description": "{{seo_description}}",
  "url": "{{canonical_url}}",
  "image": [
    "{{og_image_url}}"
  ],
  "datePublished": "{{date_published_iso}}",
  "dateModified": "{{date_modified_iso}}",
  "inLanguage": "en",
  "contributor": {
    "@type": "Person",
    "name": "Mikhail Biyun",
    "jobTitle": "Head of Special Operations"
  },
  "author": {
    "@type": "Organization",
    "name": "Ilya Boxers Institute",
    "url": "https://ibi.institute"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Ilya Boxers Institute",
    "url": "https://ibi.institute"
  },
  "isPartOf": {
    "@type": "Dataset",
    "name": "Boxer Sovereignty Index 2026",
    "description": "The Institute's 2026 assessment of boxer shorts across seven dimensions.",
    "url": "https://ibi.institute/index"
  },
  "about": {
    "@type": "Country",
    "name": "{{country}}"
  },
  "keywords": [
    "Boxer Sovereignty Index",
    "2026",
    "{{country}}",
    "boxer shorts",
    "garment assessment"
  ]
}
```

Do not add `aggregateRating`, `review`, `award`, `offers`, `government`, `headOfState`, `legislature`, or political `sameAs` values. The `about` entity is the country name only.

### 3.3 JSON-LD: Dataset variant

Use this only for a data endpoint or download, not for the public article page. It exposes the seven scores as measured variables.

```json
{
  "@context": "https://schema.org",
  "@type": "Dataset",
  "@id": "{{canonical_url}}#dataset",
  "name": "{{country}} — Boxer Sovereignty Profile 2026",
  "description": "{{seo_description}}",
  "url": "{{canonical_url}}",
  "isPartOf": {
    "@type": "Dataset",
    "name": "Boxer Sovereignty Index 2026",
    "url": "https://ibi.institute/index"
  },
  "creator": {
    "@type": "Organization",
    "name": "Ilya Boxers Institute",
    "url": "https://ibi.institute"
  },
  "datePublished": "{{date_published_iso}}",
  "dateModified": "{{date_modified_iso}}",
  "about": {
    "@type": "Country",
    "name": "{{country}}"
  },
  "spatialCoverage": {
    "@type": "Country",
    "name": "{{country}}"
  },
  "variableMeasured": [
    {
      "@type": "PropertyValue",
      "name": "Waistband Sovereignty",
      "value": "{{score_ws}}"
    },
    {
      "@type": "PropertyValue",
      "name": "Thermal Independence",
      "value": "{{score_ti}}"
    },
    {
      "@type": "PropertyValue",
      "name": "Structural Integrity",
      "value": "{{score_si}}"
    },
    {
      "@type": "PropertyValue",
      "name": "Diplomatic Comfort",
      "value": "{{score_dc}}"
    },
    {
      "@type": "PropertyValue",
      "name": "Nocturnal Security",
      "value": "{{score_ns}}"
    },
    {
      "@type": "PropertyValue",
      "name": "Pattern Jurisdiction",
      "value": "{{score_pj}}"
    },
    {
      "@type": "PropertyValue",
      "name": "Institutional Memory",
      "value": "{{score_im}}"
    }
  ],
  "measurementTechnique": "Seven-dimension IBI assessment; internal scores only."
}
```

### 3.4 Structured-data validation

- Parse the JSON-LD with a strict parser in CI.
- Confirm `@type` is `Article` or `Dataset`.
- Confirm `isPartOf` names the Boxer Sovereignty Index 2026.
- Confirm `author` and `publisher` are the Organization, not a Person.
- Confirm `datePublished` and `dateModified` are valid ISO 8601 and `dateModified` is not earlier.
- Confirm `about` contains the country name and no political properties.
- Confirm `url`, `@id`, `mainEntityOfPage`, and `image` are absolute.
- Confirm no unresolved tokens.
- Run the Schema.org validator and a rich-results test before launch.

---

## 4. CMS and data model

### 4.1 Recommended collection: `country_profiles`

| Field | Type | Required | Rules and notes |
| --- | --- | --- | --- |
| `id` | uuid | Auto | Primary key. |
| `edition` | integer | Yes | 2026 for this edition. |
| `country` | string(80) | Yes | Exact register display name. Unique per edition. |
| `slug` | string(80) | Yes | Lowercase ASCII, hyphenated, regex `^[a-z0-9]+(?:-[a-z0-9]+)*$`. Unique per edition. Immutable after publish. |
| `region` | enum | Yes | Europe, East Asia, Americas, Oceania, South Asia, Africa, Southeast Asia, Central Asia, Middle East. |
| `score_ws` | decimal(4,1) | Yes | 0–100. Waistband Sovereignty, 22%. |
| `score_ti` | decimal(4,1) | Yes | 0–100. Thermal Independence, 16%. |
| `score_si` | decimal(4,1) | Yes | 0–100. Structural Integrity, 18%. |
| `score_dc` | decimal(4,1) | Yes | 0–100. Diplomatic Comfort, 15%. |
| `score_ns` | decimal(4,1) | Yes | 0–100. Nocturnal Security, 11%. |
| `score_pj` | decimal(4,1) | Yes | 0–100. Pattern Jurisdiction, 9%. |
| `score_im` | decimal(4,1) | Yes | 0–100. Institutional Memory, 9%. |
| `composite` | decimal(4,1) | Derived | Read-only. Weighted average, half-up to one decimal. Recompute on write. |
| `rank` | integer | Derived | Read-only. 1–60, unique per edition, computed from the frozen roster and tie order. |
| `tier` | enum | Derived | Read-only. Sovereign, Autonomous, Dependent, Ceremonial. |
| `obs_ws` … `obs_im` | text | Yes | Seven one-line observations, 20–180 characters each. Resolved from the bank or an editor override. |
| `standfirst` | string(240) | No | Auto default; override must be one sentence and must not make a national claim. |
| `national_observation` | rich text | Yes | 180–250 words. |
| `field_note` | rich text | Yes | 80–120 words. |
| `conditions_brief` | jsonb | No | Internal worksheet: climate, laundry, architecture, retail, repair. Not rendered directly. |
| `image` | media relation | No | Optional profile image for OG/social. No flags, maps, or third-party marks. |
| `image_alt` | string(140) | Conditional | Required when an image exists. |
| `seo_title` | string(60) | No | Auto-generated; override validated. |
| `seo_description` | string(155) | No | Auto-generated; override validated. |
| `og_title` | string(60) | No | Auto-generated; override validated. |
| `og_description` | string(155) | No | Auto-generated; override validated. |
| `status` | enum | Yes | `draft`, `in_review`, `published`, `archived`. |
| `published_at` | datetime | Conditional | Required when status becomes `published`. |
| `updated_at` | datetime | Auto | Drives `dateModified` and sitemap `lastmod`. |
| `correction_log` | jsonb array | No | Each entry: `date`, `field`, `summary`, `reference`. Empty array allowed. |
| `panel_tie_order` | integer | Yes | Hidden. Imported from `rank_2026`; not editor-editable. Used only to order equal composites. |
| `editorial_note` | text | No | Internal reason for an observation override or a manual editorial decision. Not rendered. |
| `created_at`, `created_by`, `updated_by` | audit | Auto | Staff accountability. |

Use `decimal`, not floating point, for all scores and the composite. Use a database uniqueness constraint on `(edition, rank)` and `(edition, slug)`.

### 4.2 Option comparison: new collection vs `/news` with `type=index`

| Option | Advantages | Disadvantages | Verdict |
| --- | --- | --- | --- |
| New `country_profiles` collection | Typed fields and server-side validation; automatic composite, rank, and tier; clean canonical route; region/rank queries; separate editorial workflow; no collision with Notices. | Requires API, admin SPA, public template, sitemap, and search changes. New permissions and audit surfaces. | Recommended. |
| `/news` with `type=index` | Reuses the existing admin, editor training, media handling, and publication workflow. Faster to stand up. | Generic schema; scores stored in unstructured JSON; weak validation; `/news` is Notices, so profiles pollute a different editorial surface; `/news/{{slug}}` duplicates or conflicts with the canonical route; sitemap, feeds, and search need filters; corrections and notices mix; rank/composite are easy to mistype. | Not recommended as a permanent home. Acceptable only as a temporary staging container with custom validation. |

If `/news` is unavoidable:

- Add `type=index` and a structured `index_payload` field.
- Add `region`, seven scores, seven observations, `national_observation`, `field_note`, and `panel_tie_order`.
- Add custom API validation equivalent to Section 5.
- Set `canonical_path` to `/index/2026/{{slug}}`.
- Exclude `type=index` records from the public `/news` listing, feeds, and default search results, except for a single editorial link.
- Make `/news/{{slug}}` `noindex` and canonicalize it to the profile, or make it return 404.
- Plan a migration to `country_profiles` before the next edition.

### 4.3 Admin mapping

For the new collection:

- Admin endpoint: `/country_profiles`.
- List view: filter by edition, region, status, tier; sort by rank; show missing-field warnings.
- Detail view: identity, scores, observations, long copy, conditions brief, image, SEO, status, correction log.
- Computed fields are read-only and update on save.
- Preview link opens the draft route with `noindex`.
- Permissions: editor creates and edits drafts; reviewer approves long copy and observations; publisher publishes and archives. Only a publisher can change a published score.
- Bulk actions: validate, preview, publish, archive. A bulk publish is blocked if any record fails Section 5.
- Media: use `/files` and `/upload/single`; require alt text. No old brand assets.
- Redirects: use `/redirects` for old country URLs; export the table to the repository.
- Notices: corrections are published through `/news`; the profile's correction log links to the notice.
- Settings: canonical host, default OG image, newsletter URL, and credit remain in `/settings`.

### 4.4 CSV-to-API import outline

This is a specification. No import is executed here, and no network write is made by this document.

1. **Freeze the source.** Record the SHA-256 of `countries-2026.csv` and the row count (60). Do not import from a moving file.
2. **Parse.** Expect 14 columns: `slug`, `country`, `region`, `waistband_sovereignty`, `thermal_independence`, `structural_integrity`, `diplomatic_comfort`, `nocturnal_security`, `pattern_jurisdiction`, `institutional_memory`, `composite`, `tier`, `note`, `rank_2026`.
3. **Map.** CSV scores map to `score_ws`, `score_ti`, `score_si`, `score_dc`, `score_ns`, `score_pj`, `score_im`. `rank_2026` maps to the hidden `panel_tie_order` and to the verification report. `composite` and `tier` are verification-only and are recomputed.
4. **Validate.** Apply every rule in Section 5. Collect errors by row and field. Do not partially import a row.
5. **Recompute.** Use decimal arithmetic. Compute composite and tier. Compare with the CSV. Report every difference. The known differences are Mexico and Peru; see Section 5.5.
6. **Resolve observations.** Fill `obs_*` from the default bank by dimension and score band. Mark rows for editorial review. Do not publish the CSV `note` column verbatim; it is a working register note, may contain generalizations, and is not a garment-level observation.
7. **Author long copy.** The national observation and field note are editorial fields. They are not generated from the CSV. The import creates drafts with empty long-copy fields and a validation warning.
8. **Dry run.** Produce a JSON payload and a report. No API writes.
9. **Review.** Editor signs off observations, national observation, field note, image rights, alt text, SEO, scope line, and credit. Reviewer checks the no-politics and no-invented-statistics rules.
10. **Import as draft.** Upsert by `(edition, slug)`. Idempotent. Never overwrite a published record's long copy without a new revision and a correction entry.
11. **Preview and publish.** Validate all 60 drafts. Publish as a set so ranks and related links are stable. Record the import checksum in the release log.

Dry-run report shape:

```text
rows_read: 60
rows_structurally_valid: 60
rows_publishable: 0 (long copy pending)
composite_corrections: 2 (Mexico +0.1, Peru +0.1)
tier_changes: 0
rank_changes: 0
missing_national_observation: 60 (expected before editorial)
missing_field_note: 60 (expected before editorial)
missing_image_alt: 0 (no profile images; default OG in use)
```

Example draft payload (observations and long copy added after editorial):

```json
{
  "edition": 2026,
  "country": "Portugal",
  "slug": "portugal",
  "region": "Europe",
  "scores": {
    "ws": 96,
    "ti": 92,
    "si": 95,
    "dc": 94,
    "ns": 93,
    "pj": 90,
    "im": 94
  },
  "panel_tie_order": 1,
  "status": "draft"
}
```

### 4.5 Data quality notes

- **Rows:** 60. **Regions:** Europe 25, Americas 9, Africa 6, Southeast Asia 6, East Asia 4, Middle East 4, South Asia 3, Oceania 2, Central Asia 1.
- **Composite arithmetic:** Use decimal half-up. The register reproduces under half-up except Mexico (87.25 → 87.3, register 87.2) and Peru (74.05 → 74.1, register 74.0). Correct the source values before import. No rank changes result.
- **Norway:** 77.65 → 77.7 under half-up, matching the register.
- **Ties:** Five composites are tied: 90.4 (France 6, United Kingdom 7), 80.4 (Malaysia 21, Morocco 22), 74.8 (Poland 40, Sweden 41), 74.5 (Hungary 42, Thailand 43), 70.8 (Kenya 53, Spain 54). `panel_tie_order` preserves the published order. Do not recompute tie order alphabetically.
- **CSV `note`:** Not publishable copy. Use only as an editorial hint; rewrite all country observations at garment level.
- **Images:** No flags, maps, portraits, or third-party marks. Use a neutral garment detail or the default Index 2026 OG image.
- **Corrections:** The composite corrections are arithmetic, not taste. Log them in the release notes; if published, add a correction entry.

---

## 5. Validation rules

### 5.1 Score and composite rules

- Each of the seven dimension scores is numeric, required, and between 0 and 100 inclusive.
- Store scores as decimal with at most one decimal place. The current register uses integers.
- Weights are fixed: WS 0.22, TI 0.16, SI 0.18, DC 0.15, NS 0.11, PJ 0.09, IM 0.09. The sum must equal 1.00.
- Composite formula:

```text
composite = 0.22*WS + 0.16*TI + 0.18*SI + 0.15*DC + 0.11*NS + 0.09*PJ + 0.09*IM
composite = round_half_up(composite, 1)
```

- Use decimal arithmetic, not binary floating point.
- `composite` is derived and read-only. A stored value that differs from the formula blocks publish.
- The 2026 source corrections in Section 5.5 are the only permitted arithmetic differences.

### 5.2 Rank rules

- `rank` is derived and read-only.
- Compute over the frozen edition roster, including drafts, so phased publishing does not change ranks.
- Sort by `composite` descending, then `panel_tie_order` ascending.
- `rank` is the 1-based position. It must be an integer from 1 to 60, unique per edition, with no gaps for a complete edition.
- `panel_tie_order` is imported from `rank_2026`. It is not the public rank and is not editor-editable.
- A score edit that changes the sort order requires publisher approval and, if already published, a correction entry.
- A new tie requires a controlled tie-order update by a publisher, recorded in the editorial note; otherwise block publish.
- Removing or archiving a profile does not silently renumber the edition. Treat it as an editorial exception and record it.

### 5.3 Tier rules

```text
composite >= 90.0  -> Sovereign
composite >= 80.0  -> Autonomous
composite >= 70.0  -> Dependent
composite < 70.0   -> Ceremonial
```

Use the rounded composite. `90.0` is Sovereign, `80.0` is Autonomous, `70.0` is Dependent, `69.9` is Ceremonial.

### 5.4 Required fields and publication gates

A record may publish only when all of the following are true:

- `country`, `slug`, `region`, and all seven scores are present and valid.
- All seven `obs_*` fields are present, one line, 20–180 characters.
- `national_observation` is present and 180–250 words.
- `field_note` is present and 80–120 words.
- `seo_title`, `seo_description`, `og_title`, and `og_description` pass their limits.
- `canonical_url` is `https://ibi.institute/index/2026/{{slug}}`.
- `status` is `published` and `published_at` is set.
- The rendered page contains the exact scope line and the exact RRN credit.
- Exactly three related profiles resolve to published pages.
- No unresolved `{{` tokens, no "TBD", no "Lorem", no placeholder text.
- No political claims, no invented statistics, no real third-party brands.

### 5.5 Known 2026 arithmetic corrections

| Country | Published dimensions (WS, TI, SI, DC, NS, PJ, IM) | Weighted result | Register value | Action |
| --- | --- | ---: | ---: | --- |
| Mexico | 85, 93, 86, 84, 87, 91, 87 | 87.3 | 87.2 | Correct the source to 87.3. Rank remains 13. |
| Peru | 72, 71, 76, 71, 82, 72, 78 | 74.1 | 74.0 | Correct the source to 74.1. Rank remains 44. |

No other row changes under half-up. Norway is 77.7 and matches. These are arithmetic corrections, not score revisions. If the source is not corrected, block import; do not let the page disagree with the register.

### 5.6 Related-profile validation

- Exactly three, distinct, published, canonical, 200-status profiles.
- Same region first, sorted by absolute rank distance, then rank, then country name.
- Fallback to all other regions by the same sort when the region has fewer than three others.
- Never include the current profile, drafts, archived pages, or `noindex` pages.
- Recompute on publish, rank change, or archive.
- If fewer than three published profiles exist, block the full-edition build rather than rendering a partial related block.

### 5.7 SEO and structured-data validation

- Title at most 60 characters; description at most 155; OG title at most 60; OG description at most 155.
- Canonical is absolute and matches the route.
- JSON-LD parses; `@type` is `Article` or `Dataset`; `isPartOf`, `author`, dates, and `about` are present.
- No `aggregateRating`, `review`, `award`, `offers`, or political properties.
- OG image is absolute, 1200×630, and has alt text.
- Robots is `index,follow` for published and `noindex,nofollow` for drafts.

### 5.8 Editorial validation

- No government, leader, party, election, policy, border, conflict, sanction, or state-capacity reference outside the scope and limitations block.
- No national character claim, demonym generalization, climate determinism, or cultural shorthand.
- No real statistics, prices, incomes, populations, market sizes, growth rates, weather records, or dates of real events.
- No real third-party brand names, logos, or retailer names.
- No founder or family detail.
- No joke at the country's expense. The joke is the method.

---

## 6. QA checklist for a country page

- [ ] **Scope disclaimer.** The exact line “This profile measures garment conditions only — not governance, state capacity, or national sovereignty.” is present in the rendered page.
- [ ] **No political claims.** The page contains no reference to government, leaders, parties, elections, policies, borders, conflicts, sanctions, or state capacity outside the scope and limitations block. A second editor has read the page for tone.
- [ ] **No invented statistics.** Every number is an IBI score, composite, rank, weight, scale bound, year, or 60. No external statistic, price, income, population, market size, or weather record appears.
- [ ] **Score integrity.** Seven dimension scores match the register; composite equals the weighted formula; tier matches the bands; rank is unique and correct; the two arithmetic corrections are resolved.
- [ ] **Canonical.** The canonical is `https://ibi.institute/index/2026/{{slug}}`, self-referential, absolute, no trailing slash, no query, and `og:url` matches.
- [ ] **Sitemap and robots.** The page is in the sitemap with `lastmod` equal to `updated_at`; published robots is `index,follow`; draft and preview are excluded and `noindex`.
- [ ] **Related links.** Exactly three distinct published profiles; same-region preference applied; fallback used where required; all three links return 200.
- [ ] **SEO and JSON-LD.** Title at most 60 characters; description at most 155; OG fields present; JSON-LD parses, names the BSI in `isPartOf`, uses Organization as author, has both dates, and `about` is the country.
- [ ] **Image and layout.** If an image is present, it has alt text and no flags, maps, logos, or third-party marks; the table is readable on mobile; no unresolved tokens or placeholder text.
- [ ] **Credit and CTA.** The exact RRN credit appears at the foot of the page; the Quarterly Dispatch CTA is present once; the corrections block renders the table or the exact empty state.

---

## 7. Tone guidance for country pages

The country is the setting. The garment is the subject. The Institute is the narrator. The joke is the method, not the country.

| Do | Do not |
| --- | --- |
| Write “in Portugal”, “the assessed sample”, “the panel recorded”. | Write “the Portuguese”, “a nation of”, “the local mentality”. |
| Describe conditions in the assessment rooms. | Describe a national climate, culture, or character. |
| Use hedges: “in the assessed sample”, “the file records”, “the panel did not infer”. | Use “always”, “never”, “typically”, “everyone”, “the market demands”. |
| Keep the prose dry, short, and procedural. | Use exclamation marks, puns at national expense, or travel-brochure lyricism. |
| Let the methodology supply the humor: panels, tiers, adjustments, elastic. | Use the country as the punchline. |
| Treat the score as a finding about a garment. | Treat the score as a verdict on a country, economy, or people. |
| Use “sovereignty” in body copy only in its garment sense, as defined by the Index. | Draw political analogies to borders, governments, or statehood. |
| Use neutral archetypes for garments. | Name real brands, retailers, or logos. |
| Say “not recorded” when the file is silent. | Invent a detail to make the paragraph flow. |
| Keep the scope line exact. | Soften it into “this is only a bit of fun”. |

Additional rules:

- No demonyms. Use the country name as a noun.
- No “developed”, “developing”, “rich”, “poor”, “modern”, “traditional”, “Western”, or “Eastern”.
- No climate determinism. Indoor and transitional conditions are the assessment setting, not a national explanation.
- No economic claims: no prices, availability, market size, growth, income, or retail performance.
- No real events, leaders, policies, or news. The field note is a fictional assessment setting.
- No personal or family details about the founder. Country pages do not discuss the founder.
- No third-party brand names. If a garment type is needed, use an archetype: “a plain-weave brief”, “a hotel-drawer short”, “an airport emergency short”.
- Governance, state capacity, and national sovereignty are named only in the scope and limitations block. Do not elaborate elsewhere.

A useful test: remove the country name from the national observation. If the remaining text could be a political, economic, or cultural claim about any country, rewrite it. The paragraph should read as a garment assessment that happens to be set somewhere.

---

## Appendix: Filled example (illustrative)

This example uses the 2026 register data for Portugal and the default observation bank. It is a rendering test, not final copy. The national observation and field note are illustrative and use a fictional assessment setting.

**Breadcrumb:** Home / Research / Boxer Sovereignty Index / 2026 Edition / Portugal

# Portugal — Boxer Sovereignty Profile 2026

The Institute's 2026 assessment of the boxer short in Portugal, scored across seven dimensions and placed 1 of 60.

| Composite | Rank | Tier | Region |
| ---: | ---: | --- | --- |
| 93.8 | 1 of 60 | Sovereign | Europe |

*Tier bands: Sovereign 90.0+, Autonomous 80.0–89.9, Dependent 70.0–79.9, Ceremonial below 70.0.*

## Profile at a glance

Scores are internal IBI assessments, 0–100. Weights are fixed for the 2026 Edition. Observations describe the assessed sample.

| Dimension | Score | Observation |
| --- | ---: | --- |
| Waistband Sovereignty (22%) | 96 | The waistband governed without being noticed; no adjustment was recorded in the seated assessment. |
| Thermal Independence (16%) | 92 | The interior climate remained stable across heated, unheated, and transitional rooms. |
| Structural Integrity (18%) | 95 | Seams and hem retained their geometry after the wash cycle; no repair was required. |
| Diplomatic Comfort (15%) | 94 | Under the outer layer, the garment remained silent through a long sitting. |
| Nocturnal Security (11%) | 93 | Displacement was not recorded between lights-out and the first movement. |
| Pattern Jurisdiction (9%) | 90 | The pattern governed its territory and survived the hem without argument. |
| Institutional Memory (9%) | 94 | Shape and hand memory remained after the season; the archive and the equipment agreed. |

## National observation

*A conditions portrait, not a verdict on the country.*

Portugal enters the 2026 Edition with a composite of 93.8 and a Sovereign classification, placed 1 of 60. This profile applies the Index's seven dimensions to the conditions of the first layer in Portugal. It assesses garments and conditions, not the country or its people.

The climate note records a mix of heated interiors, unheated transitional spaces, and mild outdoor intervals. Thermal Independence scored 92: the interior climate remained stable across the assessed rooms. Climate is treated as indoor, outdoor, and transitional conditions, not as a national characteristic.

The laundry note records ordinary domestic laundering within the assessed sample, with drying space available. Structural Integrity scored 95: seams and hem retained their geometry after the wash cycle. The panel follows the garment through ordinary cycles and the intervals between; it does not estimate household behavior.

The domestic-architecture note records compact assessment rooms with limited dedicated laundry space, and the retail note records conventional access with no specialist supplier assumed. Diplomatic Comfort scored 94 and Pattern Jurisdiction 90, describing how the garment sits within those arrangements, not as a claim about them.

The repair note records repair as available within the assessed sample; no repair was required for this garment. Institutional Memory scored 94: shape and hand memory remained after the season. The profile closes on the garment's record, not the country's. Findings are provisional, based on a small panel, and subject to correction. Where a condition was not recorded, the profile says so; it does not infer.

## Field note

Field note — Lisbon, March 2026. Conditions: a heated office, a cool evening, and a long seated period. The assessment ran from 09:40 to 17:10 and covered a meeting, a lunch, and a walk between buildings. The garment was checked at the waistband, the seat, and the hem before and after the seated period. It held its position; no adjustment was recorded during the seated period. Adjustments: none. The panel's finding is limited to the conditions described and the sample assessed; the setting is illustrative and does not characterize Portugal.

## Scope and limitations

**Scope:** This profile measures garment conditions only — not governance, state capacity, or national sovereignty.

The Boxer Sovereignty Index assesses boxer shorts, not countries. A country profile applies the seven Index dimensions to the conditions in which the first layer is worn in the named market. It does not rank states, governments, or peoples, and it is not a measure of national sovereignty. A country profile is not a country ranking.

Scores are internal IBI assessments produced by IBI assessors using IBI criteria. No external data were consulted. The panel is small and the sample is purposive. Findings are provisional, describe the assessed sample and conditions, and are not a national statistic.

A high score is a finding, not a recommendation. Scores are not revised for taste. Corrections are limited to factual error, arithmetic, or a change in elastic. Where a condition was not recorded, the profile says so; it does not infer.

## Corrections

No corrections recorded for this profile.

## Cite this profile

Ilya Boxers Institute. “Portugal — Boxer Sovereignty Profile 2026.” *Boxer Sovereignty Index, 2026 Edition*. https://ibi.institute/index/2026/portugal. Published 2026-06-01T09:00:00+03:00; last revised 2026-06-01T09:00:00+03:00.

## Related profiles

- [Russia — Sovereign, 92.1](https://ibi.institute/index/2026/russia)
- [Italy — Sovereign, 91.6](https://ibi.institute/index/2026/italy)
- [Germany — Sovereign, 91.1](https://ibi.institute/index/2026/germany)

*Related profiles are selected automatically by region and rank proximity.*

## The Quarterly Dispatch

The Quarterly Dispatch. One letter each quarter: new field notes, index revisions, and occasional notices from the Collection. No campaign literature. [Subscribe](/newsletter).

**Colophon:** Country profiles are maintained by the Ilya Boxers Institute as part of the public record of the Boxer Sovereignty Index. Methodology and editorial direction: the Institute. Field operations: Mikhail Biyun, Head of Special Operations. Digital production and institutional voice: RRN.world — Rotten Reliable News.

Credit: With love, made by RRN.world — Rotten Reliable News.
