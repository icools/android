---
title: Google AI Studio
tags:
  - ai
  - google
  - ai-studio
  - gemini
  - live-api
  - firebase
  - antigravity
desc: Google AI Studio 現在不只是 prompt playground，而是把模型測試、Live API、生成媒體、Build mode、GitHub 與 Firebase 串成一條從 idea 到部署的工作流。
author: icools
date: 2026-03-30
source: https://ai.google.dev/aistudio
status: active
---

# Google AI Studio

如果現在還把 `Google AI Studio` 只看成一個「測 prompt 的 playground」，其實已經有點低估它了。

以我現在看到的產品走向，它比較像一個 browser-first 的 AI workbench：前半段負責讓你快速試模型、試語音、試圖片、試 video，後半段則開始把 `Build`、`GitHub`、`Firebase`、`Cloud Run` 這些真正把東西做成 app 的流程接上來。

換句話說，這裡不只是「問模型問題」的地方，而更像是 Google 把 `idea → prototype → app → deploy` 這條線逐步收斂到同一個入口。

> 先校正一個時間點：`Firebase Studio` 不是已經停掉，而是官方在 `2026-03-19` 公告 sunset；`2026-06-22` 起停止建立新 workspace，`2027-03-22` 正式關閉。官方同時說明，會把 preview 期間的成果整合到 `Google AI Studio` 和 `Google Antigravity`。

## 我會怎麼定位它

我現在會把 `Google AI Studio` 分成四個主用途來看：

- `Chat / Prompt`：先把模型能力、system instruction、function calling、structured output、grounding 試清楚。
- `Stream`：直接測 `Live API` 的低延遲語音或影音互動，確認語音 agent 的對話節奏與工具能力。
- `Generate Media`：集中試 `Gemini` 原生生圖、`Veo` 影片、語音生成等媒體能力。
- `Build`：從一句 prompt 直接生成全端 app，預設前端是 `React`，後面可接 `Node.js` runtime、Secrets、Firebase、GitHub 與部署。

所以它現在已經不是單一路徑的 playground，而是一個把「模型探索」與「產品原型化」接在一起的入口。

## 介面怎麼看比較快

如果從現在的 AI Studio 介面來拆，我會這樣理解：

| 區塊 | 主要作用 | 我會怎麼用 |
| --- | --- | --- |
| 左側模式入口 | 在 `Chat`、`Stream`、`Generate Media`、`Build` 之間切換 | 先決定你是在驗模型，還是在做產品 |
| Prompt / Chat 區 | 輸入需求、追問、貼圖、貼檔案、調整 system instruction | 拿來快速收斂提示詞與功能需求 |
| Run settings | 選模型、調參數、安全設定，打開 structured output / function calling / code execution / grounding | 把能力測試和 API 設定先對齊 |
| Preview / Code 區 | 一邊看預覽，一邊看產生出的檔案與差異 | Build mode 最核心的地方，方便直接驗 UI 與回頭修 |
| Share / Publish / Settings | 分享 app、管理 secrets、發布到 Cloud Run，或串 GitHub / Firebase | 真正把 demo 往 production 靠攏 |

如果看你貼的那種 `Build mode` 畫面，辨識方式會更直覺：

- 左邊像 agent chat 與變更摘要區，會顯示功能說明、錯誤、checkpoint 與後續 prompt。
- 右邊上方是 `Preview / Code` 切換，可以一邊看跑起來的頁面，一邊看實際檔案。
- 右上通常會有 `Share`、`Publish`、`Remix` 這類動作，代表它不是只讓你看結果，而是準備讓你繼續迭代與發佈。
- 中央 preview 區本質上就是 app canvas，你可以一邊看介面，一邊再用 prompt 要求它改 UI、改流程、補功能。

這種配置很像把 `chat-first` agent、雲端 IDE、prototype preview、deployment panel 疊在同一個工作檯上。

## 三條最值得用的操作流程

### 1. 先把模型玩法試清楚

這條線還是 AI Studio 最基本、也最常用的入口：

1. 進 `Chat`。
2. 選模型，先把 `System Instructions`、temperature、tools 開關調好。
3. 視需求打開 `structured output`、`function calling`、`code execution`、`grounding`。
4. 反覆對話，把 prompt 和輸出格式收斂。
5. 確定方向後按 `Get code`，把同一組設定帶進實作。

官方 quickstart 其實把這條路講得很清楚：AI Studio 讓你先把 prompt prototype 好，再帶出程式碼，這對「先驗證，再進 repo」很有用。

### 2. 直接拿來測語音 Live API

如果你要測的是即時語音或多模態互動，我會直接走 `Stream`：

1. 切到 `Stream`。
2. 選 Live API 對應模型，開麥克風或影音來源。
3. 直接做來回對話，感受延遲、語音風格、插話行為與回應節奏。
4. 視需求測 `tool use`、`session management`、`ephemeral tokens` 這些真正上產品會碰到的能力。
5. 測完再把同一條設定導回 SDK 或 WebSocket 實作。

官方文件現在明寫 `Live API` 不是只有即時回話而已，還包含 `Voice Activity Detection`、tool use、session management、ephemeral tokens。這表示它已經不是 demo 級玩具，而是往可實作語音 agent 的骨架去走。

### 3. 把 idea 直接變成 app

這條是現在最值得注意的部分，也是 AI Studio 跟以前最大差異。

1. 進 `Build mode`。
2. 用一句 prompt 描述 app，要不要加生圖、Live API、Google Maps 之類能力。
3. AI Studio 生成整個 app，右側立刻出現 preview，並且同步建立檔案。
4. 在 chat 裡繼續追需求，或直接切到 `Code` 改細節。
5. 需要外部服務時，到 `Settings` 放 secrets，或要求 agent 幫你接 `Firebase`。
6. 成品可匯出 zip、推到 `GitHub`，或直接 `Publish` 到 `Cloud Run`。

<pre class="mermaid">
flowchart LR
    A["想法 / Prompt"] --> B["Build mode"]
    B --> C["React UI + Node.js runtime"]
    C --> D["Preview / Code 反覆修"]
    D --> E["Secrets / Firebase / npm"]
    E --> F["GitHub 匯出或同步"]
    E --> G["Cloud Run 發布"]
</pre>

我覺得這條流程最強的地方，不是「一句話生網站」本身，而是它把後面的工程感補起來了：多檔案管理、preview、checkpoint、server-side runtime、secrets、GitHub、deployment，都開始是一條完整線。

## 它現在為什麼比以前更像 AI 建置平台

`Build mode` 官方已經直接寫到幾個很關鍵的點：

- 預設可生成 `React` 前端。
- 可以帶 `Node.js` server-side runtime。
- Agent 會幫你管理多個檔案與依賴。
- 能自動裝 npm package。
- 可以在 Settings 裡管理 secrets。
- 能直接串 `Firebase`，目前官方明寫的是 `Firestore` 和 `Firebase Authentication`。
- 可以匯出到 `GitHub`，或發到 `Cloud Run`。

這就是為什麼我現在不太會把它只叫 playground。

更準確地說，它像是一個：

- 前段是模型試煉場
- 中段是 prompt-to-app 工作台
- 後段是雲端原型到部署的交接點

## AI Studio、Firebase Studio、Antigravity，三者現在的分工

這一段我覺得一定要拆清楚，不然很容易混在一起。

### Google AI Studio

偏 `browser-first`。

適合：

- 快速試模型
- 快速做語音 / 影像 / 生圖實驗
- 快速從 prompt 生成可跑的 app
- 在瀏覽器裡完成一版 prototype

### Google Antigravity

偏 `code-first`、`agent-first` 的本地 IDE。

適合：

- repo 已經變大
- 需要本地環境、CLI、長時間開發
- 要更深的多檔案改動與驗證
- 想把 AI 當成真正的 implementation partner

### Firebase Studio

它是 preview 階段的實驗場，但官方已經公告 sunset。

現在比較準確的理解方式是：

- `Firebase Studio` 負責驗證過去那種 browser-based full-stack prototyping 的方向
- 這些能力正在回收到 `Google AI Studio`
- 更 code-first、偏本地開發的那一側，則交給 `Antigravity`

也就是說，Google 現在不是把這條路砍掉，而是把它重新分工。

## Firebase 現在接進 AI Studio 到什麼程度

這裡要稍微保守一點寫，因為官方明寫的範圍和大家想像中的「整個 Firebase 全都一鍵接好」還是有差。

目前我有看到官方直接寫出來、而且可以比較放心講的是：

- `Firestore`
- `Firebase Authentication`

官方說可以直接請 agent 幫你「加資料庫」或「設定 Google Sign-in」，它會處理 provisioning、設定與 code generation。

但像你提到的這些方向：

- `Realtime Database`
- `Cloud Messaging / Push`
- `Apps Script`
- 其他更完整的 Firebase / Google Workspace 服務串接

我目前還沒有在 AI Studio 官方頁面看到它們被明講成 Build mode 內建的一鍵整合項目。

所以我會比較保守地寫成：

- `AI Studio` 現在已經開始直接接上 Firebase 的核心後端能力
- 更完整的 Firebase 生態與 Google 服務整合，仍然很可能是後續擴充方向
- 如果需求超出內建整合範圍，現階段比較實際的做法仍然是先生成 app，再透過 GitHub / 本地 IDE / Antigravity 持續開發

這樣比較不會把「官方已提供」和「很合理的未來方向」混成同一件事。

## Key、分享、GitHub、發布，哪些地方最容易搞混

這段很值得記一下，因為 AI Studio 在分享與正式部署時的 key 邏輯不完全一樣。

### 先測：免費 / 付費 tier

Gemini Developer API 官方 pricing 頁面現在把使用分成 `Free`、`Paid`、`Enterprise` 三層。`Paid` 會給更高 rate limits、context caching、batch API，且資料不會被用來改善產品。

所以如果你說「綁自己的 key 或用 Premium key」，比較準確的說法會是：

- 在 AI Studio 內可以升級到付費層，讓 API 配額與能力更適合正式應用
- 如果 app 需要外部 API 或敏感憑證，則放進 `Secrets`
- 真正部署到 AI Studio 外部時，要明確區分哪些 key 在 client、哪些在 server

### 分享 app：不是直接燒你自己的 quota

這點很有意思。Google 在 2025-05-21 的官方 blog 明講，AI Studio 生成的 app 在 AI Studio 內分享時，Gemini API 呼叫會透過一個 placeholder API key 由 AI Studio 代理，所以共用 app 的使用者不會直接吃掉你的 API key 與 quota。

這代表：

- 在 AI Studio 內分享 demo，比較像平台代管
- 但一旦你把 app 帶到 AI Studio 外面正式部署，就不能再用這種心態

### 正式部署：要回到 server-side 與 secrets 的基本功

官方 build mode 文件也提醒得很直接：

- client side 不要直接放真實 API key
- 真正部署到外部環境時，要把吃 key 的邏輯搬到 server side
- 如果部署到 `Cloud Run`，可以走比較正規的 secrets / server runtime 路線

### GitHub：現在已經是很實際的交接點

這條線也越來越像正式 workflow：

- `Build mode` 文件已經把 `Push to GitHub` 寫成標準出口之一
- Firebase Studio migration 文件也直接寫到，在 AI Studio 裡可以登入 GitHub、建立 repository、`Stage and commit all changes`
- 如果你原本是 Firebase Studio 使用者，還可以用 GitHub sync 接 `Firebase App Hosting`

所以現在很合理的工作方式就是：

1. 在 `AI Studio` 先把概念跑出來。
2. 需要更深開發時，推到 `GitHub`。
3. 之後交給本地 IDE 或 `Antigravity` 繼續做長線開發。

## 我自己會怎麼用這條流程

如果把它放進實際工作流，我會這樣用：

1. 先在 `AI Studio` 測 prompt、測 Live API、測模型風格。
2. 確定概念值得做，再進 `Build mode` 生出第一版 `React` app。
3. 如果要登入、資料持久化，就先接 `Firestore` / `Firebase Auth`。
4. 如果 app 還要長線維護，就推到 `GitHub`。
5. 接著再搬到 `Antigravity` 或自己常用的 IDE 裡繼續做。

也就是說，`AI Studio` 很適合當前場的試煉場，而不是最終唯一開發環境。

它最強的地方，是把一個 AI idea 的前 60% 摩擦大幅降低；後面要不要進 repo、進 CI/CD、進完整工程流程，再看 app 進入什麼成熟度。

## 參考資料

- [Google AI Studio](https://ai.google.dev/aistudio)
- [Google AI Studio quickstart](https://ai.google.dev/gemini-api/docs/ai-studio-quickstart)
- [Build apps in Google AI Studio](https://ai.google.dev/gemini-api/docs/aistudio-build-mode)
- [Develop Full-Stack Apps in Google AI Studio](https://ai.google.dev/gemini-api/docs/aistudio-fullstack)
- [Get started with Live API](https://ai.google.dev/gemini-api/docs/live)
- [Gemini Developer API pricing](https://ai.google.dev/gemini-api/docs/pricing)
- [An upgraded dev experience in Google AI Studio](https://developers.googleblog.com/en/google-ai-studio-native-code-generation-agentic-tools-upgrade/)
- [Firebase Studio sunset and project migration](https://firebase.google.com/docs/studio/migrating-project)
