---
title: Gemma 3 與 Gemma 3n
tags:
  - ai
  - llm
  - google
  - gemma
  - gemma-3
  - gemma-3n
  - model
desc: 整理 Google Gemma 3 主系列與 Gemma 3n 的定位、尺寸差異，以及本地部署時的選型方式。
author: icools
date: 2026-03-29
source: https://ai.google.dev/gemma/docs/core
status: active
---

# Gemma 3 與 Gemma 3n

我會先把 `Gemma 3` 看成 Google 開放權重模型裡比較通用的主力線，再把 `Gemma 3n` 看成更明確往手機、平板、筆電這類 on-device 情境優化的分支。

官方 `Gemma 3` 概覽頁目前列出 `270M`、`1B`、`4B`、`12B`、`27B` 五種尺寸。其中 `4B`、`12B`、`27B` 支援圖文輸入與 `128K` context，`270M` 和 `1B` 則偏文字任務，context 上限是 `32K`。整個家族也把多語支援擴到 `140+` 語言，並保留 function calling 能力。

## 我會怎麼理解 Gemma 3 主系列

- `270M`：超小型文字基座，適合拿來做專項微調或做 edge 方向的特殊模型基底。
- `1B`：仍偏輕量，適合資源比較有限、但又不想用到更大模型的文字場景。
- `4B`：開始進入多模態與長上下文，比較像真正可落地的單機通用款。
- `12B`：通常是品質與本地部署成本之間比較平衡的一檔。
- `27B`：如果你想在仍可自行部署的範圍內追求更高品質，通常會先看這個尺寸。

上面這種分法是我根據官方尺寸與能力整理出來的實務理解，不是 Google 官方的正式分級名稱。

## Gemma 3n 是什麼

`Gemma 3n` 不是單純把 `Gemma 3` 縮小，而是更直接朝 on-device generative AI 去設計。Google 在 `2025-05-20` 公布的開發者文章裡，把它描述成能在手機、平板、筆電上跑的多模態模型，支援文字、影像、音訊與影片理解，並用 `MatFormer`、`Per-Layer Embeddings` 與快取策略來降低實際記憶體與計算負擔。

目前最常看到的是兩個標記：

- `E2B`：較小的有效參數版本，偏向更吃重記憶體與功耗控制。
- `E4B`：較大的有效參數版本，品質更高，也比較接近大家口中說的「3n 大版」。

如果你看到有人口語上說 `Gemma 3n 4B` 或 `3n4`，通常大多是在指 `Gemma 3n E4B` 這個方向。

## Gemma 3n 值得記住的點

- 它是為裝置端而不是純雲端優先設計。
- 官方主打 `32K` context 與 `140+` 語言。
- `E2B` / `E4B` 的 `E` 指的是 effective parameters，比較接近實際執行時的有效負載概念，不只是表面總參數量。
- 這條線比較適合做個人裝置助理、即時多模態理解、私有推論與低延遲互動。

## 我會怎麼選

- 如果要一個比較通用、可做多模態理解、長上下文與 function calling 的 open model，我會先看 `Gemma 3 4B/12B/27B`。
- 如果要的是手機或筆電上的低延遲本地推論，我會優先看 `Gemma 3n E2B/E4B`。
- 如果只是要一個超小基座，再往特定任務 fine-tune，`Gemma 3 270M` 會是很值得注意的起點，因為後面的 `FunctionGemma` 也是從這條線延伸出來的。

## 延伸閱讀

- [FunctionGemma](functiongemma.md)
- [TranslateGemma](translationgemma.md)

## 參考

- [Gemma 3 model overview](https://ai.google.dev/gemma/docs/core)
- [Gemma explained: What's new in Gemma 3](https://developers.googleblog.com/en/gemma-explained-whats-new-in-gemma-3/)
- [Introducing Gemma 3n](https://developers.googleblog.com/en/introducing-gemma-3n/)
