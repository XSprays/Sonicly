# Sonicly

**A modern, cross-platform media player for music, videos, and photos — with synchronized lyrics, Veylara visualizers, and dedicated media-server connectivity.**

Sonicly brings a personal media library into one polished desktop experience. Browse local folders or connect to a Sonicly Dedicated Media Server, listen with album artwork and synchronized lyrics, watch videos with lyric overlays, explore photos, and bring music to life with the original Veylara visualizer themes.

The desktop application is built with Electron, React, and TypeScript. The current desktop release targets **Windows x64** and **Linux x64 (AppImage)**. Android and Android TV companion clients are developed as separate projects and can connect to a Sonicly server.

> **Project status:** Active development. Sonicly is evolving quickly; features, packaging, and platform support may change between releases. Check the release notes for the exact contents of each build.

---

## Contents

- [Highlights](#highlights)
- [Features](#features)
  - [Music player](#music-player)
  - [Video player](#video-player)
  - [Photo gallery](#photo-gallery)
  - [Unified library](#unified-library)
  - [Synchronized lyrics](#synchronized-lyrics)
  - [Veylara visualizer](#veylara-visualizer)
  - [Dedicated media-server client](#dedicated-media-server-client)
  - [Android companion connection](#android-companion-connection)
  - [Personalization and settings](#personalization-and-settings)
- [Supported media formats](#supported-media-formats)
- [Platforms and downloads](#platforms-and-downloads)
- [Build from source](#build-from-source)
- [Connecting a dedicated server](#connecting-a-dedicated-server)
- [Connecting an Android companion](#connecting-an-android-companion)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Security and privacy](#security-and-privacy)
- [Project structure](#project-structure)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

---

## Highlights

- **One player, multiple libraries:** use folders and files on your computer or switch to a paired Sonicly Dedicated Media Server.
- **Music discovery:** browse by artist and album, search tracks, and view album artwork and metadata.
- **Video playback:** browse videos, use theatre or fullscreen viewing, and show synchronized lyrics over supported videos.
- **Photo gallery:** browse and preview images from the active library.
- **Automatic lyrics:** Sonicly looks up lyrics through LRCLIB and displays timed lyrics when available.
- **Six Veylara visualizer themes:** Ring, Blobs, Solar, Vinyl, Ducks, and Cat.
- **Remote streaming:** stream server-hosted music and video, load artwork and photos, and seek through supported streams.
- **Portable Linux build:** distribute Sonicly as a single `.AppImage` file.
- **Android connectivity:** the installed desktop app can host a private LAN connection for compatible Sonicly Android clients.

## Features

### Music player

Sonicly's music view organizes audio into **Artists**, **Albums**, and **Tracks**. Select an artist or album to narrow the list, or search by track, artist, or album name.

The persistent transport bar provides:

- Play and pause.
- Previous and next track.
- Shuffle.
- Repeat off, repeat all, and repeat one.
- Playback timeline and seeking.
- Volume and mute controls.
- Quick access to the lyrics panel and fullscreen lyrics while audio is playing.

Add individual files, link folders, or drag and drop media into Sonicly. Linked folders are remembered between launches and can be rescanned, reconnected if permission is lost, or forgotten from the library. Sonicly reads available metadata and uses filename/folder information as a fallback when tags are missing. Common embedded metadata formats handled by the desktop metadata reader include ID3, FLAC/Vorbis comments, and MP4/M4A atoms. For local libraries, the Library editor can override a track's displayed title, artist, or album and can assign or clear its cover image. These edits change Sonicly's library presentation; they do not rewrite the original media file's embedded tags.

### Video player

The Videos view provides a searchable video list, folder filtering, and an integrated player. The video experience includes:

- Theatre mode and fullscreen playback.
- Playback controls and seeking.
- Optional video view counts.
- A local-library option to capture the current video frame as its thumbnail.
- A synchronized lyric overlay when timed lyrics can be matched to the video.
- A fullscreen lyrics view that can be opened while watching.

Codec support depends on the codecs available to the Chromium/Electron media stack on the target operating system. A file extension being recognized by Sonicly does not guarantee that every codec inside that container can be decoded on every machine.

### Photo gallery

The Gallery view displays images from the active library. Open an image for a larger preview and move between images using the on-screen controls or the left/right arrow keys. Gallery items are browsable alongside music and videos without requiring a separate application.

### Unified library

The Library view brings the active media source together in one searchable list. Filter by media type, inspect metadata, and manage local items.

For local files, the Library editor supports:

- Changing the title shown in Sonicly.
- Changing artist and album labels for audio.
- Assigning or clearing cover art or a video thumbnail.
- Removing an item from Sonicly's library without deleting the underlying file.

Remote server items are read-only from the desktop library view. Files are managed on the server that hosts them.

### Synchronized lyrics

Sonicly can look up lyrics automatically through [LRCLIB](https://lrclib.net/), using the current track's title, artist, album, and duration when available. Results may include timed (LRC-style) lyrics or plain text.

For music:

- Timed lyrics follow playback and highlight the current line.
- The lyrics panel is available globally while audio is playing, not only from the Music page.
- A fullscreen lyrics view is available.
- Lyrics results are cached in memory during the session to avoid repeated lookups for the same track metadata.

For videos:

- Sonicly searches for a matching track and artist.
- The previous/current/next timed lyric lines can appear over the video.
- The overlay enables automatically when timed lines are found and can be toggled.
- Fullscreen lyrics can be opened without creating a second video player.

Lyrics require an internet connection to query LRCLIB. Availability and accuracy depend on the metadata match and the LRCLIB catalog. Instrumental tracks and tracks without a matching result may not have lyrics.

### Veylara visualizer

The Visualizer page ports the original Veylara theme pack into Sonicly. It analyzes the currently playing Sonicly audio element using the Web Audio API; it does not create a second music player.

The six themes are:

| Theme | Visual style |
|---|---|
| **Ring** | A reactive, glowing ring with a flowing visual field. |
| **Blobs** | Soft, morphing organic forms. |
| **Solar** | A planet and orbital visual elements. |
| **Vinyl** | A stylized record deck with album artwork. |
| **Ducks** | Animated ducks floating over a moving water scene. |
| **Cat** | A playful animated character and stage. |

Visualizer options include theme selection, sensitivity adjustment, and Vinyl artwork presentation modes. Album artwork and Sonicly branding are supplied to the theme renderer. Preferences are saved locally by the app.

The audio graph is shared with the existing audio element and retained across visualizer page changes so the element is not attached to multiple analyser graphs. Audio is routed back to the normal output device.

### Dedicated media-server client

Sonicly Desktop can connect to a separately hosted **Sonicly Dedicated Media Server** that implements Sonicly API v1. The desktop app is a client; the dedicated server is a separate Node.js service and is not bundled into the desktop application.

The Server page supports:

- Entering a server IP address or DNS name and port.
- Discovering and validating a compatible Sonicly server.
- Requesting a pairing challenge and entering the four-digit code printed in the server console.
- Saving this desktop's pairing so it can reconnect later.
- Refreshing the remote library, searching remote media, and filtering by music, videos, or photos.
- Viewing server status, protocol version, server version, and remote library counts.
- Disconnecting this desktop from the saved server connection.

After pairing, choose **Settings → Active media library → Server files** to make the remote library active throughout Music, Videos, Gallery, Library, playback, lyrics, and the Veylara visualizer. Choose **Local files** to return to the computer's own folders. Switching sources does not move or delete files.

The desktop client uses the server's paginated `/api/v1/media?type=all` endpoint and currently has a safety ceiling of 50,000 remote items. The client displays when this limit truncates the loaded library.

Remote music, video, photo, and artwork requests are authenticated. Audio/video streaming uses an Electron-hosted local media proxy that forwards byte-range requests so supported players can seek without downloading the entire file first.

For the server API, configuration, hosting, and device administration, see the documentation in the **Sonicly Dedicated Media Server** project.

### Android companion connection

The installed Electron desktop app can also host a small authenticated HTTP API on the local network so a compatible Sonicly Android client can browse and stream the media linked to the desktop computer.

- Default LAN port: `47831`.
- Pairing is initiated by the Android device; Sonicly Desktop displays a temporary four-digit confirmation code in a separate pairing window.
- Each paired device receives its own credential and can be unpaired from **Settings → Sonicly mobile connection**.
- The desktop-hosted service runs only while Sonicly Desktop is open.
- This connection is intended for devices on the same trusted Wi-Fi/LAN, not for direct public internet exposure.

This LAN companion host is distinct from connecting the desktop app to a separately hosted Sonicly Dedicated Media Server. They are two different connection directions:

1. **Desktop as a host:** Android connects to the desktop's linked local library.
2. **Desktop as a client:** the desktop connects to a dedicated Sonicly server and uses its remote library.

Android and Android TV applications are maintained as companion projects rather than being part of this desktop source package.

### Personalization and settings

Settings include:

- Active library source: Local files or Server files.
- Independent visibility switches for Music, Videos, and Gallery.
- Optional video view counts.
- Discord Rich Presence configuration for supported installed desktop builds.
- Custom background image selection and removal.
- Linked local-folder management, including rescan, permission reconnect, and forget/remove-from-library actions.
- Local library cleanup controls.

**Clear library** only clears Sonicly's local library records, artwork overrides, and background. It does not delete media files from disk or remove content from a dedicated server.

Discord Rich Presence is exposed through the installed Electron desktop app. Discord must be running, and a Discord Application ID must be configured. Discord bot tokens are not required. Availability can depend on the installed Discord desktop client and operating-system IPC support; verify it with the Settings test button on your platform.

---

## Supported media formats

Sonicly recognizes the following file types in its current desktop library scanner:

| Type | Extensions |
|---|---|
| Audio | `.mp3`, `.wav`, `.ogg`, `.oga`, `.opus`, `.m4a`, `.aac`, `.flac`, `.weba` |
| Video | `.mp4`, `.m4v`, `.webm`, `.mkv`, `.avi`, `.mov`, `.mpeg`, `.mpg`, `.ogv` |
| Images | `.jpg`, `.jpeg`, `.png`, `.webp`, `.gif`, `.bmp`, `.avif` |

Sonicly identifies media by extension and MIME type. Actual playback depends on operating-system and Electron/Chromium codec support. For example, two `.mkv` files may contain different video/audio codecs, and one may play while the other does not.

---

## Platforms and downloads

### Windows

The current Electron Builder configuration creates a Windows x64 NSIS installer. The installer supports choosing an installation directory and creating Start Menu and desktop shortcuts.

Download the Windows installer from the project's GitHub **Releases** page when a release is published.

### Linux

The current Linux target is a portable **x86_64 AppImage**. It is intended for common x86_64 Linux desktop distributions, including Ubuntu, Debian-based distributions, Fedora-based distributions, and Arch-based distributions such as CachyOS.

Download the `.AppImage` from GitHub Releases, then run:

```bash
chmod +x Sonicly-*-Linux-x64.AppImage
./Sonicly-*-Linux-x64.AppImage
```

On Ubuntu 22.04, if launching reports that `libfuse.so.2` is missing, install FUSE 2 compatibility:

```bash
sudo apt update
sudo apt install -y libfuse2
```

On Arch-based distributions, install the distribution's FUSE 2 compatibility package if required (commonly `fuse2`).

### Platform scope

- **Windows x64:** configured installer target.
- **Linux x64:** configured AppImage target.
- **macOS:** not currently configured as a release target.
- **Linux ARM:** not currently configured as a release target.

---

## Build from source

### Requirements

- Node.js **22 or newer** (use [NVM](https://github.com/nvm-sh/nvm) to manage versions if desired).
- npm (included with Node.js).
- Git.
- A supported build environment for the target platform.

For the most reliable Linux AppImage build, build on a native Linux x86_64 installation rather than cross-packaging from Windows.

### 1. Clone the repository

```bash
git clone <YOUR-GITHUB-REPOSITORY-URL>
cd <YOUR-REPOSITORY>/desktop/media-player
```

Replace the placeholder URL and folder name with your repository's actual values.

### 2. Install dependencies

```bash
npm install
```

### 3. Build a Linux AppImage

```bash
npm run appimage
```

The Electron Builder `beforePack` hook builds the desktop renderer and copies it into the packaged desktop output before Electron creates the AppImage.

The generated file is written to:

```text
release/Sonicly-<version>-Linux-x64.AppImage
```

Make it executable and launch it:

```bash
chmod +x release/Sonicly-*-Linux-x64.AppImage
./release/Sonicly-*-Linux-x64.AppImage
```

### 4. Build the Windows installer

Run this on Windows from `desktop/media-player`:

```powershell
npm install
npm run exe
```

The generated installer is written to the `release/` directory.

### Useful development commands

| Command | Purpose |
|---|---|
| `npm run dev` | Start the Vite development server. |
| `npm run typecheck` | Run the TypeScript compiler without emitting files. |
| `npm test` | Run the project's Node and TypeScript tests. |
| `npm run lint` | Run ESLint. |
| `npm run appimage` | Package the Linux x64 AppImage. |
| `npm run exe` | Build the Windows installer. |

The package scripts and build targets are defined in `desktop/media-player/package.json`.

---

## Connecting a dedicated server

1. Deploy a compatible Sonicly Dedicated Media Server (API v1; v1.0.2 is the documented integration baseline).
2. Make the server reachable from the computer running Sonicly Desktop.
3. In Sonicly Desktop, open **Server**.
4. Enter the server's IP/DNS name and port. The default port is `47831`.
5. Select **Find server and pair**.
6. Read the unique four-digit code printed in the dedicated server's console for that pairing request.
7. Enter the code in Sonicly Desktop before the challenge expires.
8. After the library loads, open **Settings** and select **Server files** as the active library source.

For connections across the public internet, use HTTPS or a trusted VPN. Plain HTTP should be used only on a trusted local network. Do not expose bearer-token traffic or the pairing endpoint directly to an untrusted public network.

Disconnecting from the desktop app forgets the saved credential on this computer; it does not delete server files or revoke the paired device on the server. The server administrator can revoke a device using the server's device-management tools.

---

## Connecting an Android companion

1. Install and open Sonicly Desktop on the host computer.
2. Open **Settings → Sonicly mobile connection**.
3. Connect the Android device to the same trusted Wi-Fi/LAN.
4. Start the connection flow from the compatible Android app.
5. Confirm the temporary four-digit code shown in the Sonicly Desktop pairing popup.
6. Use the Android app to browse and stream the desktop's linked media.

Allow Sonicly through the computer's firewall on a **Private** network when prompted. Do not port-forward the desktop companion port to the public internet. Unpair a device from the desktop Settings page when access is no longer needed.

---

## Keyboard shortcuts

Shortcuts are active when focus is not inside an input, text area, select, or button.

| Key | Action |
|---|---|
| `Space` | Toggle play/pause. |
| `Left Arrow` | Seek backward 5 seconds (except in Gallery). |
| `Right Arrow` | Seek forward 5 seconds (except in Gallery). |
| `Up Arrow` | Increase volume by 5%. |
| `Down Arrow` | Decrease volume by 5%. |
| `Left Arrow` / `Right Arrow` in Gallery | Move to previous/next image. |

---

## Security and privacy

- Dedicated-server bearer tokens are encrypted at rest using Electron `safeStorage`; tokens are not written as plaintext JSON.
- On Linux, secure server-token storage requires an available desktop keyring such as **KWallet** (KDE Plasma) or **GNOME Keyring**. Sonicly refuses to save a dedicated-server token when Electron reports the weak `basic_text` storage backend.
- Authenticated media is requested through the Electron main-process proxy. The bearer token is added to upstream requests and is not embedded in media URLs.
- The proxy only accepts the expected media resource paths and `GET`/`HEAD` requests. It forwards byte-range headers for streaming and seeking.
- The desktop-to-Android companion service is designed for trusted LAN use. Do not expose it directly to the internet.
- Local library actions such as forgetting a folder or clearing Sonicly's library do not delete the original media files.

Keep your server addresses and access credentials private. Use HTTPS or a VPN for remote server access.

---

## Project structure

```text
<repository>/
├── desktop/
│   └── media-player/
│       ├── electron/          # Electron main process, preload, metadata support
│       ├── public/            # App assets and Veylara visualizer theme pack
│       ├── scripts/           # Build hooks, checks, migration and tests
│       ├── src/
│       │   ├── components/    # Music, Videos, Gallery, Library, Server, Visualizer, Settings
│       │   ├── player/        # Playback state, media scanning, persistence and helpers
│       │   └── routes/        # Application routes
│       ├── build/             # Platform icons and packaging resources
│       ├── package.json       # Dependencies and Electron Builder targets
│       └── ...
└── README.md
```

The dedicated Node.js media server and the Android/Android TV companion apps are separate projects. This repository contains the desktop client and its Electron-hosted LAN companion service.

---

## Troubleshooting

### AppImage does not launch

Make sure the file is executable:

```bash
chmod +x Sonicly-*-Linux-x64.AppImage
```

If the error mentions `libfuse.so.2`, install the FUSE 2 compatibility package for your distribution. On Ubuntu 22.04:

```bash
sudo apt install libfuse2
```

### Dedicated-server pairing cannot save

On Linux, unlock or enable KWallet or GNOME Keyring, restart Sonicly, and pair again. Sonicly intentionally refuses to save the token using Electron's insecure `basic_text` fallback.

### Remote library is empty

- Confirm the dedicated server is running and reachable from this computer.
- Check the server address and port (default `47831`).
- Confirm the server is a compatible Sonicly API v1 dedicated server.
- Complete pairing with the code created for the current pairing request.
- Refresh the remote library from the Server page.
- Select **Server files** under Settings → Active media library.

### A media file appears but will not play

Sonicly recognizes file types by extension and MIME type, but playback also requires the underlying codec to be supported by Electron/Chromium on that operating system. Try another encoding/container if the file is unsupported.

### Lyrics are unavailable

Lyrics depend on a matching LRCLIB entry and an internet connection. Check that the track's title and artist metadata are correct. Instrumental tracks and songs without a matching LRCLIB result may not display lyrics.

### Android cannot connect to the desktop

- Confirm both devices are on the same trusted Wi-Fi/LAN.
- Make sure Sonicly Desktop is open.
- Allow the app through the host firewall on Private networks.
- Use the address shown in Settings → Sonicly mobile connection.
- Start pairing from the Android app and confirm the temporary code in the desktop popup.
- Do not attempt to connect from outside the LAN unless you have deliberately configured a secure VPN path.

---

## Contributing

Contributions, bug reports, and feature ideas are welcome. Before submitting a change:

1. Describe the issue or motivation clearly.
2. Keep changes focused and avoid unrelated UI or playback regressions.
3. Run the relevant checks where possible:

   ```bash
   npm run typecheck
   npm test
   npm run lint
   ```

4. For desktop packaging changes, test the target build on its native operating system.
5. Include reproduction steps and logs for bug fixes when possible. Never include server bearer tokens, pairing codes, or personal media in issue reports.

---

## License

No license has been specified in this source package yet. Until a license file is added by the project owner, all rights are reserved by default; please do not assume the code is available for unrestricted reuse, redistribution, or modification.

---

## A note from the project

Sonicly is built around the idea of bringing your personal media together in one place: local libraries, remote server libraries, synchronized lyrics, expressive visualizers, and companion-device playback — with a consistent experience across Windows and Linux.

Thanks for checking out Sonicly. 💙
