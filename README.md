# SR Image Cleanup Tool

A browser-based tool to clear captured images left behind when a record's
status changes to "No Update." Runs entirely client-side — no server, no
installs, no data leaves the user's machine.

## Deploy (GitHub Pages)

1. Create a new GitHub repository (e.g. `sr-cleanup-tool`).
2. Upload `index.html` to the root of the repo.
3. Go to **Settings → Pages**. Under "Source," choose the `main` branch and
   `/ (root)` folder, then save.
4. GitHub gives you a live URL within a minute or two, e.g.
   `https://yourusername.github.io/sr-cleanup-tool/`.
5. Share that URL with your team.

## Requirements for users

- **Chrome or Edge on desktop.** The tool uses the File System Access API,
  which is not supported in Firefox, Safari, or mobile browsers.
- No installation, no login, no account.

## How it works

1. **Choose folder** — user grants read-write access to their images folder.
   Optionally remembered on that device for next time (stored locally via
   IndexedDB; nothing is sent anywhere).
2. **Load list** — CSV upload or pasted filenames of records now marked
   "No Update." Extensions in the list are ignored/optional.
3. **Scan** — matches files in the folder (optionally including subfolders)
   whose name, minus extension, appears in the list.
4. **Preview** — every match is shown with its size before anything happens.
5. **Confirm** — a custom modal confirms the action. By default, matched
   files are moved into a timestamped `_DELETED_BACKUP/<timestamp>/`
   subfolder rather than deleted outright. A "permanently delete" checkbox
   is available once the team trusts the results.
6. **Log** — every run produces a downloadable CSV log of what was moved or
   deleted, for audit purposes.

## Notes for IT / security review

- No backend, no external requests other than loading the Google Fonts
  stylesheet on first load.
- File access is scoped to only the folder the user explicitly picks via
  the browser's native picker — the page cannot access anything else on
  the machine.
- Some enterprise Chrome policies can disable the File System Access API;
  confirm it isn't blocked org-wide before rollout.
- Source is a single static HTML file — fully readable/auditable, no build
  step, no dependencies beyond the CDN font.

## Local testing

Just open `index.html` directly in Chrome — note that some browsers treat
`file://` pages as a non-secure context and may block the folder picker.
If that happens, serve it locally instead:

```
python3 -m http.server 8000
```

then visit `http://localhost:8000/index.html`.
