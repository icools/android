---
title: Android Bench
tags:
  - ai
  - google
  - android-studio
  - benchmark
  - gemini
desc: 整理 Google Android Bench 的定位、排行榜結果與 Android 開發模型選型觀察。
author: icools
date: 2026-03-29
source: https://developer.android.com/bench
status: active
---

# Android Bench

`Android Bench` 是 Google 在 Android Developers 上公開的 benchmark，目的是衡量不同 LLM 處理真實 Android 開發任務的能力。

它不是泛用聊天能力排行，而是更偏向 Android 工程場景，所以比起一般 benchmark，更適合拿來當 Android Studio 與 Android 開發工作流的模型選型參考。

## 我怎麼看這份榜單

以官方 2026-03-04 這版 leaderboard 來看，第一名是 `Gemini 3.1 Pro Preview`，分數 `72.4`。接著是 `Claude Opus 4.6` 的 `66.6`，以及 `GPT-5.2-Codex` 的 `62.5`。

如果我的目標是做 Android 開發相關工作，尤其是希望模型更貼近 Android API、專案結構、Gradle、UI 與實際修 bug 的情境，那這份榜單代表優先順序大致可以抓成：

- 優先考慮 `Gemini 3.1 Pro Preview`
- 次選可看 `Claude Opus 4.6`
- 也可以把 `GPT-5.2-Codex` 放進主要候選

後面的 `Gemini 3 Pro Preview`、`Claude Sonnet 4.6`、`Claude Sonnet 4.5` 雖然不是完全不能用，但在這份 benchmark 上表現就明顯落後前段模型。

## 對 Android Studio 選型的意義

如果是 Android Studio 內的 AI 輔助場景，這份資料至少說明一件事：選對模型，比單純把任何 agent 接進 IDE 更重要。

就這份榜單來看，優先使用 `Gemini 3.1 Pro Preview` 這類明顯領先的模型，通常會比選到表現較弱的模型更合理。`GPT-5.2-Codex` 也仍然是值得保留的選項，因為它在 Android 任務上仍落在前段班。

反過來說，如果只是因為模型名稱熱門就直接選 `Claude Sonnet 4.6` 或 `Claude Sonnet 4.5`，從 Android Bench 的結果來看，未必是最好的 Android 開發方案。

## 讀這份榜單時要注意

- 這份 benchmark 測的是 Android 開發任務，不是完整 IDE 使用體驗。
- 排名反映的是該次測試資料與方法下的結果，不代表所有專案都會完全一樣。
- 官方頁面標示這批結果是截至 `2026-03-05` 的最新結果，之後可能還會更新。

## 目前可記住的結論

如果要替 Android Studio 或 Android 開發工作流挑模型，現階段可以優先以 `Gemini 3.1 Pro Preview`、`Claude Opus 4.6`、`GPT-5.2-Codex` 這幾個前段模型為主。

如果目標是提高 Android 任務成功率，而不是只求通用對話表現，那盡量避免優先選用 `Claude Sonnet 4.6` 或 `Claude Sonnet 4.5`，會是比較穩妥的方向。

## 參考

- [Android Bench](https://developer.android.com/bench)
- [Android Bench methodology](https://developer.android.com/bench/methodology)
