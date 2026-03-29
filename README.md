---
title: Android 開發工具與心得筆記
tags:
  - android
  - notes
  - tools
  - kotlin
  - codex
desc: 個人的 Android 開發知識庫，整理工具、除錯、效能、Kotlin 與工作流程筆記，內容由 Codex 協助產生與修改。
author: icools
date: 2026-03-29
source: local
status: active
---

> 本專案內容完全是我透過 Codex 協助產生與修改，並會持續透過 Codex 協助整理與更新。

# Android 開發工具與心得筆記

這個專案是我自己的 Android 開發筆記倉庫。

我會把平常開發 Android 時用到的工具、除錯流程、效能觀察方式、學到的技巧、踩過的坑，以及一些還沒完全整理好的內容，慢慢累積在這裡。細節不會全部堆在 `README.md`，而是分散到各分類索引頁與資料夾內容中。

內容撰寫、分類、命名與索引更新規範，統一以 [`CONTENT_CHARTER.md`](CONTENT_CHARTER.md) 為主。

## 預計目錄規劃

目前先規劃以下結構：

```text
.
├─ README.md
├─ todo.md
├─ kotlin.md
├─ tools.md
├─ projects.md
├─ debugging.md
├─ performance.md
├─ workflow.md
├─ todo/
├─ kotlin/
├─ tools/
├─ projects/
├─ debugging/
├─ performance/
└─ workflow/
```

## 分類入口

### Todo [`todo.md`](todo.md)

放還沒完全消化、先暫存待整理的內容。

### Kotlin [`kotlin.md`](kotlin.md)

放 Kotlin 語法、實務寫法與 Android 開發中的常見用法整理。

### Tools [`tools.md`](tools.md)

放 Android 開發工具索引、簡介與各工具筆記入口。

### Projects [`projects.md`](projects.md)

放值得研究的 Android app / 開源專案介紹與觀察筆記。

### Debugging [`debugging.md`](debugging.md)

放除錯流程、log 觀察、Crash / ANR 排查與問題定位心得。

### Performance [`performance.md`](performance.md)

放效能觀察、卡頓分析、GPU / memory / startup 相關筆記。

### Workflow [`workflow.md`](workflow.md)

放開發流程、測試流程、自動化習慣與工作流整理。

## 與 Codex 的協作方式

之後我可以直接請 Codex 幫我做這些事：

- 把零散筆記整理成一篇結構化 Markdown
- 幫我補上章節、標題與範例
- 幫我把口語內容改寫成可讀性更好的筆記
- 幫我整理工具比較表
- 幫我建立新的主題資料夾與索引頁
- 幫我把 `todo/` 裡的內容重新分類

## 備註

這個專案會以 Markdown 為主，並盡量使用類似 Obsidian 的 frontmatter。`README.md` 只保留總覽與入口，細節會放在各分類索引頁與對應資料夾內容中。
