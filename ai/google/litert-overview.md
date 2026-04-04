---
title: LiteRT Overview：Google 裝置端 AI 執行框架
tags:
  - ai
  - google
  - litert
  - on-device-ai
  - edge-ai
  - draft
desc: LiteRT 是 Google 在裝置端部署 ML 與生成式 AI 的執行框架，適合從 Android 與 edge AI 脈絡理解 on-device inference 的技術底座。
author: icools
date: 2026-04-04
source: https://ai.google.dev/edge/litert/overview
status: active
---

# LiteRT Overview

## 這篇在看什麼

這篇其實是我先把一個之後一定會常遇到的 Google AI Edge 入口記下來。

`LiteRT` 可以把它理解成 Google 現在用來承接裝置端 ML 與生成式 AI 推論的重要框架。從 Android / edge AI 的角度來看，它不是單一模型，而是比較接近整個 on-device inference 的技術底座。

我原本收進 `draft/` 的是 `LiteRT-LM` 相關入口，但現在看官方脈絡，直接用 `LiteRT overview` 當正式整理入口更合理。

## 我為什麼先收進來

- 只要之後持續看 Android 上的 on-device AI、Gemma、Google AI Edge Gallery，就很難繞過 `LiteRT`
- 它不只是舊的 TensorFlow Lite 換名字，而是 Google 在 edge / on-device AI 上更完整的 runtime 與部署方向
- 對想把 LLM、vision 或 generative AI 帶到裝置端的人來說，這是很核心的基礎材料

## 主要重點

- 裝置端 ML 與 GenAI 的執行框架
- 強調轉換、runtime 與硬體加速整合
- 和 Android、Edge、Gemma、AI Edge Gallery 這些主題有很強連動
- 文件裡也會看到 `.litertlm` 這類和 on-device LLM 相關的格式與脈絡

## 我接下來會怎麼看

之後如果我要繼續整理：

- Android 上的 on-device LLM
- Gemma 4 與 agentic workflow
- Google AI Edge Gallery 的 skill / action 執行方式

那這篇 `LiteRT` 入口會是很好的基礎參考點。

## 官方來源

- 官方文件: [LiteRT overview](https://ai.google.dev/edge/litert/overview)

## 來源說明

Google AI for Developers 的 `LiteRT overview` 會從整體框架角度介紹 LiteRT 在裝置端 ML 與生成式 AI 的定位。對我來說，這比只記單一功能頁更適合當成後續整理入口。
