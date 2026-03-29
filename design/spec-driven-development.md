---
title: Coding 的 SDD：Spec-Driven Development 與 Design Document 的差異
tags:
  - design
  - sdd
  - spec-driven-development
  - software-design-document
  - coding
  - workflow
desc: 整理 Coding 脈絡常提到的 SDD 是什麼，並區分 Software Design Document 與 Spec-Driven Development 的差異與實際用法。
author: icools
date: 2026-03-29
source: local
status: active
---

# Coding 的 SDD：Spec-Driven Development 與 Design Document 的差異

最近在 AI Coding、agent workflow 或一些工程討論裡面，常常會看到 `SDD` 這個縮寫。

但它其實很容易讓人誤會，因為 `SDD` 可能代表兩種東西：

- `Software Design Document`
- `Spec-Driven Development`

如果沒有先講清楚，就很容易雞同鴨講。有人在講的是設計文件，有人講的是先寫 spec 再讓人或 AI 依 spec 去實作的流程。

## 我會怎麼理解這兩種 SDD

### 1. Software Design Document

這比較偏傳統工程文件。

它的重點通常是：

- 這個功能或系統要怎麼設計
- 模組怎麼切
- 資料流怎麼走
- API 怎麼長
- 邊界條件有哪些
- 風險與限制是什麼

它的用途比較像是把設計講清楚，方便討論、審查與後續維護。

### 2. Spec-Driven Development

這比較像一種開發節奏與工作方式。

它的核心不是先急著寫 code，而是先把下面這些事情說清楚：

- 問題到底是什麼
- 目標是什麼
- 成功條件是什麼
- 不做什麼
- 要遵守哪些限制
- 輸入輸出該長怎樣
- 驗證方式是什麼

講白一點，就是先把規格釘好，再進入實作。

## 為什麼現在 Coding 很愛講 Spec-Driven

因為現在不只是人寫 code，很多時候是人和 AI 一起寫。

如果你一開始只丟一句模糊需求，例如：

- 幫我做一個登入頁
- 幫我把這段 code 重構一下
- 幫我支援 Android Auto

AI 很可能會很勤勞地幫你寫出一堆東西，但方向不一定對。

真正容易出事的，通常不是模型不會寫，而是：

- 需求不夠清楚
- 邊界沒講清楚
- 驗收標準沒定義
- 原本專案的規則沒有一起帶進去

所以 `Spec-Driven Development` 的價值，在 AI 時代反而變得更高。

而且它現在不只是抽象概念。我在 `GDG Taipei` 聽到把 `Antigravity` 和 `SDD` 一起講的內容後，最大的感受就是：當 agent 類工具開始真的會 `Plan`、`Act`、`Verify`，spec 的角色就會直接從文件升級成工作流控制面板。

如果再把這件事往前推一步，我會覺得現在很多工具開始強調的 `plan mode`，其實就是把 `SDD` 拉進日常工作流的入口。你不是先叫 AI 寫，而是先讓它問清楚需求、列出規劃、讓你 review，再往下拆 task。

## 我自己偏好的 SDD 長相

如果今天是偏功能或模組開發，我會希望 spec 至少要有下面幾塊：

### 問題定義

這次到底要解決什麼問題，不要直接跳解法。

### 目標與非目標

這次要做到什麼，以及明確列出這次不打算處理什麼。

### 使用情境

誰會用、什麼時候用、使用流程長怎樣。

### 資料與輸入輸出

輸入是什麼，輸出是什麼，錯誤情況怎麼處理。

### 限制條件

例如：

- 只能改某幾層
- 不能破壞既有 API
- 要符合既有架構
- 要可測試
- 要支援某版本 Android

### 驗證方式

最後怎麼知道你真的做對了。

例如：

- 單元測試
- 手動測試步驟
- UI 驗證
- log 驗證
- benchmark

## 它和 Design Document 的關係

我不會把兩者看成互斥。

比較像是：

- `Software Design Document` 偏設計描述
- `Spec-Driven Development` 偏執行流程與工作方法

有時候一份好的 spec，本身也會包含 design thinking。

有時候一份設計文件，也會直接成為後續開發的 spec。

差別只是在重點放哪裡。

## 我覺得它最適合的地方

`Spec-Driven Development` 很適合用在這幾種情境：

- 跟 AI 協作寫 code
- 需求很容易被誤解的功能
- 需要跨人協作的模組
- 想降低反覆重工的任務
- 要讓未來的自己回來看也看得懂

如果再用更實戰的角度看，我會再補三種很典型的場景：

- `新功能還在收斂中`：先寫 spec，再讓 AI 補 plan 和限制條件，比直接開做穩很多。
- `UI / 互動細節很多`：像相簿、lightbox、Dark / Light mode、切換手勢、循環瀏覽這種功能，如果不先把行為規格講清楚，AI 很容易做出看似能跑、但其實不符合需求的版本。
- `多人協作、跨 session、重構`：spec、plan、acceptance criteria 一旦寫清楚，交接與驗收都會順很多，也比較能保護原本不能壞掉的行為。

我覺得這也是它最值得的地方：它不只是幫你把需求寫漂亮，而是讓未來的實作、測試、驗收和維護都有共同依據。

它不是讓你多寫一份官樣文章，而是讓你先把方向釘住，避免開發過程一路滑坡。

如果想看我把 `plan mode`、`spec kit`、`Mermaid`、`pseudo code` 和這整套工程化思路整理成另一篇完整心得，也可以接著看這篇：[Plan Mode、SpecKit 與 SDD：先把需求問清楚，再讓 AI 開始做](../workflow/plan-mode-spec-kit-and-sdd.md)。

## 很務實的一句話

如果你今天只是寫個超小 utility，也不一定要搞得像在寫論文。

但只要需求開始變複雜、會牽涉多人、會交給 AI、或者會影響核心邏輯，先把 spec 寫出來，通常都比事後猜來猜去省很多時間。

## 我的簡單結論

如果你在 Coding 討論裡看到 `SDD`，第一件事不是急著點頭，而是先確認他講的是哪一種。

我自己會這樣記：

- `Software Design Document`：偏設計文件
- `Spec-Driven Development`：偏規格驅動的開發方式

而在現在的 AI Coding 脈絡下，後者真的越來越重要。

如果想看我把這件事放回實際活動心得裡怎麼理解，也可以參考這篇：[不要當 code monkey：我在 GDG Taipei 聽完 Antigravity 與 Spec-Driven Development 的心得](../workflow/android-gdg-taipei.md)。
