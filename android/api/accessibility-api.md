---
title: Android Accessibility API 筆記
tags:
  - android
  - api
  - accessibility
  - automation
  - testing
desc: 整理 Android Accessibility API 的能力邊界、UI tree 存取、事件監聽、手勢操作、自動化整合與上架限制。
author: icools
date: 2026-03-29
source: https://developer.android.com/reference/android/accessibilityservice/AccessibilityService
status: active
---

# Android Accessibility API

## 先講一句結論

如果 `Shizuku` 比較像系統權限橋，那 `Accessibility API` 幾乎就是 Android 畫面互動世界裡最強的 UI 自動化橋。

它最厲害的地方在於，你不只是「看到畫面」，而是有機會：

- 讀到目前畫面的 UI tree
- 拿到節點上的文字、描述、view id
- 監聽畫面切換、點擊、文字改變、捲動等事件
- 主動對節點做 click、long click、scroll、set text 等操作
- 在必要時直接發手勢
- 依照事件結果，再接上你自己的程式邏輯、Broadcast、Intent 或其他自動化流程

這一層能力非常強，也因此它同時是 Android 平台上最敏感、最容易碰到政策與上架審查的一層。

## 核心能力：讀 UI tree

`AccessibilityService` 最核心的能力之一，是存取目前視窗中的 `AccessibilityNodeInfo` 樹。

官方 API 讓 service 可以透過目前 active window 與 windows 清單取得節點樹，接著做：

- 往下遍歷子節點
- 讀取 `text`、`contentDescription`
- 依文字搜尋節點
- 依 view id 搜尋節點
- 判斷節點是否可點擊、可捲動、可編輯

這代表如果畫面上的元素有正確暴露在 accessibility tree 中，你就不一定要靠 OCR 或純座標點擊，而是可以用比較穩定的節點邏輯來操作。

<pre class="mermaid">
flowchart LR
    A["畫面內容 / App UI"] --> B["AccessibilityEvent"]
    B --> C["AccessibilityService"]
    C --> D["讀取 UI tree"]
    D --> E["分析文字 / view id / 狀態"]
    E --> F["performAction / dispatchGesture"]
    F --> G["送出 Broadcast / Intent / 後續工作流"]
</pre>

## 可以做哪些互動操作

只要節點本身支援對應 action，你可以從 `AccessibilityNodeInfo` 做很多事：

- `ACTION_CLICK`
- `ACTION_LONG_CLICK`
- `ACTION_SCROLL_FORWARD` / `ACTION_SCROLL_BACKWARD`
- `ACTION_SET_TEXT`
- `ACTION_COPY`
- `ACTION_PASTE`

此外，`AccessibilityService` 本身也能做全域操作，例如：

- Back
- Home
- Recents
- Notifications

如果節點 action 不夠，或畫面元素根本沒有暴露成好用的節點，也還可以考慮用 gesture 方式補上，例如點擊、滑動、拖曳等。

所以實務上常見的模式會是：

- 先用 tree 找節點，因為這比較穩
- 找不到再退回 gesture / 座標操作

## 文字取得、字幕擷取、複製貼上

這個 API 很實用的一點，是它不只會告訴你「有個按鈕」，還可能讓你直接拿到畫面上的文字。

常見可用方向包括：

- 擷取目前畫面上的按鈕文字、標題、提示文字
- 監控輸入框文字變化
- 抓取聊天、列表、表單中的可存取文字
- 對可編輯欄位執行自動填入
- 對節點執行 copy / paste 類操作

如果某些字幕或畫面文案本身就存在 accessibility tree 中，那麼拿來做字幕擷取、關鍵字監控、畫面文字提取會很方便。

不過這裡有一個很重要的現實限制：

- 不是所有 App 都會把畫面資訊完整暴露出來
- 遊戲、Canvas、OpenGL、影片畫面、某些自訂 view，常常就沒有好用的 tree

所以 `Accessibility API` 很強，但不是等於「全畫面 OCR 替代品」。它比較像是「先吃 UI tree，tree 沒資料時再想別的方法」。

## 事件監控能力非常強

`AccessibilityService` 可以持續接收事件，像是：

- `TYPE_VIEW_CLICKED`
- `TYPE_VIEW_TEXT_CHANGED`
- `TYPE_WINDOW_STATE_CHANGED`
- `TYPE_WINDOW_CONTENT_CHANGED`
- `TYPE_VIEW_SCROLLED`

這意味著你可以做事件驅動型自動化，而不是一直無腦輪詢。

例如：

- 偵測某個畫面打開了，就開始執行下一步
- 偵測某個按鈕出現了，就點擊
- 偵測文字改變了，就做分析或紀錄
- 偵測頁面滾動完成了，再讀新內容

如果從工程角度看，它很像是一個 UI 事件匯流排。

## 可以做到類似「攔截 / hook」的效果，但要講精確

你提到的「攔截 hook」，我會建議用比較精確的說法來寫：

`Accessibility API` 很適合做的是「UI 層與事件層的監聽、攔截與回應」，但它不是一般講的 native hook 或 method hook。

它能做的比較像：

- 監聽畫面事件
- 觀察節點狀態
- 在特定條件成立時插入自己的自動化邏輯
- 攔截某些鍵盤或互動事件後，改走自己的處理流程

所以如果你是要做：

- 畫面流程接管
- 特定 App 的快捷操作
- 依畫面狀態自動執行一連串步驟

那它非常強。

但如果你是要：

- 直接 hook 對方 App 的內部 method
- 任意改寫別人 process 內部邏輯

那就不是 Accessibility API 的主要能力範圍。

## 進階整合：Broadcast、Intent、捷徑、自動化鏈

真正有趣的不是單用 Accessibility，而是把它接到你自己的 automation pipeline。

例如：

- 偵測到某個畫面事件後，送出自定義 `Broadcast`
- 看到特定條件後，`startActivity` 或送 `Intent`
- 搭配快捷方式 / Quick Settings Tile 觸發一段自動化
- 把 Accessibility 當成前端感知層，再把其他事情交給 service、worker 或其他 automation app

這樣就可以做出很多進階玩法：

- 自動複製貼上
- 表單半自動填寫
- 自動切頁與確認
- 畫面字幕或文字監控
- 自定義捷徑與一鍵流程
- 依畫面狀態推動下一步測試或操作

## 對自動化和測試來說為什麼這麼強

我會把 `Accessibility API` 看成 Android 上非常強的「灰箱 UI 自動化層」。

它介在純黑箱點座標與真正 App 內部 instrumentation 之間。

它的優勢是：

- 不一定需要改目標 App 程式碼
- 有機會拿到比座標點擊更多的語意資訊
- 可以做跨 App 工作流
- 可以做事件驅動，而不是只會 sleep + click

所以拿來做：

- 自動化流程
- 內部 QA helper
- 字幕或畫面文字擷取
- 跨 App 捷徑
- 輔助功能導向的工具

都很有發揮空間。

## 但客製化成本其實不低

這一層很強，代價就是每一套流程幾乎都會有不少客製化。

常見原因包括：

- 每個 App 暴露出來的 tree 品質不一樣
- selector 很容易跟著版本更新而失效
- 有些畫面只能讀文字，不能直接操作
- 有些畫面只能靠 gesture 補
- 不同品牌 ROM 的行為可能略有差異

所以如果真的要做成穩定產品，通常不能只靠一兩個 click script，而是要做：

- 節點選擇策略
- fallback 策略
- timeout / retry
- 版本相容性觀察
- 使用者授權與設定引導

## 上架限制與政策風險

這部分一定要寫清楚。

Google Play 對 Accessibility API 的使用非常敏感，因為它能接觸的畫面與互動資訊很多。官方政策大意是：

- 這個 API 的使用必須和 app 的核心功能有直接關係
- 如果 app 不是明確的 accessibility tool，必須有清楚揭露與使用者同意
- 使用方式必須在 Play listing 中寫清楚
- 不能拿來做未揭露的高風險行為
- 不能把它包裝成會自己規劃、自己決策、自己執行的黑箱代理

所以如果你的目標是公開上架，`Accessibility API` 往往不是「技術做得出來就好」，而是還要同時過：

- disclosure
- consent
- policy review
- 核心功能正當性

這也是為什麼很多很猛的 Accessibility 自動化工具，會比較適合：

- 內部工具
- 側載使用
- 非 Play 發布
- 非商店主路徑的 power-user 工具

## 我會怎麼定位它

如果是我自己的工具箱，我會把 `Accessibility API` 定位成：

- 畫面感知層
- UI 操作層
- 事件觸發層
- 自動化工作流的前端觀察器

然後再依需求和其他能力混用：

- 和 `Shizuku` 混用：一個看畫面，一個做高權限系統操作
- 和 `Tasker` / `MacroDroid` / `Automate` 混用：負責把工作流串起來
- 和測試工具混用：拿來做灰箱驗證、前置準備、狀態觀察

## 之後想再補的內容

這篇先把能力範圍整理出來，之後我還想再寫：

- `AccessibilityService` 實作骨架與權限設定
- 怎麼遍歷 UI tree 與寫 selector 策略
- 怎麼做穩定的 click / scroll / set text
- `Accessibility + Shizuku` 的混用模式
- 字幕擷取與畫面事件監控的實戰筆記
- Play 上架時實際會踩到哪些審查問題

## 官方與參考來源

- Android Developers：[AccessibilityService](https://developer.android.com/reference/android/accessibilityservice/AccessibilityService)
- Android Developers：[AccessibilityNodeInfo](https://developer.android.com/reference/android/view/accessibility/AccessibilityNodeInfo)
- Android Developers：[AccessibilityServiceInfo](https://developer.android.com/reference/android/accessibilityservice/AccessibilityServiceInfo)
- Google Play Policy：[Permissions and APIs that access sensitive information](https://support.google.com/googleplay/android-developer/answer/16558241)
- Android Developers：[Principles for improving app accessibility](https://developer.android.com/guide/topics/ui/accessibility/principles)

## 來源說明

這篇筆記主要依據 Android Developers 的 `AccessibilityService`、`AccessibilityNodeInfo`、`AccessibilityServiceInfo` 文件，以及 Google Play 對 Accessibility API 的政策要求整理而成。文中對字幕擷取、複製貼上、事件驅動自動化與 `Shizuku` 混用的段落，則是我根據 API 能力邊界所做的實務推論與整理。
