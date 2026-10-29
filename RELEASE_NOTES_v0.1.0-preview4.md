# PhoenixPanelX Windows v0.1.0-preview4 — Windows 11 real-hardware validation

**Experimental prerelease. Windows 11 Pro x64 real-hardware tests and private Windows CI tests completed.** Application Python/C source is kept PRIVATE.

## Fixes since preview3

- Fixed a unit test that incorrectly assumed disabling `psutil` also disables independent NVIDIA GPU telemetry. Verified on a GeForce RTX 3080 host.
- Added a 60-second retry/backoff when the LibreHardwareMonitor WMI namespace is absent. This avoids an unnecessary PowerShell failure on every update without inventing temperatures.
- Saving non-threshold settings no longer resets the 300-second thermal-warning cooldown. A deliberate threshold change still resets the relevant state.
- Saving with auto-start enabled refreshes the current portable EXE path to avoid stale entries after the executable is moved. A real Windows restart/sign-in was not performed.
- When the usable display height is too small for fixed gadget mode, scrolling is retained to prevent clipped cards.
- `--self-test` now works from the one-file, windowed EXE without external Python sources or a tests directory; it writes a small PASS/FAIL log to `%TEMP%\PhoenixPanelX-selftest.log`.

## Verified

- Physical Windows 11 Pro, build 26100, Ryzen 7 5700X3D, NVIDIA GeForce RTX 3080.
- Python 3.12.10, Tk GUI, psutil 7.2.2: **21 tests passed**, source self-test passed.
- GUI renders six cards; CPU/RAM/disk and NVIDIA name, load, real GPU temperature and watts displayed. Settings Save behavior and forced small-screen scrolling passed isolated GUI tests.
- A locally built PyInstaller executable launched a responsive GUI and its `--self-test` returned exit 0.
- **Private GitHub Actions Windows CI** built the distributed executable from commit `94f1506` and passed the source tests.
- The exact downloadable CI executable is a distinct binary from the locally built EXE. It was **separately downloaded and verified on the physical Windows 11 machine**: matching SHA-256, bundled self-test exit 0 with PASS log, GUI launched, main window responsive and captured. See the physical-hardware test report.

## Not verified

Physical motherboard Beep, audible alert playback, actual overtemperature sensor condition, real sign-in autostart behavior, AMD/Intel dedicated GPU sensors and Windows 10 runtime of this revision. CPU/board temperatures and CPU fan speeds require an appropriate LibreHardwareMonitor WMI provider, which was not installed on the test PC. Values without a source remain unavailable. Windows/Linux pixel-perfect parity, Linux tray, OS notifications and alert log parity are not yet implemented.

The EXE is unsigned and is not a safety-critical thermal controller. Only executable/checksum and public documents are published; source remains PRIVATE. See [Windows 11 physical test report](./WINDOWS11_PHYSICAL_TEST_REPORT_20261029.md).
