---
title: Changelog
tags:
  - workflow
  - changelog
desc: 知識庫變更紀錄，以時間軸方式整理每日的新增與更新內容。
author: icools
date: 2026-03-30
source: local
status: active
---

# Changelog

## 2026-03-30：Google AI Studio 章節補強

今天把 `Google AI Studio` 從原本索引裡的一個關鍵字，擴成一篇正式文章，重點放在它現在如何從單純 playground 演進成可以串 `Live API`、`Build mode`、`GitHub`、`Firebase` 與 `Cloud Run` 的 AI 建置工作流。

### 重點更新

- 新增 `Google AI Studio` 正式頁面，整理介面拆解、操作流程與平台定位
- 補充 `Firebase Studio` sunset 的明確日期：`2026-03-19` 公告、`2026-06-22` 停止新 workspace、`2027-03-22` 關閉
- 釐清 `Google AI Studio`、`Google Antigravity`、`Firebase Studio` 三者分工
- 補充 `Build mode`、`Live API`、`Generate Media`、`GitHub`、`Firebase`、`Cloud Run` 的串接方式
- 更新 `AI` 與 `Google AI` 索引，並把新頁面連結改成 GitHub Pages 友善寫法

---

## 2026-03-29：知識庫建立日

今天從零開始把整個 Android 開發知識庫架起來，完成 GitHub Pages 部署，並持續擴充各分類內容。

### 18:00 – 19:00｜基礎建設

- 建立初始 `README.md` 與專案結構
- 新增 Tools 文件（scrcpy、Maestro）
- 重構目錄結構，建立 Debugging / Kotlin / Performance / Projects / Todo / Tools / Workflow 索引頁
- 建立 `CONTENT_CHARTER.md` 內容憲章
- 實作 GitHub Pages 部署流程：navigation、404、CSS 樣式、`default.html` layout
- 新增 Typealias Kotlin 學習資源頁

### 19:00 – 20:00｜AI 與語言分類擴充

- 建立 AI 子分類索引：OpenAI / Google / Mistral / LLM / Content Engineering
- 新增 Kotlin 學習資源：Google Android Kotlin、JetBrains Blog、Kotlin Playground、Taiwan KUG
- 新增 Android Studio Skills 頁面
- 擴充 AI Tools：GitHub Copilot（含 Codespaces / Actions 整合）、Hugging Face、LM Studio、Ollama
- 新增 Google AI 工具：Cloud IDE、Colab、Gemini CLI、Anti-gravity、Android Bench
- 建立 LLM 模型頁面：Qwen 3.5、FunctionGemma、TranslateGemma
- 新增 Google AI Edge Gallery 測試筆記
- 新增 Rust 分類與 Android 整合概述
- 建立 Codex 使用筆記（Draft）
- 建立 Draft 索引與 navigation 串接

### 20:00 – 21:00｜App 實測與 AI 工具 PR

- 撰寫 Android AppFunctions 整合與觀察筆記
- PR #1：新增 fal.ai 工具介紹
- PR #2：新增 Krea AI 工具介紹

### 21:00 – 22:00｜Design、Books 與 Workflow

- 建立 Design 分類索引
- 新增設計筆記：Spec-Driven Development、BDD/TDD、DDD/Design Patterns、Paddy Coding、UML/Mermaid Tools
- 建立 Books 分類索引
- 新增讀書筆記：Clean Code、Clean Architecture、Beautiful Code
- 新增 Workflow：Android GDG Taipei
- PR #3：Design & Books 知識庫合併

### 22:00 – 23:00｜首頁強化與 AI 內容深化

- 建立 `readlater.md` 待讀資源收集頁
- 首頁新增 Mermaid mindmap 知識庫全覽
- AI 導覽加入 navigation（primary + library）
- 更新 OpenAI 索引：ChatGPT 功能詳述（Code Interpreter、Canvas、Connectors/Apps、Work with Apps、Desktop、語音輸入）
- 更新 Google 索引：Gemini Chat 功能詳述（Canvas/簡報、Deep Research、Veo 3.1、Nano Banana 圖卡、Gem）
- Design：新增 Kotlin + UML 整合段落（JetBrains 工具、LLM 產圖、AI 動態 Mermaid 構想）

---

### Mermaid 時間軸

<pre class="mermaid">
timeline
    title 2026-03-29 知識庫建立日
    section 18:00 基礎建設
        README / 專案結構 : 初始化 repo
        Tools 文件 : scrcpy、Maestro
        目錄重構 : 7 個分類索引頁
        CONTENT_CHARTER : 內容憲章
        GitHub Pages : 部署流程 / layout / CSS
    section 19:00 AI 與語言擴充
        AI 子分類 : OpenAI / Google / Mistral / LLM
        Kotlin 資源 : 5 個學習頁面
        AI Tools : Copilot / HF / LM Studio / Ollama
        Google AI : Cloud IDE / Colab / Gemini CLI
        LLM 模型 : Qwen 3.5 / Gemma 系列
        Rust : Android 整合概述
    section 20:00 App 實測與 PR
        AppFunctions : 整合觀察筆記
        PR 1 : fal.ai 工具介紹
        PR 2 : Krea AI 工具介紹
    section 21:00 Design 與 Books
        Design : Spec-Driven / BDD / DDD / UML
        Books : Clean Code / Clean Architecture
        Workflow : GDG Taipei
    section 22:00 首頁與 AI 深化
        首頁 Mindmap : Mermaid 知識庫全覽
        AI Navigation : 加入主導覽
        ChatGPT : 6 項功能詳述
        Gemini Chat : 5 項功能詳述
        Kotlin + UML : LLM 動態產圖構想
</pre>
