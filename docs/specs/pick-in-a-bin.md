# Spec: Pick in a bin

Draft v0.1, 2026-10-13. For Tiff's review. Rules it touches: Engine (bins), Identity (never two picks in a row from one region).

## What the user sees
- A **"Pick in a bin"** page laid out like a record-shop rack: one bin card per sound bin, plus **"Outside my bins"** first.
- Each card: the bin name, a one-line "sounds like", a colour, and 2-3 artists from the user's own library as labels
  (labels only, never seeds: see "No examples" in CLAUDE.md).
- Tap a card: the next album is picked from that bin. One album, same album screen as usual.
- The mood filter gets the same bins as toggles, so a user can also say "tonight: Late-night hush or Dark hypnotic grooves".

## What a bin is
- A bin is one of the user's sound clusters (6-10, built once at import from the sound fingerprint, rebuilt on re-import).
- Bin names are written once at import from the cluster's sound (energy, groove, warmth, voice, tempo), never from genre labels.
  The user can rename a bin.
- Tiff's 8 bins are the reference set: Bright dance-floor bands, Sunny crooners, Campfire rebels, Sunny instrumental grooves,
  Dark hypnotic grooves, Fast & wild, Late-night hush, Spoken word & rap.

## How a pick from a bin works
1. Candidate albums = pool albums whose fingerprint is closest to that bin's centre (nearest bin wins).
2. Apply the usual hard filters: living, active artist; never picked before; not already in the library; support filters.
3. Apply the adventure slider *inside* the bin: Home = albums near the bin centre, Edges = the bin's outer ring,
   Elsewhere = albums between this bin and an outside zone.
4. Same no-repeat rule as today, using bins instead of genre: never two picks in a row from the same bin,
   unless the user tapped that bin twice on purpose.

## "Outside my bins"
- Candidates = pool albums farther from every bin centre than 90% of the user's own tracks (the rule used for Tiff's 4 outside zones).
- Picks rotate across outside zones so it never settles into one new place.
- Reached from an artist the user loves where possible (the "edge" idea), so it feels like a lead, not noise.

## Edge cases
- A bin with fewer than ~20 candidate albums after filters: show it greyed with "Few albums here for now".
- Support filters can empty a bin: say so on the card instead of hiding it.

## Done when
- Each bin card picks an album from that bin, Outside picks land outside all bins.
- Tiff's ten 5/5 prototype albums each land in the bin (or outside zone) she'd expect.
