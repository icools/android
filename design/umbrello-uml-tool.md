---
title: Umbrello UML 工具介紹
tags:
  - design
  - uml
  - diagram
  - umbrello
  - tool
  - draft
desc: Umbrello 是一套免費開源的 GUI UML 工具，適合用拖拉方式畫 class diagram、sequence diagram，或把程式碼匯入成圖。
author: icools
date: 2026-04-04
source: https://invent.kde.org/sdk/umbrello
status: active
---

# Umbrello

## 工具簡介

`Umbrello` 是一套讓我覺得很值得先記下來的 GUI UML 工具。

我原本是在找那種既能拖拉、又不要太重、最好還能和程式碼互相轉換的圖表工具。很多工具不是太偏手動畫圖，就是要花一堆時間拉元件；而 `Umbrello` 這種傳統但還活著的開源工具，反而在某些情境下很實際。

它支援 class diagram、sequence diagram 等標準 UML 圖，也有把程式碼匯入成模型的能力。對我來說，它比較像是「如果我真的想用 GUI 畫 UML，而不是只靠 Mermaid / PlantUML」時可以先試的一個選項。

<figure>
  <img src="{{ '/design/image/umbrello-home.jpg' | relative_url }}" alt="Umbrello 官方畫面截圖">
  <figcaption>Umbrello 是 KDE 生態下的 UML 建模工具，介面偏傳統，但功能方向很明確。</figcaption>
</figure>

<figure>
  <img src="{{ '/design/image/umbrello-class-diagram.jpg' | relative_url }}" alt="Umbrello class diagram 示意畫面">
  <figcaption>如果想拖拉 class diagram、sequence diagram 這類傳統 UML 圖，Umbrello 這種 GUI 工具還是有它的價值。</figcaption>
</figure>

## 我為什麼會先收進來

我會把 `Umbrello` 收進 `design/`，主要是因為它剛好補在我平常工具選擇裡的一個空位。

- `Mermaid` 很適合文字化、可版控與 AI 生成
- `Draw.io` 很適合手動排版
- `Umbrello` 這類工具則比較偏傳統 UML GUI 與模型匯入匯出

它不是我現在最常用的首選，但如果哪天我要更認真整理 class diagram，或測試從程式碼匯圖的流程，它是值得先試的一個工具。

## 主要用途

- 畫 class diagram、sequence diagram 等 UML 圖
- 用 GUI 拖拉方式整理模型
- 嘗試從程式碼匯入 UML 模型
- 在傳統 UML 工具和 Docs-as-Code 工具之間找平衡

## 我覺得特別適合的情境

- 想要比 `Draw.io` 更接近 UML 建模語意
- 想測試 code import / export 這類傳統 UML workflow
- 不想一開始就上很重、很貴的商業 UML 工具

## 補充筆記

- 目前以我的工作流來看，`Mermaid` 還是更日常；`Umbrello` 比較像保留給需要 GUI UML 的情境。
- 它對 Kotlin 的支援不會像 Java 那麼直接，所以如果要從 Android 專案導 UML，可能還是需要多一層轉換或測試。
- 這類工具的價值常常不是「全自動」，而是你需要一個可以拖拉又比較懂 UML 語意的畫布。

## 官方來源

- 官方原始碼: [SDK / Umbrello](https://invent.kde.org/sdk/umbrello)
- 產品頁: [Umbrello UML Modeller](https://apps.kde.org/umbrello/)

## 來源說明

KDE 的官方專案頁把 `Umbrello` 定位成 UML Modeller，支援多種標準 UML 圖表類型。對我來說，它最值得關注的點不是「最新潮」，而是它仍然提供一條相對完整的 GUI UML 工作路線。
