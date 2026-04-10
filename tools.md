---
title: Tools 索引
tags:
  - android
  - index
  - tools
desc: Android 開發工具索引頁，整理工具入口與簡單介紹。
author: icools
date: 2026-04-04
source: local
status: active
---

# Tools

這一頁主要整理工具索引與簡單介紹，細節再放到 `tools/` 內各自的筆記。

- [`scrcpy`](tools/scrcpy.md) - 一個可以用來控制 Android 裝置的實用工具，我最常拿它做螢幕 mirror、電腦操作真機，以及日常測試時的視覺化控制。
- [`maestro`](tools/testing/maestro.md) - 一套很適合做 UI 自動化與 smoke test 的工具，能用可讀性高的 YAML 方式描述 App 操作流程。
- [`runmaestro-ai`](tools/testing/runmaestro-ai.md) - 跟原本 mobile-dev 的測試工具不是同一個東西，這篇整理的是 runmaestro.ai 這個用來管理多個 AI coding agent、Auto Run 與 worktree 的 desktop command center。
- [`shizuku`](tools/shizuku.md) - 一個很關鍵的 Android 權限橋接工具，讓一般 App 能在無 Root 情況下透過 adb / Binder 路線使用部分系統級能力，特別適合進階工具、測試輔助與自動化場景。
- [`DroidRunScript`](tools/testing/droidrun-script.md) - 我自己的 private repo，把 `LM Studio`、本地模型、`Python` 與 `ADB` 串起來做 Android 手機自動操作，是很值得繼續追的 local AI agent 實驗工具。
- [`jadx-ui`](tools/decompile/jadx-ui.md) - 很適合拿來查看 APK、反編譯程式碼、搜尋 class 與資源，是理解陌生 Android app 結構時很好用的工具。
- [`LibChecker`](tools/decompile/libchecker.md) - 一個用來直接查看已安裝 App 使用哪些函式庫的 Android 工具，適合在正式反編譯前先快速理解第三方 App 的技術堆疊。
- [`Autonomous Mobile Pentesting`](tools/autonomous-mobile-pentesting.md) - 讀 Workers IO：把 `Claude Code` 接上 rooted Android（ADB/UI Automator/mitmproxy/Frida），讓 agent 自己操作 app、攔流量、做 runtime instrumentation 的 workflow。
- [`simvyn`](tools/simvyn.md) - 一個整合 Android / iOS 裝置控制的 dashboard + CLI，我目前最看重的是它很適合長成 AI 可操作的測試與除錯控制層。
- [`android-studio-mcp-server`](tools/android-studio-mcp-server.md) - 這篇整理 Android Studio 接 MCP server 的價值、適合情境，以及舊版 JetBrains proxy 與新版內建 MCP 支援之間的差異。
- [`JetBrains AST/PSI 分析`](tools/jetbrains-ast-psi-analysis.md) - 深入 JetBrains IDE 底層結構，解讀 AST 與 PSI 的差異與應用。
- [`JetBrains Mermaid 自動化`](tools/jetbrains-ast-psi-mermaid-guide.md) - 結合 AI 從 IDE PSI 結構自動生成 Mermaid 架構圖的進階指南。
- [`NotebookLM PPTX 轉 Markdown`](tools/markdown-notebooklm-pptx-workflow.md) - 利用 AI Agent 將 NotebookLM 生成的 PPTX 轉換為 Markdown 與圖片的自動化流程。
- [`Emerge Tools`](tools/emerge-tools.md) - 把 Android App 的體積、資產與 native library 組成拆成 dashboard，適合觀察 APK 結構與 size regression。
- [`SonarQube for IDE`](tools/sonarqube-for-ide.md) - 在 Android Studio / IntelliJ 裡即時指出程式碼品質與安全性問題，適合把靜態分析前移到日常開發流程。

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
