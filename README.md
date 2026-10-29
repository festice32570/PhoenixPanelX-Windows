# PhoenixPanelX — Windows preview (Windows 11 physical hardware tested)

**Windows 11 Pro x64の実機（Ryzen 7 5700X3D / GeForce RTX 3080）でテストしました。Windows 10 HomeのVMでも旧版を検証済みです。** 配布は実験的なプレビューです。

本リポジトリには **アプリケーションのソースコードを公開しません**。GitHub が自動作成する Source code アーカイブには説明文のみ含まれ、アプリのPythonソースは含まれません。ただしバイナリからの解析を完全に防げるわけではありません。

## ダウンロード・起動

[Releases](../../releases) から最新の `PhoenixPanelX-Windows-preview4.exe` を入手し、SHA-256を照合してください。インストーラー不要の実行ファイルです。コード署名はありません。出所不明の警告を安易に回避しないでください。

Windows 10 / Windows 11を対象とし、**Windows 11 Pro実機で21件のテストとGUI/RTX 3080センサー取得を確認**しました。Windows 10は標準サポート終了済みのため、必要な保護対策のある検証環境で利用してください。

## Linux版との対応・制限

Linux版のクラシックな6カード構成（CPU/GPU使用率・温度、メモリ、ストレージ、GPU実測電力、冷却）を基に移植しました。日本語・英語、6配色、温度しきい値、警告音選択、固定表示・縦横スクロール、自動起動の設定を備えます。

- CPU使用率・メモリ・ストレージはpsutil、モデル名はWindows CIMを利用します。
- NVIDIA GPUは対応した `nvidia-smi` が利用できる場合にのみ計測します。
- AMD/Intel GPUは、**LibreHardwareMonitor WMIが稼働し、モデルとの対応を単一GPUとして確認できる場合だけ**一部計測します。複数GPUでの識別が曖昧な場合、値を混ぜずに「取得不可」とします。
- CPU温度・基板温度・CPUファンはLibreHardwareMonitor WMIなど、対応する取得元が必要です。取得できない場合は「取得不可」と表示し、負荷から温度を推測しません。
- 温度警告は2回連続の閾値到達で発報、5°Cのヒステリシス、300秒後の再通知判定を備えます。利用者のハードウェア保護の代用にはなりません。
- 「Beep」はWindowsの`winsound.Beep`です。物理マザーボードスピーカーの鳴動は保証できません。VMでの物理Beep、実際の警告音の聞こえ方は未検証です。通常動作に管理者権限は不要です。
- 自動起動はユーザー単位のRunキーで初期OFF。ポータブルEXEでの起動パス不具合は修正済みですが、Windows再ログオンでの実動作は未検証です。
- **Linux版との完全な機能・外観一致はまだ未達**です。Windows版はTkinter、Linux版はGTK/Cairoを使用します。Windows版にはLinuxのトレイ常駐、OSデスクトップ通知・警告ログ、詳細なディスク転送レート等の未移植機能があります。

## 検証結果

2026年10月29日：Windows 10 Home x64 / VMware 7,1 / Python 3.12.10 / Tk 8.6.15 / psutil 7.2.2 / PyInstaller 6.16.0。17件のテスト成功。GUIの起動・終了、画面幅360px時の横スクロール、VMのCPU・メモリ・ディスク取得、センサー欠損時の表示を確認しました。独立したCIのWindowsビルドと同じソースですが、CIバイナリのWindows 10 VMでの再検証は未実施です。

[詳細な検証・制限事項](./WINDOWS10_VM_TEST_REPORT.md)

Linux版：[PhoenixPanelX-Linux](https://github.com/festice32570/PhoenixPanelX-Linux)

## English

**Experimental Windows binary preview. Tested on actual Windows 11 Pro x64 hardware (Ryzen 7 5700X3D / RTX 3080) on October 29, 2026, as well as an earlier Windows 10 Home VMware VM.** The six-card Linux-classic-inspired dashboard supports bilingual UI, color themes, CPU/GPU monitoring when real sensors are available, threshold alerts, sound options and scrollable viewing. Actual motherboard Beep audio, reboot/autostart persistence, physical CPU temperature sensors are not available without a compatible provider. Linux tray, desktop notifications and event history are not yet ported; exact parity is not claimed. No code signing. The public repository contains documentation and binaries only; the application source stays private.

## 2026-10-29 追加修正（preview3・履歴）

温度センサーの欠損値が発生した際に、5分間の温度警告再通知クールダウンが誤って解除される不具合を修正しました。19件のテストを発見し、Linux上で18件成功、Tk依存の1件はスキップとなりました。非公開GitHub ActionsのWindowsランナーによるビルド・テストも実施します。**今回の修正版バイナリはWindows 10 VMへの接続が応答せず、VM上の再検証は未実施**です。前回preview2に行ったWindows 10 VMの動作試験とは区別してください。Linux版と完全な機能一致は未達です。

## Windows 11実機での追加検証（preview4）

Windows 11 Pro実機で19件の既存テストを実行したところ、NVIDIA搭載機の想定が誤っていたテスト1件の失敗を発見して修正しました。修正後は21件の回帰テストとソースのセルフテストが成功し、実機でGUI表示・設定画面操作・RTX 3080実測値・実行ファイル生成・単体セルフテストを確認しました。LibreHardwareMonitor WMIがないためCPU温度・CPUファン・基板温度は取得不可です。警告音の実聴・物理Beep・Windowsログオン後の自動起動は未確認です。配布EXEの検証結果と差異は [Windows 11実機検証報告](./WINDOWS11_PHYSICAL_TEST_REPORT_20261029.md) を参照してください。

**preview4公開EXEの再検証：** GitHubで公開したEXEをWindows 11実機へダウンロードし、SHA-256一致、EXE単体セルフテスト成功、GUI起動・画面応答を確認しました。CPU温度などの未取得項目、および警告音・自動起動の未検証事項は残ります。
