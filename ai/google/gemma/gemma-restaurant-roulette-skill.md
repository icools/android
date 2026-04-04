---
title: 用 JavaScript Skill + Gemini API + WebView 做出餐廳轉盤
tags:
  - ai
  - google
  - gemma
  - javascript
  - webview
  - skill
desc: 剖析如何結合 LLM 結構化輸出、JavaScript 邏輯控制與 WebView 互動 UI，開發一個基於 Gemini API 的餐廳轉盤 Skill。
author: icools
date: 2026-04-04
source: local
status: active
---

# 用 JavaScript Skill + Gemini API + WebView 做出餐廳轉盤：這段程式到底在做什麼？

最近在看一段很有意思的 JavaScript skill 程式碼，它的功能不是單純「問 AI 拿一個文字答案」而已，而是把 **AI 產生的資料** 進一步轉成 **可互動的 WebView UI**。

這種做法很有代表性，因為它展示了一種現在在 edge AI / Android skill runtime 上很實用的模式：

> **LLM 負責產生資料，JavaScript 負責控制流程與格式，WebView 負責互動式 UI。**

如果用這個案例來說，它做的事情就是：

- 使用者輸入地點與料理類型
- JavaScript skill 呼叫 Gemini API
- Gemini 回傳餐廳清單
- JavaScript 將結果整理並塞進 WebView URL
- App 顯示一個可點擊的餐廳轉盤 UI

也就是說，它不是只回一段答案，而是回一個 **可以真的拿來互動的頁面結果**。

---

## 這段程式的角色是什麼？

這段程式是掛在 `window` 上的一個函式：

```js
window['ai_edge_gallery_get_result'] = async (dataStr, secret) => {
```

這表示外部系統可以直接呼叫它。以 Android skill runtime 的角度來看，它就像是一個 **固定入口點**。

當 runtime 把參數傳進來後，這個函式就會開始做以下事情：

1. 解析輸入參數
2. 準備 prompt
3. 呼叫 Gemini API
4. 取得餐廳名單
5. 整理成 WebView 可使用的格式
6. 回傳 WebView URL 與文字結果

所以它其實很像一個小型的 **skill adapter**，負責把使用者需求轉換成最終可執行的 UI 結果。

---

## 呼叫流程圖

下面這張圖可以幫助理解整體資料流：

![Restaurant roulette skill flowchart](/assets/images/restaurant-roulette-flow.jpg)

從這張圖可以看到，整個流程並不是單純的：

> 使用者輸入 → AI 回答

而是變成：

> 使用者輸入 → Skill 執行 → AI 取資料 → WebView 組 UI → App 顯示互動結果

這個差異很重要，因為它代表 AI 在這裡已經不只是聊天，而是整個功能鏈的一部分。

---

## 第一段：輸入參數解析

程式一開始會先嘗試把 `dataStr` 當成 JSON 解析：

```js
const jsonData = JSON.parse(dataStr || '{}');
const location = jsonData.location || 'Mountain View, CA';
const cuisine = jsonData.cuisine || 'Sushi';
```

這裡代表它預期外部傳進來的資料長這樣：

```json
{
  "location": "Taipei",
  "cuisine": "Ramen"
}
```

如果沒有提供資料，它也會有預設值，不至於直接掛掉。

這種設計在 skill runtime 裡很常見，因為 skill 不一定每次都能拿到完整輸入，所以通常都會準備 fallback。

---

## 第二段：API Key 的來源

接著它會從 `secret` 拿 Gemini API key：

```js
const GEMINI_API_KEY = secret || 'YOUR_GEMINI_API_KEY';
```

也就是說，這個函式本身並不把 API key 寫死在程式邏輯裡，而是允許外部傳入。

這個概念很像 skill runtime 或 host app 提供執行環境的能力：

- App 端管理 secret
- Skill 端只負責使用它

如果沒有提供 key，就會印出 warning。這樣做至少讓除錯時可以很快知道問題出在哪。

---

## 第三段：它怎麼問 Gemini？

這段 prompt 很簡單：

```js
const prompt = `List 10 real, highly-rated ${cuisine} restaurants in ${location}. Within 15 miles location range`;
```

這裡不是要 Gemini 幫你寫文章，也不是讓它自由發揮，而是只要它產生 **10 間真實且高評價的餐廳名稱**。

但更關鍵的不是 prompt 本身，而是下面的 `generationConfig`。

---

## 第四段：為什麼這段設定很重要？

真正厲害的地方在這裡：

```js
body: JSON.stringify({
  contents: [{parts: [{text: prompt}]}],
  generationConfig: {
    responseMimeType: 'application/json',
    responseSchema: {type: 'ARRAY', items: {type: 'STRING'}}
  }
})
```

這裡等於是在告訴 Gemini：

- 不要回一般聊天文字
- 不要加前言後語
- 不要給 markdown
- 我只要 JSON
- 而且格式必須是「字串陣列」

也就是像這樣：

```json
[
  "Restaurant A",
  "Restaurant B",
  "Restaurant C"
]
```

這一招非常重要。因為只要 skill 的下一步要把資料丟給 UI、圖表、表單、WebView，**結構化輸出** 幾乎都是必要的。

不然模型只要多講一句話，整個 parser 就可能出錯。

所以這段程式不是單純在 call AI，而是在做一件更工程化的事情：

> **把模型輸出約束成固定 contract，讓後面的 runtime 可以穩定消費。**

---

## 第五段：API 回來之後怎麼處理？

取得 Gemini 回應後，它會去抓：

```js
const rawText = data.candidates[0].content.parts[0].text;
places = JSON.parse(rawText);
places.sort(() => 0.5 - Math.random());
```

這裡有三件事：

### 1. 拿出 Gemini 的文字結果
雖然要求的是 JSON，但 Gemini API 回傳外層結構還是會包在 `candidates` 裡面，所以要先把真正的文字內容取出來。

### 2. 再 parse 一次
因為那段 `text` 內容本身是 JSON 字串，所以需要 `JSON.parse(rawText)` 才能變成 JavaScript 陣列。

### 3. 打亂順序
最後把餐廳順序隨機化，這樣轉盤看起來才比較像真的在玩隨機抽選。

---

## 第六段：失敗時怎麼辦？

這段程式的錯誤處理也很有意思：

```js
places = ['Error:', errorMessage, 'Check', 'Console'];
```

也就是說，若 API 失敗，它不是完全停止 UI，而是把錯誤訊息當作轉盤內容的一部分。

這種設計的好處是：

- UI 仍然能顯示
- 使用者知道有錯
- 開發者也知道該去 console 找詳細資訊

這種方式雖然很 demo 味，但在開發 skill 或 prototype 的時候其實滿實用。因為你不用每次看到空白畫面才猜到底哪裡壞掉。

---

## 第七段：為什麼還要 encode？

接著它會把餐廳清單整理成字串後再做 Base64 編碼：

```js
const placeString = places.join('|');
const compressedData = btoa(unescape(encodeURIComponent(placeString)));
```

它名稱叫 `compressedData`，但嚴格來說這不是傳統壓縮，而比較像是：

> **為了安全地放進 URL query parameter 而做的編碼**

例如原本是：

```text
Restaurant A|Restaurant B|Restaurant C
```

經過 Base64 後，就比較適合傳到 `webview.html` 裡。

---

## 第八段：真正的 UI 是誰在做？

接下來它會組出一個本地頁面 URL：

```js
const baseUrl = 'webview.html';
const fullUrl = `${baseUrl}?c=${encodeURIComponent(cuisine)}&l=${encodeURIComponent(location)}&data=${compressedData}&v=${Date.now()}`;
```

這表示真正畫出轉盤、讓使用者點擊旋轉的，並不是 Gemini 本身，而是本地的 `webview.html`。

也就是說：

- Gemini 負責提供資料
- HTML / JS 負責視覺效果與互動

這個拆法其實非常合理。因為模型很擅長生成資料、做推理、產生結構化內容，但互動式 UI 還是應該交給前端頁面去做。

這就是為什麼現在很多所謂的 AI skill、tool、agent UI，最後都還是會回到 WebView / HTML / JS 這種方式來承接互動。

---

## 第九段：最後回傳什麼給 App？

函式最後會回傳：

```js
return JSON.stringify({
  webview: {url: fullUrl},
  result: 'Here is the restaurant roulette wheel you requested! Tap the preview card to spin it and pick a winner!'
});
```

這個設計代表 skill runtime 預期的輸出格式大概是：

- `webview.url`：指定要載入哪個頁面
- `result`：給文字區塊或聊天氣泡使用的說明文字

也就是它同時回了兩種資訊：

1. **機器可讀的 UI 指令**
2. **人類可讀的文字說明**

這種格式很適合 agent skill，因為宿主 App 可以依照欄位決定：

- 要不要顯示 preview card
- 要不要顯示結果描述
- 點下去之後載入哪個 WebView

---

## 這段程式反映了什麼樣的 skill 思維？

如果從架構角度來看，這段程式反映的是一種很典型的設計：

### 1. LLM 不是最終 UI
模型不是最後畫畫面的人，它只提供內容。

### 2. Skill 要有固定輸出格式
你不能完全相信模型自由發揮，該鎖格式的地方還是要鎖。

### 3. WebView 是很實用的 runtime 容器
特別是在 Android 這類環境裡，WebView 幾乎可以當成一種通用 skill sandbox。

### 4. JavaScript 是中間協調層
它負責接參數、呼叫 API、控制格式、處理錯誤、最後組成 UI 可用的資料。

---

## 如果用一句話總結這段程式

這段程式本質上是在做這件事：

> **把使用者的需求轉成 Gemini API 查詢，再把 AI 回傳的資料包裝成一個可在 WebView 執行的互動式餐廳轉盤。**

所以它不是單純「AI 回答問題」，而是更像：

> **AI + skill runtime + WebView UI 的組合應用。**

---

## 為什麼這種做法有意思？

因為它其實透露出一個很重要的方向：

未來很多所謂的 agent skill，不一定只是回一句話，而是：

- 回一張卡片
- 回一個可互動頁面
- 回一個表單
- 回一個圖表
- 回一個 mini tool

而 LLM 的角色，會越來越像是：

- 幫你決定要做什麼
- 幫你生成結構化資料
- 幫你決定要呼叫哪個 skill
- 幫你把結果整理成適合 UI 顯示的內容

這樣整體就不只是 chat，而會慢慢變成真正的 **agentic UI / skill-based app architecture**。

---

## 小結

這段程式碼雖然看起來只是個餐廳轉盤 demo，但背後其實示範了幾個很值得注意的概念：

- Skill 入口函式如何設計
- 如何把輸入參數轉成 prompt
- 如何用 JSON mode / response schema 固定模型輸出
- 如何把 LLM 結果轉成 WebView 可用的資料
- 如何讓 App 顯示互動式 skill UI

所以如果你今天不是要做餐廳轉盤，而是想做：

- QR Code 產生器
- 表單填寫助手
- 行程建議小工具
- 資訊查詢轉卡片
- 裝置端互動 skill

那這個模式其實都很值得參考。

---

## 延伸思考

如果再往下延伸，這類 skill 架構還可以繼續進化：

- 將 skill 定義成 md + js + html 的組合
- 讓模型根據情境自動選擇 skill
- 讓 App 動態載入新的 skill package
- 把 WebView skill 與 Android native API 做更深整合
- 將結果不只回傳 webview，也回傳 structured actions

這樣一來，skill 就不只是 demo，而會慢慢變成可擴展的能力模組。

如果從這個角度看，這段小小的程式其實滿有代表性，因為它已經把 **模型、資料、格式、UI、執行容器** 這幾個層次串起來了。

而這，也正是現在很多 AI app 開始變有趣的地方。

