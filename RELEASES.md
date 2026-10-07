# Release history

Newest first. Downloads are on the [Releases](https://github.com/mattpetosa/empower-smart-deploy-releases/releases) page.

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

