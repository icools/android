---
title: Tools 索引
tags:
  - android
  - index
  - tools
desc: Android 開發工具索引頁，整理工具入口與簡單介紹。
author: icools
date: 2026-03-29
source: local
status: active
---

# Tools

這一頁主要整理工具索引與簡單介紹，細節再放到 `tools/` 內各自的筆記。

- [`scrcpy`](tools/scrcpy.md) - 一個可以用來控制 Android 裝置的實用工具，我最常拿它做螢幕 mirror、電腦操作真機，以及日常測試時的視覺化控制。
- [`maestro`](tools/maestro.md) - 一套很適合做 UI 自動化與 smoke test 的工具，能用可讀性高的 YAML 方式描述 App 操作流程。
- [`shizuku`](tools/shizuku.md) - 一個很關鍵的 Android 權限橋接工具，讓一般 App 能在無 Root 情況下透過 adb / Binder 路線使用部分系統級能力，特別適合進階工具、測試輔助與自動化場景。
- [`DroidRunScript`](tools/droidrun-script.md) - 我自己的 private repo，把 `LM Studio`、本地模型、`Python` 與 `ADB` 串起來做 Android 手機自動操作，是很值得繼續追的 local AI agent 實驗工具。
- [`jadx-ui`](tools/jadx-ui.md) - 很適合拿來查看 APK、反編譯程式碼、搜尋 class 與資源，是理解陌生 Android app 結構時很好用的工具。
- [`simvyn`](tools/simvyn.md) - 一個整合 Android / iOS 裝置控制的 dashboard + CLI，我目前最看重的是它很適合長成 AI 可操作的測試與除錯控制層。

## 之後想補上的工具

- `adb`
- `logcat`
- `Layout Inspector`
- `Android Studio Profiler`
- `GPU Profile`
- `uiautomator`
- `bundletool`
- `apktool`
- `aapt`
