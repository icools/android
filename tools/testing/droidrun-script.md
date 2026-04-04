---
title: DroidRunScript 工具介紹
tags:
  - android
  - tool
  - automation
  - adb
  - python
  - lm-studio
  - local-llm
desc: DroidRunScript 是一個把 LM Studio、本地模型、Python 與 ADB 串起來的 Android 手機自動操作實驗工具。
author: icools
date: 2026-03-29
utl: https://github.com/droidrun/droidrun
source: local
status: active
---

# DroidRunScript

## News 

Update🚨: Now you can run tasks directly on your mobile device with Droidrun's portal APK. 


https://x.com/droid_run/status/2040100261666926661

## 工具簡介

`DroidRunScript` 是我自己在玩的 private repo，核心想法很直接：

把 `LM Studio` 的本地模型、`Python` 腳本、`ADB` 與 Android 上的輔助服務串起來，讓電腦端可以用自然語言去驅動手機操作。

如果換成比較白話的說法，它就是一個：

「讓本地 AI 模型幫你理解任務，再透過 Python 與 ADB / UI 工具把手機操作執行出去」的實驗型控制器。

目前它還是我自己的 private repo，現階段比較偏個人研究與實驗用途，暫時不是打算直接公開給別人用；之後如果我把它磨得更穩，才會考慮是不是要開放。

## 它大概在做什麼

根據目前 repo 裡的 `README` 和 `app.py`，這個工具的核心流程是：

1. 用 `LM Studio` 提供一個本地 OpenAI-like API endpoint。
2. 在 Python 端透過 `llama_index.llms.openai_like.OpenAILike` 接上這個 endpoint。
3. 用 `DroidAgent` + `AdbTools` 理解任務、分析手機畫面，並執行點擊、滑動、輸入等操作。
4. 透過 Android 端的輔助服務 / UIAutomator 類能力讀取畫面元素，讓 agent 不只是盲點座標。

也就是說，這不是單純「丟一條 `adb shell input tap` 指令」，而是往「AI 代理幫你理解手機畫面並決定下一步」的方向走。

<pre class="mermaid">
flowchart LR
    USER["自然語言任務"] --> PY["Python Script"]
    PY --> LLM["LM Studio Local Model"]
    PY --> ADB["ADB / AdbTools"]
    ADB --> PHONE["Android Phone"]
    PHONE --> ACCESS["Accessibility / UIAutomator Helper"]
    ACCESS --> PY
    LLM --> PY
</pre>

## 為什麼我覺得它很值得試

我覺得 `DroidRunScript` 最有趣的地方，不只是「AI 可以操作手機」，而是它把一條原本偏昂貴的代理路線，往本地模型方向拉。

這件事很重要，因為很多手機自動化 / agent 類工具一旦走到雲端模型，就會碰到：

- token 成本
- latency
- private data 要不要送出本地
- 長期跑 workflow 的費用問題

而 `LM Studio + local model + Python + ADB` 這條線，提供了一個很實際的替代方案：

- 本地可跑
- 成本低很多
- 可直接和自己手邊的腳本、測試工具、ADB 工作流串接
- 很適合當實驗平台

## 它能做哪些事情

從 README 的描述來看，這套東西目前瞄準的是 Android 裝置自動操作，例如：

- 打開 App
- 搜尋畫面內容
- 點擊按鈕
- 滑動頁面
- 輸入文字
- 讀取目前畫面的 UI / 文字資訊
- 依任務描述連續執行多步驟操作

README 裡甚至已經把它放到比較像「手機代理」的方向，例如：

- 打開設定查看資訊
- 操作 Google 搜尋
- 開 App 後繼續做後續任務
- 根據畫面狀態調整下一步

這跟單純腳本式自動化最大的差異，是它多了一層模型理解能力。

## 它和一般 ADB 腳本有什麼不同

傳統 ADB 腳本比較像：

- 你自己先知道要點哪裡
- 你自己先寫死流程
- 畫面一改，腳本就容易壞

`DroidRunScript` 比較想走的方向則是：

- 由模型理解任務
- 再結合畫面資訊決定要操作哪個元素
- Python 腳本負責把模型決策和裝置控制串起來

當然，這不代表它一定比傳統腳本穩。反而更精確地說，它是把「流程控制」從純規則式，往「模型 + 工具」的 agent workflow 推進。

## LM Studio 在這裡的角色

這個 repo 很值得注意的一點，是它不是直接綁某個雲端模型，而是刻意走 `LM Studio` 的 OpenAI-like API 路線。

這代表：

- 模型可以替換
- 本地模型可以升級
- 未來不一定要綁死單一 provider

README 目前明確提到的範例模型是 `qwen2.5-vl-7b-instruct`，而 `app.py` 也是用 OpenAI-like client 連 LM Studio endpoint。這讓它的整體設計比較像一個本地 AI automation sandbox，而不是一次性 demo。

以我的看法，現階段如果追求穩定度與理解能力，雲端模型通常還是會更強，這點很誠實地說沒什麼好硬拗。

但這個 repo 的魅力就在於：

- 如果本地模型越來越聰明
- 如果 vision + tool use 能力越來越穩
- 如果 Mac / PC 本地推理速度繼續變好

那 `LM Studio` 上的 local model 很可能就會開始進入「夠用」甚至「很好用」的區間。

這也是為什麼我覺得這條線非常值得繼續試。

## 關於模型選擇

從 repo 內容看，現在比較偏向用 `Qwen 2.5 VL` 這一代模型來跑。這很合理，因為它本來就比較貼近視覺理解與畫面判讀。

如果之後要再往上試，我自己會很想看：

- `Qwen 3.5` 系列能不能把推理與決策品質再往上拉
- 本地 vision model 在 UI 理解上的穩定度有沒有明顯提升
- local model 能不能把多步驟手機任務的成功率拉到實用水準

這一段是我基於目前 repo 方向做的推測與延伸，不是 README 已經保證的能力，但我覺得非常值得後續實測。

## 為什麼它適合拿來做手機自動化控制

我很喜歡這個 repo 的一點，是它把幾個原本分散的能力黏在一起了：

- `Python` 負責 orchestration
- `LM Studio` 負責本地模型 serving
- `ADB` 負責裝置連接與控制
- Android 端輔助服務負責畫面資訊讀取

這種組合很適合拿來做：

- 手機操作代理實驗
- Android 自動化研究
- AI + ADB 的工具原型
- 本地模型能不能接手機控制的實驗平台
- 不想花雲端模型費用時的本地替代路線

如果你平常就在玩：

- Android automation
- AI agent
- 本地模型
- ADB / UIAutomator / Accessibility

那這個工具真的很對味。

## 我會怎麼看它的定位

我不會把 `DroidRunScript` 看成「已經成熟的產品」，而是看成一個很值得投資的實驗工具與原型平台。

它很適合回答這些問題：

- 本地模型現在到底能不能操作手機？
- 哪些任務 local model 已經夠用？
- 哪些任務還是得交給更強的雲端模型？
- Python + ADB + UI tree + vision 這條組合要怎麼拆才穩？

如果未來這條線再成熟一點，它很可能會變成一個非常有意思的本地手機 agent 控制層。

## 限制與現實面

這類工具的現實限制也要講清楚：

- 模型夠不夠聰明，直接決定成功率
- 手機畫面變動大時，流程穩定性仍然是挑戰
- Android 端需要額外輔助服務與權限設定
- 本地模型雖然省錢，但推理速度與品質仍然要看硬體
- 和成熟雲端模型相比，local model 目前通常還是弱一些

所以我會把它理解成：

「今天已經很值得玩，而且非常值得持續跟進，但還是要帶著工程實驗的心態。」

## 和這個知識庫裡其他主題的關係

如果把它放回我自己的知識庫脈絡中，它其實剛好站在幾個主題的交叉點：

- [LM Studio](../ai/tools/lm-studio.md)：本地模型 serving 與模型替換工作台
- [Shizuku](shizuku.md)：高權限系統操作的另一條延伸路線
- [Android Accessibility API](../android/api/accessibility-api.md)：畫面 UI tree、事件與互動操作的重要基礎
- `ADB` / Python 腳本：裝置控制與流程編排的基本功

所以這篇我放在 Android `tools` 很合理，但它同時也是一個很典型的 AI tool + Android automation 混血案例。

## 參考連結

- 私有 repo：[`icools/DroidRunScript`](https://github.com/icools/DroidRunScript)（目前為 private repo，暫時不對外開放）
- 官方 DroidRun 文件：[droidrun/droidrun](https://github.com/droidrun/droidrun)
- [LM Studio Docs](https://lmstudio.ai/docs/)
- [Android Debug Bridge (adb)](https://developer.android.com/studio/command-line/adb)

## 來源說明

這篇筆記主要整理自我自己的 private repo `icools/DroidRunScript` 中的 `README.md` 與 `app.py`，另外補上 DroidRun 官方 repo、LM Studio 文件與 ADB 文件作為背景脈絡。文中對「雲端模型仍然通常更強」與「未來 `Qwen 3.5` 值得再試」的段落，屬於我的實務判斷與延伸觀察。
