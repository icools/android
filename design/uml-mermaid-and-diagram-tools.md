---
title: UML、Mermaid 與 Diagram Tools：我會怎麼選
tags:
  - design
  - uml
  - mermaid
  - plantuml
  - c4
  - diagram
desc: 整理 UML、Mermaid 與其他圖表工具的差異、適合情境與我會怎麼選擇。
author: icools
date: 2026-03-29
source: local
status: active
---

# UML、Mermaid 與 Diagram Tools：我會怎麼選

當系統開始變複雜之後，只靠嘴巴講，很快就會進入一種大家都覺得自己懂、但其實每個人腦中圖都不一樣的狀態。

所以圖表工具很重要。

但圖表工具也不是越正式越好，而是要看你現在到底想表達什麼。

## UML 比較像什麼

`UML` 比較像一套經典的圖表語言。

它適合描述：

- class 關係
- sequence 互動
- state 轉換
- use case
- activity flow

如果你今天想把系統互動或 class 關係講得比較嚴謹，UML 還是很有價值。

## Mermaid 比較像什麼

`Mermaid` 對我來說最大的優勢，是它很適合 Markdown 與 GitHub 型知識庫。

因為它的圖是用文字寫的，所以有幾個很好用的特點：

- 可以直接版本控制
- 可以跟文章一起放
- 可以用 diff 看修改
- 很適合快速畫 flowchart、sequence diagram、state diagram

如果今天我要在知識庫、文件或 PR 裡快速補一張圖，我通常會優先想到 Mermaid。

## PlantUML 比較像什麼

如果你想要更正式、更完整地表達 UML，`PlantUML` 會是很值得考慮的選項。

它比 Mermaid 更偏正式工程圖表，對 UML 表達也比較完整。

## C4 類工具適合什麼

如果你的重點不是 class 細節，而是整個系統邊界、容器、元件與互動，我會更偏向 `C4` 思維。

這種圖比較適合回答：

- 系統和外部世界怎麼互動
- 有哪些 container
- module 或 component 怎麼切
- 哪些責任不該混在一起

## 我自己會怎麼選

### 想快速畫、直接放 Markdown

我會選 `Mermaid`。

### 想正式畫 UML

我會選 `PlantUML`。

### 想講高階架構

我會用 `C4` 思維來表達。

### 想快速手繪討論

我會考慮 `Excalidraw` 這種比較輕量、討論感強的工具。

## 我覺得很值得補進知識庫的相關工具

- `Mermaid`：最適合和 Markdown 筆記整合
- `PlantUML`：正式 UML 表達很強
- `Excalidraw`：快速畫草圖超舒服
- `Structurizr` / `C4`：適合架構層級說明
- `D2`：偏現代感、宣告式圖表語言

## 我的結論

如果是我的知識庫，我會把 `Mermaid` 當成第一優先，因為它最適合直接和 Markdown 一起長期維護。

但如果之後想把架構表達得更完整，`PlantUML`、`C4` 與其他 diagram tools 也很值得補進來。
