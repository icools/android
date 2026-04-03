---
title: Gemma 基本介紹
tags:
   - ai
   - google
   - gemma
   - overview
desc: Gemma 開放權重模型家族的定位，以及它和 Gemini、on-device AI、tool use 之間的關係。
author: icools
date: 2026-04-03
source: local
status: active
---

# Gemma 基本介紹

Gemini 是 Google 推出的生成式 AI 模型家族，可以用來處理 **文字、圖片、語音、影片、程式碼** 等不同類型的內容。  
如果講白話一點，它不是只有「聊天機器人」而已，而是一套可以拿來做理解、生成、分析、摘要、工具呼叫，甚至進一步做 agent 的模型系統。

## Gemini 可以做什麼？

Gemini 常見的用途包含：

- 文字問答與對話
- 文件摘要與重點整理
- 圖片理解與描述
- 程式碼生成與修改
- 結構化輸出（例如 JSON）
- Function Calling / Tool Use
- 多步驟推理與 agent workflow

所以它比較像是一個通用型 AI 大腦，而不是只會回你幾句話的聊天工具。

## Gemini 的特色

Gemini 比較重要的幾個特色，大致可以這樣理解：

1. **多模態**  
   不只是處理文字，也能看圖片、理解畫面、接收不同型態輸入。

2. **長上下文能力**  
   可以一次讀比較長的內容，像是長文件、對話紀錄、程式碼專案片段。

3. **工具呼叫能力**  
   可以搭配 function calling 或外部工具，讓模型不只是回答問題，還能去執行某些行為。

4. **雲端與裝置端生態延伸**  
   Gemini 比較偏向雲端大型模型能力，而 Gemma 則更偏向開放權重與 edge / on-device 的延伸方向。兩者雖然定位不同，但可以放在同一個 Google AI 生態裡一起看。

## Gemini 跟 Gemma 差在哪？

這兩個名字很像，很容易一開始看混 😆

可以先用這種方式理解：

- **Gemini**：Google 的旗艦生成式 AI 模型家族，偏向雲端能力、API、產品整合、多模態大模型。
- **Gemma**：Google 推出的開放權重模型家族，偏向研究、開發、自行部署、edge / on-device 使用。

也就是說：

- 想用 Google 很完整的雲端 AI 能力，通常會先想到 Gemini
- 想自己下載模型、研究架構、跑在本地或手機上，會更常看到 Gemma

## 為什麼 Gemini 很重要？

因為現在很多 AI 應用，已經不是單純只問答而已，而是會結合：

- 搜尋
- 文件處理
- 圖像理解
- 自動化流程
- 工具呼叫
- 多輪互動

Gemini 在這個脈絡下，就是 Google 用來支撐這些能力的一個核心模型系列。

## 一句話理解

**Gemini 是 Google 的生成式 AI 主力模型家族，主打多模態、長上下文、工具使用與整合能力；而 Gemma 則比較偏向可自行部署、可延伸到本地與邊緣裝置的開放權重路線。**