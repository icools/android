---
title: Clean Architecture
tags:
  - books
  - clean-architecture
  - architecture
  - boundaries
  - dependency
desc: 整理我怎麼看 Clean Architecture 這本書，以及它對邊界、依賴方向與系統結構的價值。
author: icools
date: 2026-03-29
source: local
status: active
---

# Clean Architecture

如果 `Clean Code` 比較像是在修房間，那 `Clean Architecture` 比較像是在看整棟房子的格局有沒有蓋歪。

它關心的事情比較不是函式怎麼命名，而是：

- 模組怎麼切
- 依賴方向該怎麼流動
- 哪些東西不該互相綁死
- 業務邏輯應不應該被框架綁住

## 我覺得這本書最有價值的地方

它會一直逼你回頭問：

- 這個 dependency 合理嗎
- 這層知不知道太多不該知道的事情
- UI、framework、database 到底是不是核心
- 真正重要的 business rules 放在哪裡

很多系統會越寫越痛苦，不是因為工程師不努力，而是因為依賴方向從一開始就亂掉。

## 適合誰看

- 開始碰架構設計的人
- 對分層、模組化、依賴反轉有興趣的人
- 覺得專案越改越黏、越難抽換的人

## 我怎麼看它的風險

跟很多經典書一樣，它很有價值，但也不要把它神化成唯一答案。

現實世界很多產品都有歷史包袱、交期壓力與團隊限制，不可能什麼都完美切層。

所以我會把這本書看成：

- 幫你建立結構判斷力
- 幫你看出哪些依賴正在變危險
- 幫你在現實限制下，至少不要一路往更糟的方向走

## 我的簡單結論

如果你開始不滿足於「功能有出來就好」，而是想理解系統為什麼有些越改越痛、有些卻可以慢慢撐大，那 `Clean Architecture` 很值得看。
