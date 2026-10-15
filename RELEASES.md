# Release history

Newest first. Downloads are on the [Releases](https://github.com/mattpetosa/empower-smart-deploy-releases/releases) page.

## v3.10.0.1-1.46.0 (2026-10-14)

### Added
- **Install Empower checks the account before a server install.** It runs the same administrator check Oracle's database build uses (admin rights through a domain group such as Domain Admins count). If Oracle would refuse the account, the install used to hang with no database; now you're offered to add the account to the computer's Administrators group before anything starts (in an automatic deployment, on the first screen).
- **Install Empower watches Oracle's database build.** If the build fails, Smart Deploy stops Waters' installer within seconds and shows Oracle's reason and what to do, instead of waiting for hours. If the installer looks stuck (sqlplus waiting with no database behind it) or runs longer than two hours, the step says so.
- **Post-Install installs Waters Database Manager if it's missing.** On the database server, a new step installs WDM from the Empower media the same way the Empower installer does, for when the main install stopped before reaching it. It's skipped when WDM is already installed.
- **Post-Install tests the connection to each Empower database.** Every name in the machine's tnsnames.ora is tried through the Empower Oracle client, and the step says plainly what failed: the name or server couldn't be found, nothing is listening on port 1521, the server answered but the Empower database isn't open, or a firewall timed it out. It uses a made-up login, so no real account or password is needed.
- **Clients and LAC/E machines can connect to several databases.** The install form's **Databases** box takes `NAME = SERVER` pairs, for example `PROD = EMPSRV01; VAL = EMPSRV02; DEV = EMPSRV03`. NAME is what Empower shows at login, and a server on its own is named after itself. A pasted `\\SERVER\Client$` path works too.

### Changed
- **Clients write their own tnsnames.ora instead of copying it from the database server.** Each database gets an entry with Empower's standard port (1521) and database (EMPPDB1), so a client no longer needs the server to be finished or its Client$ share to be reachable. A tnsnames.ora already on the machine keeps its entries, and is backed up before anything is added. If a name is already there pointing at a different server, it isn't changed and the step says so.
- **IQ folders are named after the organization and the machine type,** for example `C:\Client\Acme_ServerIQ`, `Acme_ClientIQ` or `Acme_LaceIQ`, using the install form's Username / Organization. Prep asks for both when it's run on its own (an automatic deployment already has them). Anything in an older `C:\Client\IQ` folder is moved into the new one.
- **Install Empower reports "Full installation failed" from Waters' log as a failure** and points to the MSI and Oracle logs.
- **Post-Install's WDM backup steps try Waters' default SYS password first.** It's read from the Empower media, so on a fresh install the backup location and schedules are set without asking (and an automatic deployment no longer leaves them for the Completed page). You're asked for the SYS password only if it has been changed.

## v3.10.0.1-1.45.0 (2026-10-13)

### Changed
- **Built on the shared Smart Tools engine, the same one Empower Smart Recovery uses.** Steps, prompts, activation, updates and run logs now work the same way in both apps.
- **Prompts no longer replace each other.** A second prompt opens on top of the first, and the first one comes back once the second is answered.
- **Cancelling a prompt ends the run as cancelled.** If you cancel a question a step needs (time zone, .NET version, install type or options, machine type, Oracle jobs account, Waters Driver Pack, activation), that step shows "Skipped", the steps after it show "Not run — cancelled", and the run log records the run as cancelled instead of reporting errors. A network timeout inside a step now fails that step instead of ending the run as if you had cancelled.
- **Pressing Start rebuilds the checklist first,** so it reflects the computer as it is at that moment (for example, Install Drivers looks in C:\Client\Drivers again).
- **Activation.** The activation prompt links to the license page ("Open the license page"); request a key there and choose Deploy. A key for another Smart Tools app is now explained as such when it doesn't validate.
- **Check for updates keeps checking even when the license can't be confirmed.** The check for a newer version still runs, and you can enter a new key at the end. Its steps are now Activation server, Check for a newer version, Download and Ready.
- **The "update ready" prompt waits until a running task finishes.** It no longer interrupts a run; the Relaunch button at the top offers the update afterwards.
- **Updates are applied by a safer swap.** The new version is renamed into place, and the previous one is put back if that fails. Updates are no longer downloaded into C:\Client\Updates.
- **Roll back lists only builds that carry a release signature, newest first.** If a build can't be downloaded or verified, one short message says so.
- **The window fits small screens,** such as 1024×768 Remote Desktop sessions. After an update relaunch it is restored and brought to the front.
- **The step list follows the running step.** Scroll up and it stays where you are; it follows again when you scroll back to the bottom or the next step starts.
- **Run logs:** small wording changes in the header lines.

### Added
- **Unexpected errors are written to a crash log:** `C:\Client\Logs\Empower Smart Deploy\ESD_crash_<time>.log`.

### Fixed
- **Activating from the top of the window no longer saves a key the licensing server refuses.** (1.44.1 fixed this for the download step only.)
- **Single-button notices show one OK button,** without an empty second button beside it.

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
