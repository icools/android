---
title: Android Studio Skills
tags:
  - ai
  - google
  - android-studio
  - skills
  - gemini
desc: 整理 Android Studio Agent Mode 的 Skills 機制、目錄結構與實作重點。
author: icools
date: 2026-03-29
source: https://developer.android.com/studio/preview/skills
status: active
---

# Android Studio Skills

Android Studio Preview 已經可以在 Agent Mode 裡加入 project-local skill，代表現在可以直接在純 Android Studio 裡維護 skill，不一定要依賴外部 AI agent 工具鏈。

這套機制是以 skill folder 為單位，讓 agent 在需要時動態載入特定專長，而不是把所有規則都塞進固定上下文。

## 這功能在做什麼

- 把特定工作流包成 skill，例如 UI 調整、版本升級、測試流程、review checklist。
- Agent 先只讀 skill metadata，不會一開始把全部內容吃進 context。
- 當使用者請 agent 做和 skill 相關的事時，Android Studio 才會載入完整 `SKILL.md` 與必要資源。
- 也可以用 `@` 明確指定某個 skill。

## Skill 放哪裡

Android Studio 會從 project root 的以下位置找 skill：

- `.skills/`
- `.agent/skills/`

每個 skill 都要有自己獨立的資料夾，而且裡面必須有一個大小寫固定的 `SKILL.md`。

## 最小結構

```text
.skills/
└─ android-review/
   └─ SKILL.md
```

也可以再往下分組：

```text
.skills/
└─ testing/
   └─ screenshot-regression/
      └─ SKILL.md
```

## SKILL.md 格式

`SKILL.md` 會分成兩段：

- 上半段是 YAML metadata
- 下半段是 Markdown instructions

範例：

```md
---
name: android-review
description: Review Android UI and implementation changes, and use this skill when the task involves screenshots, Compose regressions, or review checklists.
metadata:
  author: icools
  version: "1.0"
---

檢查 UI 變更時，先確認畫面層級、互動流程與裝置差異。
如果需要比對 screenshot，優先檢查 baseline 與目前版面的差異來源。
```

## 格式限制

- `name` 要和 skill directory name 對齊。
- `name` 最多 64 字元，只能用小寫英數與 `-`。
- `description` 最多 1024 字元。
- `SKILL.md` 內容建議控制在約 10k 到 20k 字元，太長的細節可以拆到外部資源。

## 可選目錄

如果 skill 不只是一段說明，還可以把配套資源一起放進 skill folder：

- `scripts/`：可執行腳本，例如 Python 或 shell。
- `references/`：較長的技術文件、規範或內部指南。
- `assets/`：模板、schema、圖示或其他靜態素材。

在 `SKILL.md` 內可以直接用相對路徑引用，例如：

```md
先執行 `scripts/check-snapshots.sh`，再依照 `references/review-guide.md` 的規則整理結果。
```

## 和 AGENTS.md 的差別

- `AGENTS.md` 比較像全域工作規則。
- skill 比較像按需載入的專門能力。
- skill 適合拆出高專業度、低頻但可重用的流程。
- 對大型專案比較重要，因為不用把所有規範都塞進 agent 的預設上下文。

## 適合放進 Android Studio Skill 的內容

- Compose UI 檢查流程
- screenshot regression 檢查步驟
- app module 命名與分層規範
- release checklist
- migration playbook，例如 AGP / Kotlin / Compose 升版
- team 內部 code review rubric

## 我目前的理解

這功能很適合把 Android 團隊內本來散落在 wiki、README、PR comment 裡的隱性流程，收斂成 Android Studio 可以直接調用的 skill。對日常開發來說，重點不是「多一份 prompt」，而是把可重用工作流直接放進 repo，讓 agent 在 IDE 內就能吃到正確脈絡。

如果 focus 不只是 skill，而是 `Agent Mode` 怎麼先規劃、再 review、再動手，我也另外整理了一篇把 `Android Studio`、`Codex`、`Antigravity` 放在一起看的心得：[Plan Mode、SpecKit 與 SDD：先把需求問清楚，再讓 AI 開始做](../../workflow/plan-mode-spec-kit-and-sdd.md)。

## 參考

- [Extend Agent Mode with skills](https://developer.android.com/studio/preview/skills)
