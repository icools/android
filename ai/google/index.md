---
title: Google AI 索引
tags:
  - ai
  - google
  - gemini
  - index
desc: Google AI 工具與平台索引頁。
author: icools
date: 2026-03-30
source: local
status: active
---

# Google AI

這一頁整理 Google AI 相關筆記，詳細文章會放在 `ai/google/`。

- [Google AI Studio]({{ '/ai/google/google-ai-studio/' | relative_url }}) - 現在不只是 prompt playground，而是把 `Chat`、`Stream`、`Generate Media`、`Build`、`GitHub`、`Firebase` 與 `Cloud Run` 串成一條從 idea 到 app 的工作流。
- [Gemini CLI](gemini-cli.md) - 把 Gemini 能力帶進 terminal 工作流，適合查詢、生成與輕量開發輔助。
- `Vertex AI`
- `NotebookLM`

## Gemini Chat

Gemini 的網頁對話介面，除了基本問答外，以下幾個功能非常實用：

- **Canvas** — 可以在畫布上撰寫互動流程，並且直接加入 AI 功能在上面，非常方便；同時也能產生簡報，適合快速把想法視覺化。
- **Deep Research** — 深度研究模式，讓 Gemini 自動進行多輪搜尋與資料彙整，產出接近報告等級的長篇回答，適合需要全面了解一個主題時使用。
- **Veo 3.1** — Google 的影片生成模型，直接在 Gemini Chat 中使用，可以從文字描述生成短影片。
- **Nano Banana 資訊圖卡** — 超實用的圖片產生功能，可以快速產出資訊圖卡（infographic），適合做社群內容、簡報素材或快速視覺化資料重點。
- **Gem** — 自訂的 Gemini 人格與指令預設，類似 ChatGPT 的 GPTs 概念，可以建立特定用途的對話代理，省去每次重複下指令。
- [Google Colab](google-colab.md) - 雲端 notebook 環境，適合快速跑 Python、資料處理與模型實驗，也逐步內建 AI 協助能力。
- [Google Cloud IDE](google-cloud-ide.md) - 接近線上版 `VS Code` 的雲端 IDE，整合開發環境與 code assist。
- [Google Anti-gravity](google-antigravity.md) - 偏 chat-first 的工作代理工具，適合以需求與驗收條件驅動多步驟任務。
- [Android Studio Skills](android-studio-skills.md) - Android Studio Agent Mode 的 project-local skill 機制與結構。
- [Android Bench](android-bench.md) - Google 公開的 Android 開發 benchmark，可拿來參考 Android 相關模型選型。

## 之後可整理的方向

- Gemini 與 ChatGPT / Codex 的差異
- Vertex AI 的平台定位
- NotebookLM 適合拿來做什麼
- Google Cloud IDE 和本地 IDE 的分工
- Anti-gravity 適合的任務型態與限制
- Android Studio 裡 skill 怎麼和團隊 workflow 結合
