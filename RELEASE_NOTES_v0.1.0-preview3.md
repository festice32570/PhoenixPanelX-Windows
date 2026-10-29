# PhoenixPanelX Windows v0.1.0-preview3 — thermal alert cooldown hotfix

**Experimental Windows build. Source remains PRIVATE.** This binary was built from private commit `778be87` by the Windows GitHub Actions runner.

## Fixes
- A temporary invalid/missing temperature sensor sample now resets the current visible hot/debounce state without clearing the last notification time. This prevents 5-minute cooldown bypass and rapid duplicate alerts. A real cool-down (5°C below the threshold) still rearms warnings.
- Added two specific regression tests for missing sensor samples and real cooling.

## Validation
- 19 Python unit/regression tests discovered and passed in private GitHub Actions Windows build. The Windows CI binary was built successfully as a PE32+ x86_64 GUI executable.
- Portable Linux-side test execution: 18 passed, 1 Tkinter graphical test skipped on that runner.
- **This preview3 build has NOT been rerun on the Windows 10 VMware VM** because the VM's remote command connection stopped responding. The preceding preview2 had native VM tests; do not extend those results to this exact executable.
- Windows 11 physical machine testing, real speaker/beep audibility, startup persistence and AMD/Intel hardware sensor accuracy remain unverified.

## Known Linux parity gaps
Windows uses Tkinter rather than Linux GTK/Cairo. Exact visual parity is not guaranteed. System tray behavior, desktop notifications and warning history from the Linux edition are not yet implemented on Windows. Neither version should be treated as a hardware safety device.

## Privacy and licensing
This release contains a compiled executable and checksum only; Python/C application source is in a separate PRIVATE repository and is not included here. The binary is unsigned, experimental, and may be reverse engineered.
