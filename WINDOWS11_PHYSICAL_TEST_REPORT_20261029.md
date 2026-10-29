# Windows 11 physical hardware verification — 2026-10-29

## Machine and build
- Computer: FESTICE-MAIN-PC
- OS: **Microsoft Windows 11 Pro x64** (10.0.26100)
- CPU: **AMD Ryzen 7 5700X3D**
- GPU: **NVIDIA GeForce RTX 3080** with working NVIDIA driver and `nvidia-smi.exe`
- Memory: approximately 64 GiB
- Development environment: Python 3.12.10, psutil 7.2.2, PyInstaller 6.16.0
- Revision: PRIVATE source commit `94f1506`

## Executed tests

1. Downloaded the exact preview3 EXE published on GitHub; verified its SHA-256 checksum against the release.
2. Launched preview3 on the real Windows 11 machine; confirmed a responsive, titled PhoenixPanelX desktop window and captured the app window. The six-card dashboard displayed CPU usage, GPU load, NVIDIA GPU temperature/power, RAM, and system-drive usage.
3. Found that one old synthetic unit test failed on a real NVIDIA system because it incorrectly assumed disabling psutil would disable every sensor backend. Added an explicit NVIDIA backend mock. The full suite then passed.
4. Installed Python 3.12 for the current user, created an isolated development virtual environment with psutil/PyInstaller, and ran source checks: **21 unit tests passed and source self-test passed**.
5. Tested a source-GUI settings dialog with configuration and Run-key writes mocked, including save, auto-start path update request, preserving alert cooldown after a theme/settings save, and scrollability on a simulated low-resolution screen.
6. Queried actual live hardware through `nvidia-smi`: GeForce RTX 3080 model, usage, temperature, and watts returned measurable values. CPU model, RAM and storage were obtained. CPU temperature/board temperature/CPU fan remained unavailable because LibreHardwareMonitor WMI was not installed. **Do not interpret unavailable as safe temperatures.**
7. Fixed repeated PowerShell WMI failures when the LHM namespace is absent. On the real Windows 11 machine, the first sampling call took approximately **1.60 s**; subsequent calls took about **0.045–0.047 s** in the short validation loop. A 60-second backoff allows later retry.
8. Built a local `--onefile --windowed` executable on actual Windows 11, ran its **bundled frozen `--self-test` successfully (exit 0)**, and launched the GUI with a responsive titled window; captured the new GUI window.
9. Private GitHub Actions Windows build for the same committed source succeeded. Downloaded the **exact released preview4 EXE** onto the physical Windows 11 machine and verified SHA-256 `8bf80064f1bf9202debabed726f96a61ea38b05c998c57675919527acd394214`. Ran its bundled frozen `--self-test` (exit code **0**, PASS log), launched its GUI and confirmed the responsive PhoenixPanelX window, and captured its application window. The published executable itself has therefore passed physical Windows 11 startup and offline smoke tests.

## Fixes in preview4
- Synthetic tests are now independent of installed NVIDIA drivers.
- Failed LHM namespace probes use bounded, timed retries instead of delaying every sensor sample.
- Existing auto-start registry settings are refreshed to the currently running EXE path when the user saves preferences with auto-start enabled.
- Changing unrelated settings no longer discards the thermal alert cooldown history; a threshold change intentionally creates fresh alert state.
- Automatic scrollbar fallback prevents clipping on small/high-DPI work areas.
- One-file EXEs use a bundled, non-destructive offline self-test rather than relying on absent source-level test files.

## Remaining limitations
- CPU/core/board temperature and fan sensor readings were not available because there was no LibreHardwareMonitor WMI provider on this machine. No privileged kernel driver was installed.
- Physical motherboard Beep, audible sound warning, real over-temperature behavior, and a real Windows sign-in/restart with auto-start have not been tested.
- UI appearance is based on the Linux classic theme, but the implementations use different GUI frameworks and are not pixel-perfect equivalents.
- Linux tray integration, OS notification history, and some detailed hardware statistics are not implemented on Windows.
- The preview EXE is unsigned and intended for informational monitoring, not hardware protection.
- Application Python/C source remains PRIVATE. Binary packaging does not prevent reverse engineering.
