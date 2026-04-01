---
title: Android Studio MCP Server 工具介紹
tags:
  - android
  - tool
  - android-studio
  - mcp
  - ai
  - gemini
desc: 整理 Android Studio 與 MCP server 的連接方式、適合情境，以及舊版 JetBrains proxy 路線和新版內建支援之間的差異。
author: icools
date: 2026-04-01
source: https://skywork.ai/skypage/zh-hant/%E6%B7%B1%E5%BA%A6%E8%A7%A3%E6%9E%90-android-studio-mcp-server%EF%BC%9A%E8%B3%A6%E8%83%BD-AI-%E9%A9%85%E5%8B%95%E7%9A%84-Android-%E9%96%8B%E7%99%BC%E6%96%B0%E7%AF%84%E5%BC%8F/1972200879378501632
status: active
---

# Android Studio MCP Server

這篇是我看完 Skywork 那篇 `android-studio-mcp-server` 文章之後，順手整理成比較適合自己之後回查的版本。

我覺得它最值得記的，不只是「Android Studio 可以接 MCP」，而是 Android 開發這件事終於開始有一條比較正式的路，把 IDE 內的程式上下文、AI agent 跟外部工具串在一起。

不過也要先講清楚一件事：現在看到的 `android-studio-mcp-server` 資訊，實際上有兩個不同時期的脈絡。

- 舊一點的路線，是 JetBrains 那套 `mcp-jetbrains` / `@jetbrains/mcp-proxy`，讓外部 AI client 透過 proxy 去連 IDE。
- 新一點的路線，則是 Android Studio 自己在 `Gemini` 的 Agent mode 裡直接支援 MCP server 設定。

所以如果今天只是想理解「Android Studio + MCP 到底能做什麼」，這篇文章是值得看的；但如果是要真的開始配置，我會優先看新版官方文件，而不是只照舊版 proxy 教學走。

## 這工具在做什麼

`MCP` 的核心價值，是讓 AI 不只是聊程式，而是真的能對工具出手。

放到 Android Studio 這個脈絡裡，我會把它理解成：

- 讓 IDE 裡的 agent 可以呼叫外部工具
- 讓 AI 有機會拿到 repo、設計稿、Issue、文件系統或其他服務的即時上下文
- 讓 Android 開發流程從「IDE 內聊天」往「IDE 內協作執行」再往前走一步

Skywork 那篇文把它拆成幾個典型場景，我自己覺得整理得不錯，尤其是這幾個方向很有代表性：

- 自動化程式碼審查與文件生成
- 複雜錯誤的快速定位與修復
- 跨檔案的大範圍重構
- 和其他 MCP server 串起來做多工具協作

## 我為什麼會想記這個

這個主題我會想另外留一篇，是因為它碰到我最近很在意的一個問題：

AI 到底只是 IDE 裡比較會補字的聊天窗，還是真的開始變成開發工作流的操作層？

如果 Android Studio 只是把 chat 做得更漂亮，那其實意義有限。

但 MCP 把外部工具接進來之後，事情就不一樣了。它代表 agent 不只看得到目前開著的檔案，還能往 Issue、設計資訊、版本控制、文件或其他服務延伸。這種感覺比較像是把 IDE 變成 agent 的工作台，而不是只多一個側邊欄。

## 我目前怎麼理解它的價值

### 1. 它補的是「工具可操作性」，不是單純更多上下文

很多 AI IDE 功能講的是 context window、codebase understanding。

但 MCP 真正讓我在意的是，它把「理解」往前推到「可以執行工具動作」。

對 Android 開發來說，這個差別很大。因為很多事情不是看懂 code 就能完成，還要碰到：

- GitHub 或 GitLab
- 設計稿
- issue tracker
- 文件系統
- 內部知識庫
- 之後可能還會接到測試、CI 或裝置控制工具

## 2. 它很適合 Android 這種跨層級又跨工具的開發環境

Android 專案很少只是改一個 Kotlin 檔案就結束。

很多任務會同時碰到：

- Gradle 與 module 結構
- UI 與 design token
- API 資料模型
- log、crash、回報單
- 文件與 release 流程

這也是為什麼我覺得 Android Studio 接 MCP，比單純在 editor 裡加聊天框更有感。因為 Android 開發本來就是工具鏈密度很高的工作。

## 3. 它是一種「AI 原生 IDE 工作流」的雛形

Skywork 那篇文把這件事講成 AI 驅動的 Android 開發新範式，我覺得這個說法雖然有點宣傳味，但方向沒有錯。

真正關鍵不是名字，而是 IDE 開始提供一個比較正式的介面，讓 agent 可以往外接能力。這會比每個 AI 工具各自發明私有 plugin 方式更健康，因為 MCP 至少是一個比較通用的協定層。

## 實際使用時我會怎麼看

如果今天我要用它，我會分成兩種情境。

### 情境一：在 Android Studio 內直接用 Gemini Agent Mode 接 MCP

這是我現在認為比較應該優先看的新路線。

Android Developers 官方文件已經有 `Add an MCP server` 說明，路徑是在：

`Settings > Tools > AI > MCP Servers`

官方文件目前提到幾個重點：

- 可以把多個 remote MCP server 配到 Android Studio
- 設定會存進 `mcp.json`
- 可以在 chat 裡用 `/mcp` 看有哪些工具
- 目前限制不少，像是 `stdio` transport、MCP resources、prompt templates，以及某些 OAuth login 還不支援

這個版本比較像「Gemini in Android Studio 主動去接外部 MCP server」。

### 情境二：讓外部 AI client 連進 JetBrains / Android Studio

這是比較舊、但還是值得知道的脈絡。

JetBrains 之前提供過 `mcp-jetbrains` / `@jetbrains/mcp-proxy`，讓像 `Claude Desktop`、`Cursor` 或其他 client 可以透過 proxy 跟 IDE 互動。

但現在官方 repo 已經直接標示 deprecated，原因是核心功能已整合進 2025.2 之後的 IntelliJ 系列 IDE。也就是說，這條路不是完全不能理解，而是比較不適合當成現在的主推薦路線。

## 我覺得特別適合的情境

- 想把 Android Studio 裡的 agent 連到 GitHub、Figma、Notion 這類外部工具
- 想讓 AI 不只是看 code，而是真的能幫忙查資料、叫工具、串工作流
- 要做跨檔案重構、文件同步、issue 對照這種多上下文任務
- 想觀察 Android Studio 的 AI 工作流是不是正在從 chat assistant 走向 agent workbench

## 補充筆記

- 現在要查設定方式，應優先看 Android Developers 的新版文件。
- 如果你查到很多 `@jetbrains/mcp-proxy` 教學，要先確認那是不是舊資料。
- Android Studio 這波 MCP 支援，比較偏向讓 IDE 內的 Gemini agent 使用外部工具，不等於所有 MCP 功能都完整支援。
- 官方文件目前有明確列出限制，所以做規劃時不要先假設 `resources`、`prompt templates` 或所有 auth flow 都一定能用。

## 我自己的簡短評價

我會把 `Android Studio MCP Server` 看成一個很重要的轉折點，但不是因為某個單一 repo 本身有多神，而是因為 Android IDE 正在開始形成一個比較像樣的 AI 工具接入層。

如果你想追的是「AI 怎麼真的改變 Android 開發流程」，這題很值得關注。

如果你只是想抄一份舊教學把 proxy 接起來，那反而要小心版本落差。現在比較穩的做法，是把 Skywork 這篇當作理解脈絡的入口，再回到官方文件確認當前支援範圍。

## 參考

- 文章來源: [深度解析 android-studio-mcp-server：賦能 AI 驅動的 Android 開發新範式](https://skywork.ai/skypage/zh-hant/%E6%B7%B1%E5%BA%A6%E8%A7%A3%E6%9E%90-android-studio-mcp-server%EF%BC%9A%E8%B3%A6%E8%83%BD-AI-%E9%A9%85%E5%8B%95%E7%9A%84-Android-%E9%96%8B%E7%99%BC%E6%96%B0%E7%AF%84%E5%BC%8F/1972200879378501632)
- Android Developers: [Add an MCP server](https://developer.android.com/studio/gemini/add-mcp-server?hl=zh-TW)
- JetBrains repo: [JetBrains/mcp-jetbrains](https://github.com/JetBrains/mcp-jetbrains)

## 來源說明

這篇是從 Skywork 那篇整理脈絡出發，但內容判斷有另外對照 Android Developers 官方文件與 JetBrains 官方 repo。

我特別保留了「為什麼這題值得看」的那一層，但在實作建議上，會優先採用目前還在更新的官方文件，而不是只跟著舊版 proxy 流程走。
