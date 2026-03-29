---
title: Worker / WorkManager 筆記
tags:
  - android
  - api
  - workmanager
  - worker
  - background
desc: WorkManager 與 Worker 的使用情境整理，包含背景工作、constraints、retry、unique work、chain 與 Hilt 整合。
author: icools
date: 2026-03-29
source: https://developer.android.com/develop/background-work/background-tasks/persistent/getting-started
status: active
---

# Worker / WorkManager

## 它是做什麼的

如果你要在 Android 上做「即使 app 離開畫面、甚至重啟後還想繼續執行」的背景工作，`WorkManager` 幾乎是官方首推方案。

`Worker` 是你實際執行工作的 class，`WorkManager` 則是排程、約束、重試、持久化與狀態管理的入口。

我會把它理解成：

- 背景工作的標準排程層
- 幫你把 work 撐過 app process 結束與裝置重啟
- 幫你處理 constraints、retry、chain、觀察狀態

## 什麼時候適合用

很適合的情境：

- 上傳檔案
- 背景同步
- 延後執行的資料清理
- 需要 retry 的網路工作
- 週期性 refresh
- app 不在前景時也希望有機會完成的任務

## 什麼時候不要硬用

如果是下面這些情況，通常不要先想到 `WorkManager`：

- 畫面還在前景，單純要跑一段 async logic
- 必須精準在某一秒執行
- 和使用者當下互動強綁定的短工作

這些情況很多時候用 coroutine、foreground service 或其他 background API 會更適合。

## Worker 幾種常見型別

- `Worker`：最基本型，適合同步風格實作。
- `CoroutineWorker`：Kotlin 專案通常最順手，適合 suspend API。
- `ListenableWorker`：彈性最高，但也最偏底層。

如果你是 Kotlin 專案，我通常會優先從 `CoroutineWorker` 開始想。

## 核心概念

### 1. WorkRequest

`Worker` 定義要做什麼，`WorkRequest` 定義它什麼時候跑、如何跑。

常見有：

- one-time work
- periodic work

### 2. Constraints

你可以限制工作必須在特定條件下才跑，例如：

- 有網路
- 充電中
- 裝置閒置
- 電量狀況符合要求

這很適合用在同步與批次背景任務。

### 3. Result

worker 執行完後通常會回：

- `success`
- `failure`
- `retry`

這也是 `WorkManager` 很實用的一點，因為失敗後不必你自己從頭兜整個重試機制。

### 4. Unique work / chain

如果你不想同一類工作重複排一堆，可以用 unique work。

如果一段工作有順序，例如：

1. 先抓 token
2. 再上傳資料
3. 再同步結果

那就很適合用 chain 方式串起來。

### 5. Input / Output Data

worker 可以接 input，也可以回 output，適合在 chain 中往後傳遞必要資料。

## 官方邊界上很重要的一點

Android Developers 的說法很明確：`WorkManager` 是拿來做 persistent work 的推薦方案。

它不是萬能背景 API，也不是所有事情都該塞進 worker。特別是當工作其實只是畫面內的 async 任務時，直接用 coroutine 通常更單純。

另外，`Worker` 有執行時間限制。官方 reference 也提到一般 background work 不是給你無限長跑的，所以設計上要把工作切成可完成、可重試、可觀察的小單元。

## 和 MVVM 的關係

我通常不會把 `Worker` 看成 MVVM 主流程的一部分，而是比較像：

- data / sync pipeline 的 sidecar
- app lifecycle 之外的背景執行者

常見關係會是：

- `ViewModel` 觸發 enqueue
- `Worker` 執行 repository / use case
- `Repository` 寫回 source of truth
- `UI` 再透過 flow 看到資料更新

## 和 Hilt 怎麼整合

如果 worker 需要注入 dependency，可以配合：

- `@HiltWorker`
- `HiltWorkerFactory`

這樣 `Worker` 可以直接吃到 repository、api client 或 local storage 等依賴，而不用手動組裝。

## 我會怎麼拆

- `Worker` 只做工作流程控制
- 真正的 business logic 盡量放在 `UseCase` 或 `Repository`
- worker 裡避免長太多 UI 或 feature-specific 判斷
- 複雜工作用 unique work + chain 管理
- 有 retry 的工作一定要把 idempotency 想清楚

## 常見使用情境

- 離線佇列補送
- 定期同步遠端資料
- 圖片 / 影片背景上傳
- 失敗重試型 API 呼叫
- 清理 cache / housekeeping
- app 啟動後延後執行的 maintenance 工作

## 這篇之後還想補

- `CoroutineWorker` 範例
- input / output data 怎麼設計
- unique work policy 比較
- chain work 的實戰拆法
- 和 foreground service 的分工
- `HiltWorker` 實戰

## 官方與參考來源

- Android Developers：[Getting started with WorkManager](https://developer.android.com/develop/background-work/background-tasks/persistent/getting-started)
- Android Developers：[Define work requests](https://developer.android.com/develop/background-work/background-tasks/persistent/getting-started/define-work)
- Android Developers：[WorkManager](https://developer.android.com/reference/androidx/work/WorkManager)
- Android Developers：[Background tasks overview](https://developer.android.com/develop/background-work/background-tasks)

## 來源說明

這篇筆記主要根據 Android Developers 對 WorkManager 與背景工作的官方說明整理，並補上我自己在 Android 專案裡的實際分工理解。
