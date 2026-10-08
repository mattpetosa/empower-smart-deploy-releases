# Release history

Newest first. Downloads are on the [Releases](https://github.com/mattpetosa/empower-smart-deploy-releases/releases) page.

## v3.10.0.1-1.43.3 (2026-10-08)

### Fixed
- **The ring on the running step turns.** Since 1.43.0 it stood still on many computers while the step was running. It now turns once a second for as long as the step runs.

## v3.10.0.1-1.43.2 (2026-10-08)

### Fixed
- **The running step's ring turns smoothly, and the window stays responsive.** Each step now does its work in the background, so the ring keeps turning while the step runs — before, it froze or jumped while a step was busy — and the window can still be moved or cancelled.

## v3.10.0.1-1.43.1 (2026-10-08)

### Fixed
- **The running step's ring spins again.** On computers where Windows' window animations are turned off — Remote Desktop sessions and servers set for best performance — the ring on the running step stood still. It now always turns while a step is running.

## v3.10.0.1-1.43.0 (2026-10-08)

### Changed
- **A refreshed step list.** Numbered steps, a spinning ring on the step that is running, and a progress bar that fills one segment per step.

## v3.10.0.1-1.42.0 (2026-10-07)

### Added
- **Offline discs fill C:\Client by themselves.** Run the app from an offline ISO built on the Smart Tools website (or from a copy of its files) and **Update & Download → Sync** first copies the disc's files into C:\Client — no internet and no download needed. Each file is checked against the disc's signed file list as it is copied; anything already in C:\Client is left as it is, and nothing in C:\Client is ever deleted. A disc whose file list isn't genuine is refused before anything is copied. If a file can't be read from the disc, the rest are still copied and running Sync again picks up what's missing.
- **Restart from C:\Client.** After the disc has been copied, the app offers to restart from its copy in C:\Client so the disc or USB stick can be ejected.
- **Extras on top of a disc.** Once the disc has been copied, if the computer can reach the server, Select Downloads opens with the disc's packages already ticked and marked "on disc — copied". Tick anything else you need and only those extras are downloaded, or choose **No extras** to finish. Without a connection, Sync stops after the copy and says nothing was downloaded.
- **Automatic deployment from a disc uses only the disc.** It copies the disc's files and downloads nothing.

### Changed
- **Downloading drivers and software into C:\Client needs an activated copy.** The app proves its license to the server for each download session without sending the license key. An unactivated copy is told to activate before any file is tried. App updates are not affected — every copy still updates itself.

## v3.10.0.1-1.41.3 (2026-10-06)

### Changed
- **The app tells the license site it's in use.** When an activated copy opens (at most twice a day), it quietly checks in, so the license holder's activations page shows when each computer last used the app and on which version. It sends a one-way fingerprint of the license key — never the key itself — and does nothing if the computer is offline.
- **Check for app updates from the header.** A small round arrow button beside the version opens a window that shows each step as it happens: the license confirmed with the activation server, the check for a newer version, the download (verified before it's kept), and finally **Restart now** / **Later** — or "You have the latest version". After an update the version in the header reads "Updated to v…" in green for that session.
- **Every copy updates, activated or not** — at startup and from the button. The license still unlocks the app's actions and the C:\Client downloads.

## v3.10.0.1-1.41.2 (2026-10-06)

### Fixed
- **Activation reaches the licensing server again, and the "install complete" email is sent again.** Since the app's code was obfuscated (September), its requests to the licensing server failed inside the app before they were sent: activation quietly fell back to offline activation, and the email the license holder gets when an Empower install finishes was never sent. Both work again.

## v3.10.0.1-1.41.1 (2026-10-06)

### Changed
- **Logs go into a folder named for this app:** `C:\Client\Logs\Empower Smart Deploy`, so each Smart Tools app's logs are kept apart. The folder can be read by administrators only (C:\Client is shared on the network). Logs this app wrote straight into `C:\Client\Logs` before are moved there the first time it opens.

