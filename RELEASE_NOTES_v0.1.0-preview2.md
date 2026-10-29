# PhoenixPanelX Windows v0.1.0-preview2 — Windows 10 VM validation

**Experimental prerelease. Windows 10 Home x64 VMware virtual machine test completed; Windows 11 physical hardware and motherboard beep audio remain unverified.**

## Changes since preview1
- Fixed portable EXE autostart command to point to the EXE rather than a missing source file.
- Corrected window layout to provide working horizontal scrolling in narrow windows (tested at 360 × 400).
- Added AMD/Intel GPU telemetry support only when LibreHardwareMonitor WMI exposes a uniquely identified single adapter; all ambiguous sensor values remain unavailable.
- Added retry after transient Windows CPU/GPU model discovery failures.
- Added regression tests for packaging, sensor mapping, and retry.

## Verification
- 17 unit tests passed on Windows 10 Home x64 VM; native Tk 8.6.15 GUI started and exited.
- CPU name and usage, memory and drive statistics were read on the VM.
- VMware SVGA 3D exposes no reliable physical GPU temperature or power, and CPU temperature was unavailable.
- A separately built Windows 10 VM EXE was run successfully.
- The **downloadable executable attached to this release was built by a private GitHub Actions Windows runner from the same committed source**, not by the VM. Windows 10 VM runtime testing of this exact CI-generated binary was not performed.
- Windows 11 real hardware, warning sound and physical motherboard Beep, login-time autostart persistence, and AMD/Intel real sensor hardware remain unverified.

## Known parity differences
The Windows dashboard uses Tkinter and follows the Linux classic six-card design; pixel-perfect equivalence and full Linux feature parity are not yet achieved. Linux tray integration, OS notifications/alert-event logs and additional hardware telemetry are not yet implemented on Windows.

**Only binaries and public documents are in this public repository. Source remains in a separate private repository.** The EXE is unsigned. This is an informational hardware monitor, not a replacement for hardware thermal protection.

See [Windows 10 VM test report](./WINDOWS10_VM_TEST_REPORT.md) and [README](./README.md).
