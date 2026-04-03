---
title: Gemma 4 從 Function Calling 到 LLM to Action
tags:
  - ai
  - google
  - gemma4
  - function-calling
  - android
desc: 從 Google AI Edge Gallery 的 Agent Skill 架構切入，整理 Gemma 4 在 Android 上的 tool workflow 與 agent 能力。
author: icools
date: 2026-04-03
source: local
status: active
---

# Gemma 4 從 Function Calling 到 LLM to Action：Android 上的 Agent Skill 能力深度解析

本學習指南旨在深入探討 Google DeepMind 推出之 Gemma 4 模型在行動裝置端（特別是 Android 平台）實現的代理（Agentic）能力。透過 Google AI Edge Gallery 的 SKILL 框架，開發者能將大型語言模型（LLM）從單純的聊天機器人轉化為具備自主行動能力的 Agent。


--------------------------------------------------------------------------------


一、 Agent Skill 的核心架構與運作機制

在 Google AI Edge 的生態系中，Agent Skill 是一種模組化能力，旨在擴展 LLM 的功能邊界。其底層技術核心為 函式呼叫（Function Calling） 與 代理工作流（Agent Workflow）。

1. SKILL 框架載入機制

Google AI Edge Gallery 引入了一種創新的 SKILL 框架載入機制，其最大的技術優勢在於：

* 動態載入：開發者可以隨時從不同來源（如 URL、本地文件或社群分享）載入新的 SKILL。
* 無需重新構建：新增或更新功能時，不需要重新編譯或發布 Android App 應用程式，實現了功能擴充的靈活性。

2. 四種不同的使用方式（Usage Patterns）

根據複雜度與執行路徑，Agent Skill 可分為以下四種方式：

模式	描述	技術實現細節
1. 純 Skill (Pure Skill)	最簡單的模式，提供特定人格或場景數據。	僅需 SKILL.md 文件，包含 MetaData 與指令（Instructions），不涉及外部代碼。
2. Skill + 純 JS (Skill + Pure JS)	執行背景邏輯而不顯示介面。	使用隱藏的 Webview 執行 JavaScript，透過 ai_edge_gallery_get_result 函數回傳 JSON 結果。
3. Skill + Web UI + JS	在對話中嵌入交互式網頁界面。	除了 JS 邏輯外，可回傳 webview 字段，載入 assets 資料夾中的 HTML 進行 UI 渲染。
4. 原生 Intent 調用 (Native Intent Call)	直接觸發 Android/iOS 系統內建功能。	模型預測並調用 run_intent 工具，執行如發送郵件、設置提醒或開啟地圖等 OS 級操作。


--------------------------------------------------------------------------------


二、 技術組件與執行環境

1. Gemma 4 的角色

Gemma 4 是一系列最先進的開放模型，重新定義了裝置端硬體的可能性。其關鍵特性包括：

* 自主行動能力：支援多步規劃（Multi-step planning）與自動化執行，無需專門微調。
* 長上下文支援：具備 128K 上下文視窗，能處理複雜的代理任務。
* 模型多樣性：包含 E2B（2.58 GB）與 E4B（3.65 GB）等不同規模，適配各類行動裝置。

2. LiteRT-LM 推論優化

LiteRT-LM 是專為裝置端 LLM 推論設計的高性能框架，為 Agentic 使用場景提供技術支撐：

* 硬體加速：利用 GPU 與 NPU 實現極速推論。在 S26 Ultra 上，GPU 預填速可達每秒 3808 tokens。
* 受限解碼（Constrained Decoding）：確保模型產生結構化的 JSON 輸出，這對於工具調用（Tool Calling）的穩定性至關重要。
* 動態上下文：靈活處理單一模型在 CPU 與 GPU 上的執行，最大化利用硬體資源。

3. Skill-Creator 的未來展望

文件進一步思考了透過更強大的模型（如 Gemini Pro 3.1）來動態產生所需 Skill 的可能性。這種「Skill-Creator」模式可以根據用戶需求，即時建立新的 Skill 或小型 Web App，從而實現無限的功能擴展空間。


--------------------------------------------------------------------------------


三、 術語表 (Glossary)

* Function Calling (函式呼叫)：模型辨識出用戶需求需要外部工具支援時，輸出特定格式的參數（通常為 JSON），交由系統執行相應函數。
* On-Device AI (裝置端 AI)：所有推論過程均在本地硬體執行，不需網路連接，具備高度隱私性與低延遲。
* LiteRT：Google AI Edge 的輕量級執行階段，前身為 TensorFlow Lite，用於優化模型執行。
* AICore：Android 系統服務，負責管理與分發如 Gemini Nano 或 Gemma 4 等裝置端基礎模型。
* Kebab-case：Skill 資料夾命名規範（例如 fitness-coach），所有字母小寫並以連字號分隔。
* SKILL.md：定義 Skill 的核心文件，包含模型決定是否觸發該技能的名稱與描述 metadata。


--------------------------------------------------------------------------------


四、 知識測驗 (Quiz)

問題 1： 根據文件，Agent Skill 底層依賴的兩項核心技術是什麼？ 問題 2： 在 Google AI Edge Gallery 中，如果開發者想要在不重新構建 App 的情況下增加新功能，應利用哪種機制？ 問題 3： 哪一種 Skill 使用方式最適合需要透過 Android 系統發送電子郵件的場景？ 問題 4： JavaScript Skill 是如何在受到安全限制的行動裝置沙盒環境中執行邏輯的？ 問題 5： LiteRT-LM 的哪項特性可以確保 AI 驅動的應用程式在工具調用時保持可靠的輸出格式？


--------------------------------------------------------------------------------


五、 答案對照 (Answer Key)

答案 1： 函式呼叫（Function Calling）與代理工作流（Agent Workflow）。 答案 2： SKILL 框架載入機制（可隨時從不同來源載入新的 SKILL）。 答案 3： 原生 Intent 調用 (Native Intent Call)，具體是透過模型調用 run_intent 工具。 答案 4： 透過在一個隱藏的 Webview 中加載 HTML 並執行腳本。 答案 5： 受限解碼（Constrained Decoding）。
