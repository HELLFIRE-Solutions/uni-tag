# Etap 1 — GEO/AEO dogfood on hellfiresol.com

**Status:** done, 2026-07-21. Applied to `website/` (session 03's site), pushed
and deployed. TETA+PI was in scope per the kickoff prompt ("якщо є доступ") —
no access to a TETA+PI website/repo was available from this workspace, so
Etap 1 covers hellfiresol.com only.

## What was already there (session 03, not duplicated)

- `public/llms.txt` — minimal hand-written version, already flagged in its own
  text as "authoritative until [uni-tag ships] a fuller... version".
- `schema.org` `Organization` JSON-LD in `Layout.astro`, applied site-wide.

## What this module added

1. **`public/llms.txt`** rewritten to the fuller version session 03 promised:
   full page/module/segment detail, the two-engagement-path explanation, a
   contact section restricted to channels that are actually live (dropped
   placeholder phone number and unfilled Nostr/blog links rather than
   fabricate them), and an FAQ block answering the questions an agent is
   most likely to be asked about HELLFIRE (SaaS or service? pricing model?
   location? compliance? how to verify a specialist?).
2. **`Organization` schema enriched** — added `@id`, `logo`, `image`,
   `contactPoint`, `knowsAbout`, `slogan`. Added a sitewide `WebSite` schema
   linked to the org via `@id`. No fabricated data (no fake phone number,
   no founding date we don't have).
3. **New `Service`/`OfferCatalog` schema on the homepage** — lists the six
   sellable modules as `Offer` → `Service` items under HELLFIRE's
   `Organization`, built directly from the `modules` array already on the
   page (single source of truth, no drift risk).
4. **Agent-readable meta tags** — `og:image`, `og:site_name`, `og:locale`,
   full Twitter card, explicit `robots: index, follow`, `author`. Previously
   missing `og:image` meant link previews and some AI crawlers had nothing
   to anchor a visual/entity check against.
5. Confirmed `robots.txt` already allows all crawlers (`Allow: /`, no
   AI-bot-specific blocks) — no change needed there.

Verified locally: `npm run build` (zero errors), JSON-LD on every built page
parses as valid JSON and resolves to the expected `@type`s (3 blocks on
`/`: Organization, WebSite, Service; 2 on `/communicator` and `/legal`),
`og:image` resolves to the real logo asset, dev-server screenshot confirms
the homepage renders correctly with the new markup.

## Visibility measurement — до / після

**Method:** web search (which underlies retrieval for AI answer engines) for
`"HELLFIRE AI Solutions" hellfiresol.com`, `site:hellfiresol.com`, and bare
`hellfiresol.com`. Direct Perplexity chat query was attempted but blocked by
a login wall mid-session; not re-attempted after deploy for consistency —
flagged as a follow-up, not a blocker for the search-index measurement.

**До (2026-07-21, before this module's changes, but after session 03's
manual llms.txt/JSON-LD pass — site had been live only ~1 day):**

| Query | Result |
|---|---|
| `"HELLFIRE AI Solutions" hellfiresol.com` | No match. Results are unrelated "Hellfire"-branded entities (Lockheed Martin missile, hellfire.tech, Hellfire Security, etc.) |
| `site:hellfiresol.com` | Zero indexed pages |
| `hellfiresol.com` | Zero results referencing the domain |

**Conclusion:** hellfiresol.com had no search-index footprint at all — not a
ranking problem, an indexing problem. This module's changes (llms.txt, richer
JSON-LD, OG image) address on-page legibility, but indexing/crawl lag is a
separate, time-dependent variable outside this session's control.

**Після:** not yet measured. Search engines and AI crawlers (GPTBot,
PerplexityBot, ClaudeBot, Google-Extended) need to actually crawl the site
again after deploy, which typically takes days, not minutes — re-checking
immediately after push would just reproduce the "до" result and wouldn't be
a meaningful signal either way. **Follow-up needed:** re-run the same three
queries (plus a direct Perplexity/ChatGPT-search/Claude question) no sooner
than ~1–2 weeks after deploy, once re-crawl has had time to happen. Session
Manager should schedule that as a short check-in on this module rather than
a new full session.

## Etap 2 (next, separate report)

Plugin/script for automated GEO/AEO audit + generation for a client site —
not started. This session's manual pass on hellfiresol.com is the reference
implementation Etap 2 should generalize from.
