---
title: 個人技術筆記與 Android / AI 知識庫
tags:
  - android
  - ai
  - notes
  - tools
  - kotlin
  - rust
  - codex
desc: 個人的技術知識庫，整理 Android、Rust、AI 相關工具、流程、除錯、效能、Kotlin 與工作方式，內容由 Codex 協助產生與修改。
author: icools
date: 2026-03-29
source: local
status: active
---

> 本專案內容完全是我透過 Codex 協助產生與修改，並會持續透過 Codex 協助整理與更新。

# 個人技術筆記與 Android / AI 知識庫

這個專案是我自己的技術筆記倉庫。

我會把平常開發 Android、使用 AI 工具、整理工作流時的心得、踩過的坑、觀察與筆記，慢慢累積在這裡。細節不會全部堆在 `README.md`，而是分散到各分類索引頁與資料夾內容中。

內容撰寫、分類、命名與索引更新規範，統一以 [`CONTENT_CHARTER.md`](CONTENT_CHARTER.md) 為主。

## 預計目錄規劃

目前先規劃以下結構：

```text
.
├─ README.md
├─ todo.md
├─ kotlin.md
├─ rust.md
├─ ai.md
├─ tools.md
├─ projects.md
├─ debugging.md
├─ performance.md
├─ workflow.md
├─ todo/
├─ kotlin/
├─ rust/
├─ ai/
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

### Rust [`rust.md`](rust.md)

放 Rust 在 Android 上的整合方式、NDK / JNI、框架選型與導入判斷。

### AI [`ai.md`](ai.md)

放平常使用的 AI 工具、模型平台、LLM 概念、content engineering 與 AI workflow 筆記。

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

## 這個 Repo 的用途與會用到的 Skill

這個 repo 的定位，不只是單純的 Markdown 筆記資料夾，而是一個同時兼具以下兩種用途的知識庫：

- 作為 `Obsidian` 可直接開啟與整理的 Markdown vault
- 作為 `GitHub Pages` 對外發布的技術筆記網站

也因為如此，這個 repo 內的內容不只是要「寫得出來」，還要能同時兼顧：

- 在 `Obsidian` 中好整理、好搜尋、好回看
- 在 `GitHub Pages` 上可以正確顯示、分類與索引
- 後續可以持續交給 `Codex` 協助補完、搬移、重寫與維護

目前和這個 repo 最直接相關的整理能力，主要分成「repo 內既有 skill / 規範」以及「外部通用 skill」兩類。

### Repo 內既有 skill / 規範

- `CONTENT_CHARTER.md`
  這是整個知識庫的最高準則。它定義了分類方式、根目錄索引頁角色、文章應放的位置、frontmatter 欄位、命名規則、圖片規則，以及新增文章時應同步更新哪些地方。

- `_agents/skills/github_pages_doc_audit/SKILL.md`
  這個 skill 主要處理 GitHub Pages 與 Markdown 結構整理，包含：
  - 檢查與補齊 frontmatter
  - 檢查 `draft/` 內容是否該搬移到正式分類
  - 幫草稿自動判斷分類與調整檔名 slug
  - 檢查資料夾是否缺少 `index.md`
  - 視情況更新父層索引
  - 檢查相對連結
  - 協助處理圖片格式優化

- `_agents/skills/notebooklm_pptx_to_markdown/SKILL.md`
  這個 skill 主要處理把 `NotebookLM` 產生的 `.pptx` 內容轉成 Markdown 筆記，包含擷取投影片文字、匯出圖片、壓縮圖片，以及把內容整理後插入文章。

### 外部通用 skill

- `obsidian-cli`
  這個 skill 用來操作 Obsidian CLI，適合拿來做 vault、筆記、metadata、daily note、搜尋、task 與 plugin 相關的管理。如果之後這個 repo 想更深入走 Obsidian 工作流，這個 skill 會很有用。

- `publish-github-pages-note`
  這個 skill 很貼近這個 repo 的日常用途，重點是把新筆記正確放進這個知識庫，並同步處理分類、frontmatter、索引、導覽與發布流程。它本質上就是把「新增一篇可公開發布的整理筆記」這件事做成可重複的工作流。

### 目前這個 Repo 已經具備的整理能力

從目前的規範與 skill 來看，這個 repo 已經有以下幾種整理能力：

- 草稿暫存與後續重分類
- Markdown frontmatter 補齊與統一
- 文章命名與 slug 整理
- 類別索引頁維護
- GitHub Pages 顯示與發布結構整理
- Obsidian 相容的 YAML properties / frontmatter 寫法
- PPTX 內容轉 Markdown 的匯入流程
- 後續交由 Codex 持續整理、補寫與搬移的工作流基礎

如果之後要做總整理，建議也可以把這幾個項目當成第一版的整理框架，先盤點目前「已經有什麼規範」、「哪些工作已經可以半自動處理」，再決定下一步要不要補新的 skill。

## 備註

這個專案會以 Markdown 為主，並盡量使用類似 Obsidian 的 frontmatter。`README.md` 只保留總覽與入口，細節會放在各分類索引頁與對應資料夾內容中。
