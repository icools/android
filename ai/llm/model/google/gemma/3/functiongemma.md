---
title: FunctionGemma
tags:
  - ai
  - llm
  - google
  - gemma
  - function-calling
  - model
desc: FunctionGemma 是從 Gemma 3 270M 延伸出的 function calling 專用模型，適合本地 agent、裝置端 API 動作與 edge deployment。
author: icools
date: 2026-03-29
source: https://blog.google/innovation-and-ai/technology/developers-tools/functiongemma/
status: active
---

# FunctionGemma

`FunctionGemma` 可以把它看成 `Gemma 3 270M` 的專項版。Google 在 `2025-12-18` 公布時，明確把它定位成為 function calling 微調過的模型基底，目標不是做泛用聊天，而是把自然語言穩定轉成可執行的 API action。

## 它的核心定位

我會把 `FunctionGemma` 理解成一顆偏 agent runtime 的小模型：

- 它夠小，適合放到 edge device 或手機上跑。
- 它不是只會吐結構化呼叫，也能在執行工具後再回頭用自然語言和使用者互動。
- 它的設計重點是「可再訓練」，不是只靠 zero-shot prompt 撐場。

Google 也特別提到，它可以獨立處理離線與私有任務，也可以在複合系統裡當前線路由器，把簡單動作留在本地完成，再把更複雜的事情轉交給像 `Gemma 3 27B` 這種較大的模型。

## 為什麼這條線有意思

官方文章裡有幾個點很重要：

- 在 `Mobile Actions` 評測裡，經過 fine-tuning 後，準確率從 `58%` 拉到 `85%`。
- 它強調 local-first deployment，重點是低延遲與資料隱私。
- Google 直接把它描述成可訓練成 custom、fast、private、local agents 的基礎。
- 部署工具鏈很廣，包含 `LiteRT-LM`、`vLLM`、`MLX`、`llama.cpp`、`Ollama`、`Vertex AI`、`LM Studio`。

這代表它不像大型通用模型那樣想包山包海，而是瞄準「一組已知工具 + 明確 API surface + 高一致性動作執行」這種場景。

## 什麼時候適合用它

- 智慧家庭、媒體控制、導航、裝置設定這類 action set 很固定的任務。
- 手機端或邊緣裝置上的助理功能。
- 你願意用自己的資料做 fine-tune，想換取更穩的工具呼叫結果。
- 你想把常見指令留在本地執行，只把困難任務丟給雲端大模型。

## 什麼時候不要先選它

如果你的需求是開放式推理、廣泛知識問答、長文分析，或根本沒有固定 tool schema，那 `FunctionGemma` 就不是第一選擇。這種情況通常還是該先看 `Gemma 3 4B/12B/27B` 這種比較通用的主系列。

## 參考

- [FunctionGemma: Bringing bespoke function calling to the edge](https://blog.google/innovation-and-ai/technology/developers-tools/functiongemma/)
- [Gemma function calling guides](https://ai.google.dev/gemma/docs/capabilities/function-calling)
- [Gemma fine-tuning guides](https://ai.google.dev/gemma/docs/core/tune)
