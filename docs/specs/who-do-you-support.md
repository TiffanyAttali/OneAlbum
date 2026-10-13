# Spec: Who do you support

Draft v0.1, 2026-10-13. For Tiff's review. Rules it touches: Identity (living artists), Album pool rules, Privacy.

## The idea (Tiff)
Users choose whose work their listening time supports. "Alive" is the base; others narrow it:
women, African artists, queer artists, mothers, artists from Paris 11e... Several can combine.

## How it behaves
- Support filters cut the pool **before** picking. Taste still decides the pick inside what's left.
- Several filters combine with AND by default ("women" + "from Africa"); a user can switch a group to OR.
- If a filter leaves too few albums for an edge pick (under ~300), say so and offer to widen it. Never silently drop it.
- Filters are stored on the device like everything personal. The server never learns who a user supports.
  The map ships each artist's public tags so filtering happens on the device.

## Filters and where the data comes from
| Filter | Source | Confidence | Notes |
|---|---|---|---|
| Alive and active | MusicBrainz artist end date + a release in the last 10 years | High | Already a pool rule |
| Country / region of origin | MusicBrainz artist area, Wikidata country of citizenship / origin | High | Region = continent grouping of countries |
| City / neighbourhood | MusicBrainz begin area, Wikidata place of birth or formation | Medium | Neighbourhoods (Paris 11e) are sparse: needs curated lists |
| Women / gender | MusicBrainz artist gender (people only), Wikidata sex or gender | Medium | Bands: count a band if its members' data says so; otherwise unknown |
| Queer / LGBTQ+ | Wikidata "sexual orientation" only when the entry cites the artist's own words | Low coverage | Never inferred |
| Parent ("mothers") | Artist's own public statements only | Very low coverage | Likely curated, opt-in by artists |
| Independent / self-released | MusicBrainz label (none, or artist's own label) | Medium | Optional extra idea |

## The open question: identity tags
- Tags like queer, mother or gender come **only from what artists say publicly about themselves**
  (their own bio, interviews, or a database entry that cites them). Never inferred from a name, photo, voice, lyrics or genre.
- Unknown is a normal value: an artist without data is not "not queer"; they are simply outside that filter.
- Proposal for later: let artists claim their own profile and add their tags (self-declared, removable any time).
- Before launch: a short legal check (sensitive data under GDPR: sexual orientation is a special category even when public).

## Done when
- Launch list of filters agreed with Tiff, with a source per filter.
- Pool count per filter measured on the real map, so we know which filters are big enough.
