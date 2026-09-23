# DEYMFLIX App v1.4 — Sketchware Pro Upgrade Guide (v1.4e build — TRANSFER-PROOF)

**Start from:** your existing project (608). Do NOT re-import — just re-paste the Java.
**Estimated time:** ~10 minutes. No new libraries.

> ## ⚠️ HOW TO MOVE THIS FILE TO YOUR PHONE (read first)
>
> Your compile errors were caused by the code traveling through **Telegram**:
> the `||` operator triggers spoiler-formatting and gets eaten (chunks vanish),
> and symbols get re-encoded into garbage. v1.4e is immune (100% ASCII, zero
> pipes) — but the transfer path still matters:
>
> 1. Upload `DEYMFLIX-APP-v1.4.java` to your **GitHub repo** (same repo as the site)
> 2. On your phone, open the file's **RAW** url in the browser
>    (`github.com/<you>/<repo>/raw/main/DEYMFLIX-APP-v1.4.java`)
> 3. Select All → Copy → paste into the Sketchware injection tabs
>
> Never copy code from a chat bubble. RAW text has no formatting — nothing can be eaten.
>
> **Paste check:** each tab's LAST line must be its `END OF SECTION` marker.
> **Tab check:** SECTION 4 belongs ONLY in DownloadsActivity — a past error
> showed it inside MainActivity (code about `COLUMN_TOTAL_SIZE_BYTES` in
> MainActivity.java = wrong tab).

---

## What's new in v1.4b

| Feature | How it works |
|---|---|
| **Animated splash** | Hexagon play logo (rotating + pulsing glow, like app.html) over the DEYMFLIX wordmark; fades out when the site finishes loading, 6s safety timeout |
| **App-mode gate** | WebView UA gets ` DeymflixApp/1.4` → site shows ⬇ nav + Download button only inside the app |
| **Themed confirm dialog** | Dark card + red Download button matching the site theme, shows title/quality/size |
| **Private downloads** | Files save to the app's private storage (`Android/data/...`) — NOT the user's gallery; playable only inside DEYMFLIX |
| **Real DownloadsActivity** | ⬇ nav INTENTs to your DownloadsActivity (full-screen page, no popup) with live progress, Cancel/Delete |
| **Fullscreen fixed** | Landscape lock + hidden bars only — nothing covers the video (black-screen bug gone); back exits fullscreen first |

---

## STEP 0 — Manifest (Manifest tab) — IMPORTANT for the rotation fix

1. Add permission (Android 13+ notification):
```
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
```
2. On **MainActivity**, add this attribute (stops the black reload on rotate):
```
android:configChanges="orientation|screenSize|screenLayout|smallestScreenSize|keyboardHidden"
```
(In Sketchware Pro: Manifest tab → find `<activity android:name=".MainActivity"` → add the attribute inside that tag.)

WRITE_EXTERNAL_STORAGE is **no longer needed** — downloads are private now.

## STEP 1 — Paste the v1.4e Java

**MainActivity → Logic → ⋮ → Java/Kotlin Injection** — in each tab: **Select All → Delete → paste**:

| Section | Tab | Size |
|---|---|---|
| SECTION 1 | `onCreate` | big block (contains the class-level helpers) |
| SECTION 2 | `onBackPressed` | **ONE LINE**: `handleBack();` |
| SECTION 3 | `onResume` | **ONE LINE**: `handleResume();` |

**DownloadsActivity → Logic → ⋮ → Java/Kotlin Injection**:

| Section | File | Tab | Size |
|---|---|---|---|
| SECTION 4A | `SECTION-4A-paste.txt` | `onCreate` (FULL CLEAR first) | ~2.4 KB |
| SECTION 4B | `SECTION-4B-paste.txt` | same tab, **directly below 4A** | ~2 KB |
| SECTION 4C | `SECTION-4C-paste.txt` | same tab, **directly below 4B** | ~4.3 KB |
| SECTION 4D | `SECTION-4D-paste.txt` | same tab, **directly below 4C** | ~5.8 KB |
| SECTION 4E | `SECTION-4E-paste.txt` | same tab, **directly below 4D** | ~1.5 KB |

Paste **4A → 4B → 4C → 4D → 4E** in order, all in the same onCreate tab.
Leave `downloads.xml` **empty** — the screen builds itself in code.

The new card layout matches the Netflix-style reference: poster thumbnail on the left,
title + status + thin red progress bar on the right, and Play / Cancel / Resume / Retry /
Delete buttons under each card. The confirm dialog now also shows the movie's poster.
Posters come from the site through the download bridge — upload the updated `app.js`
(?v=160.2) and `player.html` to GitHub Pages together with these app changes.

### STEP 1.5 — Play button for downloaded movies (in-app player)

Downloads get a red **Play** button (only on finished ones) that opens a native in-app
player — playback never leaves DEYMFLIX (no VLC, no gallery). One-time setup:

1. **Activity manager → Add Activity** → name it EXACTLY `LocalPlayerActivity`
   (empty layout is fine — the code builds everything)
2. **LocalPlayerActivity → Logic → ⋮ → Java/Kotlin Injection → onCreate**:
   Select All → Delete → paste **SECTION 6** from the master file
   (also saved standalone as `.freebuff/section6.txt` — upload to GitHub RAW to copy)
3. In Sketchware's **asset manager**, add `local-player.html` (in your Deymflix folder)
   as an app asset — same place offline.html lives
4. **DownloadsActivity**: full-clear the onCreate tab and paste the **new five-part
   layout** — `SECTION-4A-paste.txt` through `SECTION-4E-paste.txt` in order (the old
   two-part 4A/4B is replaced; the card design is now the Netflix-style thumbnail layout
   and SECTION 4C contains the Play button + player launch code).

Leave `downloads.xml` **empty** — the screen builds itself in code.

**Clean-paste check:** each part must end with its `END OF SECTION 4x` marker
(4A → 4B → 4C → 4D → 4E). If a marker is missing, that paste was truncated →
re-copy that section (from GitHub RAW, not a chat app).

⚠️ **Replace, don't append** — duplicate methods = compile errors.

ℹ️ Note: SECTION 1 contains **one unmatched `}`** on purpose (it closes onCreate so the
helpers live at class level). This is the exact pattern v1.3 used and it compiled — do
not "fix" it. Sections 2, 3 and 4 are fully balanced.

## STEP 2 — Build & install

Build the APK in Sketchware Pro and install. Expected on launch:

1. **Animated splash** — red hexagon spinning with pulsing glow + DEYMFLIX wordmark; fades when the site loads (max 6s)
2. **Bottom nav** — 6 items ending with ⬇ Downloads; footer APK button **hidden**
3. **Download flow** — movie → ⬇ beside bookmark → themed dark dialog with quality + size → notification progress
4. **My Downloads** — full-screen page (your DownloadsActivity), live %, Cancel/Delete. Files are private to the app
5. **Fullscreen** — tap fullscreen → landscape + bars hidden, **video visible** (no black screen); back exits fullscreen first

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| Nav shows 5 items in the app | Site files not uploaded yet (app.js ?v=160.1) or one-time cache — pull to refresh once |
| Splash hangs | It can't (6s timer). If seen, SECTION 1 paste is incomplete |
| "This title cannot be downloaded." | iframe/HLS-only title — expected; direct MP4s download fine |
| `confirmAndDownload ... not applicable for arguments (String, String, String, long)` | Old v1.4a/b block still in the DownloadListener — full-clear the onCreate tab and re-paste SECTION 1 |
| `Duplicate method isAppFullscreen` | Old v1.4a/b block still present — full-clear the onCreate tab and re-paste SECTION 1 |
| Black screen on rotate (still) | MainActivity `android:configChanges` attribute missing (STEP 0.2) |
| Duplicate-method compile error | Old block not fully deleted — Select All first |

---

## File map

- `DEYMFLIX-APP-v1.4.java` — 4 sections (3 MainActivity + 1 DownloadsActivity)
- `downloads.html` — web fallback (bridge safety net)
- `app.js` / `player.html` (website) — app-mode gate + download button + native fullscreen call
