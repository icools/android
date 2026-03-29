---
title: UML、Mermaid 與 Diagram Tools：我會怎麼選
tags:
  - design
  - uml
  - mermaid
  - plantuml
  - c4
  - diagram
desc: 整理 UML、Mermaid 與其他圖表工具的差異、適合情境與我會怎麼選擇。
author: icools
date: 2026-03-29
source: local
status: active
---

# UML、Mermaid 與 Diagram Tools：我會怎麼選

當系統開始變複雜之後，只靠嘴巴講，很快就會進入一種大家都覺得自己懂、但其實每個人腦中圖都不一樣的狀態。

所以圖表工具很重要。

但圖表工具也不是越正式越好，而是要看你現在到底想表達什麼。

## UML 比較像什麼

`UML` 比較像一套經典的圖表語言。

它適合描述：

- class 關係
- sequence 互動
- state 轉換
- use case
- activity flow

如果你今天想把系統互動或 class 關係講得比較嚴謹，UML 還是很有價值。

## Mermaid 比較像什麼

`Mermaid` 對我來說最大的優勢，是它很適合 Markdown 與 GitHub 型知識庫。

因為它的圖是用文字寫的，所以有幾個很好用的特點：

- 可以直接版本控制
- 可以跟文章一起放
- 可以用 diff 看修改
- 很適合快速畫 flowchart、sequence diagram、state diagram

如果今天我要在知識庫、文件或 PR 裡快速補一張圖，我通常會優先想到 Mermaid。

## PlantUML 比較像什麼

如果你想要更正式、更完整地表達 UML，`PlantUML` 會是很值得考慮的選項。

它比 Mermaid 更偏正式工程圖表，對 UML 表達也比較完整。

## C4 類工具適合什麼

如果你的重點不是 class 細節，而是整個系統邊界、容器、元件與互動，我會更偏向 `C4` 思維。

這種圖比較適合回答：

- 系統和外部世界怎麼互動
- 有哪些 container
- module 或 component 怎麼切
- 哪些責任不該混在一起

## 我自己會怎麼選

### 想快速畫、直接放 Markdown

我會選 `Mermaid`。

### 想正式畫 UML

我會選 `PlantUML`。

### 想講高階架構

我會用 `C4` 思維來表達。

### 想快速手繪討論

我會考慮 `Excalidraw` 這種比較輕量、討論感強的工具。

## 我覺得很值得補進知識庫的相關工具

- `Mermaid`：最適合和 Markdown 筆記整合
- `PlantUML`：正式 UML 表達很強
- `Excalidraw`：快速畫草圖超舒服
- `Structurizr` / `C4`：適合架構層級說明
- `D2`：偏現代感、宣告式圖表語言

---

## Kotlin + UML：在 IDEA 中視覺化程式碼架構

一直在尋找能夠將程式碼在開發中進行視覺化、或者流程化的工具，之前也找了很多相關的 UML 工具。

### JetBrains 上支援 Kotlin 的 UML 工具

幾個值得關注的選項：

- **SequenceDiagram** — IDEA 上的 sequence diagram 生成外掛，可以從 Kotlin/Java 程式碼自動產出序列圖，適用於理解呼叫流程。  
  參考：[一個有趣的 LLM 開發工具視覺化助手 IDEA](https://jefflin1982.medium.com/%E4%B8%80%E5%80%8B%E6%9C%89%E8%B6%A3%E7%9A%84llm-%E9%96%8B%E7%99%BC%E5%B7%A5%E5%85%B7%E8%A6%96%E8%A6%BA%E5%8C%96%E5%8A%A9%E6%89%8Bidea-0dcd824c396f)

- **Gerador UML Mermaid** — JetBrains IDE 外掛，可以從 Kotlin/Java class 自動產出 Mermaid 語法的 UML 圖表，直接整合到 Markdown 文件中。  
  連結：[JetBrains Plugin - Gerador UML Mermaid](https://plugins.jetbrains.com/plugin/28262-gerador-uml-mermaid-)

- **JetBrains 付費版 IDE 內建 UML Tools** — IntelliJ IDEA Ultimate 內建的 UML class diagram 功能（依賴 Diagrams plugin），最大優勢是**與程式碼即時同步**，不需要手動維護，缺點是需要付費版 IDE。  
  連結：[UML class diagrams - IntelliJ IDEA](https://www.jetbrains.com/help/idea/class-diagram.html)

### 現有工具的痛點

目前這些工具都有一個共同問題：**與程式碼無法即時同步**。

花了很多時間拉出圖表，當未來程式碼有任何異動，還是需要花時間重新整理，久了就無法持續下去。JetBrains 官方的 UML 工具雖然能和程式碼同步，可惜只有付費版 IDE 才有。

> 工程師的想法：「既然要改，那就改天吧。」  
> 最後就會無疾而終。

### 與 LLM（AI）的整合

現在 UML 描述語言主要有 `PlantUML` 與 `Mermaid` 等，搭配 LLM 可以快速產出圖表：

- **Claude Artifacts** — 可以即時 preview 由 LLM 產生的 HTML 或 Mermaid 語法，直接視覺化看到結果，立刻進行修正或調整。
- **Gemini Canvas** — 同樣支援互動式產圖，適合快速迭代。
- **GitHub Copilot Chat** — 在 IDE sidebar 中選擇檔案，讓 AI 針對特定 class 產出圖表。

只要將 Kotlin class 定義丟幾個給 LLM，加上一些提示，就能產生 class diagram。例如用 Mermaid 的 classDiagram 語法：

<pre class="mermaid">
classDiagram
    class ParkExperienceSettings {
        -experienceType: ExperienceType
        +currentMode: String
        +currentAttractionType: String
        +currentPriorityPass: String
        +DEFAULT_SETTINGS: ParkExperienceSettings$
    }
    class ExperienceType {
        &lt;&lt;enumeration&gt;&gt;
        EXPERIENCE_TYPE_A
        EXPERIENCE_TYPE_B
        EXPERIENCE_TYPE_C
        EXPERIENCE_TYPE_SPECIAL
        +value: Int
        +parse(value: Int): ExperienceType$
    }
    class ExperienceOption {
        -experienceIdx: ExperienceType
        +attractionValue: AttractionType
        +experienceMethod: Int
        +isPriorityPass: Boolean
        +attractionProperties: AttractionProperties?
        +getExperienceIdx(): ExperienceType
        +isUsingFastTrack: Boolean
    }
    class AttractionProperties {
        +id: String?
        +attractionName: String?
        +attractionData: AttractionData
        +capacityLimit: Int?
        +heightRequirement: Int?
        +familyFriendlyOnly: Boolean
        +experienceMethod: ExperienceMethod
    }
    class AttractionData {
        +nameId: Int
        +mapKey: String
        +attractionTypeValue: AttractionType
        +attraction: Attraction
    }
    class AttractionType {
        &lt;&lt;enumeration&gt;&gt;
        ATTRACTION_ROLLERCOASTER
        ATTRACTION_WATERRIDE
        ATTRACTION_FAMILYRIDE
        ATTRACTION_THRILL_RIDE
        +value: Int
        +fromInt(value: Int): AttractionType$
    }
    class ExperienceMethod {
        &lt;&lt;enumeration&gt;&gt;
        EXP_METHOD_STANDARD
        EXP_METHOD_EXPRESS
        EXP_METHOD_NO_THRILL
        +value: Int
        +fromInt(value: Int): ExperienceMethod$
    }
    class Attraction {
        &lt;&lt;interface&gt;&gt;
    }
    ParkExperienceSettings --> ExperienceType
    ExperienceOption --> ExperienceType
    ExperienceOption --> AttractionType
    ExperienceOption --> AttractionProperties
    AttractionProperties --> AttractionData
    AttractionProperties --> ExperienceMethod
    AttractionData --> AttractionType
    AttractionData --> "1" Attraction
</pre>

對於一次性的架構分析算是很方便，但如果經常要回頭看架構跟分析，每次都必須手動操作就非常麻煩。

### IDEA：AI 動態產生 Mermaid（構想中）

一個還沒有時間實作的構想：**LLM + JetBrains IDE Plugin + Mermaid Preview 介面**。

有時候我們不需要看到所有的流程與物件，僅需要關注某些重點 class。參考 GitHub Copilot Chat 在 sidebar 讓你選擇針對詢問的檔案的介面，初步構想的流程：

1. **選擇想要繪製的 class** — 在 IDE 中手動挑選要視覺化的檔案
2. **AI 建議其他相關 class** — 透過 LLM 分析 import / dependency，建議應該一起加入的 class
3. **Local LLM 隱私處理** — 透過 Local LLM 進行敏感資料去除或提醒（例如包含公司機密資訊或不想被看到的內容）
4. **Cloud LLM 產生 Mermaid** — 經過 Local LLM 處理後的 class 定義，丟到 Cloud LLM + Prompt 產生 Mermaid 語法
5. **Plugin 內即時 Preview** — 在 Plugin 中直接用 HTML + mermaid.js 顯示結果

這樣預期可以快速分析或更新目前程式碼的架構與流程，搭配不同的 UML diagram 類型，甚至可以在上面加上註解或標示，對於 debug 或流程追溯都會非常方便。

<pre class="mermaid">
flowchart LR
    A[選擇 Class 檔案] --> B[AI 建議相關 Class]
    B --> C[Local LLM 隱私過濾]
    C --> D[Cloud LLM 產生 Mermaid]
    D --> E[Plugin Preview 顯示]
    E -->|調整| A
</pre>

### Mermaid classDiagram 語法參考

Mermaid 的 classDiagram 支援完整的 UML class 表達，包含屬性、方法、可見度、關聯、繼承、interface 與 enum 標記等。

語法參考：[Mermaid classDiagram 語法](https://mermaid.ai/open-source/syntax/classDiagram.html)

延伸閱讀：[Using Mermaid for UML Diagrams in GitHub and Notion](https://levelup.gitconnected.com/using-mermaid-for-uml-diagrams-in-github-and-notion-734e52f64c9b)

### 相關工具參考

- [10 Best UML Diagram Software](https://clickup.com/blog/uml-diagram-software/)

---

## 我的結論

如果是我的知識庫，我會把 `Mermaid` 當成第一優先，因為它最適合直接和 Markdown 一起長期維護。

但如果之後想把架構表達得更完整，`PlantUML`、`C4` 與其他 diagram tools 也很值得補進來。
