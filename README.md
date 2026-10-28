# PhoenixPanelX — Windows 11 (experimental)

**Windows 11版は実機未検証です。動作確認は後日行う予定です。**

PhoenixPanelXのWindows 11向けバイナリ配布リポジトリです。**ソースコードは公開していません。** GitHubの自動生成するSource codeアーカイブにもアプリソースは含まれません。

## 日本語

- [Releases](../../releases) から `PhoenixPanelX-Windows-11-UNTESTED.exe` を取得できます。
- Windows 11実機での画面表示、CPU・GPU・温度・音声・自動起動は **未確認** です。Windowsビルド環境での自動テスト成功は、Windows 11での正常動作を保証しません。
- Linux版のクラシックレイアウトを元にWindowsネイティブのTkinterで移植しました。CPU/GPU名、使用率、メモリ、ストレージ、温度警告、全体カラー、日英切替などを試験実装しています。
- 実測できない温度・消費電力は「取得不可」にします。NVIDIA GPUではnvidia-smiを利用できる場合があります。CPU温度は別途LibreHardwareMonitorがセンサーを公開している場合などに限られます。AMD/Intel GPUの詳細センサーは未対応です。
- 管理者権限は要求しません。自動起動は初期OFF、ONの場合でも現在のユーザーのRunキーだけを使用します。BeepはWindowsの音声APIによるベストエフォートで、マザーボードの物理Beepを保証しません。
- **コード署名なし**。身元・ハッシュを確認し、セキュリティ警告を無理に回避しないでください。ハードウェア保護機能の代わりにしないでください。

## English

**UNTESTED on real Windows 11 hardware. Actual testing will be carried out later.** GUI, sensor readings, audio and autostart are not yet verified. This is an experimental binary preview, without code signing. The public repository contains documentation and binaries only; application source remains private. Unsupported telemetry shows unavailable. No administrator privileges are needed. Use at your own risk.

Linux version: [PhoenixPanelX-Linux](https://github.com/festice32570/PhoenixPanelX-Linux).
