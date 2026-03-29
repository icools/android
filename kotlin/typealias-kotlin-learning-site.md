---
title: Typealias Kotlin 學習網站推薦
tags:
  - android
  - kotlin
  - learning-resource
  - website
  - typealias
  - dave-leeds
desc: Typealias 是一個非常值得收藏的 Kotlin 學習網站，用口語、插圖與循序漸進的方式講解 Kotlin，很適合直接讀英文原文。
author: icools
date: 2026-03-29
source: https://typealias.com/
status: active
---

# Typealias

## 網站簡介

`Typealias` 是一個我非常推薦的 Kotlin 學習網站，主要由 Dave Leeds 經營。

我很喜歡它的原因，是它不是只把語法條列出來而已，而是真的會用很口語、很細、很有步驟感的方式，把 Kotlin 觀念慢慢講清楚。就算是比較抽象的主題，也常常會先用生活化例子、卡通式插圖或情境故事把直覺建立起來，再回到程式碼本身。

如果可以的話，我會很建議直接看英文原文。它的英文其實非常好懂，敘述節奏也很友善，不會有那種只看官方規格、但腦中還是沒有畫面的感覺。

<figure>
  <img src="{{ '/kotlin/image/typealias-illustrated-guide-home.jpg' | relative_url }}" alt="Typealias Illustrated Guide 首頁截圖">
  <figcaption>Typealias 的 <code>Kotlin: An Illustrated Guide</code> 首頁，會直接整理章節、翻譯進度與程式碼列表。</figcaption>
</figure>

## 我為什麼非常推薦

- 它的文風非常口語，但不會因為口語就失去精準度。
- 很多 Kotlin 初學者卡住的點，它都會先把心智模型講清楚，再帶到語法。
- 插圖、卡通感畫面和情境設計做得很好，學起來不會那麼乾。
- 不是只講「怎麼寫」，而是會講「為什麼 Kotlin 要這樣設計」。
- 官網首頁本身也會整理最新文章與影片，很適合持續追。

## 這個網站特別適合拿來補什麼

我覺得它特別適合拿來補這些主題：

- null safety
- scope functions
- classes / objects / interfaces
- data classes / sealed types / delegation
- lambdas 與 function references
- coroutines 的基本觀念

它很適合從初學一路看到中階，尤其是當你已經會寫一點 Kotlin，但觀念還沒有真正穩下來的時候，這個網站很有幫助。

## 我特別喜歡的地方

我很喜歡它會用一個完整情境，把抽象語法講成真的可以腦補出畫面的東西。

例如 `Nulls and Null Safety` 那一章，不是直接丟 `String?`、`?.`、`?:`、`!!` 的規則給你背，而是先用咖啡店評論卡的故事，去解釋什麼叫做「值存在」和「值不存在」，再一路帶到 nullable type、compile time / runtime、safe-call、elvis operator 跟 not-null assertion。

這種寫法的好處是，你看完之後不只知道語法長什麼樣子，而是真的會比較理解 Kotlin 想幫你避免什麼問題。

<figure>
  <img src="{{ '/kotlin/image/typealias-null-safety-chapter.jpg' | relative_url }}" alt="Typealias Nulls and Null Safety 章節截圖">
  <figcaption><code>Nulls and Null Safety</code> 章節用插圖和情境故事拆解 Kotlin null safety，是很典型也很有代表性的 Typealias 風格。</figcaption>
</figure>

## 我會怎麼看這個網站

- 遇到某個 Kotlin 主題卡住時，先找它有沒有對應章節或概念文。
- 能看英文原文就直接看英文原文，通常理解速度會比想像中快。
- 看完文章後，回 Android Studio 自己打一遍小例子，把語法變成手感。
- 如果首頁剛好有相關影片，也可以一起看，補強記憶和理解。

## 我的簡短評價

如果要我在 Kotlin 學習資源裡面挑一個「很值得反覆看，而且真的能幫助你把觀念講清楚」的網站，`Typealias` 會是我很前面的推薦。

它不是那種只適合快速查語法的網站，而是那種你願意花時間讀，讀完會真的覺得自己對 Kotlin 更有感覺的內容。

## 官方來源

- 官網首頁: [typealias.com](https://typealias.com/)
- Illustrated Guide: [Kotlin: An Illustrated Guide](https://typealias.com/start/)
- Null Safety 章節: [Nulls and Null Safety](https://typealias.com/start/kotlin-nulls/)
- Code listings: [typealias-studios/kotlin-illustrated-guide](https://github.com/typealias-studios/kotlin-illustrated-guide)

## 來源說明

官方首頁會整理最新文章與影片；`Kotlin: An Illustrated Guide` 首頁則明確說明它會從 Kotlin fundamentals 開始，循序漸進帶讀者建立觀念。`Nulls and Null Safety` 章節則很能代表這個網站的特色：用具體故事、插圖與逐步推進的方式，解釋 Kotlin 的核心語言設計。
