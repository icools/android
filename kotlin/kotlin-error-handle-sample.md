---
title: KotlinErrorHandleSample 錯誤處理筆記
tags:
  - android
  - kotlin
  - error-handling
  - result
  - sealed-class
  - arrow
desc: KotlinErrorHandleSample 是我自己長期整理 Kotlin / Android 錯誤處理方式的 private repo，涵蓋 try-catch、Result、Arrow 風格與 sealed class 錯誤建模。
author: icools
date: 2026-03-29
source: local
status: active
---

# KotlinErrorHandleSample

## 專案簡介

`KotlinErrorHandleSample` 是我自己另外整理的一個 private repo，主題很聚焦，就是：

Kotlin / Android 專案裡，錯誤處理到底可以怎麼寫、怎麼拆、怎麼回傳，才會比較順手、比較能維護。

這個 repo 我其實陸陸續續寫了很多年，但到現在都還沒有完全整理完成，原因很單純，因為我一直很想把它寫得更完整、更清楚，所以反而拖很久。

不過就算還沒完全 finish，我還是覺得它已經很有價值，因為裡面整理的是我自己在實際寫 Kotlin / Android 時，反覆碰到的錯誤處理選擇與心得。

## 它大概在整理什麼

這個 repo 主要不是在講某一個單一框架，而是在整理「錯誤應該怎麼表達」這件事。

目前大致包含的方向會像這些：

- 最傳統的 `try-catch`
- Kotlin 內建的 `Result`
- 類似 Arrow 生態常提到的 typed error handling 思路
- 用 `sealed class` 建模多型錯誤
- 透過 wrapper / extension 的方式包裝回傳
- 把 success / error 統一成可傳遞、可組合的回傳模型

如果再講得更白話一點，這個 repo 就是在研究：

「當 Kotlin / Android 專案開始變複雜之後，錯誤不要只剩下 `throw Exception`，那還有哪些比較可讀、可組合、可維護的寫法？」

## 為什麼我會想特別做這個主題

我自己一直覺得，錯誤處理是很容易被低估的主題。

因為一開始大家常常都先把功能做出來，但專案一長大，就會開始遇到：

- 到底這個錯誤要不要丟 exception
- 到底要不要往上包成 `Result`
- 到底 UI 層該拿到什麼型態
- 到底 domain / repository 應不應該知道具體錯誤細節
- 到底 error message、error type、retry info 要放哪裡

而且 Kotlin 剛好又提供了幾條不同路線，所以這個題目很容易越寫越深。

## 我關注的幾種錯誤處理路線

### 1. try-catch

這是最直覺也最傳統的方式。

它的優點是：

- 大家都看得懂
- 對 exception-based API 很自然
- 上手成本低

但缺點也很明顯：

- 容易散落在各層
- 容易讓真正的錯誤語意不清楚
- 複雜流程裡常常會變成一坨

所以我會把它當成基礎，但不一定會把整個 app 的錯誤策略都建立在它上面。

### 2. Kotlin 內建 Result

Kotlin 內建的 `Result` 很適合拿來處理：

- 成功 / 失敗二分的操作
- 想避免一路 throw exception
- 想把結果顯式地往上傳

它的好處是：

- 標準庫就有
- 很容易搭配 `runCatching`
- 對小型到中型流程很實用

但如果你想表達更複雜的錯誤語意，它有時又會顯得不夠細。

## 3. Arrow 風格的 typed error handling

我自己也很在意 Arrow 這條線，因為它代表的是另一種思路：

不是只把錯誤當成「失敗」，而是把錯誤當成有型別、有語意、可以被組合與推導的資料。

這類思路通常會讓你更認真思考：

- 這是什麼錯
- 誰應該知道這個錯
- 這個錯能不能 recover
- 這個錯要不要轉譯成別的層級語意

對我來說，Arrow 相關概念的價值不一定只在於你最後是不是完全採用它，而是它會逼你把錯誤模型想得更清楚。

## 4. sealed class 多型錯誤建模

如果你想表達的不只是 success / fail，而是：

- network error
- auth error
- validation error
- business rule error
- unexpected error

那 `sealed class` 幾乎就是 Kotlin 非常自然的一條路。

我自己很喜歡這條線，因為它能讓：

- 錯誤型態明確
- `when` 分支可讀
- UI / domain / data layer 的責任比較容易切

這種做法也很適合和 `Result`、Arrow 風格、wrapper pattern 互相混搭。

## 5. extension / wrapper 式回傳整理

我也很喜歡研究另外一條路，就是不要讓每個地方都手寫重複的錯誤轉換，而是透過：

- extension
- wrapper
- helper function

把常見錯誤回傳規則整理成一組比較可重用的風格。

這樣的好處是：

- 呼叫端比較乾淨
- 錯誤轉譯比較集中
- 專案裡比較容易形成一致慣例

## 我會怎麼看這個 repo 的價值

對我來說，`KotlinErrorHandleSample` 不只是某幾個 sample code，而是一個持續整理中的思考筆記庫。

它很適合拿來思考：

- exception 和 return type 到底怎麼分工
- `Result` 夠不夠用
- 什麼時候要往 typed error 前進
- 什麼時候要讓 UI 拿到可顯示的錯誤模型
- 什麼時候該用 `sealed class` 明確建模

如果未來我把它整理得更完整，我覺得它會很適合拿來當：

- Kotlin error handling 的參考 repo
- Android 專案架構討論素材
- 團隊之間對齊 error strategy 的起點

## 這一篇我想保留的語氣

這篇我也想刻意寫得很誠實。

我並不會把自己定位成「錯誤處理專家」，也不會說我對這一塊已經有非常完整、非常標準答案的掌握。

比較準確的說法是：

這是我自己長期在 Kotlin / Android 專案裡使用、踩坑、比較、思考之後留下來的心得整理。

所以我會希望它被看成：

- 個人使用心得
- 持續整理中的實驗與思考
- 可以交流與一起討論的素材

而不是一份自稱權威結論的教科書。

## Todo

- 之後把 repo 再整理完整，讓結構更清楚。
- 把不同錯誤處理方式的適用情境寫得更具體。
- 補更多 Kotlin / Android 實際範例。
- 如果整理成熟，希望未來可以開放給大家參考。
- 如果未來公開，也歡迎大家發 issue 給我，一起交流和討論。
- 我會盡量把它寫成「真實使用心得 + 可對話」的形式，而不是假裝自己是這領域的絕對專家。

## 我的簡短定位

如果要我幫這個 repo 下一句話，我會說：

這是一個還在持續長大的 Kotlin / Android 錯誤處理樣本集，重點不只是教你怎麼 catch error，而是整理「錯誤在不同層到底應該怎麼被表達」。

## 參考連結

- 私有 repo：[`icools/KotlinErrorHandleSample`](https://github.com/icools/KotlinErrorHandleSample)（目前為 private repo，尚未對外開放）

## 來源說明

這篇筆記主要根據我自己對 `icools/KotlinErrorHandleSample` 這個 private repo 的整理方向與使用心得撰寫而成。由於目前 repo 還在持續整理、也尚未公開，因此這篇先作為知識庫中的介紹與備忘。
