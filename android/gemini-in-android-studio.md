---
title: Gemini in Android Studio 筆記
tags:
  - android
  - android-studio
  - gemini
  - ide
  - ai
desc: 整理 Gemini in Android Studio 的定位、主要能力、版本限制，以及我怎麼看它在 Android IDE 裡的角色。
author: icools
date: 2026-04-01
source: https://developer.android.com/studio/gemini/overview
status: active
---

# Gemini in Android Studio

這篇我想記的不是「Android Studio 裡有 AI」這麼泛的事情，而是 Google 現在到底怎麼定義 `Gemini in Android Studio`。

官方 overview 寫得很直接，它是 Android development 的 AI-powered coding companion。這個說法表面上很常見，但 Android Studio 這版的差異在於，它不是只做通用 code chat，而是明顯往 Android-specific workflow 深挖。

像官方就直接點出幾個它特別適合的事情：

- 回答開發問題
- 生成程式碼
- 找相關資源
- 鼓勵最佳實務
- 幫忙 mock 與 troubleshoot Compose UI
- 修 Gradle build error
- 透過 Logcat 與 App Quality Insights 幫忙分析問題

我覺得這幾個點放在一起看，意思很明確：這不是單純把 Gemini 塞進 IDE，而是想把它做成一個有 Android 上下文感知能力的工作夥伴。

## 我怎麼看它的定位

如果只把它當成「IDE 內建聊天機器人」，其實會低估這個功能。

我目前更傾向把它看成 Android Studio 裡的一層 AI 工作介面。它的價值不只是問答，而是因為它開始碰到 Android 開發裡幾個真的很痛的環節：

- Compose UI 的生成與除錯
- Gradle 與 build error 解讀
- Logcat runtime error 分析
- App Quality Insights 這種 Android 專屬整合
- Agent mode 下更進一步的自動化操作

這也是為什麼我覺得它跟一般 editor 裡的 AI plugin 不是完全同一回事。因為 Android Studio 本身就掌握了專案、建置、UI 與診斷工具鏈，AI 一旦接到這些脈絡，能做的事情就不只是 autocomplete。

## 官方文件透露出的產品結構

從官方 overview 左側導航來看，`Gemini in Android Studio` 已經不是單一功能，而是一整串能力集合。

我自己會把它粗分成幾層：

### 1. Chat 能力

- 一般 chat
- attach files
- attach images
- Prompt Library
- Rules
- Prompt Gallery
- local model / remote model

這層比較像 IDE 內的基本 AI 互動面。

### 2. Agent Mode

這層就開始比較有意思了。

官方現在已經把這些放進 Agent Mode：

- create a new project with AI
- automate dependency updates
- add an API key
- add an MCP server
- `AGENTS.md` file support

也就是說，Android Studio 現在不是只打算讓你「問 AI」，而是想把它往比較接近 agent workflow 的方向推。

### 3. Android-specific integration

這層是我最在意的地方，因為它才是 Android Studio 版本真正的辨識度：

- Compose UI
- Logcat
- App Quality Insights
- code/document/test generation
- commit message 生成

如果少掉這層，它就比較像一般 IDE AI 助理；有了這層，才比較像真正長在 Android 開發環境裡的工具。

## 使用前要注意的事

官方 overview 有一個很實際的限制，我覺得很值得先記起來：

`Gemini in Android Studio` 只支援最新版 stable channel，以及前 10 個月內釋出的主要版本。

這代表如果你的 Android Studio 太舊，就算你看到文件也不一定能直接用。這種限制很常被忽略，但實際上對團隊導入很重要，因為很多公司不一定會一直跟最新 IDE。

## 免費版和 business 版怎麼看

官方目前把它分成：

- no-cost tier
- business tier

如果是個人開發者、學生、freelancer 或 hobby project，官方是建議先用免費版。文件提到這一層會提供較輕量版的 Gemini 2.5 Pro，context window 較小，但對多數任務已經夠用。

如果任務比較複雜，需要完整 1M token context window，官方文件提到可以另外加 Gemini API key，改走按 token 計費。

這個切法我覺得很像是在說：

- 日常 IDE 協助可以先用內建免費能力
- 真正大型任務再往更高成本模型或 business 功能升級

## 我覺得它特別適合的情境

- 想在 Android Studio 內直接問 Android / Compose / Gradle 問題
- 卡在 build error，但不想只盯著錯誤訊息自己拆
- 想從 screenshot、UI 圖或設計意圖快速生成 Compose 雛形
- 想讓 Logcat、AQI、IDE 上下文一起進到 AI 分析流程
- 想開始試 Agent Mode、`AGENTS.md`、MCP 這些新工作流

## 我自己的簡短評價

我現在會把 `Gemini in Android Studio` 看成 Android IDE 正在重新長出一層 AI-native workflow 的起點。

它當然還不是萬能，也不是有了之後就不用懂 Android；但它已經不是那種只有「幫你補幾行 code」的 AI 功能了。

如果未來 Android Studio 持續把 chat、agent、MCP、IDE diagnostics 這幾條線整合得更深，這個東西會越來越像 Android 開發的真正操作台，而不只是附加功能。

## 參考

- 官方文件: [About Gemini in Android Studio](https://developer.android.com/studio/gemini/overview)

## 來源說明

這篇主要依據 Android Developers 官方 overview 整理，重點放在它的產品定位、功能分層與實際導入時要注意的版本條件。
