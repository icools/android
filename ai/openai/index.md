---
title: OpenAI 索引
tags:
  - ai
  - openai
  - index
desc: OpenAI 相關工具、平台與使用習慣索引頁。
author: icools
date: 2026-03-29
source: local
status: active
---

# OpenAI

這一頁整理 OpenAI 相關筆記，詳細文章會放在 `ai/openai/`。

- [Codex CLI](codex/codex-cli.md) - terminal-first 的 coding workflow，適合直接在 repo 裡讀、改、執行與驗證。
- [Codex Desktop](codex/codex-desktop.md) - 很多 ChatGPT 訂閱戶其實還沒把 Codex 用滿，這篇整理 desktop app、訂閱方案可用範圍，以及它為什麼已經不職是寫 code 工具。
- [ChatGPT Connect / Apps 生態心得](chatgpt/chatgpt-connect-workflow.md) - 整理我最近實際使用 ChatGPT Apps / Connectors 後的觀察，包含 Google 文件整合、Notion / Gemini / Codex 分工、ClickUp 串接與目前的限制。
- [GPT Realtime 1.5](api/gpt-realtime-1.5.md) - OpenAI 語音/即時視訊 API 詳細使用心法。

## ChatGPT

ChatGPT 是目前日常使用頻率最高的 AI 助手，除了基本對話以外，以下幾個功能特別值得關注：

- **Code Interpreter** — 內建 Python 沙盒環境，可以直接上傳檔案、跑程式、畫圖、做資料分析，不用另開 notebook，適合快速驗證想法與處理一次性資料任務。
- **Canvas** — 獨立的寫作與程式編輯畫布，可以針對局部段落修改、重寫、加註解，比在對話框裡來回貼程式碼流暢得多，適合長文撰寫與 code review 場景。
- **Connectors / Apps** — 將 ChatGPT 與外部服務串接（如 Google Drive、Zapier、Slack 等），讓 AI 能直接存取資料源或觸發動作，降低手動搬資料的成本。
- **Work with Apps** — 非常好用且省 token 的功能，讓 ChatGPT 直接與已開啟的桌面應用互動（如 VS Code、終端機），省去來回複製貼上的步驟，大幅提升工作流效率。
- **Desktop** — 桌面版客戶端，常駐在系統中隨時呼叫，支援快捷鍵叫出對話框，比開瀏覽器分頁更快進入工作狀態。
- **語音輸入** — 強力推薦的功能，直接用語音和 ChatGPT 對話，適合在散步、通勤或不方便打字時梳理想法、做 brainstorming，辨識品質與回應速度都很好。

## 之後可整理的方向

- 我平常怎麼用 Codex
- ChatGPT 各功能的適用情境比較
- Code Interpreter vs Google Colab 的分工
- Work with Apps 的實際使用案例
- OpenAI 工具之間的差異
- 適合拿來配合開發流程的用法
