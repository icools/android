---
title: Gemma 4 Restaurant Roulette: Medium Article Style
tags:
  - ai
  - google
  - gemma
  - medium
  - project
desc: 以 Medium 文章風格撰寫的 Gemma 4 餐廳轉盤專案總結，涵蓋架構設計、實作經驗與未來展望。
author: icools
date: 2026-04-04
source: local
status: active
---

# 我研究了 Google AI Edge Gallery 的 Restaurant Roulette Skill，發現它其實是在 Android 上實作一個「LLM + JS Runtime + WebView」的小型 Agent 框架

當我最近在看 Google 推出的 Gemma 4 與 Google AI Edge Gallery 時，本來只是想理解一下：所謂的 Agent Skills，到底是不是只是換個名字的 function calling？ 結果往下挖之後，我發現事情沒有那麼簡單。

我這次特別去看的是 Google AI Edge Gallery 裡的一個官方 skill 範例：**Restaurant Roulette**。它表面上看起來只是「幫你找附近餐廳，再做成輪盤」，但實際上它示範的，是一種非常值得注意的設計模式： **LLM 不只是吐文字，而是先理解意圖、再交給 JS runtime 執行、最後透過 WebView 呈現成一個可互動的小型應用。**

---

## 為什麼這個範例值得研究？

因為它不是單純在展示「模型能回答餐廳推薦」而已。 它真正展示的是：

1. 怎麼定義一個 skill 給模型使用
2. 怎麼讓模型輸出結構化參數
3. 怎麼在 Android 這種不適合直接跑 Python 或 Bash 工具的環境裡，建立一個可執行 skill 的 runtime
4. 怎麼把 skill 的結果做成互動式 UI，而不只是聊天文字

這種設計一旦成立，餐廳輪盤其實只是其中一個 demo。 未來你完全可以把同一個模式複製去做：

- 旅遊建議卡片
- 附近停車場選擇器
- 股票摘要面板
- POI 比較工具
- 互動式圖表
- 小型 agent app

這才是這個範例真正迷人的地方 😎

---

# 先說結論：它不是只有 function calling，而是更像一個小型 Agent App Runtime

如果我要用一句話描述這個 skill，我會這樣說：

> Restaurant Roulette 不是只有「模型呼叫工具」而已，而是在 Google AI Edge Gallery 裡，透過 `SKILL.md + JavaScript runtime + WebView UI`，把 Gemma 4 的 tool use 能力延伸成一個可串接外部 API、可生成互動介面的微型 agent 應用。

這個觀念很重要，因為它代表 skill 的最終輸出不一定是：

- 一段文字
- 一個 JSON
- 一個函式回傳值

它也可以是：

- 一個網頁卡片
- 一個輪盤
- 一個圖表
- 一個可點擊的互動頁面

---

# 這個 skill 的架構，可以拆成 3 層

## 1. `SKILL.md`：給模型看的規格書

官方 `restaurant-roulette` skill 在 `SKILL.md` 裡，先定義了這個 skill 的描述、範例、需要的 secret、要呼叫的工具，以及禁止事項。

它的核心功能像是在告訴模型：

- 這個 skill 是做什麼的
- 哪些使用者說法代表應該觸發它
- 需要哪些輸入欄位
- 要呼叫哪個工具
- 不要做哪些事

這表示 `SKILL.md` 在這套架構裡，其實就是一種：

- 給 LLM 看的工具說明書
- 自然語言版的 function schema
- 一個 skill contract

也就是說，它不只是「描述功能」而已，它其實在規範模型的行為：

- 什麼情況應該觸發這個 skill
- 觸發後要傳什麼資料
- 不能做什麼
- 最後應該怎麼回應使用者

這點很像我們平常在做 function calling 時，會提供 schema、description、examples、constraints；只是這裡的形式更貼近 prompt 與工具規格的混合體。

---

## 2. `index.js`：真正執行 skill 邏輯的 runtime

這個範例的核心入口，是你看到的這段：

```js
window['ai_edge_gallery_get_result'] = async (dataStr, secret) => {
  ...
}
```

這個函式做的事情其實非常經典，而且很值得學。

### 第一步：吃進模型整理好的 JSON

它先 `JSON.parse(dataStr)`，取得像這樣的欄位：

- `location`
- `cuisine`

也就是說，模型前面的工作並不是直接查餐廳，而是先把使用者自然語言轉成結構化參數。

例如使用者說：

> 台北幫我找壽司，隨機選一間

模型實際上更像是在做：

```json
{
  "location": "Taipei",
  "cuisine": "Sushi"
}
```

這個步驟超重要，因為它代表 skill 的前半段，其實是 **自然語言理解 → 結構化資料**。

### 第二步：用 secret 去呼叫外部 API

這個 demo 最有趣的地方，就是它不是純本地邏輯。

從程式可以看出來，`secret` 會被當成 Gemini API key 使用，然後 JS 直接透過 `fetch()` 打到 Gemini 的 API。 也就是說，這個 skill 的資料來源不是預先寫死的假資料，而是可以利用使用者填入的 key，即時取得真實結果。

也因此，這個 skill 的角色分工很清楚：

- Gemma / 小模型：判斷是否該觸發 skill，並整理出參數
- JS runtime：負責實際執行、呼叫 API、整理結果
- WebView UI：把結果顯示成互動式體驗

換句話說，這是一種 **本地 agent orchestration + 外部服務擴充** 的做法。 它不是傳統意義上的「全部離線」，而是混合型設計。

### 第三步：強制 Gemini 回傳 JSON，而不是廢話

範例裡透過 `generationConfig` 指定：

- `responseMimeType: 'application/json'`
- `responseSchema: { type: 'ARRAY', items: { type: 'STRING' } }`

這件事的意義是： 它不是在「期待模型乖乖照格式輸出」，而是直接把輸出限制成機器可解析的形式。

這種做法對 agent 來說非常重要。 因為只要 skill 的輸出還要餵給下一層流程，或還要轉給 UI 或後端處理，就不能靠「希望模型聽話」這種玄學。

應該要靠：

- schema
- 約束輸出
- 可驗證格式
- 錯誤處理

這才是能走向產品化的寫法。

---

## 3. `webview.html`：把 skill 變成真正可互動的前端介面

最後，這個 skill 並不是回傳一串餐廳文字而已。 它會把資料組好後，回傳一個帶參數的 `webview` URL，並搭配結果文字，讓使用者點擊 preview card 去轉輪盤。

這個設計的重點在於：

> skill 的最終結果不是純文字，而是 UI。

這就把原本「聊天 + 工具呼叫」的模型能力，推進成「聊天 + 工具呼叫 + 前端互動」的體驗。

在 Android 這種環境裡，這尤其有意思。 因為你未必適合像桌面或伺服器環境那樣，讓 skill 去直接執行 Bash、Python、CLI 工具；但你可以很自然地用：

- WebView
- HTML
- CSS
- JavaScript
- JS bridge / Android bridge

來當作 skill 的執行與呈現空間。

換句話說，Google AI Edge Gallery 在這裡示範的，其實是：

> 把 WebView 當成 Android 上 skill runtime 與互動 UI 的承載層。

---

# 為什麼我覺得這比傳統 function calling 更有趣？

因為傳統 function calling 多半是這樣：

1. 使用者提問
2. 模型決定呼叫函式
3. 後端回傳資料
4. 模型再把資料整理成文字

但這個 skill 的流程更像：

1. 使用者提問
2. 模型理解意圖並整理參數
3. skill runtime 執行資料查詢與處理
4. skill 產生一個互動式介面
5. App 顯示 preview card / webview
6. 使用者直接操作 UI

這種模式等於讓 skill 不只是「工具」，而更像是「一個會被 LLM 呼叫的微型應用」。

所以如果用比較貼切的說法，它不只是：

- reasoning + function calling

而比較像：

- **reasoning + structured input + runtime execution + interactive UI**

這件事一旦想通，你就會發現這套模式其實很能擴充。

---

# 這種 skill 架構可以被複製到哪些場景？

這裡才是真正讓人開始腦補產品方向的地方 😏

## 1. 導航 / 地圖 / POI 相關

這種模式超適合做：

- 隨機挑附近餐廳
- 幫我找附近停車場並做比較卡
- 幫我在目的地附近找評價高的咖啡店
- 幫我做旅遊景點輪盤
- 幫我整理休息站與加油站選項

## 2. 資料視覺化 skill

例如：

- 把每日心情資料轉折線圖
- 把體重變化做成 dashboard
- 把支出分類做成圓餅圖
- 把股票或觀測數據變成迷你儀表板

模型負責理解「我要看什麼」， JS 負責整理資料， WebView 負責把圖畫出來。

## 3. 外部服務整合型 agent

因為 skill 可以有 secret，也可以在 JS 裡串 API，所以它很自然就能變成外部服務接點。

例如：

- Gemini API
- Google Maps / Places
- 你自己的後端
- Firebase
- Supabase
- GitHub
- Notion
- 任何 REST API

也就是說，skill 可以是一個：

> 以 LLM 為入口、以 JS 為執行器、以 WebView 為結果介面的 API adapter。

這就很像一個 agent 插槽。

---

# 這個範例也透露出一件現實：edge agent 不代表所有東西都必須離線

比較合理的理解應該是：

- 意圖理解可以在本地
- 流程編排可以在本地
- 資料來源可以是本地，也可以是雲端
- 互動呈現可以在本地
- 需要即時資訊的部分則可以外接 API

這樣其實比較符合真實應用。

因為很多情境下，使用者真正需要的是：

- 最新資訊
- 即時地點資料
- 可互動 UI
- 低延遲入口

而不是強迫每一步都離線。

---

# 如果把它抽象成一個通用模型，長這樣

我會把這種架構整理成下面這個概念：

## Agent Skill 的通用分層

### A. Skill Definition Layer

負責：

- skill 名稱
- skill 描述
- 範例 utterances
- 輸入結構
- 工具限制
- 執行規則

對應：`SKILL.md`

### B. Runtime Execution Layer

負責：

- parse 參數
- 呼叫 API
- 查詢資料
- 做轉換與錯誤處理
- 準備輸出結果

對應：`index.js`

### C. Presentation Layer

負責：

- 顯示輪盤 / 卡片 / 圖表
- 做互動效果
- 把資料視覺化
- 提供 preview card / webview 入口

對應：`webview.html`

### D. Host App Layer

負責：

- 模型選擇
- skill 管理
- secret 管理
- tool routing
- WebView 載入
- Android UI / lifecycle

對應：Google AI Edge Gallery App 本體

這其實已經是一個很完整的小型 agent framework 雛形了。

---

# 這個設計最值得學的，不是餐廳，而是「邊界」

我覺得這個範例最聰明的地方，不是做了輪盤，而是它很清楚地畫出邊界：

- 模型負責理解與決策
- skill 規格負責約束模型
- JS 負責執行
- WebView 負責互動呈現
- App 負責承載與管理

這些規則看起來像小細節，但其實非常重要。 因為 agent 系統最怕的，往往不是模型不夠聰明，而是**責任邊界模糊**。

當模型開始同時想要：

- 幫你理解需求
- 幫你執行工具
- 幫你自作主張完成最後決策
- 還想取代 UI

那整個體驗就會亂掉。

這個 skill 的寫法某種程度上就是在說：

> 模型可以聰明，但不能越界。

這點我覺得很有工程味，也很值得學。

---

# 如果未來我要把這種模式拿來做自己的 Agent，我會怎麼做？

如果今天要把這套模式複製到自己的產品或 side project，我會這樣規劃：

## 1. 先把每個 skill 定義成獨立規格

每個 skill 至少有：

- 名稱
- 功能描述
- 觸發範例
- 結構化輸入欄位
- 禁止事項
- 執行入口

也就是一份乾淨的 `SKILL.md`。

## 2. JS / Native runtime 不直接綁死資料來源

因為未來 skill 可能要接：

- 本地資料
- 雲端 API
- 公司內部服務
- 其他模型

所以 runtime 最好是可替換的。

## 3. UI 結果盡量用可互動的 card / webview 呈現

因為很多 skill 的價值，根本不在於那一句回答文字， 而在於最後那個：

- 輪盤
- 比較表
- 地圖卡
- 圖表
- 面板

讓 skill 有自己的 UI，體驗會差很多。

## 4. secret 管理不要只停在 demo 等級

Restaurant Roulette 這種 demo 讓使用者自己填 secret 很合理，但如果真的要正式化，我會更偏向：

- 前端只拿 token / session
- 真正 API key 放後端
- skill 透過自己後端 proxy 去查資料

不然只要 skill 可被修改、console 可被觀察，安全面一定要多想一步。

---

# 我從這個範例看到的，不只是 demo，而是 Android 上 Agent Skill 的一種未來形狀

以前講 agent，很多人第一個想到的還是：

- 桌面
- 雲端
- Python
- CLI
- 多工具鏈

但 Google AI Edge Gallery 這個範例讓我覺得另一條路很清楚：

> 在 Android / edge 裝置上，skill 不一定要長得像 shell script，也不一定要直接跑 Python。 它完全可以長成：**LLM 做意圖理解，JS 做執行，WebView 做介面，App 做 orchestration。**

這個方向的好處是：

- 更貼近行動裝置環境
- 容易做互動 UI
- 好整合現有 App
- skill 本身比較像可插拔功能模組
- 更適合做成使用者看得見、點得到、玩得到的 agent 體驗

所以我會說，Restaurant Roulette 雖然只是個小 demo， 但它其實已經偷偷透露出一種很值得關注的思路：

**Agent 不一定只是對話框裡的一次性工具呼叫。它也可以是一個被模型動態喚起的微型應用。**

---

# 結語

我認為這個範例最值得看的，不是它有沒有真的幫你挑中一家好餐廳，而是它把整個 skill 的分工畫得很清楚：

- skill 可以有自己的規格
- skill 可以有自己的 runtime
- skill 可以串接外部 API
- skill 可以輸出自己的 UI
- skill 不只是函式，也可以是一個迷你 app

這不是單純的「模型比較聰明了」。 這更像是：**模型開始有能力去驅動一個可被擴充、可互動、可產品化的執行框架。**

而這件事，我覺得才是真的好玩 🔥

---

## 標題備選

### 版本 A

**我研究了 Google AI Edge Gallery 的 Restaurant Roulette Skill，發現它其實是在 Android 上做一個小型 Agent App Runtime**

### 版本 B

**Gemma 4 不只是 Function Calling：從 Restaurant Roulette 看 Google AI Edge Gallery 的 Agent Skill 架構**

### 版本 C

**Google AI Edge Gallery 的 Agent Skills 怎麼運作？從 Restaurant Roulette 範例看懂 **``** 架構**

---

## 建議副標

**這不只是模型呼叫工具，而是把 LLM、JavaScript runtime 與 WebView 組成一個可互動的微型 agent 應用框架。**

---

## 建議 Tags

`Gemma` `Android` `LLM` `Google AI Edge` `Agent` `WebView`

