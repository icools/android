---
title: 使用 Rust 開發 Android 應用程式的完整介紹
tags:
  - android
  - rust
  - ndk
  - jni
  - tauri
  - dioxus
  - bevy
desc: 整理 Rust 在 Android 上的主要開發方式、特性、優缺點與導入建議，並附上可追的官方與社群來源。
author: icools
date: 2026-03-29
source: local
status: active
---

# 使用 Rust 開發 Android 應用程式的完整介紹

Rust 已經不只是「可以塞進 Android 專案的原生語言」，而是可以從效能模組、跨平台邏輯，到部分完整 App 形態都派上用場的實際選項。就 2026 年 3 月我整理這篇時，Google 仍持續在 AOSP 文件中提供 Android 平台元件的 Rust 建置與模組說明，而 Rust 社群也已經把 Android 上常見的整合路徑整理得更成熟。

如果目標是幫 Android 專案導入 Rust，我會把整體路線分成三類：`Rust + Android NDK`、`全 Rust / Rust 主導的跨平台 UI`、以及 `Rust 作為核心邏輯層的混合開發`。這樣分類最方便做技術選型，也比較符合實際專案導入時的決策流程。

## 1. 主要開發方式

### 方式一：Rust + Android NDK

這是目前最穩、也最容易落地到既有 Android 專案的路線。做法是用 Rust 撰寫原生模組，編譯成 `.so`，再透過 JNI 或其他 FFI 邊界與 Kotlin / Java 溝通。

常見工具與元件：

- `cargo-ndk`：協助處理 Android 多 ABI 交叉編譯。
- `jni` crate：處理 JVM 與 Rust 間的資料交換。
- `android-activity`：讓 Rust 直接作為 Android 原生 Activity glue layer，支援 `NativeActivity` 與 `GameActivity`。
- `mozilla/rust-android-gradle`：把 Cargo build 整合進 Gradle 流程，方便 Android Studio 專案接入。

適合情境：

- 影像處理
- 加密 / 壓縮
- 遊戲引擎或物理模組
- 本地推論前後處理
- 想在 iOS / Android 共用核心邏輯

這條路的優點是你不必重寫整個 App。對已經以 Kotlin 為主的團隊來說，通常從這裡開始最實際。

### 方式二：全 Rust 或 Rust 主導的完整 App

如果你想用較高比例的 Rust 來做完整 App，現在比較值得看的方向主要有三種。

#### Dioxus

Dioxus 走的是跨平台 declarative UI 路線，語法風格接近 React。官方文件把 mobile 視為 first-class target，Android 可透過 Rust Android target 與 Dioxus CLI 開發。它目前的 mobile renderer 主要依賴平台 WebView，也有實驗性的 WGPU 路線，因此比較適合想用單一 Rust codebase 覆蓋 Web / Desktop / Mobile 的團隊。

#### Tauri 2

Tauri 2 已正式把 Android / iOS 納入 mobile 平台支援。它的模型是 Rust 後端加 WebView 前端，適合本來就有 Web 技術堆疊，或希望兼顧體積、安全與跨平台重用的專案。對內容型 App、工具型 App、管理後台型 App 特別有吸引力。

#### Bevy

Bevy 是 Rust 生態裡很重要的遊戲與互動式應用引擎。官方站點已把 Android 列為支援平台，適合 2D / 3D 遊戲、互動視覺化、或偏即時渲染的體驗型 App。如果需求偏遊戲或大量自訂渲染，Bevy 會比一般 App framework 更合理。

#### 關於 Freya

Freya 很值得關注，因為它提供以 Skia 為核心的 declarative GUI 體驗，也沿用 RSX 風格；但我不會把它列成目前 Android 主力推薦。原因是官方文件現階段更明確聚焦在桌面 GUI，而不是把 Android 作為成熟主戰場。若要研究高性能原生 GUI 的 Rust 路線，可以追，但不建議把它當成現階段 Android 專案的預設首選。

### 方式三：混合開發，把 Rust 當核心邏輯

還有一條很實用的路線，是前端沿用熟悉框架，核心邏輯由 Rust 負責。

- `Flutter + flutter_rust_bridge`：UI 繼續用 Flutter，將演算法、資料處理、共享 domain logic 放進 Rust。
- `Crux`：把應用邏輯做成 headless Rust core，再分別接上 Android、iOS、Web 的殼層。

這類做法很適合團隊想先保留既有前端投資，但又希望逐步把高價值邏輯抽到 Rust。

## 2. Rust 在 Android 上的主要特性

- 記憶體安全：Rust 透過所有權與型別系統，把不少常見記憶體錯誤擋在編譯期。
- 高效能：沒有垃圾回收停頓，效能表現通常接近 C / C++。
- 跨平台重用：核心模組可在 Android、iOS、Desktop，甚至 WebAssembly 間重用。
- 現代工具鏈：Cargo 把依賴、測試、workspace、release profile 都整理得很一致。
- 可接 Android 原生能力：從 NDK 層切入時，仍能結合感測器、輸入事件、圖形與系統 API。
- 宣告式 UI 選項增加：Dioxus 等框架讓 Rust 在完整應用層不再只侷限於底層函式庫。

## 3. 優缺點分析

### 優點

- 安全性高：很適合取代高風險的 C / C++ 原生模組。
- 效能好：對即時運算、低延遲與電量敏感場景特別有價值。
- 模組可重用：共用核心邏輯時，長期維護成本通常比雙平台各寫一份低。
- 語言設計現代：型別系統、錯誤處理、模式匹配與並行模型都很強。
- 官方與生態支持持續增加：Google 在 AOSP 文件中已長期維護 Rust 建置與模組說明，代表它不是純社群實驗。

### 缺點

- 學習曲線高：所有權、生命週期與 borrow checker 對新手確實有門檻。
- Android 整合成本不低：NDK、target、ABI、JNI 邊界與 Gradle 串接都需要時間熟悉。
- UI 生態仍不如 Kotlin 成熟：若需求高度依賴 Jetpack Compose 或各種 Android UI 元件，Rust 現階段通常得混合使用。
- 工具體驗不完全一致：Android Studio 對 Rust 的整合感仍不如 Kotlin / Java 原生順手。
- 建置與除錯較複雜：尤其跨 ABI、symbol、crash trace 與 native debugging 時，心智負擔會比較高。

## 4. 我會怎麼選

- 如果你是既有 Android App，要補高效能模組：先選 `Rust + NDK`。
- 如果你想共用大量跨平台邏輯，但 UI 不一定要全 Rust：選 `Rust core + 混合框架`。
- 如果你想做跨平台工具型 App，且接受 WebView UI：優先看 `Tauri 2`。
- 如果你想用 Rust 做單一 codebase 的跨平台介面：看 `Dioxus`。
- 如果你是遊戲、互動視覺化、即時渲染：看 `Bevy`。

總體來說，我不建議一開始就把整個 Android App 全面改寫成 Rust。比較合理的導入順序是：先從一個高價值模組切入，等團隊熟悉 toolchain、跨語言邊界與除錯流程後，再決定要不要往更高層擴張。

## 5. 推薦來源

以下是我整理這篇時優先參考、也值得長期追的來源：

- [Android Rust introduction](https://source.android.com/docs/setup/build/rust/building-rust-modules/overview) - AOSP 官方文件，說明 Android 平台對 Rust 模組的支援定位。
- [Android Rust modules](https://source.android.com/docs/setup/build/rust/building-rust-modules/android-rust-modules) - AOSP 官方文件，列出 `rust_*` 模組定義與 Soong / build system 整合方式。
- [Comprehensive Rust for Android](https://google.github.io/comprehensive-rust/android.html) - Google 的 Rust 教材入口，適合拿來建立 Android 場景中的 Rust 心智模型。
- [android-activity crate docs](https://docs.rs/android-activity/latest/android_activity/) - Rust Android 原生 Activity glue layer 說明。
- [mozilla/rust-android-gradle](https://github.com/mozilla/rust-android-gradle) - Gradle 與 Cargo 整合插件。
- [Dioxus Mobile](https://dioxuslabs.com/learn/0.7/guides/platforms/mobile/) - Dioxus 官方 mobile 指南。
- [Tauri v2](https://v2.tauri.app/) - Tauri 官方文件首頁，已明確列出 Android / iOS 支援。
- [Tauri CLI reference](https://v2.tauri.app/reference/cli/) - 可進一步查看 `tauri android init/dev/build/run` 指令。
- [Bevy Engine](https://bevy.org/) - 官方首頁已將 Android 列為支援平台。
- [Freya](https://freyaui.dev/) - 值得追蹤的 Rust GUI 專案，但 Android 成熟度建議再觀察。

## 6. 簡短結論

Rust 在 Android 上最成熟的用法，仍然是拿來做高效能、可重用、且安全性要求高的原生模組；而在完整 App 開發層，Dioxus、Tauri 2、Bevy 等路線已經讓「Rust 不只寫底層」這件事越來越可行。真正要不要導入，不是看 Rust 酷不酷，而是看你的專案是不是剛好需要它帶來的效能、安全與跨平台收益。
