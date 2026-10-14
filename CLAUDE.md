# CLAUDE.md — album picker (name to come)

Draft v0.6, 2026-10-14 (v0.5 added optional accounts and on-device picking; v0.6 fact-checks pool rules and privacy wording). Written from the Notion dashboard for Tiff's review. Once reviewed, commit at the repo root.

## What we're building
A standalone web app that hands you one full album at a time, by a living artist, picked at the edges of your own sound.
Tagline: "Spotify keeps you home. This takes you to the edges."
Who you support: the user chooses who gets their listening time. "Alive" is the base filter; others can narrow it
(women, African artists, queer artists, mothers, artists from Paris 11e...). See "Who do you support" below.
The working prototype (claude.ai artifact, v13) is the reference for features. Every prototype feature stays.

## Identity rules (never break these)
- One album at a time. No feeds, no infinite lists, no streaks.
- Adventure slider sets the mix of close / edge / far picks:
  Home 40/50/10 · Edges 15/60/25 (default) · Elsewhere 5/35/60. Never 100% close.
- Never two picks in a row from the same region of the sound map.
- Taste first, mood second. Mood genre toggles are firm; mood sliders are soft targets.
- Ratings are a quality cue, never a direction (small, capped, fading weight).
  "Know it already" pushes further out. "Stopped early" is weak. "Not the mood" is ignored.
- "Coup de cœur" button next to the rating (Tiff, 2026-10-13): marks an album the user fell for and keeps it on a
  "Coups de cœur" shelf. It counts as the strongest quality cue but, like every rating, never steers picks toward
  similar albums (loved albums create loops).
- Every pick: artist alive and active, checked against data. Never trust a model to remember this.
- Genre is only ever a penalty (no-repeat on main genre over the last 2 picks), never a target.
- No examples in prompts or seeds: they become anchors. Each pick starts from a random thread in the user's library.

## Privacy rules (never break these)
- Everything personal lives on the device first: library, sound profile, ratings, history, tags. Private, secured, local whenever possible.
- Accounts are optional (decided by Tiff 2026-10-14). If a user signs in to back up or sync, their personal data is encrypted
  on the device before it leaves (end-to-end). The server only stores data it cannot read.
- The server holds the public album map. It never receives anything readable about a user's taste.
- No analytics, no tracking, no server logs that tie a request to a user's IP address. Check the host's default logs before launch
  (Vercel and Supabase keep request logs with IPs unless configured otherwise).
- Album links (Odesli) are pre-computed into the public map, so no third party sees what a user was picked.
- Say it honestly: the Plus licence check sends the licence key to Lemon Squeezy, which knows the buyer's name and email.
- Screenshots are read on the device (Tesseract.js); the image is never uploaded.
- AI writes per album, never per person. The personal note is assembled on the device.
  Cloud "richer notes" are opt-in only.
- Plus is unlocked by a Lemon Squeezy licence key; paying never requires an account. Lemon Squeezy (and the opt-in newsletter)
  hold an email address, never linked to what the user listens to.
- One-tap export and one-tap erase of all local data.

## Engine
- Album map: curated pool (see "Album pool rules"), 50k albums at launch.
- The whole album map is downloaded to the device and picking (vector search) runs there. The user's profile is never sent to a server to search.
- Streaming links (Odesli) and album notes are prepared once per album on our side and shipped with the map,
  so no outside service learns which album a user is about to play.
- Each album has a sound fingerprint: rhythm, energy, timbre, harmony, voice, era, credits.
- The user's sonic profile is built once at import (6-10 sound clusters) and rebuilt only on re-import.
- Distance from the profile decides close / edge / far. Genre labels only add a penalty.
- The user's sound clusters are shown as named **bins** (like record-shop bins). Bins are part of the mood filter.
- "Pick in a bin" page: tapping a bin triggers the next album from that bin. One bin is always "Outside my bins" and sits first (it is the product promise; decided by Tiff 2026-10-13).

## Who do you support (idea, Tiff 2026-10-13)
- Extends the "living artists" rule: users pick whose work their listening time supports, from broad ("alive") to specific
  (gender, region, city or neighbourhood, queer, parent...). Several can combine.
- These are support filters on the pool, applied before picking. They never replace taste: picks still come from the edges of the user's sound.
- Open question: identity tags (queer, mother, gender...) only come from what artists say publicly about themselves
  (self-declared sources such as their own bios or interviews, or MusicBrainz/Wikidata entries citing them). Never inferred from a name, photo, voice or genre.
- Origin filters (country, city, neighbourhood) can use MusicBrainz/Wikidata area data.

## Album pool rules (draft, see Notion task)
- Studio albums only (stand-in, MusicBrainz has no "studio" flag): release group primary type Album with no secondary
  type at all. Full secondary list (verified 2026-10-14, musicbrainz.org/doc/Release_Group/Type): Compilation, Soundtrack,
  Spokenword, Interview, Audiobook, Audio drama, Live, Remix, DJ-mix, Mixtape/Street, Demo, Field recording.
  Note: excluding Mixtape/Street drops many rap releases; review this choice with Tiff.
- Remasters are releases inside the same release group: pick the release group, use its earliest release for track data.
- At least 25 minutes long: summed from the track lengths of that release (release groups have no length; skip if lengths are missing).
- Artist alive and active: artist not marked "ended" and no end date in MusicBrainz, plus a release in the last 10 years.
  A missing end date is not proof of life: cross-check Wikidata date of death before an artist enters the pool.
- Sound data for at least 60% of the album's tracks.
- No known AI-generated releases; plus Tiff's blocklist of artists and labels.
- Listenable floor: enough community signal (ListenBrainz listener counts via its popularity API; Discogs have/want is not in
  the CC0 dumps, only in the rate-limited API) so far picks stay listenable. Threshold to tune: ListenBrainz is sparse for niche artists.
- Balance: no single region or main genre over 15% of the pool.

## Data sources and licences
- MusicBrainz core data: CC0. MetaBrainz Stealth Start-Up tier while not public.
- AcousticBrainz: CC0, about 7M recordings up to 2022. Dumps only.
- Discogs monthly dumps: CC0.
- Soundcharts API for post-2022 releases (paid, from about $50 a month). Commercial use confirmed in writing first.
- Odesli for cross-platform album links. Commercial terms confirmed in writing first.

## Never do
- Never call the Spotify Web API (5-user cap; no extended access).
- Never use the Deezer API (non-commercial terms) or Essentia models (non-commercial without a paid licence).
- Never download, store or analyse audio.
- Never send personal data to any server or AI.
- Never show real album artwork we don't have rights to: covers are generated from the album's sound.

## Stack (proposed)
Next.js on Vercel · Supabase (EU region) to build and serve the album map, and for optional sign-in + encrypted backup ·
picking runs on the device · Claude API for per-album notes ·
Tesseract.js on the device · Lemon Squeezy for payments.

## How to work in this repo
- Plan before editing anything that touches the engine or privacy rules, and say which rule it touches.
- Small commits, each with a plain-English message.
- Test picking changes against Tiff's rated prototype albums (the ten 5/5 albums should land in the edge band).
