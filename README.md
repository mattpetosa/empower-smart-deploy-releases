# Empower Smart Deploy

**Automated Empower 3 installer** — a windowed prep-and-deploy tool for Waters Empower 3 workstations, LAC/E devices, and database servers. It runs the full pre-install prep (locale, services, UAC/firewall, power, DCOM, folders) as a verified checklist and drives Waters' silent install, then harvests IQ evidence.

> This is the **public distribution** repository — it hosts downloads and issue tracking only. The application source is maintained privately.

## Download

Get the latest build from the [**Releases**](../../releases/latest) page, or from the project website:

- **https://empower.mhpwebserver.com** — Lite Web Installer (self-updating) and Offline Installer (ISO)

Each release here is the same signed, single-file Windows build the app self-updates to.

## Report an issue

Found a bug or have a request? [**Open an issue**](../../issues/new). Please include the app version (shown in the title bar), the machine role (Client / LAC-E / Server), and the relevant lines from `C:\Client\Logs\` or `C:\Windows\Empower.log`.

## Requirements

- Windows 10/11 or Windows Server 2016+ (x64)
- Administrator rights (the app self-elevates)
- Empower 3.10.0 install media (synced into `C:\Client` by the app)
