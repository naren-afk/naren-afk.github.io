# NK-01 — Naren Kumar S · Portfolio

Evangelion-inspired terminal portfolio, live at **https://naren-afk.github.io**.

Single-page static site — no build step, no framework, no dependencies to install.
Deploys anywhere that serves files (GitHub Pages, Netlify, Vercel, a USB stick).

---

## Repo structure

```
index.html      ← the machine. Design, layout, animations, live features. DON'T edit for content.
content.json    ← YOUR file. Every visible text on the site lives here. Edit this.
covers/         ← all images + music (see asset list below)
README.md       ← this file
```

**Golden rule: content changes go in `content.json`, never `index.html`.**
On GitHub: open `content.json` → pencil icon → edit → *Commit changes* → refresh the site. Done.
Only people with write access to this repo can edit — visitors just read.

**Fail-safe:** `index.html` carries a complete built-in copy of the content. If `content.json`
is ever missing or broken (a stray comma), the site silently falls back to that copy instead
of breaking. Fix the JSON and it takes over again.

---

## Assets — the `covers/` folder

| File(s)                  | What it is                                             |
|--------------------------|--------------------------------------------------------|
| `g1.jpg` … `g12.jpg`     | Game covers, in the order games appear in content.json |
| `m1.jpg` … `m5.jpg`      | Song covers, in top-songs order                        |
| `cert1.jpg` … `cert3.jpg`| Certificate images (shown inside cert pop-ups)         |
| `theme.mp3`, `theme2.mp3`, `theme3.mp3` | Site music playlist (♪ toggle in the nav) |

- `.jpg` **or** `.png` both work — the site tries `.jpg` first, then `.png`, then falls
  back to the built-in gradient art. Missing files never break anything.
- Game/cert numbering follows **array order in content.json**, so if you reorder or add
  entries, point each entry's `"img"` at the right file.
- Keep music files small (a few MB) so first play is fast on mobile.

---

## Editing `content.json` — section by section

General rules:
- Keep the JSON structure intact: every string in `"quotes"`, commas between items,
  matching `{ } [ ]`. One missing comma invalidates the whole file (→ fallback kicks in).
  Unsure? Paste the file into **jsonlint.com** before committing.
- Fields ending in `_html`, plus `bullets`, modal `b` paragraphs, and `ticker` items may
  contain HTML — e.g. `<b>bold</b>` (renders green) or a link
  `<a href='...' target='_blank' rel='noopener'>text</a>`. Everything else is plain text.
- **Delete any whole top-level section** (e.g. all of `"ticker"`) to use the built-in default
  for that part.

### `topbar`
`sig` (the SYS:// label), `status` (blinking status text), `nav` (list of
`["Label", "#section-id"]` pairs), `resume` (`["Button label", "url"]`),
`sound` (labels for the music toggle: `on` / `off` / `none`).

### `ticker`
A flat list of phrases for the scrolling tape. Order = display order; `<b>…</b>` allowed.

### `hero`
`eyebrow`, `title_html` (the big name — keep the `glitch` spans for the glitch effect),
`kata` (vertical Japanese text), `sub_html` (your bio paragraph),
`tags` (`["LABEL","VALUE"]` chips), `ctas` (buttons: `["Label","url","acid"]` —
third value `"acid"` = green style, `""` = violet).

### `headers`
The `FILE 0x / TITLE / 日本語` strip for every section, keyed by section id
(`skills`, `experience`, `projects`, `sortie`, `sideops`, `record`, `offduty`, `contact`).

### `skills`
`cats` — list of `{ "name": "...", "chips": [...] }`. A chip is either a plain string
or `{ "t": "Python", "hot": true }` (`hot` = amber highlight).

### `field_record` (the internship card)
`rail_big` / `rail_sub` / `status` (the left rail), `title`, `meta`
(list of `{"text": "..."}`, add `"live": true` for green), `bullets` (HTML allowed),
`modal` (which pop-up id it opens — see **modals**), `button` label.

### `projects`
List of project cards. Each: `rail_big` ("01"), `rail_label` (small rail text,
`<br>` allowed), `status`, `title`, `stack`, `bullets`, `repo` (GitHub URL),
`modal` (pop-up id). **To add a project:** copy a block here *and* add a matching
entry in `modals` with the same id.

### `sortie` (featured latest work)
`badge`, `title`, `meta`, `lead_html`, `stats` (list of `["0.949","Label"]` tiles),
`pipeline` (step names), `repo`, `modal`.

### `side_ops`
Categories: `{ "name", "color", "tagline", "items": [...] }`.
`color` ∈ `acid` (green) · `vio` (violet) · `amber` · `mag` (magenta) · `green`.
Items: `{ "title", "lang", "desc", "url" }`.

### `record`
- `heads`: the three column headings.
- `education`: `degree`, `school`, `years` (HTML ok), `extra` (CGPA), `note`.
- `certs`: cards — `{ "modal", "title", "org", "desc" }` (image lives in the modal).
- `achievements`: timeline — `{ "year", "title", "desc" }`, optional
  `"badge": "1st Place"` for the gold tag.

### `off_duty`
- Labels: `username`, `since`, `deck_title`, `deck_hint`, `snd_title`,
  `artists_head`, `beacon_note` (HTML ok).
- `games`: list of `{ "img", "art", "initials", "tag", "name", "about" }`.
  `art` = fallback gradient class: `ga-field ga-royal ga-anemo ga-tac ga-dao ga-race
  ga-ring ga-west ga-crimson ga-rust ga-steel ga-ink`.
  Any number of games works — the grid adapts; on phones it pages 6 at a time.
- `soundtrack` — **the monthly ritual** (open Apple Music Replay, 30 seconds):
  - `label` ("Oct 2026 · Apple Music"), `minutes` ("12,181"), `minutes_label`
  - `artists`: `[["Name","7,934 min"], ...]`
  - `songs`: `{ "img", "initials", "bg" (fallback gradient CSS), "title", "artist", "plays" }`
    — rank numbers are automatic from order
  - `apple_link` (the Full Overview button), `footnote` (top-album line)
- `music_files`: the ♪ playlist — paths to audio files in `covers/`.

### `contact`
- `cards`: `{ "label", "name", "url", "pfp_text", "pfp_img" (optional image),
  "pfp_id" (leave `ccDiscordPfp` on the Discord card so the live avatar loads),
  "title" (hover tooltip) }`. All cards open their `url` in a new tab.
- `cta` (the Open Frequency banner): `kicker`, `text_html`, `buttons`
  (`["Label","url"]`).

### `footer`
Flat list of footer strings.

### `modals`
Every pop-up on the site, keyed by id. Each:
- `k` — small kicker line · `t` — title · `s` — subtitle/stack line
- `img` — optional image shown at the top (certificates use this:
  `covers/cert1.jpg` etc.). **Delete all `b` paragraphs to show the image alone.**
- `b` — list of HTML blocks. Available: `<h4>Section head</h4>`, `<p>…</p>`,
  `<ul><li>…</li></ul>`, `<div class='arch'>ASCII diagram (use \n for line breaks)</div>`,
  `<p class='mdim'>dim footnote</p>`.
- `links` — buttons: `[["Label","url"]]` (new tab). Point cert links at real
  Coursera/GUVI verification URLs when you have them.

---

## Live features (work on the deployed site, not in local previews of just the HTML)

- **Discord beacon** (bottom-right): live presence via the Lanyard API
  (`api.lanyard.rest`, Discord ID is set in `index.html`). Spinning vinyl + album art
  when listening (Spotify or Apple Music), game icon + pulse when playing. Requires
  staying in the Lanyard Discord server (you already are).
- **Avatars**: GitHub card pulls your GitHub avatar; Discord card/avatar fills from Lanyard.

## Deploy / preview

- **GitHub Pages**: repo named `naren-afk.github.io`, Settings → Pages → deploy from `main`.
- **Local preview**: run `python -m http.server` in the repo folder and open
  `http://localhost:8000` (opening `index.html` directly via `file://` blocks
  `content.json` loading in some browsers).

## Troubleshooting

| Symptom | Cause / fix |
|---|---|
| Edits don't show | Hard-refresh (Ctrl/Cmd+Shift+R); check the commit landed |
| Site shows old default text | `content.json` is invalid — validate at jsonlint.com |
| An image shows gradient art | Filename/extension mismatch in `covers/` vs the `img` path |
| ♪ says "No tracks" | No file from `music_files` exists in `covers/` |
| Beacon says "Signal lost" | Lanyard unreachable, or previewing in a sandboxed viewer |
