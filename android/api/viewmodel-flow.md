---
title: ViewModel、StateFlow、SharedFlow、Flow、Channel 筆記
tags:
  - android
  - api
  - architecture
  - viewmodel
  - flow
  - stateflow
  - sharedflow
  - channel
desc: Android 畫面狀態與事件流的常見拆法整理，包含 ViewModel、Flow、StateFlow、SharedFlow 與 Channel 的使用時機。
author: icools
date: 2026-03-29
source: https://developer.android.com/kotlin/flow/stateflow-and-sharedflow
status: active
---

# ViewModel、StateFlow、SharedFlow、Flow、Channel

## 先講我最常用的答案

如果是在 Android 畫面層，我通常會先這樣想：

- 畫面狀態：`StateFlow`
- repository / use case 的資料流：`Flow`
- 廣播型事件或共享訊號：`SharedFlow`
- 真正需要 queue / producer-consumer 語意時：`Channel`

也就是說，大多數畫面都不是從 `Channel` 開始想，而是先問：

「這是狀態，還是事件？」

## ViewModel 的角色

官方文件把 `ViewModel` 定位成 screen-level state holder，這個定位我很認同。

它最核心的責任是：

- 持有畫面狀態
- 處理使用者事件
- 呼叫 repository / use case
- 對 UI 暴露穩定、可觀察、可重建的 state

所以在 Kotlin Android 專案裡，`ViewModel` 很適合成為 `StateFlow` 的擁有者。

## 五個概念的差異

| 類型 | 熱 / 冷 | 有沒有目前值 | 最適合 | 我通常放哪 |
|---|---|---|---|---|
| `Flow` | 冷 | 沒有 | 資料流轉換、上游資料來源 | repository / use case |
| `StateFlow` | 熱 | 有 | 畫面狀態 | ViewModel -> UI |
| `SharedFlow` | 熱 | 可 replay | 廣播型事件、共享訊號 | ViewModel 或 app scope |
| `Channel` | 熱 | 沒有 | queue、單次消費、producer-consumer | 特定 pipeline |
| `ViewModel` | 狀態持有者 | 可持有 state | 畫面狀態管理 | UI layer |

## Flow

`Flow` 最適合拿來描述「一段可被收集的資料流」。

我通常用它在：

- repository 對外暴露資料
- DAO / DataStore / network stream 往上傳
- use case 對資料做 map / combine / filter

它是 cold stream，所以每次 collect 都可能重新啟動上游。

這很適合 data pipeline，但單靠它本身不一定是最好的畫面 state holder。

## StateFlow

`StateFlow` 很適合畫面狀態，因為它：

- 永遠有一個 current value
- 新 collector 一上來就能拿到最新值
- 非常符合 UI render 需求

Android 官方也明確把 `StateFlow` 視為很適合維護 observable mutable state 的做法。

所以我通常會讓 `ViewModel` 對 UI 暴露：

- `uiState: StateFlow<UiState>`

這種結構最好理解，也最符合 state-driven UI。

## SharedFlow

`SharedFlow` 是 hot flow，而且可以把值廣播給多個 collector。

我通常會拿它處理：

- app 級共享訊號
- tick / refresh signal
- 需要 fan-out 的事件流

如果只是單一畫面的一次性 UI effect，我會先想清楚：

- 這其實能不能建模成 state？
- 如果不是 state，才再考慮 `SharedFlow`

## Channel

`Channel` 比較像 coroutine 之間的 queue。

它適合：

- producer-consumer pipeline
- 一筆一筆消費的任務流
- 明確需要 send / receive 語意的場景

Kotlin 官方文件也把它描述成 coroutine 間傳遞 stream values 的通信原語。

但在 Android UI 層，我通常不會先用 `Channel` 當第一選擇，因為：

- 它比較偏 queue 語意
- 比較容易讓畫面事件流變難懂
- 很多一次性 UI 問題其實應該先用 state-first 思維處理

## 我自己的實戰拆法

### Repository / UseCase

- 儘量回傳 `Flow`
- 保持資料流乾淨、可組合

### ViewModel

- 將上游 `Flow` 轉成 `StateFlow`
- 用 immutable `UiState` 給 UI

### UI

- collect `StateFlow`
- 依 state render，不直接保存第二份來源不明的狀態

## 一個很常見的心智模型

<pre class="mermaid">
flowchart LR
    DATA["Repository / UseCase Flow"] --> VM["ViewModel"]
    VM --> STATE["StateFlow<UiState>"]
    VM --> EVENT["SharedFlow / Channel (if needed)"]
    STATE --> UI["UI Render"]
    EVENT --> UI
</pre>

## 我會怎麼選

- 畫面常駐狀態：`StateFlow`
- 多來源資料轉換：`Flow`
- 多 collector 共享訊號：`SharedFlow`
- 真正 queue 型工作流：`Channel`

如果你不確定，先從：

- `ViewModel + StateFlow`
- `Repository + Flow`

開始，通常最穩。

## 常見反模式

- 把所有東西都丟進 `SharedFlow`
- 把 `Channel` 當成萬用一次性事件容器
- UI 自己再存第二份 mutable state
- `ViewModel` 對 UI 同時暴露很多互相打架的 streams

## 這篇之後還想補

- Compose collect 最佳實務
- `stateIn` / `shareIn` 的使用時機
- one-off event 的建模方式
- `SavedStateHandle` 與 state restore
- `Channel` 在非 UI pipeline 的更適合情境

## 官方與參考來源

- Android Developers：[ViewModel overview](https://developer.android.com/topic/libraries/architecture/viewmodel)
- Android Developers：[State holders and UI state](https://developer.android.com/topic/architecture/ui-layer/stateholders)
- Android Developers：[StateFlow and SharedFlow](https://developer.android.com/kotlin/flow/stateflow-and-sharedflow)
- Kotlin Documentation：[Asynchronous Flow](https://kotlinlang.org/docs/flow.html)
- Kotlin Documentation：[Channels](https://kotlinlang.org/docs/channels.html)

## 來源說明

這篇筆記主要根據 Android Developers 對 `ViewModel`、`StateFlow`、`SharedFlow` 的文件，以及 Kotlin 官方對 `Flow`、`Channel` 的說明整理，再加上我自己在 Android UI 層的常用拆法。
