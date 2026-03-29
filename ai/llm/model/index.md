---
title: LLM 模型家族索引
tags:
  - ai
  - llm
  - model
  - index
desc: 整理不同 LLM 模型家族與版本筆記，目前收錄 Gemma 3、FunctionGemma、TranslateGemma 與 Qwen 3.5。
author: icools
date: 2026-03-29
source: local
status: active
---

# LLM 模型家族

這一頁整理不同模型家族的定位、版本差異、部署條件與選型筆記。

- [Google Gemma 3](google/gemma/3/index.md) - 整理 `Gemma 3` 主系列，以及 `Gemma 3n`、`E2B`、`E4B` 這些常一起被提到的版本差異。
- [FunctionGemma](google/gemma/3/functiongemma.md) - Google 針對 function calling 做專項調整的輕量模型，偏向本地 agent 與 edge device。
- [TranslateGemma](google/gemma/3/translationgemma.md) - 建立在 `Gemma 3` 上、專門做翻譯的模型家族，重點在高效率多語翻譯。
- [Qwen 3.5](alibaba/qwen/3-5/index.md) - 阿里通義千問 `Qwen 3.5` 家族的定位、架構方向與 hosted / self-host 差異。

## 這個子分類會放什麼

- 模型家族的定位
- 主要版本與尺寸差異
- 本地部署與雲端部署的取捨
- 哪種任務比較適合哪條模型線
