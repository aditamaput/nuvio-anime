# Unified anime season engine

## Scope

A meta page may combine a title's main TV/ONA season chain. The resolver follows only AniList PREQUEL and SEQUEL relations and only accepts related entries whose format is TV or ONA.

The resolver deliberately excludes movies, OVAs, specials, music entries, and relation types such as SIDE_STORY, SPIN_OFF, ALTERNATIVE, and SUMMARY from the episode chain. Those related titles are instead exposed in the metadata `links` collection, grouped by relation/format and linked to their official AniList detail pages. This is an external-link fallback, not an internal Nuvio related-card UI. The chain is capped at 12 entries to protect request latency and avoid malformed relation graphs.

## Episode identity

Each linked AniList entry is fetched independently. The visible Nuvio season and episode numbers restart locally for each AniList entry (for example, Season 2 Episode 1). The video ID suffix retains the source mapping's TMDB season and episode coordinates (for example, TMDB Season 1 Episode 25). This separation is essential for shows whose later AniList seasons continue inside one TMDB season. When no TMDB season is known, the chronological position in the resolved chain is used as the source-season fallback.

Every video keeps the existing Nuvio/Stremio shape: id, season, episode, number, and optional release/still metadata. Duplicate season/episode pairs are removed.

## Safety and limitations

- The canonical page uses the earliest dated entry in the resolved main-series chain.
- For a mapped franchise, the returned metadata ID uses the canonical external IMDb/TMDB ID so every season can share one scraper identity. If no external mapping exists, the requested AniList ID is retained.
- Dedicated Anime Movies, Anime OVA & Specials, and Anime ONA catalogues provide separate browsing shelves.
- Franchise links are external AniList links because the current Nuvio metadata UI does not expose a portable internal related-meta card field.
- This is metadata aggregation only; it does not add stream providers or alter scraper sources.
- A live catalogue check is still required before enabling the branch in a production Nuvio installation.
- If AniList's relation graph is incomplete or dates are missing, chronological fallback ordering may need a title-specific mapping review.