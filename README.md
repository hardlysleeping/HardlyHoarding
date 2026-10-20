# HardlyHoarding

> ## HOARD MEDIA.
>
> Everything runs on your own machine. Local web UI, local files, no account,
> no cloud, no telemetry — and a queue that survives reboots instead of a
> folder of half-finished downloads.

Two projects, each standalone:

| | Project | What it hoards | Stack |
|---|---|---|---|
| 🐴 | **[HardlyReader](https://github.com/hardlysleeping/HardlyReader)** | Magazines, comics and PDFs from Internet Archive | Python stdlib + aria2c |
| 📥 | **[insta-desktop](https://github.com/hardlysleeping/insta-desktop)** | Instagram and TikTok profiles | Python + instaloader + yt-dlp |

Both are web UIs you open in a browser. Neither reimplements a downloader:
HardlyReader drives `aria2c`, insta-desktop drives `instaloader` and `yt-dlp`.

---

## 🐴 HardlyReader

**Grab whole collections from Internet Archive and read them.**

Paste a collection link, an item link, or search in-app. It resolves every
member, downloads one good file per issue — **CBZ/CBR first, then PDF** —
verifies each one against archive.org's checksum, and files it into a
Komga-compatible folder. Then it lets you read them.

<img src="docs/search.png" width="820" alt="Searching Internet Archive">

- **Collections, items and searches** — paste or search; collections pull every member
- **Best file per issue** — CBR → CBZ → image-PDF → text-PDF, originals preferred
- **Verified downloads** — byte + MD5 against archive.org, because it sometimes serves an error page with HTTP 200
- **Resumable** — restart mid-download and it continues; finished files are skipped
- **Retry with backoff** — 15m → 1h → 3h → 8h, and genuinely missing items are marked unavailable rather than retried forever
- **Komga-ready** — one folder per series, `<Series> NNN (Year).ext` so files sort in reading order
- **Library and reader** — CBZ and PDF, with reading position kept server-side in SQLite so it follows you between devices

<img src="docs/reader.png" width="820" alt="Reading a CBZ">

**Requirements:** Python 3.8+ and [`aria2c`](https://github.com/aria2/aria2/releases).
Run it with `python run.py`.

→ **[Full documentation](https://github.com/hardlysleeping/HardlyReader)**

---

## 📥 insta-desktop

**Paste 200 profiles once and let them drain over days.**

A browser interface for instaloader and yt-dlp. Persistent queue, pacing,
auto-resume after rate limits, and a gallery to browse what you kept —
including a timeline view and your own phone uploads alongside the
downloaded profiles.

<img src="docs/gallery.png" width="820" alt="Instagram profile gallery">

- **Persistent queue** — survives reboots, drains one profile at a time with pacing
- **Auto-resume** — rate limits pause the queue instead of failing it
- **Gallery and timeline** — browse by profile or by date, favourites, filters
- **Your own photos too** — any folder appears as a profile, so phone uploads browse in the same grid
- **Recoverable removals** — deleting never loses anything; restore from the Removed list
- **Reels as a separate pass** — optionally queue reels as their own follow-up job

<img src="docs/timeline.png" width="820" alt="Timeline view">

**Requirements:** Python, [`instaloader`](https://instaloader.github.io/),
[`yt-dlp`](https://github.com/yt-dlp/yt-dlp) and `ffmpeg`.

> **Use at your own risk.** Downloading at scale can get your IP blocked,
> rate limited, or your account banned. Check the terms of any service you
> point these at.

→ **[Full documentation](https://github.com/hardlysleeping/insta-desktop)**

---

## Why two repos

They share a philosophy, not a codebase. One talks to a public archive with an
open API; the other drives two mature CLI downloaders against services that
rate limit and ban. Merging them would mean one install dragging in
dependencies the other does not need. Keep them separate; this repo is the
shelf they sit on.

Neither project redistributes anything. What you download is your
responsibility — check the rights on anything you grab.

## License

GPL-3.0
