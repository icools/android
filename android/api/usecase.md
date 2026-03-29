---
title: UseCase 筆記
tags:
  - android
  - api
  - architecture
  - usecase
  - domain-layer
desc: Android domain layer 中 UseCase 的責任整理，包含何時需要、如何命名，以及和 ViewModel / Repository 的關係。
author: icools
date: 2026-03-29
source: https://developer.android.com/topic/architecture/domain-layer
status: active
---

# UseCase

## 它的定位

`UseCase` 通常放在 optional 的 domain layer。

Android 官方的說法很清楚：domain layer 是可選的，它位在 UI layer 與 data layer 中間，主要用來承接：

- 複雜 business logic
- 多個 `ViewModel` 重用的邏輯

所以我不會把 `UseCase` 當成每個專案都必須有的標配，而是當成規模長大後很好用的分層工具。

## 什麼時候值得加

我會考慮加 `UseCase` 的情況：

- 一段邏輯被多個畫面重用
- 一個畫面要協調多個 repository
- `ViewModel` 已經開始變胖
- 邏輯本身值得單獨測試與命名

## 什麼時候先不要硬加

如果 app 很小，只有：

- `ViewModel`
- `Repository`

就能清楚表達責任，那我通常不會為了形式硬塞一層。

## 它常見在做什麼

- 整合多個 repository 的資料
- 格式轉換
- 權限 / session / business rule 判斷
- 將一連串 domain 行為包成單一操作

例如：

- `GetProfileUseCase`
- `RefreshFeedUseCase`
- `SubmitOrderUseCase`

## 一個 UseCase 最好只做一件事

Android 官方也建議每個 use case 專注單一功能，並避免持有 mutable data。

這點我很認同，因為：

- 可讀性高
- 好測
- 好重用
- 也不容易變成另一個 god class

## 和 ViewModel / Repository 的關係

典型關係會是：

- `ViewModel` 注入 `UseCase`
- `UseCase` 注入 `Repository`

這樣 `ViewModel` 比較專注在畫面狀態，而不是自己拼裝整段 business logic。

## 命名方式

官方文件裡的慣例是：

- 動詞 + 名詞 + `UseCase`

例如：

- `LogOutUserUseCase`
- `GetLatestNewsUseCase`
- `FormatDateUseCase`

我自己也很常用這種命名，因為一眼就知道它做什麼。

## 我自己的原則

- 不要為了架構層數而架構層數
- 一個 use case 做一件明確的事
- 盡量無狀態
- 可以被單獨測試
- 複雜邏輯再抽，不要一開始全拆碎

## 這篇之後還想補

- `operator fun invoke()` 的寫法
- coroutine / dispatcher 放在哪一層
- use case 組合 use case
- feature module 裡的 domain layer 拆法

## 官方與參考來源

- Android Developers：[Domain layer](https://developer.android.com/topic/architecture/domain-layer)
- Android Developers：[Guide to app architecture](https://developer.android.com/topic/libraries/architecture/guide)

## 來源說明

這篇筆記主要依據 Android Developers 對 domain layer 與 use case 的官方說明整理，並補上我自己對何時值得加這一層的判斷。
