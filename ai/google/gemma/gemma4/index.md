---
title: Gemma 4 索引
tags:
  - ai
  - google
  - gemma4
  - index
desc: Gemma 4 相關筆記入口，聚焦模型定位、tool workflow、function calling 與 Android skill runtime。
author: icools
date: 2026-04-03
source: local
status: active
---

# Gemma 4

這一頁整理 Gemma 4 相關筆記，重點放在 Android、tool use、agent workflow 與 on-device runtime。

- [Gemma 4 介紹]({{ '/ai/google/gemma/gemma4/gemma4/' | relative_url }}) - 從模型家族定位、多模態、長上下文與 edge 使用場景開始看。
- [Gemma 4 從 Function Calling 到 LLM to Action]({{ '/ai/google/gemma/gemma4/gemma4_function_calling/' | relative_url }}) - 聚焦 Agent Skill 架構、四種 skill 型態與 LiteRT-LM 的角色。
- [Gemma 4 與 LLM to Action]({{ '/ai/google/gemma/gemma4/gemma4_llm_to_action/' | relative_url }}) - 拆解 function calling 與 agentic workflow 的差異，說清楚模型與 runtime 的責任分工。
- [Android 上的 Gemma Agent Skill 是怎麼實作的？]({{ '/ai/google/gemma/gemma4/gemma4_how_android_skill_made/' | relative_url }}) - 追 Kotlin、SKILL.md、hidden WebView 與 runtime orchestration 的技術脈絡。