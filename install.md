# Installation

HeartBeatAgents ships as a desktop application for macOS, Windows, and
Linux. The app embeds a setup wizard that pulls its container runtime,
generates secrets, and provisions the database on first launch.

## Prerequisites

| Resource | Minimum | Recommended |
| --- | --- | --- |
| CPU | 4 cores | 8 cores |
| Memory | 8 GB | 16 GB |
| Disk | 20 GB free | 40 GB |
| Docker | Docker Desktop or Docker Engine, running before launch | Same |

A working Docker daemon is required at first run; the installer will
verify it during the preflight phase.

## macOS

1. Open the latest [release](https://github.com/MosaicSingularity/heartbeatagents-releases/releases/latest)
   and download the `.dmg` matching your CPU
   (`HeartBeatAgents-<version>-arm64.dmg` for Apple Silicon, the
   no-suffix `.dmg` for Intel) along with `checksums.sha256`.
2. Verify the download:
   ```bash
   cd ~/Downloads
   shasum -a 256 -c checksums.sha256 --ignore-missing
   ```
3. Mount the `.dmg`, drag `HeartBeatAgents.app` into `Applications`,
   eject the disk image.
4. Launch from `/Applications`. The application is signed and notarized
   by Mosaic Singularity Inc.; macOS Gatekeeper will allow it without
   intervention. Follow the setup wizard.

## Windows

1. Download `HeartBeatAgents-Setup-<version>.exe` and `checksums.sha256`
   from the latest [release](https://github.com/MosaicSingularity/heartbeatagents-releases/releases/latest).
2. Verify the download in PowerShell:
   ```powershell
   cd $env:USERPROFILE\Downloads
   $actual = (Get-FileHash HeartBeatAgents-Setup-*.exe -Algorithm SHA256).Hash.ToLower()
   $expected = (Select-String -Path checksums.sha256 -Pattern 'HeartBeatAgents-Setup' |
                Select-Object -First 1).Line.Split()[0]
   if ($actual -eq $expected) { 'OK' } else { 'MISMATCH' }
   ```
3. Run the installer. SmartScreen may ask for confirmation on early
   builds — the binary is signed by Mosaic Singularity Inc. via SSL.com
   EV; choose **Run anyway**. Reputation accrues automatically as the
   release matures.
4. Complete the NSIS wizard and launch HeartBeatAgents from the Start
   menu.

## Linux

Three install paths are published with each release:

| Distribution family | Artifact |
| --- | --- |
| Debian, Ubuntu | `heartbeat-agents-desktop_<version>_<arch>.deb` |
| Fedora, RHEL, Rocky, Alma | `heartbeat-agents-desktop-<version>.<arch>.rpm` |
| Universal | `HeartBeatAgents-<version>.AppImage` |

Both `amd64` (`x86_64`) and `arm64` (`aarch64`) are available for the
package formats. Verify your architecture with `uname -m`.

```bash
# Debian / Ubuntu
sudo apt-get install -y ./heartbeat-agents-desktop_<version>_amd64.deb

# Fedora / RHEL
sudo dnf install -y ./heartbeat-agents-desktop-<version>.x86_64.rpm

# AppImage (universal)
chmod +x HeartBeatAgents-<version>.AppImage
./HeartBeatAgents-<version>.AppImage
```

After install, launch HeartBeatAgents from your application menu or
run `heartbeat-agents-desktop` from a terminal.

## First-run setup

The wizard runs five phases — preflight, configure, download,
services, database — in roughly two to five minutes on a typical
broadband connection. Image pulls dominate the wall time; subsequent
launches are instant.

When the wizard completes, choose **Open Dashboard** to sign in.

## Updates

The application checks for new releases automatically and signals
availability through the system tray. Choose **Restart to Update** to
apply; the running services and your data are preserved across the
upgrade.

## Documentation and Support

Full documentation, configuration references, and operational guides
live at <https://www.heartbeatagents.com/docs>.
