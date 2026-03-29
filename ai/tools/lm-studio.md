---
title: LM Studio
tags:
  - ai
  - tools
  - lm-studio
  - local-llm
  - mac
desc: LM Studio 是本地模型下載與執行工具，適合在桌面上快速測試 LLM、切模型與提供本地 API。
author: icools
date: 2026-03-29
source: local
status: active
---

# LM Studio

`LM Studio` 是我會放進本地 AI 工具鏈裡的重要工具，因為它讓「下載模型、切模型、直接在本機跑」這件事變得很直覺。

## 它適合做什麼

如果你想在本地快速測模型效果，LM Studio 很方便，因為它通常可以把模型探索、下載與執行整合在同一個桌面工作流裡。

常見用途包括：

- 快速下載本地模型
- 比較不同模型的回應風格
- 先在本機驗證 prompt 與效果
- 開一個本地 API 給其他工具串接

## 和 Hugging Face 的關係

很多時候流程會是：

1. 在 `Hugging Face` 先找到感興趣的模型
2. 看它有哪些可用格式或社群整理版本
3. 再用 `LM Studio` 把適合本地跑的版本抓下來測

所以 Hugging Face 比較像探索入口，LM Studio 比較像本地執行工作台。

## 為什麼在 Mac 上特別有感

如果你用的是 Mac，特別是 Apple Silicon，LM Studio 這類工具的價值很明顯，因為它讓你可以很快建立一套本地模型實驗環境。

這種做法的好處是：

- 測試速度快
- private data 比較容易留在本地
- 很適合 prototype
- 可以先做模型比較，再決定要不要上雲端

如果你同時也在看 `MLX` 生態，那 Hugging Face、LM Studio 與 Apple 本地模型工具之間就會形成一條很實用的探索路徑。

## 我會怎麼用它

我會把 LM Studio 看成一個偏本地實驗室型工具。

它很適合以下情境：

- 想先在本地試模型，不想一開始就接遠端 API
- 想做 prompt 實驗
- 想把模型接到自己手邊的小工具或 script
- 想理解不同模型在同一台機器上的差異

如果目標是建立自己的 local AI playground，LM Studio 是很值得長期保留的一個基礎工具。
