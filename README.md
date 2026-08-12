<p align="center">
  <img src="assets/app-logo-256.png" alt="Empower Smart Deploy" width="128" height="128">
</p>

<h1 align="center">Empower Smart Deploy</h1>

<p align="center"><strong>Automated Empower 3 installer</strong> — a single, windowed tool that preps a Windows machine for Waters&nbsp;Empower&nbsp;3, runs the unattended install, and harvests the IQ evidence, without hand-editing a single registry key or clicking through a wizard.</p>

<p align="center">
  <a href="../../releases/latest"><img alt="Latest release" src="https://img.shields.io/github/v/release/mattpetosa/empower-smart-deploy-releases?label=download&color=5EEAD4"></a>
  &nbsp;·&nbsp; <a href="https://empower.mhpwebserver.com">Website</a>
  &nbsp;·&nbsp; <a href="../../issues/new">Report an issue</a>
</p>

---

> This is the **public distribution** repo — downloads and issue tracking only. The application source is maintained privately.

## Download

- **[Latest release &raquo;](../../releases/latest)** — download the `.zip`; it contains `Empower Smart Deploy.exe`. It's a signed, single-file Windows build that needs no installer.
- Or get it from the project website, where it **self-updates**: **https://empower.mhpwebserver.com**
  - **Lite Web Installer** — the exe; updates itself and syncs the install payload on demand.
  - **Offline Installer (ISO)** — the exe plus the full payload bundled for air-gapped machines.

Run it as an administrator (it self-elevates). No .NET install required — the runtime is bundled.

## How it works

The home screen is a grid of **actions**. Open one and you see its **checklist** — every change it will make, spelled out — then press **Start**. Each step runs and reports its own result, so nothing happens silently:

| Indicator | Meaning |
|:--:|:--|
| ⟳ spinning ring | the step is running |
| ✅ green check | done **and verified** — the app reads the setting back to confirm it actually took |
| ⚠️ amber check | applied with a caveat, or something worth noting (e.g. a third-party antivirus it can't disable) |
| ❌ red ✕ | the step failed — with the reason |

A per-step and overall **progress bar** track long operations, and every run is written to a timestamped log in **`C:\Client\Logs`** so the outcome is auditable after the fact. Because every change is verified by read-back rather than assumed, a green run is real evidence the machine is configured — not just that a command was issued.

## What it automates

Ten actions cover the full Waters Empower 3 prep-and-deploy workflow.

### 🔎 Check System &nbsp;·&nbsp; *read-only*
Scans the machine and **changes nothing** — safe to run any time. It first detects the **deployment role** (see [Deployment roles](#deployment-roles)), audits the hardware and OS against the **Waters Empower 3.10.0 Recommended Specifications** (OS edition, CPU, memory, drive layout, .NET Framework, network, computer-name rules), then audits *every setting that Prep would apply* plus the Post-Install DCOM/shares — reporting each as **already configured** or **Prep/Post-Install would change it**. Use it to confirm a machine was prepped correctly without touching it.

### 🛠️ Prep System for Empower
The core prep pass. In one verified run it:

- **Creates the `C:\Client` folder skeleton** — `Drivers`, `IQ`, `Licenses`, `Logs`, `Projects`, `Software`
- **Sets the system locale** to en-US and the **time zone** (Eastern / Central / Mountain / Pacific)
- **Enables and starts the discovery services** Empower relies on — Function Discovery Resource Publication, UPnP Device Host, SSDP Discovery, and DNS Client
- **Disables** User Account Control, scheduled defrag, the Windows Firewall, IPv6 components, and NTFS last-access updates
- **Enables** Network Discovery firewall rules and sets **DEP to AlwaysOn**
- Applies the **High-Performance power plan** and disables hibernation
- Keeps the **classic 260-character path limit**, sets **Windows Update to a controlled policy**, and makes **Microsoft Print to PDF** the default printer
- Writes an **IQ inventory** to `C:\Client\IQ` — `msinfo32` report, ipconfig, serial, systeminfo and environment

> ⚠️ Prep disables the firewall and antivirus. It offers a **Restart** afterward, since some changes (locale, UAC, DEP, IPv6) only fully apply on reboot.

### 💠 Install .NET Offline
Launches the bundled offline **.NET Framework** installer — choose 4.8, 4.5, or 3.5.

### 🌐 Install Google Chrome
Installs Chrome, then opens the Waters Database Manager login and the waters.com activation page.

### 📀 Install Empower &nbsp;·&nbsp; *silent*
Mounts the Empower ISO from `C:\Client\Software` and runs Waters' own **silent "Push Install"** with a response file tailored to the detected role — **no clicking through the installer**. It waits for the install to complete (these run 30–90 minutes) and reads the installer's own log back to report whether it actually succeeded.

### 🔧 Post-Install Setup
Everything Empower needs *after* the install:

- Sets the **`oraclejobs`** account password to never expire
- Applies Empower 3's documented **DCOM configuration** — machine-wide access/launch permissions and the per-app **WatersService** grants
- Creates the **`Client$`** and **`Waters_Projects$`** shares with the correct permissions
- Creates the **`DRBackups`** folder next to the CDS archive
- Copies **`tnsnames.ora`** and the Empower install log (as **`Empower.txt`**) into `C:\Client\IQ`
- Runs Waters' **file-verification checksum** and saves the result to `C:\Client\IQ`

### 🧾 Get Verify Files
Runs the file-verification checksum on its own and saves the checksum report to `C:\Client\IQ`.

### 🛰️ Network & Port Check
Checks that the Empower and Oracle **ports** are listening — `1521`, `8080`, `8181`, `2100`, `2300`, `3000`, `4000`, `5000`, `6000` — verifies internet connectivity, and can add the **WDM firewall rules for port 8181** on request.

### ⬇️ Update & Download
Verifies the license, self-updates the app to the latest build, and **syncs the install payload** from the backend into `C:\Client` — downloading only files that are missing or changed (verified by SHA-256), and repairing anything altered on disk.

### 🧹 Cleanup
Removes the app's own footprint — activation, config, staged updates, and its registry entries — while **leaving `C:\Client` intact**.

## Deployment roles

Check System and the Empower install adapt to the machine's role, detected automatically:

| Role | Detected when | Basis |
|:--|:--|:--|
| **Client / LAC-E** | Windows 10 / 11 | Workstation or acquisition node |
| **Database Server** | Windows Server with 4+ fixed drives | Full Empower database host |
| **Citrix / RDS** | Windows Server with 1–3 fixed drives | Terminal-server client host |

Hardware and OS checks are graded against the **Waters Empower 3.10.0 Recommended Specifications** for whichever role applies.

## Evidence & logs

- **`C:\Client\IQ`** — installation-qualification evidence: system inventory, `tnsnames.ora`, `Empower.txt`, and the verification checksum.
- **`C:\Client\Logs`** — a timestamped log of every action run (per-step results and the final verdict).

## Requirements

- Windows 10 / 11, or Windows Server 2016+ (x64)
- Administrator rights (the app self-elevates)
- Empower 3.10.0 install media — synced into `C:\Client` by **Update & Download**, or bundled in the Offline ISO

## Report an issue

Found a bug or have a request? **[Open an issue »](../../issues/new)** and include:

- the **app version** (shown in the title bar),
- the **machine role** (Client / LAC-E / Server), and
- the relevant lines from **`C:\Client\Logs`** or **`C:\Windows\Empower.log`**.
