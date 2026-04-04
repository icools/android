---
title: Android API 索引
tags:
  - android
  - api
  - index
  - architecture
desc: Android API 與常用架構元件索引頁，整理 Hilt、Worker、MVVM、ViewModel、Repository、UseCase 與 DataSource。
author: icools
date: 2026-03-29
source: local
status: active
---

# Android API

這一頁整理 Android 平台 API 與我常用的架構拆法，詳細筆記會放在 `android/api/`。

- [Accessibility API](accessibility-api.md) - 讀取 UI tree、監聽事件、執行 click / scroll / set text，以及和自動化流程混用時的能力邊界。
- [Hilt](hilt.md) - Android 上最常見的 DI 實作，重點是 component、scope、module 與 `ViewModel` / `Worker` 注入。
- [Worker / WorkManager](worker.md) - 背景工作排程、重試、constraints、chain 與持久化工作。
- [MVVM](mvvm.md) - Android 常見的分層方式，包含 `ViewModel`、`Repository`、`UseCase`、`DataSource` 的關係。
- [ViewModel、StateFlow、SharedFlow、Flow、Channel](viewmodel-flow.md) - 畫面狀態與事件流怎麼拆，什麼該用 `StateFlow`，什麼時候才考慮 `SharedFlow` 或 `Channel`。
- [Repository](repository.md) - Data layer 的主要入口，負責整合多個 data source 與對外暴露資料。
- [UseCase](usecase.md) - 可選的 domain layer，適合承接複雜或可重用的 business logic。
- [DataSource](datasource.md) - local / remote / system data source 的責任邊界。
- [JavaScriptEngine & WASM](web_javascriptengine_wasm.md) - 在 Android 10+ 上利用沙盒執行 JS/WASM 的能力，適合做為「快速解析規則層」或動態熱修補。
- [Android AppFunctions 實測](android-app-functions-first-look.md) - 深入評測 Android 17 引入的 AppFunctions 架構與實戰經驗。

## 這組筆記怎麼搭

<pre class="mermaid">
flowchart LR
    UI["UI / Compose / Views"] --> VM["ViewModel"]
    VM --> UC["UseCase (optional)"]
    VM --> REPO["Repository"]
    UC --> REPO
    REPO --> DS["DataSource"]
    HILT["Hilt"] -. inject .-> VM
    HILT -. inject .-> UC
    HILT -. inject .-> REPO
    HILT -. inject .-> DS
    WORKER["Worker"] --> REPO
</pre>

## 之後可整理的方向

- Room / DAO
- Retrofit / Ktor
- DataStore
- foreground service
- notification
- navigation
