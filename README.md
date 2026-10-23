# Muse4pc

your Muse ai on your windows PC

## Muse for Windows (unofficial)

A one-click Windows desktop client for [Muse](https://muse.ai).

**Download the `.exe`, double-click it, you're in.** No installer wizard,
no Node, no Python, no other dependencies — Electron is bundled inside
the single portable file.

> Unofficial community project. Not made by, affiliated with, or endorsed
> by Meta. Muse is a trademark of Meta. This app is just a desktop
> wrapper around the official web app at muse.ai — you sign in with your
> own Muse account exactly as you would in a browser, and no credentials
> pass through this project.

## Download (for people, not developers)

1. Go to the **Releases** page of this repo.
2. Download `Muse-for-Windows-0.2.0.exe`.
3. Double-click it. That's it.

Windows SmartScreen may say "Windows protected your PC" the first time,
because the build isn't code-signed yet. Click **More info → Run anyway**.
Only download releases from a repo you trust.

## What you get

- Muse in its own window, not buried in browser tabs
- **Steve on your desktop** — a free-roaming desktop pet (transparent,
  always on top) who wanders along the bottom of your screen, turns
  around at the edges, and bobs as he walks. Drag him anywhere and he'll
  sit there a while before wandering off again.
- **Pop-up notifications from Steve** — when Steve has something to say
  he stops, jumps, and pops a speech bubble. Click the bubble (or his
  **Chat** button) to open Muse. If Steve is hidden, it arrives as a
  normal Windows notification instead.
  Try it: tray menu → **Make Steve say hi**.
- **System tray** — closing the window minimizes to the tray, Muse keeps running
- **Quick Chat** — press `Alt + Space` anywhere in Windows
  (falls back to `Ctrl + Shift + Space` if Alt+Space is taken)
- **Start on login** — toggle it from the tray menu
- Right-click Steve for his menu: Chat, Say hi, Roam on/off, Hide Steve.
  Roaming and visibility are also toggles in the tray menu.
- External links open in your default browser; Muse / Meta sign-in
  stays inside the app
- Single-instance: launching it again just brings the window forward

## Build it yourself (developers)

You only need these if you want to build the `.exe` — end users don't.

Requirements: Windows, Node.js 20+, Git.

```powershell
git clone https://github.com/NightmareDesigns/Muse4pc.git
cd Muse4pc
npm install
npm run dist
```

The single portable file lands in `dist\Muse-for-Windows-0.2.0.exe`.

Run it without building:

```powershell
npm start
```

## Publish a release on GitHub

```powershell
git add .
git commit -m "Muse for Windows v0.2.0"
git push origin main
git tag v0.2.0
git push origin v0.2.0
```

Pushing a `v*` tag runs the GitHub Actions workflow in
`.github/workflows/release.yml`, which builds on a real Windows runner
and attaches the portable `.exe` to a GitHub Release automatically.
You can also upload `dist\Muse-for-Windows-0.2.0.exe` to a release by hand.

## How it works

- `src/main.js` — Electron main process: window, tray, shortcuts,
  single-instance lock, settings in `%APPDATA%\Muse for Windows`, and
  the desktop pet: roaming loop, edge turning, and `showPetNotification()`,
  the one function anything in the app calls to make Steve announce
  something in a speech bubble
- `src/pet.html` — Steve himself: transparent cut-out (`assets/pet.png`),
  bob/jump animations, speech bubble, Chat button, drag-to-move
- `src/quick-chat.html` — the small Alt+Space launcher window
- `src/preload.js` — a tiny, sandboxed bridge for the quick-chat page
  only. The Muse web app itself gets no Node/Electron access.
- The app loads `https://muse.ai`. Override with the `MUSE_URL`
  environment variable if you ever need to point it elsewhere.

Settings (open-at-login, close-to-tray, window size) are stored locally
by the app and never sent anywhere by this project.

## License

Apache License 2.0 — see `LICENSE`.
