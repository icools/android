---
title: Emerge Tools 工具介紹
tags:
  - android
  - tool
  - app-size
  - apk-analysis
  - emerge-tools
  - draft
desc: Emerge Tools 能把 Android App 的體積、資產與 native 組成拆成可視化分析結果，適合觀察 APK 結構與追蹤 size regression。
author: icools
date: 2026-04-04
source: https://www.emergetools.com/
status: active
---

# Emerge Tools

## 工具簡介

`Emerge Tools` 是一個把 App 分析結果做成 dashboard 的服務。

我一開始注意到它，不是因為想找什麼正式的 size tooling，而是看到它拿 `Threads` 的 APK 做拆解分析，裡面可以直接看到 APK 大小分布、asset 組成、native library 體積，甚至能很快抓到像是 `React Native`、`JNI so` 或 debug code 這類線索。

對我來說，它最有意思的地方不是單純「告訴你 App 幾 MB」，而是幫你把「到底哪裡胖」這件事可視化。

<figure>
  <img src="{{ '/tools/image/emerge-tools-threads-analysis.jpg' | relative_url }}" alt="Emerge Tools 分析 Threads APK 的畫面">
  <figcaption>Emerge Tools 可以把 Android App 的體積組成拆成更容易閱讀的 dashboard，拿來快速看出 asset、native 與其他模組的占比很直覺。</figcaption>
</figure>

## 我為什麼推薦

我會把 `Emerge Tools` 收進這個知識庫，是因為它很適合做這兩種事：

- 觀察別人的 App 大小結構
- 追自己專案的 size regression

如果只是偶爾想知道某個 APK 為什麼這麼肥、是 `resources` 大、`assets` 大、還是 native library 太重，這種 dashboard 型工具很省時間。

## 主要用途

- 分析 Android App 體積構成
- 找出 `assets`、`resources`、native library 等主要體積來源
- 追蹤版本間的 size regression
- 快速觀察陌生 App 的封裝結構

## 我覺得特別適合的情境

- release 包越來越肥，但還不確定是誰造成的
- 想把 APK 體積變化做成比較容易和團隊溝通的畫面
- 想快速研究某個大型 App 的包體積長相

## 補充筆記

- 這類服務型工具很適合用來做視覺化與趨勢觀察，但如果要真的對照 code / resources，還是得回到本地工具一起看。
- 我自己會把它跟 `jadx-ui`、`Android Studio Analyzer` 這類工具放在不同層次來看：`Emerge Tools` 比較偏 dashboard 與回顧，其他工具比較偏本地細查。

## 官方來源

- 官方網站: [Emerge Tools](https://www.emergetools.com/)
- 範例分析頁: [Threads app size analysis](https://www.emergetools.com/app/example/android/com.instagram.barcelona?buildContent=breakdown)

## 來源說明

官方網站把 `Emerge Tools` 定位成 App quality 與 release visibility 類型的工具，而公開範例分析頁則很直接展示了它如何拆解 Android App 的體積結構。這也是我覺得它最值得看的地方。
