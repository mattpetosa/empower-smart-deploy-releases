# Release history

Newest first. Downloads are on the [Releases](https://github.com/mattpetosa/empower-smart-deploy-releases/releases) page.

## v3.10.0.1-1.44.1 (2026-10-11)

### Fixed
- **A license key the licensing server refuses is no longer saved.** It used to replace the previous key and keep working while the computer was offline.
- **The settings folder in ProgramData is now open to administrators only.** It holds the license key, and the SYS password from the install options for a short time while the app restarts to update during a deployment; other users on the computer could read it before.
- **Downloads no longer re-check every file each time.** The record of files already verified couldn't be read back, so each sync re-checked the whole download folder.

### Removed
- **Unpacking a Client.zip found next to the app when the server can't be reached.** That came from the old pre-built offline ISO and wasn't signed. For a computer without internet, make an offline disc with the website's ISO builder, whose file list is signed and checked before anything is copied.

## v3.10.0.1-1.44.0 (2026-10-10)

### Added
- **Prep keeps the machine and its devices awake.** Three new Prep System steps, each checked after it is set:
  - **Never sleep:** sleep, hibernate and hard-disk power-down are set to never, and USB selective suspend and PCI Express power saving are turned off, on mains and battery power, whichever power plan is active.
  - **Devices can't be turned off to save power:** "Allow the computer to turn off this device to save power" is unticked on every network adapter, USB controller and hub, and USB device (instruments, serial adapters, dongles).
  - **Network adapter power saving off:** Energy-Efficient Ethernet, Green Ethernet and similar adapter features are turned off where the adapter offers them. Takes effect after the restart.
- **Check System reports all three** without changing anything, like the rest of the Prep checks.
- **Post-Install sets up Waters Database Manager's backups on the database server.** Done the same way WDM's own pages do it, and checked afterwards:
  - **Backup location** set to the DRBackups folder, with write access for the Oracle jobs account. The existing size limit is kept, with a warning if it looks too small for two full backups.
  - **Weekly full backup** on Sundays at 02:00 and a **nightly incremental** every day at 23:00, both enabled.
  - **First full backup** run straight away and checked: the backup, the control file and both wallet files must be in the new backup folder. If an automatic deployment has to ask for the SYS password on the Completed page, the backup is started there and its result shows in WDM.
  - **If this app had to create the Oracle jobs account**, WDM is given its new password first, so the backup jobs can log on. If the account can't be created, or WDM can't be given the password, it's an error on the Completed page that says what to set by hand.
  - **WDM's OS Job user is checked** against this computer's name, with a warning and the fix if the computer was renamed.
- **The database SYS password can be entered with the server install options** (optional, not kept after the deployment). Otherwise Post-Install asks for it, or, in an automatic deployment, asks on the Completed page.

## v3.10.0.1-1.43.4 (2026-10-08)

### Changed
- **The window stays responsive while a step runs.** Each step does its work in the background, so the window can be moved and Cancel answers straight away while a step is busy.

## v3.10.0.1-1.43.3 (2026-10-08)

### Fixed
- **The ring on the running step turns.** Since 1.43.0 it stood still on many computers while the step was running. It now turns once a second for as long as the step runs.

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
