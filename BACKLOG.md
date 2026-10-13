# Dev backlog — next build

Product features waiting to be built. Rules they must respect live in `CLAUDE.md`.
Owned by the "Dev features" thread in the project; Notion tasks mirror this list for deadlines.

## 1. "Coup de cœur" button (due Nov 22)
Asked by Tiff, 2026-10-13. Touches the identity rule "Ratings are a quality cue, never a direction".

- A heart button sits next to the star rating on every album card and in history. One tap on, one tap off.
- Independent of the stars: an album can be a coup de cœur at any rating (default; Tiff can change it).
- Coups de cœur shelf: a page listing every album marked, newest first, with the same links as history.
  Not a feed: it only ever shows albums the user marked.
- Engine: strongest quality cue (above a 5/5), still small, capped and fading like every rating.
  It never pulls the next pick toward similar albums (loved albums create loops).
- Privacy: stored on the device with the rest of the ratings, included in one-tap export and erase.
- Test: marking all ten 5/5 prototype albums as coups de cœur must leave the close / edge / far mix unchanged.

## 2. Sound bins in the mood filter, and "Pick in a bin"
Asked by Tiff, 2026-10-13. Touches the Engine section (bins) and the adventure mix.

- The user's sound clusters show as named bins and are part of the mood filter.
- "Pick in a bin" page: tapping a bin triggers the next album from that bin.
- "Outside my bins" is always listed first.
- Bin names and contents come from the "Spotify playlists export" thread's sound fingerprint work.
