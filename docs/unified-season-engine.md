# Unified anime season engine

## Scope

A meta page may combine a title's main TV/ONA season chain. The resolver follows only AniList PREQUEL and SEQUEL relations and only accepts related entries whose format is TV or ONA.

The resolver deliberately excludes movies, OVAs, specials, music entries, and relation types such as SIDE_STORY, SPIN_OFF, ALTERNATIVE, and SUMMARY. It caps the chain at 12 entries to protect request latency and avoid malformed relation graphs.

## Episode identity

Each linked AniList entry is fetched independently. Its own mapping entry is used to build episode coordinates. When a mapping has a TMDB season, that season and its episode offset take priority. When no TMDB season is known, the chronological position in the resolved chain is used as a conservative fallback.

Every video keeps the existing Nuvio/Stremio shape: id, season, episode, number, and optional release/still metadata. Duplicate season/episode pairs are removed.

## Safety and limitations

- The canonical page uses the earliest dated entry in the resolved main-series chain.
- The requested metadata ID is retained, so the caller's existing ID namespace is not silently changed.
- This is metadata aggregation only; it does not add stream providers or alter scraper sources.
- A live catalogue check is still required before enabling the branch in a production Nuvio installation.
- If AniList's relation graph is incomplete or dates are missing, chronological fallback ordering may need a title-specific mapping review.