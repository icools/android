---
title: Google AI Edge Gallery 專案介紹
tags:
  - android
  - project
  - open-source
  - ai
  - on-device-a
  - google-ai-edge
desc: Google AI Edge Gallery 是一個展示裝置端生成式 AI 能力的開源 Android 應用，可本地執行模型、聊天、看圖問答與音訊轉錄。
author: icools
date: 2026-03-29
source: https://github.com/google-ai-edge/gallery
status: active
---

# Google AI Edge Gallery

## 專案簡介

`Google AI Edge Gallery` 是 Google AI Edge 團隊推出的開源示範應用，重點是讓使用者可以直接在裝置端體驗生成式 AI 的各種能力。

這個專案的核心價值，不只是「有一個 AI App 可以玩」，而是它把 on-device AI 的使用情境、模型切換、效能觀察與互動體驗，整理成一個可直接安裝、可直接研究的 Android 專案。

## 我覺得它值得看的原因

- 它不是單一功能 demo，而是一個整理良好的 AI 能力展示 App
- 可以看到裝置端執行模型的實際體驗方向
- 很適合拿來理解 on-device AI 在 Android 上怎麼包裝成產品
- 對想研究 AI app UX、模型選擇與本地推論流程的人很有參考價值
- 是 Google AI Edge 團隊公開維護的專案，具備不錯的學習價值

## 這個專案在做什麼

根據官方 README，這個專案是一個展示 on-device ML / GenAI use cases 的 gallery app，讓使用者可以在本機載入模型後，離線體驗不同 AI 功能。

官方列出的核心能力包含：

- 本地執行、離線使用
- 切換不同模型並比較效果
- `AI Chat`
- `Prompt Lab`
- `Ask Image`
- `Audio Scribe`
- 效能指標觀察，例如 TTFT、decode speed、latency
- 載入本地模型進行測試

## 我會怎麼看這個專案

如果之後要深入研究，我會優先從這幾個角度看：

- App 的整體功能架構怎麼切
- 模型下載、選擇與切換流程怎麼設計
- UI 如何包裝多種 AI 能力
- 效能資訊怎麼呈現在畫面上
- Android 端如何接 on-device inference 能力

## 實際測試心得

這個專案不只適合「看架構」，我自己實際從 GitHub source build 跑到手機上之後，覺得它也很適合拿來當成 on-device AI app 的真實排錯樣本。

我原本只是想讓 app 透過 Hugging Face 抓 `Qwen3.5-0.8B-LiteRT`，結果前面先遇到 `Loading model list...` 卡住，後來又看到 `Failed to load model list`。一開始我以為是網路、登入或模型來源問題，最後才確認真正卡住的是本地 allowlist 與 app 版本的對齊。

這次實測最有感的幾個點是：

- 手機裡可能同時有 `com.google.ai.edge.gallery` 和 `com.google.aiedge.gallery` 兩個 package，debug 前一定要先確認自己看到的是哪一個 app
- `/data/local/tmp/model_allowlist.json` 就算路徑正確，schema 太舊還是會出事，像缺少 `commitHash` 就可能讓資料轉換直接炸掉
- source build 不只會看單一 allowlist，像我這次 `1.0.11` 版本也得一起補 `/data/local/tmp/model_allowlists/1_0_11.json`
- 前台畫面顯示的 `Loading model list...` 很像網路慢，但實際上根因可能是 Kotlin 端在解析 allowlist 時先噴錯
- 這種問題只看畫面很難猜，`adb logcat` 幾乎是必備，因為真正的錯誤通常藏在背景初始化流程裡

對我來說，`Google AI Edge Gallery` 的價值也因此更明確了。它不是只有展示功能而已，還能很具體地幫人理解一個 on-device AI Android app 在模型清單、版本化設定、模型來源和實機除錯上，實際會踩到哪些坑。

## 適合拿來學什麼

- Android AI App 的產品型態
- 裝置端模型體驗設計
- 多功能 AI App 的資訊架構
- 本地模型執行的使用者流程
- AI 功能展示型 App 要怎麼做得比較完整

## 我的簡短評價

如果目標是研究「Android 上怎麼做一個可展示、可互動、可切模型的 on-device AI App」，這個專案很值得收藏。

它很適合當作：

- Android AI app 觀察樣本
- UI / UX 參考
- on-device AI 功能盤點入口
- 後續研究 Google AI Edge 生態系的起點

## GitHub 來源

- 官方 repo: [google-ai-edge/gallery](https://github.com/google-ai-edge/gallery)

## 來源說明

官方 README 將它描述為一個展示裝置端 ML / GenAI use cases 的 gallery，讓使用者能在本地載入模型後離線體驗聊天、影像問答、音訊轉錄、prompt 實驗與效能觀察等功能。相關說明也可參考官方 Wiki：

- Wiki: [google-ai-edge/gallery Wiki](https://github.com/google-ai-edge/gallery/wiki)
