---
title: MVVM 筆記
tags:
  - android
  - api
  - architecture
  - mvvm
  - viewmodel
desc: Android 專案中 MVVM 常見拆法整理，包含 ViewModel、Repository、UseCase 與 DataSource 的責任邊界。
author: icools
date: 2026-03-29
source: https://developer.android.com/topic/libraries/architecture/guide
status: active
---

# MVVM

## 先講我的理解

在 Android 專案裡，`MVVM` 對我來說不是教條，而是一個把責任分清楚的工作分配方式。

我通常會把它拆成：

- `View`：畫面 render 與使用者互動
- `ViewModel`：畫面狀態與畫面層 business logic
- `Repository`：data layer 入口
- `UseCase`：可選的 domain layer，承接複雜或可重用邏輯
- `DataSource`：直接碰 local / remote / system source

## 這樣拆的核心好處

- UI 不直接碰資料來源
- 畫面邏輯不和網路 / DB / SDK 綁死
- 比較容易測試
- 比較容易重構
- 規模變大時不容易每個 class 都變巨石

## 推薦資料流

官方 architecture guide 的主軸其實很接近這個方向：UI layer 往下依賴 data layer，可選擇加上 domain layer。

我最常用的路徑會是：

<pre class="mermaid">
flowchart LR
    UI["UI / Compose / Views"] --> VM["ViewModel"]
    VM --> UC["UseCase (optional)"]
    VM --> REPO["Repository"]
    UC --> REPO
    REPO --> LOCAL["Local DataSource"]
    REPO --> REMOTE["Remote DataSource"]
    REPO --> SYSTEM["System DataSource"]
</pre>

## 每層在做什麼

### View

`View` 的責任是：

- render UI state
- 將 click / input / gesture 轉成 event
- 不直接處理 data source 細節

它盡量不要去寫一堆商業邏輯，也不要自己知道資料從哪來。

### ViewModel

`ViewModel` 是畫面級 state holder。

它的責任通常是：

- 持有 screen UI state
- 接收 UI event
- 呼叫 use case / repository
- 把結果整理成 UI 可直接 render 的狀態

### Repository

`Repository` 是 data layer 的主要入口。

它的責任通常是：

- 對外暴露資料
- 協調多個 data source
- 定義 source of truth
- 抽象化 local / remote / system 細節

### UseCase

`UseCase` 是 optional 的。

如果 app 很小、邏輯很單純，不一定要硬加。

但當你有：

- 多個 `ViewModel` 重用同一段邏輯
- 一段商業規則很複雜
- 一次要協調多個 repository

那 `UseCase` 就會很有價值。

### DataSource

`DataSource` 是最貼近資料來源的那層。

例如：

- Retrofit API
- Room DAO wrapper
- DataStore wrapper
- ContentResolver / system service wrapper

它應該盡量薄，不要塞一堆跨來源 business logic。

## 我會怎麼判斷要不要加 UseCase

我不會一開始就把所有專案都做成很厚的 clean architecture。

我的判斷通常是：

- 小 app：`ViewModel -> Repository` 就很夠
- 中型 app：開始抽部分 `UseCase`
- 複雜 app：`UseCase` 會越來越有必要

重點不是層數越多越高級，而是責任切得剛好。

## 和 Flow / StateFlow 的關係

`MVVM` 本身不等於一定要用某種 reactive API，但在 Kotlin Android 專案裡，我通常會搭：

- `ViewModel` 對 UI 暴露 `StateFlow`
- `Repository` 對上游暴露 `Flow`
- 特定 event 或共享訊號再考慮 `SharedFlow`

更細的整理我另外放在 [ViewModel、StateFlow、SharedFlow、Flow、Channel](viewmodel-flow.md)。

## 我自己的拆法原則

- UI state 盡量 immutable
- `ViewModel` 不直接碰 Retrofit / Room
- `Repository` 不直接長出 UI state
- `UseCase` 一個 class 只做一個明確功能
- `DataSource` 一次只面對一個 source

## 很常見的反模式

- `ViewModel` 直接打 API
- `Repository` 同時負責 UI state 與 navigation event
- `UseCase` 只是薄薄轉呼叫但沒有帶來可讀性
- `DataSource` 裡塞一堆跨來源協調邏輯
- 每個小功能都硬拆超多層，結果成本高於收益

## 這篇之後還想補

- Compose 專案裡的 MVVM 實作骨架
- 多 module feature 的拆法
- `ViewModel + SavedStateHandle`
- offline-first 下 repository 的 source-of-truth 設計
- `Worker` 放在架構裡的定位

## 官方與參考來源

- Android Developers：[Guide to app architecture](https://developer.android.com/topic/libraries/architecture/guide)
- Android Developers：[UI layer](https://developer.android.com/topic/architecture/ui-layer)
- Android Developers：[Data layer](https://developer.android.com/topic/architecture/data-layer)
- Android Developers：[Domain layer](https://developer.android.com/topic/architecture/domain-layer)

## 來源說明

這篇筆記以 Android 官方 architecture guidance 為主，並加入我自己在 Android 專案裡對 MVVM 分層的實務理解。
