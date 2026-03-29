---
title: TranslateGemma
tags:
  - ai
  - llm
  - google
  - gemma
  - translation
  - model
desc: TranslateGemma 是建立在 Gemma 3 上的開放翻譯模型家族，主打 55 語言翻譯品質與更高效率的部署成本。
author: icools
date: 2026-03-29
source: https://blog.google/innovation-and-ai/technology/developers-tools/translategemma/
status: active
---

# TranslateGemma

`TranslateGemma` 是 Google 以 `Gemma 3` 為基底做出的翻譯專用模型家族。Google 在 `2026-01-15` 公布時，把它定位成 open translation models，目前提供 `4B`、`12B`、`27B` 三種尺寸，主打 `55` 種語言翻譯。

## 它和一般 Gemma 3 的差別

雖然它仍然建立在 `Gemma 3` 上，但目標已經從通用生成任務，轉成更專注的翻譯品質與推論效率。

Google 公開的重點有幾個：

- `12B TranslateGemma` 在 `WMT24++` 上以 `MetricX` 衡量時，表現超過 `Gemma 3 27B` baseline。
- `4B` 版本也能接近較大 baseline 的翻譯表現，所以很適合 mobile inference。
- 它不是只有純文字翻譯，也能利用 `Gemma 3` 的多模態能力處理圖片內文字翻譯。
- 除了主要 `55` 語言，Google 還提到曾額外訓練接近 `500` 個語言對，讓研究者可再做 fine-tune 與延伸。

這條線的意思很直接：如果任務明確是翻譯，專項模型往往會比拿通用模型硬做翻譯更省資源，也更穩。

## 我會怎麼看三種尺寸

- `4B`：如果你在意行動端、edge 或吞吐量，這一檔最值得先看。
- `12B`：如果想要更高品質，同時又不想直接把成本拉到最大，這一檔通常最平衡。
- `27B`：偏向追求最高品質，較適合雲端或比較有資源的單機部署。

這裡的分法是我的實務理解，用來幫助選型，不是官方硬性定義。

## 什麼情況特別適合 TranslateGemma

- 你做的是翻譯產品，不是一般聊天產品。
- 你要長期跑大量翻譯，希望品質、吞吐量與成本一起顧。
- 你需要處理低資源語言，或想在既有翻譯模型上再做 domain fine-tune。
- 你需要圖片內文字翻譯，而不是只有 text-to-text。

## 什麼時候還是先回到 Gemma 3

如果你的需求其實是通用聊天、視覺理解、長文分析、function calling，翻譯只是其中一小部分，那直接用 `Gemma 3` 主系列會比較合理。`TranslateGemma` 最有價值的地方，是任務足夠聚焦在翻譯本身。

## 參考

- [TranslateGemma: A new family of open translation models](https://blog.google/innovation-and-ai/technology/developers-tools/translategemma/)
- [TranslateGemma technical report](https://arxiv.org/abs/2601.09012)
- [Gemma 3 model overview](https://ai.google.dev/gemma/docs/core)
