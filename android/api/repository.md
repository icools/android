---
title: Repository 筆記
tags:
  - android
  - api
  - architecture
  - repository
  - data-layer
desc: Android data layer 中 Repository 的責任整理，包含 source of truth、多 data source 協調與對外暴露資料。
author: icools
date: 2026-03-29
source: https://developer.android.com/topic/architecture/data-layer
status: active
---

# Repository

## 它的定位

`Repository` 是 data layer 對外的主要入口。

Android 官方也很明確地把 repository 定位成：

- 對 app 其他部分暴露資料
- 集中資料變更
- 協調多個 data source
- 抽象資料來源細節
- 承接部分 business logic

所以我通常不會讓 `ViewModel` 直接依賴 `RemoteDataSource` 或 `LocalDataSource`，而是讓它只看到 repository。

## Repository 最重要的價值

### 1. 隔離資料來源

上層不需要知道資料是來自：

- network
- room
- datastore
- cache
- system service

這樣之後要改實作時，上層不會跟著整片震。

### 2. 定義 source of truth

一份資料最終以哪裡為準，通常應該由 repository 決定。

如果沒有 source of truth，專案很容易同時有：

- API 回來一份
- DB 一份
- memory cache 一份

最後彼此不一致。

### 3. 協調多來源

例如：

- 先讀 local
- 再決定要不要打 remote
- 合併結果
- 寫回 DB

這種事情放 repository 通常最合理。

## 它不該做什麼

- 不應該直接管理 UI state
- 不應該長出 navigation 邏輯
- 不應該承擔 ViewModel 責任
- 不應該變成所有 feature 雜湊的大總管

## 我常見的拆法

- `UserRepository`
- `SessionRepository`
- `ArticleRepository`
- `PaymentsRepository`

也就是盡量按「資料責任」來命名，而不是按畫面命名。

## 單一 data source 時怎麼看

官方文件也提到一個很實際的情況：如果一開始只有單一 data source，有時 repository 跟 data source 的責任會暫時靠得很近。

我覺得這很合理，但要保留一個意識：

如果之後資料來源變多，就要願意把 data source 再拆出來。

## 和 ViewModel / UseCase 的關係

- `ViewModel` 讀 repository
- `UseCase` 也常常讀 repository
- `Repository` 再往下讀 data source

所以 repository 很像是 app 資料世界的 service boundary。

## 我自己的原則

- 對外暴露 immutable data
- 盡量 main-safe
- 把跨來源同步邏輯集中
- 命名用資料責任，不用技術細節
- 不要讓所有 repository 彼此亂依賴

## 這篇之後還想補

- offline-first repository
- cache policy
- source-of-truth 實戰
- 多 repository 協調
- repository 測試策略

## 官方與參考來源

- Android Developers：[Data layer](https://developer.android.com/topic/architecture/data-layer)
- Android Developers：[Guide to app architecture](https://developer.android.com/topic/libraries/architecture/guide)

## 來源說明

這篇筆記主要依據 Android Developers 的 data layer 與 architecture guide 整理，並補上我自己對 repository 邊界的實務理解。
