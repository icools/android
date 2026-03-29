---
title: jadx-ui 工具介紹
tags:
  - android
  - tool
  - reverse-engineering
  - jadx
  - apk
desc: jadx GUI 工具介紹，整理 APK 反編譯、結構查看、推薦原因、用途與官方來源。
author: icools
date: 2026-03-29
source: https://github.com/skylot/jadx
status: active
---

# jadx-ui

## 工具簡介

這裡的 `jadx-ui` 我會把它理解成 `jadx` 的圖形介面版本。

官方專案 repo 是 `jadx`，其中提供命令列工具與 GUI 工具；GUI 版本在官方說明裡通常叫做 `jadx-gui`。如果我要看 APK、Dex、resources 或反編譯後的 Java 程式碼，這類工具非常常用。

## 我為什麼推薦

我會推薦 `jadx-ui`，因為它是理解 Android App 內部實作、分析第三方 APK、閱讀反編譯結果時非常直接的工具。

- 可以快速打開 APK / Dex 看結構
- 有 GUI，閱讀成本比純命令列低
- 搜尋、跳轉、查看資源都很方便
- 很適合拿來理解陌生專案或做基礎逆向分析
- 對 Android 開發者來說，排查某些行為或檢查封包流程時很有幫助

## 主要用途

- 反編譯 APK / Dex
- 查看 AndroidManifest 與資源內容
- 搜尋 class、method、字串與使用位置
- 快速理解第三方 App 的大致結構
- 做基礎逆向分析與問題排查

## 我覺得特別適合的情境

- 想知道某個 APK 大概怎麼實作
- 想快速搜尋特定關鍵字、API、endpoint 或 class 名稱
- 想比對不同版本 APK 的差異
- 想從資源與 manifest 先快速理解一個 App

## 補充筆記

- 反編譯結果不一定 100% 正確，尤其遇到混淆或特殊寫法時要保留判斷
- 比起完全依賴反編譯出的 Java，很多時候也要搭配 smali、manifest、resources 一起看
- 很適合放在 Android 工具箱中當「快速理解 APK 內容」的第一站

## GitHub 來源

- 官方 repo: [skylot/jadx](https://github.com/skylot/jadx)

## 來源說明

官方 repo README 說明 `jadx` 是一套把 Dex 反編譯成 Java 的工具，並同時提供 CLI 與 GUI；GUI 版本支援語法高亮、跳轉、搜尋、find usage 等功能。
