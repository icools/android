---
title: libKTX / libGDX Kotlin 遊戲開發筆記
tags:
  - kotlin
  - libgdx
  - ktx
  - game-development
  - draft
desc: libKTX 是讓 libGDX 更適合 Kotlin 開發的一組延伸工具，適合想用 Kotlin 試作跨平台遊戲的人先入門看看。
author: icools
date: 2026-04-04
source: https://libktx.github.io/
status: active
---

# libKTX / libGDX

## 這篇在記什麼

這篇比較像是我先把一個值得回頭研究的方向收進來。

我本來是在找 Kotlin 相關資源時看到 `libKTX`，才又順手回頭看 `libGDX` 這條路。簡單講，`libGDX` 是老牌的跨平台遊戲框架，而 `libKTX` 則是在這個生態上補上一套更適合 Kotlin 開發的 API 與延伸工具。

如果我之後真的想用 Kotlin 試一些遊戲型 side project，這會是值得重新打開來看的技術組合。

<figure>
  <img src="{{ '/kotlin/image/libktx-home.jpg' | relative_url }}" alt="libKTX 官方網站首頁截圖">
  <figcaption>libKTX 官網會整理 Kotlin 對 libGDX 的支援模組與文件，當成入口很適合。</figcaption>
</figure>

## 我為什麼先記下來

- Kotlin 不只在 Android app 本體有用，連遊戲開發也已經有成熟一點的路線可以走。
- `libKTX` 這種 Kotlin-friendly 包裝，能把原本比較 Java 味的 `libGDX` 拉回到更順手的 Kotlin 風格。
- 我實際看了一個範例專案後，發現至少在 build 與跑起來這件事上，比我原本預期順很多。

## 主要用途

- 用 Kotlin 開發 `libGDX` 遊戲
- 在桌面、Android、iOS 等平台做跨平台遊戲試作
- 拿現成範例專案熟悉 ECS、資產管理與基礎遊戲 loop

## 我目前的理解

以我現在的角度來看，這條路不是「我立刻要投入的主線」，但很適合留作之後的研究支線。

特別是如果哪天想做：

- Kotlin 遊戲原型
- 輕量 2D game side project
- 或只是想理解 Kotlin 在 Android 之外還能怎麼玩

那 `libGDX + libKTX` 會是一個不錯的起點。

## 補充筆記

- 我目前還沒有把這套生態完整摸熟，所以這篇先保留探索筆記的定位。
- 這裡比較值得追的其實不是只有框架本身，還有它背後的範例專案、ECS 架構與資產組織方式。
- 如果之後真的展開，我大概會再補一篇更偏實作或範例拆解的文章。

## 官方來源

- 官網: [KTX: Kotlin support for LibGDX applications](https://libktx.github.io/)
- 範例 repo: [Quillraven/Dinoleon](https://github.com/Quillraven/Dinoleon)

## 來源說明

`libKTX` 官網把它定位成 `Kotlin support for LibGDX applications`，核心就是讓 `libGDX` 生態更適合 Kotlin 開發。搭配範例專案一起看，比只看框架名稱更容易理解它到底能做什麼。
