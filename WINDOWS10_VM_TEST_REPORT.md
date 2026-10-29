# Windows 10 VM validation — 2026-10-29

## Tested environment
- Device: DESKTOP-B5MC8T1
- OS: Microsoft Windows 10 Home x64
- Virtualization: VMware virtual machine (VMware7,1)
- CPU guest identification: AMD Ryzen 7 5700X3D
- Display: 1715 × 920 pixels
- GPU: VMware SVGA 3D (no hardware temperature or GPU power telemetry exposed)
- Python: 3.12.10; Tcl/Tk: 8.6.15; psutil: 7.2.2; PyInstaller: 6.16.0
- Development/tools installed in VM: Python 3.12, Git, psutil, PyInstaller; Node.js and Desktop Commander were already installed
- Source: private repository at commit `21d45b2`

## Validation executed
| Check | Result |
| --- | --- |
| Python syntax compilation | Passed |
| Cross-platform unit tests | 17 passed on Windows 10 VM |
| Linux-hosted synthetic unit tests | 17 discovered, 16 passed and 1 Tkinter-dependent test skipped (not installed on Linux test host) |
| Native Tk initialization | Passed (Tk 8.6.15) |
| Dashboard startup and clean exit | Passed |
| Window narrowed to 360 × 400 | Passed after fixing scrollbar layout; horizontal scrollbar visible |
| CPU model/usage and RAM/disk | Returned values in VM |
| CPU/GPU physical temperatures, fan, GPU power | Not available from this VMware guest (displayed unavailable, not guessed) |
| Model discovery failure retry | Unit-tested |
| AMD/Intel GPU single-adapter source binding | Synthetic tests passed; physical AMD/Intel LHM hardware not tested |
| Frozen executable autostart command | Unit-tested; actual Windows sign-in after setting not tested |
| Physical motherboard Beep, audible system sound | Not tested; VM hardware cannot verify physical speaker |
| Windows 11 physical installation | Not tested |
| Tray, OS desktop notification, warning-event logging | Not implemented to Linux parity yet |
| Installer/code-signing | Neither included |

## Functional corrections
1. Fixed packaged EXE auto-start command targeting a nonexistent source `run.py`.
2. Added working horizontal scroll on narrow windows and fixed geometry layout.
3. Added safe single-GPU LibreHardwareMonitor readings for AMD/Intel (when device identity matches).
4. Added retry for failed CPU/GPU model discovery after 60 seconds.
5. Added platform-portable packaging and sensor regression tests.

The user-visible dashboard follows the Linux classic six-card theme. Pixel-identical graphics or complete Linux feature parity **is not claimed**. There are known differences between GTK/Cairo and Tkinter.

## Distribution
Build outputs originate from a **private GitHub Actions workflow**. Do not add Python, C, scripts or source archives to the public release repository. The published EXE is unsigned; the public release checksum must be taken from the exact published artifact. A separately built Windows 10 local EXE was exercised in the VM, not automatically the same binary as the CI artifact.

Do not disable Windows Update or security protection just to run this app. Windows 10 Home is out of standard support. Temperature alerts are informational and cannot replace hardware safety features.
