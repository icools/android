---
title: LibChecker 工具介紹
tags:
  - android
  - tool
  - decompile
  - libchecker
  - reverse-engineering
  - draft
desc: 用 LibChecker 直接查看手機 App 使用的函式庫與封裝資訊，適合快速理解第三方 Android App 的技術堆疊。
author: icools
date: 2026-04-04
source: https://github.com/LibChecker/LibChecker
status: active
---

# LibChecker

## 工具簡介

`LibChecker` 是一個我覺得很適合 Android 開發者收進工具箱的 App。

它不是傳統那種先把 APK 丟進反編譯工具、再慢慢翻 Java / smali 的路線，而是直接把你手機裡已安裝 App 用到的函式庫、封裝方式與一些基礎資訊整理出來，讓你可以很快知道對方大概用了什麼技術堆疊。

如果你只是想快速理解一個 App 背後用了哪些常見 library，`LibChecker` 的效率會比我直接開 `jadx` 來得更輕鬆。

<figure>
  <img src="{{ '/tools/image/libchecker-library-list.jpg' | relative_url }}" alt="LibChecker 顯示 App 使用函式庫的畫面">
  <figcaption>LibChecker 可以直接列出已安裝 App 使用到的函式庫與封裝資訊，拿來做第一輪觀察很方便。</figcaption>
</figure>

<figure>
  <img src="{{ '/tools/image/libchecker-package-detail.jpg' | relative_url }}" alt="LibChecker 顯示 App 詳細資訊的畫面">
  <figcaption>除了函式庫清單，也能快速看到個別 App 的一些結構資訊，適合當 APK 深入分析前的入口。</figcaption>
</figure>

## 我為什麼推薦

我會推薦 `LibChecker`，是因為它很適合拿來做「快速 reconnaissance」。

- 想知道某個 App 用了哪些常見套件，不一定要先走完整反編譯流程。
- 想先確認對方是不是用了 `Jetpack Compose`、`React Native`、某些廣告 SDK 或常見加密 / 網路套件，這種工具很直覺。
- 用手機本機直接看，進入門檻比開整套逆向工具低很多。

## 主要用途

- 快速查看已安裝 Android App 使用的函式庫
- 觀察 App 可能使用的技術堆疊
- 當作 APK 深入分析前的第一輪入口
- 輔助理解第三方 App 的封裝與模組資訊

## 我覺得特別適合的情境

- 看到某個 App 做得不錯，想先快速知道它大概用了哪些 library
- 想確認某個 App 是偏原生、混合式，還是有引入大型跨平台框架
- 不想一開始就直接進 `jadx`，想先用更輕量的方式觀察

## 補充筆記

- `LibChecker` 很適合拿來做第一輪理解，但它不是完整反編譯工具。
- 如果真的要確認 class、resources、manifest 或更細節的實作，後面還是要搭配 `jadx-ui` 之類工具一起看。
- 我會把它當成「先聞味道，再決定要不要拆」的快速入口。

## GitHub 來源

- 官方 repo: [LibChecker/LibChecker](https://github.com/LibChecker/LibChecker)

## 來源說明

官方 GitHub repo 把 `LibChecker` 定位成 Android 上的 library 檢查工具，適合查看安裝 App 的使用函式庫與相關資訊。以 Android 開發者的角度來看，它很適合用來快速理解陌生 App 的技術組成。
