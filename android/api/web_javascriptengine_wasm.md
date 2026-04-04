---
title: Android JavaScriptEngine 與 WASM 背景執行實戰心得
tags:
  - android
  - javascriptengine
  - wasm
  - hotfix
  - automation
desc: 深入分析 Android 10+ 引入的 JavaScriptEngine 核心能力，包含 WASM 支援、沙盒機制、熱修補 (Hotfix) 架構設計以及能力偵測 (Capability check) 實務建議。
author: icools
date: 2026-04-04
source: https://developer.android.com/develop/ui/views/layout/webapps/jsengine?hl=zh-tw
status: active
---

# Android JavaScriptEngine (JS/WASM)

原來可以透過 `JavaScriptEngine` 來執行 JS 或 WASM，而且可以動態地執行替換（Hotfix），在 Android 10 以上支援。

官方連結：[Executing JavaScript and WebAssembly | Android Developers](https://developer.android.com/develop/ui/views/layout/webapps/jsengine?hl=zh-tw)


可以，**你的做法是合理的，而且很適合拿來做「快速熱修補規則層」** 👍
但我會建議你把它定位成：

> **Native parser = 主體**
> **WASM / JS sandbox = 可動態下發的 hotfix layer**

不要反過來。原因是官方對 `Ja:contentReference[oaicite:0]{index=0} **API 26+ 且底層 WebView implementation 有支援**；官方也要求先用 `JavaScriptSandbox.isSupported()` 檢查，不支援時建立 sandbox 會失敗。([Android Developers][1])你最在意的兩句

### 1) Android 10 以上，大部分裝置會支援嗎？

**主流手機上，機率偏高，但不能直接當成「幾乎全支援」來假設。**
我這樣判斷的理由是：官方說 WebView 功能是跟 **裝置 Android 版本 + WebView APK 版本** 一起決定的，而且 Android System WebView 是 Google 透過 Play 商店持續更新、更新節奏跟 Chrome 相同；Jetpack Webkit 也特別強調，即使在 Android 10 這類較舊系統上，也能透過較新的 WebView APK 使用較新的 WebView 功能。這代表 **Android 10+ 的主流、Google 認證、WebView 正常更新的手機，支援機率通常不低**。但官方沒有提供「多少比例裝置一定支援」的數字，所以這一段只能算工程推論，不是官方保證。([Android Developers][2]) 不支援裝置會很多嗎？
**從官方文件來看，最明確的不支援原因是 WebView 套件太舊。**
`SandboxUnsupportedException` 的說明直接寫了：建立 sandbox 不被支援時，可能是 **WebView package 太舊，無法提供 sandbox implementation**。所以你真正要防的，不是「Android 10」本身，而是這幾種裝置族群：

1. WebView/Chrome 長期沒更新的裝置。
2. 客製 ROM / AOSP 系統，WebView 提供者不是標準 Google 更新路徑。
3. 企業管控、車機、Kiosk、特殊裝置，WebView 更新被凍住。
4. 即使支援 sandbox，也**不一定支援所有功能**，例如 Promise return、Wasm compile 這些還要再個別查 `isFeatureSupported(...)`。([Android Developers][3])# 你的熱修補設計，工程上我會怎麼看 😎

### 這個方向是對的

拿你說的 **導航地址 parsing 規則** 來看，這類邏輯通常有幾個特性：

* 輸入是字串
* 輸出是結構化結果
* 規則可版本化
* 不一定要碰 Android framework
* 很適合快速替換

這種情境很適合做成 **遠端下發的規則模組**，用 JS/WASM 在 sandbox 裡跑。
因為官方這套就是設計給 **非互動式 JavaScript 評估**，也能放在 `Service` / `WorkManager` 執行，不需要 `WebView` 畫面。([Android Developers][1])佳姿勢不是「全押 WASM」
比較穩的做法會是：

* **App 內建 native baseline parser**
  先確保所有 Android 10+ 裝置都有基本解析能力。
* **遠端下發 patch parser**
  對支援 sandbox + 支援對應 feature 的裝置，下載新版 JS/WASM 規則。
* **解析時先跑 patch，失敗就 fallback native**
  包含 timeout、exception、結果不合法都回退。
* **下次正式 App 更新再把 patch 併回 native**
  這樣不支援裝置也會在後續版本補齊。

這樣就會變成一個很漂亮的策略：
**你先救得了大多數支援裝置，其他裝置等下次正式版補上。**
這個思路和官方的支援模型是相容的，因為官方本來就要求你做 runtime feature check，而不是寫死假設。([Android Developers][4])# 我會建議你加三層 capability gate

### 第一層：有沒有 sandbox

先看：

```kotlin
val sandboxSupported = JavaScriptSandbox.isSupported()
```

這是最基本門檻；官方要求先檢查。([Android Developers][4])層：你要的功能有沒有
例如你真的要下發 Wasm，就要再看：

```kotlin
sandbox.isFeatureSupported(JavaScriptSandbox.JS_FEATURE_WASM_COMPILATION)
```

如果你想讓 JS 用 Promise 回結果，也要檢查：

```kotlin
sandbox.isFeatureSupported(JavaScriptSandbox.JS_FEATURE_PROMISE_RETURN)
```

官方明講：不同 sandbox implementation 提供的 feature 組合可能不同，要逐項檢查。([Android Developers][5])層：你的業務規則版本相容嗎
這層是你自己定義，例如：

* parser API version
* schema version
* city/town dataset version
* fallback rule version

這不是官方規定，但在你這種「規則熱修補」場景非常重要。沒有版本治理，熱修補很容易變成熱爆炸 🌋

---

## 所以你的方案，適不適合 parsing 修補？

我給你一個很直接的判斷：

### 很適合的情況

* 地址字串切分
* 門牌／巷弄／樓層規則修正
* 特定縣市或語料的補丁規則
* 新增 alias / 同義詞 / POI 矯正
* bug fix 很急，但不想等商店審核

### 不太適合完全放進去的情況

* 需要大量本地資料庫聯查
* 需要直接碰 Android API / Binder / ContentProvider
* 效能極端敏感且每次導航都高頻呼叫
* 規則很難被沙箱隔離、還需要大量 app 內狀態

因為 `JavaScriptEngine` 適合的是 **純邏輯、純計算、純規則**。一旦邏輯跟 app 原生狀態綁太深，native 還是比較穩。這是根據官方把它定位成非互動式 JS/Wasm 執行來推論的。([Android Developers][1])# 我對你這題的實務結論

### 結論一句話

**合理，而且很值得做，但要做成「能力偵測 + 動態 patch + fallback native」的雙軌架構。**

### 我會怎麼定案

* **Android 10+ 可以納入**
* **不要假設全支援**
* **先上 capability telemetry**
* **先從「地址 parsing 補丁」這種小而純的規則開始**
* **WASM 只是加分，不是必要**

  * 其實很多 parser 修補，先用 JS 就夠了
  * 等你確認裝置支援率夠高，再把重邏輯搬到 Wasm

---

## 最誠實的答案：不支援會不會很多？

**官方文件沒有給比例，所以我不能直接跟你說「很少」或「很多」。**
但以文件內容來看，若你的目標是 **Android 10+ 的主流手機市場**，而且裝置的 WebView 維持正常更新，那這條路**有很高的實作價值**；真正風險比較高的是 **舊 WebView、特殊 ROM、被企業凍結更新的裝置**。要知道你的真實支援率，最準的方法不是猜，是在現有安裝基礎上直接埋點統計：`isSupported`、`WASM_COMPILATION`、`PROMISE_RETURN`、WebView provider version。([Android Developers][3])幫你整理成一份 **導航地址 parser 熱修補架構圖 + Android capability check 範例碼 + 遠端 patch manifest 設計**。

[1]: https://developer.android.com/develop/ui/views/layout/webapps/jsengine?utm_source=chatgpt.com "Executing JavaScript and WebAssembly | Web Apps"
[2]: https://developer.android.com/reference/androidx/webkit/WebViewFeature "WebViewFeature  |  API reference  |  Android Developers"
[3]: https://developer.android.com/reference/androidx/javascriptengine/SandboxUnsupportedException "SandboxUnsupportedException  |  API reference  |  Android Developers"
[4]: https://developer.android.com/reference/androidx/javascriptengine/JavaScriptSandbox "JavaScriptSandbox  |  API reference  |  Android Developers"
[5]: https://developer.android.com/develop/ui/views/layout/webapps/jsengine?hl=zh-tw&utm_source=chatgpt.com "執行JavaScript 和WebAssembly | Web Apps"
