---
title: Google Anti-gravity
tags:
  - ai
  - google
  - antigravity
  - agent
  - workflow
desc: Google Anti-gravity 可以看成偏 chat-first 的工作代理工具，重點在於用需求與驗收條件驅動多服務整合與任務執行。
author: icools
date: 2026-03-29
source: local
status: active
---

# Google Anti-gravity

`Google Anti-gravity` 給我的感覺，不是單純另一個 code assistant，而比較像是一個偏 `chat-first` 的工作代理工具。

它的重點不是要你一直在裡面手寫 code，而是讓你用需求、限制與驗收條件去驅動它完成事情。

最近在 `GDG Taipei` 聽完一場把 `Antigravity` 和 `Spec-Driven Development` 放在一起講的分享後，我對它的定位更清楚了一點：它比較像是一個會沿著 `Plan → Act → Verify` 循環工作的 contractor，而且每一步都需要你持續給 feedback，而不是一句話丟出去就全自動完成的黑盒子。

## 為什麼它強

我會覺得它強，是因為它不是只回答問題，而是比較偏向把多個 Google 服務、系統能力與工具操作串在一起。

也就是說，你可以比較像在下任務，而不是只是在問答。

這種模式很適合以下情境：

- 幫我做一個網站
- 幫我整理一套簡報
- 幫我生成需要的圖片與文案
- 幫我把最後成果驗收一遍

如果需求還在收斂、UI 細節很多、或者之後還要跨 session / 跨人交接，這種 spec-first 的工作方式會比「直接叫 AI 開做」穩很多。

## 比較像在寫需求，不是在寫 code

如果用法正確，很多時候你主要做的事會是：

- 描述目標
- 補充需求
- 定義限制
- 列出驗收標準

例如你不是直接去刻畫面，而是告訴它：

- 想做什麼類型的網站
- 目標受眾是誰
- 要有哪些頁面
- 圖片與文案需要什麼風格
- 最後哪些連結、圖片與操作必須驗收成功

這時候它比較像是在接 PRD 或 task spec，而不是單純接一段 prompt。

## 驗收導向很重要

我覺得這類工具最有意思的地方，是它不只是生成內容，而是能往「做完再驗」的方向走。

如果你把驗收條件講清楚，例如：

- 圖片要能正常顯示
- 連結要指向正確頁面
- 某些互動流程要可操作

那整個工作方式就會很像在管理一個會做事的 agent。

這也是為什麼我會把 `Antigravity` 和 [Spec-Driven Development](../../design/spec-driven-development.md) 放在一起看：前者比較像 contractor，後者比較像讓 contractor 不會做歪的工作基線。

## 我目前會怎麼定位它

我會把 Anti-gravity 看成：

- chat-first 的工作代理介面
- 適合需求驅動的產出工具
- 很適合把多步驟任務交給它處理的實驗平台

如果是要拿大量 code 細修，未必是它最強的地方；但如果是要從需求一路推到產出與驗收，它就很值得研究。

## 我目前的觀察

這類工具很適合拿來做網站、簡報、素材生成與流程驗收等工作。

如果之後它和更完整的企業工作流、協作工具或 code assistant 深度整合，實用性應該還會再往上拉。

如果想看我把這件事放回實際演講心得裡怎麼理解，也可以參考這篇：[不要當 code monkey：我在 GDG Taipei 聽完 Antigravity 與 Spec-Driven Development 的心得](../../workflow/android-gdg-taipei.md)。

如果想看我把 `Antigravity` 的 implementation plan / task list，和 `Codex plan mode`、`Android Studio Agent Mode` 一起放回 `SDD` / `spec kit` 的工程脈絡裡理解，也可以再看這篇：[Plan Mode、SpecKit 與 SDD：先把需求問清楚，再讓 AI 開始做](../../workflow/plan-mode-spec-kit-and-sdd.md)。
