# stagclient-download

Public download page + release host for StagClient (unofficial ZČU STAG
Android app). Deliberately separate from the app's own source repo, so this
public-facing link never exposes that repo's dev/commit history.

## What lives here

- `index.html` — the landing page, served via GitHub Pages. Links to
  `releases/latest`, and its "Install" button is filled in by a small
  client-side script that reads the latest release's APK asset via GitHub's
  public API.
- Nothing else. Releases (the actual `.apk` files + changelog notes) are
  uploaded through GitHub's own Releases feature, not committed here.

## One-time setup (after creating this repo on GitHub)

1. Push this repo's contents to `main` on GitHub, under whatever repo name
   the app's `UPDATE_REPO_OWNER`/`UPDATE_REPO_NAME` constants in
   `network/UpdateApi.kt` point at (currently `snipexos/stagclient`).
2. Repo Settings → Pages → Deploy from a branch → `main` / `/ (root)`.
   Pages will then serve `index.html` at
   `https://snipexos.github.io/stagclient/`.
3. (Optional but recommended) point the landing page domain — or just share
   the `github.io` link directly — this file needs no further changes for
   that.

## Cutting a release

1. Bump `versionCode`/`versionName` in the app's `app/build.gradle.kts` and
   build a signed release APK (`./gradlew assembleRelease`).
2. On GitHub: **Releases → Draft a new release**.
   - Tag: `vX.Y` (e.g. `v1.2`) — the app parses this as the version to
     compare against what's installed.
   - Release title: whatever you want shown as the version label (About
     screen + update dialog both prefer this over the raw tag).
   - Release notes: plain text/Markdown changelog — shown verbatim in the
     in-app update dialog and the About screen's "What's new" list.
   - Attach the signed `.apk` file as a release asset (any filename ending
     in `.apk` — the app finds it by extension, not by exact name).
3. Publish. Every StagClient install checks for this on next launch and
   offers to update; the landing page's Install button also updates itself
   automatically (no separate edit needed here).
