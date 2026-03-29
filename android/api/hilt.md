---
title: Hilt 筆記
tags:
  - android
  - api
  - hilt
  - dependency-injection
  - architecture
desc: Hilt 在 Android 專案中的定位與常用組件整理，包含 component、scope、module、ViewModel 與 Worker 注入。
author: icools
date: 2026-03-29
source: https://developer.android.com/training/dependency-injection/hilt-android
status: active
---

# Hilt

## 它是做什麼的

`Hilt` 是 Android 上最主流的依賴注入方案之一，可以把原本手動 new 出來、層層往下傳的 dependency，改成由 container 自動建立、管理生命週期，再注入到需要的 class。

我會把它理解成：

- 幫你管理 object graph
- 把建立依賴的責任從 UI / feature class 拆出去
- 讓 `ViewModel`、`Repository`、`UseCase`、`Worker` 比較容易測試與替換

它是建立在 `Dagger` 之上的 Android 封裝，重點是把 Android 常見 entry point 都先包好了。

## 為什麼在 Android 特別重要

Android app 天生就很多生命週期與 entry point：

- `Application`
- `Activity`
- `Fragment`
- `Service`
- `ViewModel`
- `Worker`

如果沒有 DI，這些地方很容易變成手動組裝依賴的大雜燴。`Hilt` 的價值就是把這些注入點標準化，讓整個專案的依賴關係比較一致。

## 最常用的幾個註解

- `@HiltAndroidApp`：放在 `Application`，建立整個 app 的 Hilt root。
- `@AndroidEntryPoint`：讓 `Activity`、`Fragment`、`Service`、`View` 等 Android class 可以被注入。
- `@Inject`：用在 constructor 或 field，表示這個依賴要交給 Hilt 建立。
- `@HiltViewModel`：讓 `ViewModel` 交給 Hilt 管理。
- `@Module + @InstallIn(...)`：定義無法用 constructor injection 直接建立的依賴。
- `@Provides`：手動提供 instance。
- `@Binds`：用介面綁到實作。

## 常見 component / scope

官方文件裡很重要的一個概念是 component 與它對應的生命週期。

- `SingletonComponent`：app 存活期間都在。
- `ActivityRetainedComponent`：跨 configuration change 保留。
- `ViewModelComponent`：跟著 `ViewModel` 活著。
- `ActivityComponent`：跟著 `Activity`。
- `FragmentComponent`：跟著 `Fragment`。
- `ServiceComponent`：跟著 `Service`。

如果一個 object 應該全 app 共用，通常會放到 `SingletonComponent`。如果它只該活在某個畫面狀態範圍內，通常就不要亂升 scope。

## 我最常用的拆法

在一般 app 中，我通常會這樣放：

- `Retrofit`、`Room`、`DataStore`、sdk client：`Singleton`
- `Repository`：多半也是 `Singleton`
- `UseCase`：通常不一定要 scope，讓 Hilt 直接建就好
- `ViewModel`：用 `@HiltViewModel`
- `Worker`：如果需要依賴注入，搭配 `HiltWorker`

## Hilt 在 MVVM 裡怎麼接

最常見的路徑會是：

`Hilt` 建立 `ViewModel`，`ViewModel` 注入 `UseCase` 或 `Repository`，`Repository` 再注入 `DataSource`。

<pre class="mermaid">
flowchart LR
    APP["Hilt Container"] --> VM["ViewModel"]
    APP --> UC["UseCase"]
    APP --> REPO["Repository"]
    APP --> DS["DataSource"]
    VM --> UC
    VM --> REPO
    UC --> REPO
    REPO --> DS
</pre>

## 什麼時候要寫 Module

不是所有 class 都需要 `@Module`。

如果 class 可以直接 constructor injection，例如：

- `RepositoryImpl(api, dao)`
- `GetProfileUseCase(repository)`

那通常只要在 constructor 上標 `@Inject` 就夠了。

比較常需要 `@Module` 的情況是：

- 第三方 library instance
- interface 綁定到 implementation
- 需要 custom config 才能建立的 object
- 不能直接改 constructor 的類型

## 和 Worker 怎麼整合

`WorkManager` 的 `Worker` 不是一般的 Android component 初始化方式，所以如果要整合 Hilt，通常會用：

- `@HiltWorker`
- `HiltWorkerFactory`

這樣 `Worker` 才能在執行背景工作時拿到 repository 或其他 singleton dependency。

## 我自己的使用原則

- 能 constructor inject 就不要先寫 module
- scope 盡量保守，不要為了省事全部做成 singleton
- `ViewModel` 注入 use case / repository，不直接碰太底層依賴
- `Repository` 才是 data source 的入口
- `Worker` 裡不要長出 UI 邏輯

## 這篇之後還想補

- multi-module 專案下的 Hilt 拆法
- `@Binds` 和 `@Provides` 的選擇時機
- `SavedStateHandle` 與 `@HiltViewModel`
- `HiltWorker` 的完整寫法
- test double / fake repository 注入

## 官方與參考來源

- Android Developers：[Dependency injection with Hilt](https://developer.android.com/training/dependency-injection/hilt-android)
- Android Developers：[HiltWorker](https://developer.android.com/reference/androidx/hilt/work/HiltWorker)
- Android Developers：[HiltWorkerFactory](https://developer.android.com/reference/androidx/hilt/work/HiltWorkerFactory)

## 來源說明

這篇筆記主要根據 Android Developers 的 Hilt 文件整理，並補上我自己在 Android 專案裡比較常用的實務拆法。
