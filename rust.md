---
title: Rust 索引
tags:
  - android
  - index
  - rust
desc: Rust 與 Android 整合方式、框架選型與實務導入筆記索引頁。
author: icools
date: 2026-04-05
source: local
status: active
---

# Rust

這一頁主要整理 Rust 在 Android 上的開發方式、框架選型與導入建議，內容會放在 `coding/rust/`。

## 目前文章

- [使用 Rust 開發 Android 應用程式的完整介紹]({{ '/coding/rust/rust-for-android-overview/' | relative_url }}) - 整理 Rust 在 Android 上的三種主要路線：NDK 原生模組整合、跨平台 GUI／WebView 框架，以及以 Rust 為核心邏輯的混合式架構，並附上優缺點與官方來源。

## 之後想補的主題

- `cargo-ndk` 實作流程
- JNI / FFI 溝通範例
- Rust 與 Kotlin module 拆分方式
- Rust native library 的 ABI、打包與發佈注意事項
- Android 上的 Rust debugging / symbolication
- Rust 適不適合導入既有 Android 團隊
