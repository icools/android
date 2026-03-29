---
title: Shizuku 工具介紹
tags:
  - android
  - tool
  - shizuku
  - adb
  - automation
  - testing
desc: Shizuku 工具介紹，整理它如何在無 Root 情況下橋接系統 API、常見使用情境、測試方向與啟動方式。
author: icools
date: 2026-03-29
source: https://shizuku.rikka.app/introduction/
status: active
---

# Shizuku

## 工具簡介

`Shizuku` 是 Android 生態裡很有代表性的開源工具，它的核心價值是讓一般 App 在「不 Root」的前提下，也能比較優雅地使用原本偏系統層、偏高權限的能力。

如果用一句話來講，我會把它理解成：

「把原本只能靠 `root shell` 或 `adb shell` 繞進去做的事情，變成一條比較像正式 API 的橋。」

這也是為什麼我覺得它非常強。它不是單純多一個命令列技巧，而是讓很多 Android 進階工具、測試工具、裝置管理工具，有機會在非 Root 裝置上也做出接近系統級的能力。

## 核心原理：權限橋樑

Shizuku 的做法不是直接幫 App 變成系統 App，也不是偷偷取得 Root。

它真正做的事情，是引導使用者先用 `adb` 或 `root` 啟動一個具備較高權限的 Java 進程，也就是常說的 `Shizuku Server`。之後一般 App 透過 Binder IPC 把請求交給這個服務，再由它去和 Android 的 system server 溝通，最後把結果回傳回來。

這跟傳統「每次都起一個 shell process 去跑 `pm`、`cmd`、`settings` 指令」很不一樣。Shizuku 走的是更貼近 Android 內部機制的 Binder 路線，所以速度、穩定性與開發體驗通常都更好。

<pre class="mermaid">
sequenceDiagram
    participant App as 一般 App
    participant Shizuku as Shizuku Server
    participant System as Android System Server
    App->>Shizuku: 請求進階操作
    Shizuku->>System: 以 adb / root 權限呼叫系統 API
    System-->>Shizuku: 回傳執行結果
    Shizuku-->>App: 回傳結果
</pre>

## 為什麼它比傳統 ADB / Shell 做法更好用

- 傳統 shell 型做法通常要反覆建立 process，慢很多，還要自己 parse 文字結果。
- `Shizuku` 透過 Binder IPC 溝通，額外耗損很小，體感上更接近直接調 API。
- 對開發者來說，可以比較像在寫正常 Android / Java / Kotlin 程式，而不是一直拼接 shell 指令。
- 如果某個工具其實只需要 `adb` 等級的權限，不一定非得要求使用者 Root，這會大幅擴大可使用的人群。

## 在無 Root 的方向，它到底能多做什麼？

我覺得 `Shizuku` 最值得注意的不是「無 Root 也能很厲害」這句口號，而是它讓很多原本卡在權限邊界的能力，變得比較可實作、可維護、可產品化。

在非 Root 裝置上，透過 Shizuku，常見可以延伸的方向包括：

- 套件與元件管理：像是啟用 / 停用 component、管理部分 package 行為。
- 權限與系統層操作：某些原本常透過 `adb shell` 做的設定、授權、狀態調整，可以做成比較像 App 內建功能的體驗。
- 裝置自動化輔助：把過去要靠電腦下指令的流程，收斂成手機端自己完成的流程。
- 進階工具整合：讓 App 不只是顯示資訊，而是真的能「動手改狀態」。
- 開發者工具化：把管理裝置、批次操作、測試前置作業，包成一個可重複使用的工具層。

不過要很誠實地說，`Shizuku` 不等於拿到完整 Root。它能做的事仍然取決於：

- 目前是用 `adb` 啟動還是 `root` 啟動
- Android 版本
- ROM / 廠商對權限的限制
- 目標 API 本身是否還有 hidden API 或額外檢查

所以比較正確的理解不是「Shizuku = 無 Root 全能」，而是：

「Shizuku 讓很多本來只存在於 adb / root 工作流裡的能力，能以更像 App 能力的方式被使用。」

## 為什麼我覺得它很適合做進階測試

`Shizuku` 很適合測試，不是因為它能取代 Espresso、Maestro 這類測試框架，而是它很適合當成測試前後的「系統控制層」。

例如可以拿來做：

- 測試前置清理：重置某些狀態、切換元件啟用狀態、準備特定裝置條件。
- 測試輔助工具：把原本要在電腦上執行的 ADB 操作，搬進手機端工具裡。
- QA / 開發內部工具：做成一個快速切換環境、切模式、清資料、開關特定行為的 helper app。
- 自動化混搭：前面用 `adb` 或無線偵錯完成一次配對，後面讓 App 端透過 `Shizuku` 做反覆操作。
- 功能驗證：觀察某些只有系統層操作才容易觸發的情境，例如 component 切換、特定 package 行為變化、權限相關測試。

如果你在做的是 Android 測試平台、裝置管理小工具、內部 QA helper、進階除錯工具，我會覺得 `Shizuku` 是非常值得研究的一層。

## 啟動方式

Shizuku 官方目前主要整理三種啟動方式：

1. `無線偵錯`：適合 Android 11 以上。這是我最推薦的無 Root 路線，因為可以不靠電腦長期連線。
2. `連接電腦`：適合 Android 10 或以下，透過電腦上的 `adb` 啟動服務。
3. `Root 模式`：如果裝置本身已經 Root，可以直接授權啟動。

其中最值得補充的一點是：

- 無線偵錯的「配對」通常只需要做一次。
- 但在非 Root 模式下，裝置重新開機後，通常還是要重新把 Shizuku 服務啟動起來。
- 換句話說，是「不一定要每次重新配對，但常常要每次重啟後重新啟動服務」。

對日常使用來說，這已經比每次都回到電腦重新折騰整套 ADB 流程方便很多。

## 和 ADB 混用的價值

我覺得 `Shizuku` 很強的一個點，就是它不是要取代 `adb`，而是很適合和 `adb` 混用。

一個很實際的工作流會像這樣：

1. 先透過 USB 或無線偵錯，把裝置和電腦完成一次 ADB 配對。
2. 啟動 Shizuku 服務。
3. 後續很多反覆操作，就交給支援 Shizuku 的 App 或你自己的工具來做。

這種混用方式的好處是：

- 初始化時仍然利用 ADB 的穩定能力
- 日常反覆操作時不必一直回到電腦下命令
- 可以把常用的高權限動作包成比較好點、好測、好重複執行的介面

如果你常做裝置準備、回歸測試前置、權限切換、元件切換、測試環境清理，這種混用其實很順。

如果再把 `Shizuku` 跟 Android 的 [Accessibility API](../android/api/accessibility-api.md) 放在一起看，事情會更有意思：

- `Shizuku` 比較像系統權限與系統 API 的橋
- `Accessibility API` 比較像畫面 UI tree、事件流與互動操作的橋
- 兩者混用時，就很容易做出「看得懂畫面，也改得動系統狀態」的進階工具

## 常見使用情境

你上面提到的幾個例子很典型，這些都很能代表 `Shizuku` 的價值：

- App freezer 類工具：免 Root 凍結不想讓它常駐或偷跑的 App。
- 備份 / 裝置管理工具：在不 Root 的前提下，把部分原本要靠進階權限的流程變得更可行。
- 進階 shell 工具：例如把常見的 ADB shell 操作搬到手機端使用。
- 特定系統行為調整工具：像是針對深色模式、元件開關、系統行為做更細的控制。

如果再往外延伸，我覺得它還很適合這幾類場景：

- 開發者自己的裝置管理工具箱
- 測試團隊內部的 helper app
- 需要大量重複裝置操作的 QA 工作流
- 針對進階使用者的 power-user 工具
- 把本來「只能寫在教學文件裡的 ADB 步驟」，產品化成 App 內可操作功能

## 搭配 Android 自動化工具來看

如果把 `Shizuku` 放進 Android 自動化脈絡裡看，我會把 `MacroDroid`、`Tasker`、`Automate` 看成三種很不同的切入角度。

### 1. MacroDroid：最適合一般使用者快速上手

它的核心模型是 `Triggers + Actions + Constraints`，整體思路很直覺。

- 中文化程度高，對一般使用者很友善
- 範本很多，很適合先抄一個能跑的 workflow 再慢慢改
- 內建一些偏 UI 模擬型的能力，對「先把事情做出來」很有幫助
- 如果你的目標是 5 分鐘做出第一個自動化，我會先推這個

### 2. Tasker：自動化世界的天花板

`Tasker` 的強大來自它的 `Profiles + Tasks + Variables + Plugins` 這一整套系統。

- 擴充性非常高，能接很多 plugin
- 變數、條件、資料流處理能力很強
- 很適合把複雜工作流做成長期可維護的 automation
- 和 `Shizuku`、`Accessibility` 這類能力搭配時，幾乎會進入 Android 極限客製化領域

如果本身有程式背景，或至少願意花時間理解事件、條件、狀態、變數，我會覺得 `Tasker` 非常值得投資。

### 3. Automate：視覺化流程圖派

`Automate` 的特色是用 flowchart / blocks 來描述邏輯，這一點很適合工程腦。

- 複雜條件流程的可視化很好
- 比較容易看出目前流程卡在哪一段
- 很適合邏輯已經稍微複雜，但又不想完全跳進 Tasker 生態的人
- 對習慣用流程圖思考的人來說，理解成本常常比純文字規則更低

### 三者簡單對比

| 特性 | MacroDroid | Tasker | Automate |
|---|---|---|---|
| 上手難度 | 簡單 | 高 | 中等 |
| 介面風格 | 清單規則 | 傳統選單 / 任務系統 | 流程圖 |
| 能力上限 | 高 | 非常高 | 高 |
| 適合對象 | 新手 / 快速實作 | 進階玩家 / 極客 | 喜歡流程圖的人 |
| 和 Shizuku 搭配 | 可用 | 很強 | 可用 |
| 和 Accessibility 搭配 | 常見 | 很常見 | 常見 |

### 我會怎麼選

- 如果是新手，先從 `MacroDroid` 開始最輕鬆。
- 如果要追求最強可塑性，我會選 `Tasker`。
- 如果你喜歡流程圖式思考，我會選 `Automate`。

如果你的目標已經走到「畫面事件監控 + UI 操作 + 系統控制 + 條件判斷 + 長鏈路工作流」，那麼很常會變成：

- `Accessibility API` 處理畫面理解與操作
- `Shizuku` 處理高權限系統動作
- `Tasker` / `MacroDroid` / `Automate` 負責把整體工作流串起來

這也是我覺得 Android 自動化真正迷人的地方。

## 對開發者的價值

如果是開發者視角，`Shizuku` 的吸引力非常大。

- 可以直接把某些高權限操作設計成 Kotlin / Java API 呼叫流程
- 不用把很多核心邏輯寫成脆弱的 shell 指令 parsing
- 可以讓「只需要 adb 權限」的工具，不必直接走到 Root 這麼重
- 可以把原型工具慢慢長成正式 App，而不是永遠停留在 shell script 階段

這也是為什麼官方文件會一直強調它讓 App「幾乎像直接呼叫系統 API 一樣」。

## 限制與注意事項

`Shizuku` 很強，但我覺得最好一開始就把邊界講清楚：

- 非 Root 模式下，重開機後通常要重新啟動服務。
- `adb` 權限不是完整 Root，能做的事情會依 Android 版本與裝置而變。
- Android 9 之後 hidden API 仍然是要注意的限制點。
- 很多中國品牌或客製 ROM 會對背景執行、本地網路、USB 偵錯、安全選項做額外限制。
- 如果把開發人員選項、USB 偵錯或無線偵錯關掉，Shizuku 服務可能會失效。

所以在實務上，我會把它當成「非常強的進階橋接層」，而不是「萬能破解器」。

## 我會怎麼用它

如果是我自己，我會把 `Shizuku` 放在這幾種定位上：

- Android power-user 工具的核心能力層
- 測試 / 除錯工具的系統操作橋樑
- ADB 工作流的手機端延伸
- 內部工具與自動化 helper 的權限放大器

它最有魅力的地方，不只是讓你少接一次電腦，而是讓很多原本零散、偏工程化、偏命令列的能力，有機會變成一個可以長期使用的產品功能。

## 之後想再補的內容

這一篇先把觀念、使用情境與啟動方式整理起來。之後我還想再往下補：

- `Shizuku + aShell` 這類工具的實際搭配心得
- `Shizuku + Accessibility + Tasker` 這種混用架構怎麼拆
- `Shizuku` 在測試自動化流程裡的定位
- 無 Root 與 Root 模式在能力邊界上的差異
- 不同 ROM / 廠牌裝置的相容性觀察
- 實際可調用哪些系統 API、哪些情境會卡權限

我覺得這一塊非常值得再做更進階的講解與實測筆記。

## 官方與參考來源

- 官方網站：[Shizuku](https://shizuku.rikka.app/)
- 官方簡介：[Introduction](https://shizuku.rikka.app/introduction/)
- 官方啟動教學：[User manual / Setup](https://shizuku.rikka.app/guide/setup/)
- 官方 GitHub：[RikkaApps/Shizuku](https://github.com/RikkaApps/Shizuku)
- Android Developers：[Android Debug Bridge (adb)](https://developer.android.com/guide/developing/tools/adb.html)

## 來源說明

這篇筆記主要根據 Shizuku 官方網站、官方 GitHub README 與 Android Developers 的 ADB 文件整理，另外也吸收了你提供的使用情境與觀察，重新整理成比較適合放進知識庫的版本。
