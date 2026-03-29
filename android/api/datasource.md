---
title: DataSource 筆記
tags:
  - android
  - api
  - architecture
  - datasource
  - data-layer
desc: Android data layer 中 DataSource 的責任整理，包含 local / remote / system source 的常見拆法。
author: icools
date: 2026-03-29
source: https://developer.android.com/topic/architecture/data-layer
status: active
---

# DataSource

## 它的定位

`DataSource` 是最貼近單一資料來源的那層。

Android 官方對 data source 的定義很直接：每個 data source class 應該只負責一個 source，例如：

- network
- local database
- file
- system source

所以它比較像是 app 和某個資料來源之間的 bridge。

## 常見類型

### RemoteDataSource

例如：

- Retrofit service wrapper
- GraphQL client wrapper
- 第三方雲端 SDK wrapper

### LocalDataSource

例如：

- Room DAO wrapper
- DataStore wrapper
- 檔案讀寫 wrapper

### SystemDataSource

例如：

- `ContentResolver`
- `ClipboardManager`
- `LocationManager`
- `PackageManager`

## 它應該有多薄

我通常希望 data source 盡量薄。

也就是說，它主要負責：

- 和 source 溝通
- 封裝 source API
- 回傳原始或接近原始的資料

而不是在這裡塞進大量跨來源 business logic。

## 它和 Repository 的分工

這個分工最重要：

- `DataSource`：只面對一個 source
- `Repository`：面對 app 其他層，協調多個 source

如果一段邏輯需要：

- network + db
- cache + api
- local + system service

通常那就比較像 repository 的工作，不是 data source 的工作。

## 命名方式

Android 官方建議用：

- `NewsRemoteDataSource`
- `NewsLocalDataSource`

這種命名很清楚，也能保留之後換實作的空間。

我通常會盡量避免把技術細節直接寫死在上層 API 裡，除非現在就是遷移期或那個技術細節本身真的很重要。

## 我自己的原則

- 一個 data source 對應一個 source
- 盡量 main-safe
- 不要長出 UI state
- 不要協調多來源
- 可以被 repository 替換或 mock

## 這篇之後還想補

- Room / DAO 與 local data source
- Retrofit 與 remote data source
- DataStore 的封裝方式
- system service wrapper 設計
- 測試時 fake data source 的寫法

## 官方與參考來源

- Android Developers：[Data layer](https://developer.android.com/topic/architecture/data-layer)
- Android Developers：[Guide to app architecture](https://developer.android.com/topic/libraries/architecture/guide)

## 來源說明

這篇筆記主要依據 Android Developers 的 data layer 文件整理，並補上我自己在 Android 專案裡對 data source 責任邊界的理解。
