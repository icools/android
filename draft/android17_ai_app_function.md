

# 我實測了 Android AppFunctions：官方已經公開，但第三方開發者現在真的能用嗎？

最近看到 Android 推出 **AppFunctions**，我第一個反應是：

這很像 Android 版的 **tool calling**。

如果這條路真的成熟，代表 Android app 可以把自己的能力直接暴露給系統與 AI 助手，讓使用者不用再一步一步操作 UI，而是直接用自然語言完成事情。這對 app 開發者來說，吸引力真的很大。

我也因此實際把 AppFunctions 接進自己的 Android 專案，升級 Gradle、調整建置、加入相關依賴、寫 annotation，最後成功 build 並安裝到手機上。

但真實結果是：**Gemini 幾乎沒有任何反應。**

那個感覺很妙。
不是那種一眼就知道自己寫錯的炸裂感，而是比較像你把整桌菜都備好了，瓦斯爐卻還沒正式開通。你看起來什麼都做了，build 也成功，專案也能跑，但整個最關鍵的「被 AI 真正呼叫」這件事，就是沒有發生。

這篇文章想整理三件事：

1. AppFunctions 現在官方進度到底到哪裡。
2. 我實際安裝、設定、測試後觀察到什麼。
3. 如果你也是 Android 開發者，現在到底該等待、觀望，還是先把架構準備好。

---

## AppFunctions 是什麼？為什麼我會注意它

簡單講，AppFunctions 想做的事情是：

**把 app 的某些能力，正式暴露成可以被系統與 AI agent 探索並執行的 function。**

這個概念一出來，對開發者來說很容易腦補出很多場景：

- 幫我新增一則 note
- 幫我把內容存進某個 app
- 幫我查詢 app 內資料
- 幫我執行某個固定流程

而且它最吸引人的地方在於，這不是單純做一個聊天介面，而是讓 app 的能力本身變成一種 **AI 可調用的入口**。

官方文件目前已經把 AppFunctions 放在 Android AI 能力的脈絡下，定位成讓 app 能把功能提供給系統與 AI 助手使用；Jetpack 版本也已經推出到 `1.0.0-alpha08`。  
官方文件：<https://developer.android.com/ai/appfunctions>  
Jetpack release notes：<https://developer.android.com/jetpack/androidx/releases/appfunctions>

看到這裡，你很難不心動。

---

## 官方已經開始動了，而且不是只停在概念

這件事最值得注意的地方是：

**它不是紙上談兵。**

Google 官方在 Android Developers Blog 已經直接展示了 **Samsung Gallery + Gemini** 的案例。也就是說，使用者可以透過 Gemini 對 Samsung Gallery 下自然語言指令，去找相簿中的內容。這代表 AppFunctions 的方向不是 PPT 技術，而是已經有一部分真正在產品裡落地了。

參考：  
<https://android-developers.googleblog.com/2026/02/the-intelligent-os-making-ai-agents.html>

看到這裡，我原本的期待值是往上拉的。

因為這意味著：

- 官方已經公開 API
- Jetpack 也已經提供開發方式
- showcase 也已經出現
- Gemini 與 app 能力整合這件事，不再只是想像

問題是——

**官方 showcase 能跑，不代表第三方開發者現在自己接上去就能順利跑。**

這中間，有一道很真實的落差。

---

## 我實際做了什麼

這次我不是只看文件，而是真的下去做。

我大概做了這幾件事：

- 升級專案中的 Gradle / 相關建置環境
- 加入 `androidx.appfunctions` 相關依賴
- 加入 `appfunctions-service`
- 配置 compiler / KSP
- 在程式中加上 `@AppFunction`
- build app 並安裝到手機上
- 實際打開 Gemini 測試看看會不會觸發

整個過程中，最讓人產生錯覺的一點是：

**build 是成功的。**

對工程師來說，build 成功常常會讓人很自然地進入下一層期待：

> 好，現在應該可以測效果了吧？

結果不是。

實際上到 Gemini 那一端時，我看到的是一種幾乎沒有反應的狀態。沒有明顯被呼叫，也沒有那種「喔，它真的選到我的 function 了」的感覺。

這時候第一個直覺當然是：

> 是不是我哪裡少設了？
> 是不是權限沒開？
> 是不是裝置版本不對？
> 還是只有某些特定手機才有？

這些懷疑都很合理，而且說真的，現在也不能完全排除裝置差異的影響。

---

## 後來我才發現：這不一定是我自己寫錯

我後來去翻官方說明、GitHub issue、社群討論，才慢慢確定一件事：

**現在的問題，很可能不只是在 app 端。**

其中一個非常關鍵的公開 issue 是：

- GitHub issue #1151  
  <https://github.com/google-gemini/cookbook/issues/1151>

這個 issue 的重點非常有代表性。

發文者描述的情況是：

- AppFunctions 已經被系統成功 index / enable
- `dumpsys app_function` 也看得到
- metadata 也存在
- 但 Gemini 端仍然無法 discover / execute
- log 還會出現：`Unable to resolve AppFunctionMetadata`

這個現象跟我自己的體感非常接近。

也就是說，現在的狀況比較像：

### 平台層已經開始成形
- annotation 可寫
- schema 可產生
- indexing 有機會成功

### 但 Gemini 對第三方 app 的這條整合鏈，還不夠穩
- 不一定能 discover
- 不一定能 execute
- 不一定真的能進到你預期的 function

換句話說，這不是單純「prompt 寫得不夠精準」的問題。
更像是整條整合鏈目前還在長牙。

---

## 為什麼官方案例可以，第三方卻不一定可以？

這是一個很值得想的問題。

如果 Samsung Gallery 可以，為什麼我自己 app 裡加 annotation 卻沒有明顯反應？

我目前的理解是：

### 1. 官方 / OEM 案例走得比較前面
Google 與 OEM 廠商本來就有更早期、更深度的整合條件。這種 showcase 很可能代表的是：

- API 方向已經被驗證
- 某些白名單或優先支援的 app 類型已經先接上
- 體驗已經開始對消費者釋出

### 2. 第三方開發者這條路還在 preview / alpha 階段
Jetpack `appfunctions` 目前還是 alpha。這本身就已經在提醒你：

**這還不是一個 production-ready 的穩定故事。**

### 3. 公開 API 與真正的生態成熟，中間永遠有時間差
這其實很常見。

技術正式公開，不代表：

- 所有裝置都 ready
- 所有 AI caller 都 ready
- 第三方 app 立刻就能穩定接入
- 文件上的路徑等於真實的產品體驗

有時候 API 已經出來，但真正的生態要再過幾個版本才會順起來。

---

## 我現在的結論：不要全押，但也不要完全不碰

如果你問我這東西現在值不值得做，我的答案不是二分法。

不是：

- 「先不要碰」
- 或「現在立刻 all in」

而是：

## 最好的策略是：先整理架構，先準備能力層

因為 AppFunctions 真正有價值的，不是那個 annotation 本身。

真正有價值的是你 app 裡面那些可以被外部呼叫的能力，例如：

- 建立內容
- 查詢資料
- 執行固定流程
- 回傳整理過的資訊

如果這些能力目前都還綁死在某個畫面、某個初始化流程、某個 session、某個 UI 狀態上，那就算未來 Gemini 完全支援了，你也不一定能很優雅地接上去。

所以我現在更傾向這樣想：

### 先做的事情
- 把功能抽成低耦合的 capability layer / use case
- 讓功能盡量不依賴 UI 畫面生命週期
- 讓 app 能力本身可以獨立呼叫

### 暫時不要全押的事情
- 不要把產品時程完全綁死在 Gemini 是否今天能成功調到你的 AppFunctions
- 不要把它當唯一入口
- 不要因為 build 成功就誤以為整條鏈都 ready

這樣的好處是什麼？

很簡單。

今天 AppFunctions 還不穩，你仍然可以透過其他入口提供功能；等未來 Gemini 對第三方 AppFunctions 的支援成熟後，你再把同一套能力掛上去就好。

這樣你不是從零開始，而是已經把基底打好了。

---

## 這次實測後，我真正學到的是什麼

我覺得這次最大的收穫，不是「我成功做出 AppFunctions 整合」。

老實說，現階段還算不上。

但我反而更確定了幾件事：

### 1. 這個方向值得繼續追
因為官方已經公開、showcase 也已經出現，這條路不是假的。

### 2. 第三方開發者現在要有現實感
不要把 preview 當完成版，不要把文件的理想狀態直接等同於每台手機上的真實體驗。

### 3. 架構準備，比賭今天能不能觸發更重要
這件事其實很像很多新平台能力的早期階段：

最值得投資的，不是賭它今天會不會 work；
而是先把自己 app 的能力整理成未來一旦生態成熟，就能立刻接上的狀態。

這樣才不會等到真的支援成熟時，才發現自己內部結構還是一坨。

---

## 結語

我目前對 Android AppFunctions 的看法是：

**方向很對，未來感很強，但第三方實戰還有點玄。**

官方已經把路開出來了，也已經有實際 showcase；但如果你是一般第三方 app 開發者，現在自己接上去之後，遇到「build 成功、裝好了、Gemini 卻沒反應」這種情況，真的不用太快懷疑人生。

你可能不是唯一一個。
而且問題也未必全都在你身上。

我的結論很簡單：

**可以現在就研究，但先不要全押。**

先把能力層整理好，先把 app 準備成可被外部呼叫的樣子。等 Gemini 與第三方 AppFunctions 的整合真的成熟時，你就能第一時間接上，而不是那時才開始重構。

這種投資，我覺得是值得的。

---

## 參考資料

- Android Developers - AppFunctions  
  <https://developer.android.com/ai/appfunctions>
- AndroidX AppFunctions release notes  
  <https://developer.android.com/jetpack/androidx/releases/appfunctions>
- Android Developers Blog - The Intelligent OS: Making AI agents more helpful for users  
  <https://android-developers.googleblog.com/2026/02/the-intelligent-os-making-ai-agents.html>
- GitHub issue #1151  
  <https://github.com/google-gemini/cookbook/issues/1151>
