---
title: OpenAI Realtime API (GPT-4o) 實戰心得：語音與視訊的即時交互
tags:
  - ai
  - openai
  - api
  - realtime
  - voice
desc: 整理 OpenAI Realtime API (gpt-4o-realtime-preview) 的核心概念，包含語音/視訊即時處理、Token 成本計算以及與傳統推論方式的差異。
author: icools
date: 2026-04-04
source: https://x.com/OpenAIDevs/status/2040094154899050515
status: active
---

# OpenAI Realtime API (GPT-4o)

目前的 `gpt-4o-realtime-preview` 正在改變語音與視訊互動的門檻。傳統的語音助手（STT -> LLM -> TTS）通常會有明顯的延遲，而 Realtime API 讓模型能直接與音頻串流互動，達成毫秒級的響應。

## 核心亮點

- **低延遲交互**：直接串流音頻而非文字，省去轉換開銷，適合做客服助手、口語教練或導航。
- **Voice + Compute Use**：除了語音模型本身的運算，還會處理即時的音頻特徵。這跟傳統的 `input_tokens` 與 `output_tokens` 計算方式有所不同。
- **Function Calling**：Realtime 模式同樣支援 Function Calling，代表你可以讓模型在對話中直接操作你的桌面應用（Work with Apps）。

## 實際案例觀察

在 [OpenAI Devs 的展示](https://x.com/OpenAIDevs/status/2040094154899050515)中，可以看到語音 Agent 能即時針對瀏覽器內的視窗內容進行 Debug 並直接回報。這也預示了未來 AI 不只是回答問題，而是能看、能聽並具備即時反應能力的代理人。

## 相關筆記
- [ChatGPT Connect / Apps 生態心得](../chatgpt/chatgpt-connect-workflow.md)
- [Codex CLI](../codex/codex-cli.md)