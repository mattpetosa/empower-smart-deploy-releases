# Release history

Newest first. Downloads are on the [Releases](https://github.com/mattpetosa/empower-smart-deploy-releases/releases) page.

## v3.10.0.1-1.41.2 (2026-10-06)

### Fixed
- **Activation reaches the licensing server again, and the "install complete" email is sent again.** Since the app's code was obfuscated (September), its requests to the licensing server failed inside the app before they were sent: activation quietly fell back to offline activation, and the email the license holder gets when an Empower install finishes was never sent. Both work again.

## v3.10.0.1-1.41.1 (2026-10-06)

### Changed
- **Logs go into a folder named for this app:** `C:\Client\Logs\Empower Smart Deploy`, so each Smart Tools app's logs are kept apart. The folder can be read by administrators only (C:\Client is shared on the network). Logs this app wrote straight into `C:\Client\Logs` before are moved there the first time it opens.

