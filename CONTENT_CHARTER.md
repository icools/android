---
title: Android 筆記庫內容憲章
tags:
  - android
  - charter
  - content
  - writing
  - codex
  - copilot
desc: 定義此 Android 筆記專案的內容撰寫、分類、命名、索引更新與格式規範，作為未來新增文章時的最高準則。
author: icools
date: 2026-03-29
source: local
status: active
---

# Android 筆記庫內容憲章

這份文件是這個專案的內容規範與維護準則。

之後不管是我自己手動新增內容，還是透過 Codex、GitHub Copilot agent 或其他 AI 工具協助撰寫，都應該以這份文件作為主要依據。

## 這份文件應該叫什麼

我不建議叫 `SDD.md`。

原因是 `SDD` 通常會被理解成：

- Software Design Document
- System Design Document

但這份文件描述的不是 app 架構設計，而是這個知識庫專案的內容規範、編輯方式、分類邏輯與維護流程。

因此我更推薦以下命名：

- `CONTENT_CHARTER.md`
- `KNOWLEDGE_BASE_CHARTER.md`
- `EDITORIAL_GUIDE.md`

其中我最推薦使用：

- `CONTENT_CHARTER.md`

因為它夠直白，也很符合這份文件「內容憲章」的用途。

## 這份文件的角色

這份文件的用途是明確定義：

- 這個專案要放什麼內容
- 內容應該放在哪裡
- 每篇文章應該長什麼樣子
- 根目錄索引與資料夾內容如何對應
- 新增文章時要同步更新哪些檔案
- AI 協助撰寫時應遵守哪些規則

## 專案定位

這個專案是一個以 Markdown 為主的 Android 開發知識庫。

內容會聚焦在：

- Android 工具使用心得
- 開發流程與工作流
- 除錯與問題定位
- 效能分析與優化觀察
- Kotlin 語法與實務寫法
- 值得研究的 Android app / 開源專案
- 尚未完全整理完成的待消化內容

這個專案不是一般 app code repository，而是一個持續累積的知識倉庫。

## 根目錄結構原則

根目錄分成兩層：

- 分類索引頁
- 實際內容資料夾

目前採用以下結構：

```text
.
├─ README.md
├─ CONTENT_CHARTER.md
├─ todo.md
├─ kotlin.md
├─ tools.md
├─ projects.md
├─ debugging.md
├─ performance.md
├─ workflow.md
├─ todo/
├─ kotlin/
├─ tools/
├─ projects/
├─ debugging/
├─ performance/
└─ workflow/
```

如果某個分類需要放圖片，則在該分類資料夾底下建立 `image/` 子資料夾，例如：

- `kotlin/image/`
- `tools/image/`
- `projects/image/`

## README 的角色

`README.md` 只做以下事情：

- 說明這個專案是什麼
- 提供分類入口
- 簡短說明每個分類在放什麼

`README.md` 不負責承載大量細節內容。

如果某個分類下有多篇文章，應該把整理與條列放到對應的根目錄索引頁，而不是一直往 `README.md` 疊內容。

## 根目錄索引頁規則

每個主要分類都必須有一個根目錄同名索引頁：

- `todo.md`
- `kotlin.md`
- `tools.md`
- `projects.md`
- `debugging.md`
- `performance.md`
- `workflow.md`

這些檔案的用途是：

- 作為該分類的總入口
- 條列該分類下有哪些文章
- 為每篇文章提供一句到兩句的簡短介紹
- 補充該分類後續想新增的主題

## 資料夾內容規則

實際文章應放在對應資料夾中，例如：

- 工具介紹放在 `tools/`
- Kotlin 筆記放在 `kotlin/`
- 專案介紹放在 `projects/`
- 除錯主題放在 `debugging/`
- 效能主題放在 `performance/`
- 工作流主題放在 `workflow/`
- 暫存待整理內容放在 `todo/`

原則上不要把具體內容直接寫進根目錄索引頁，除非只是非常簡短的索引型摘要。

如果文章需要圖片，圖片也應跟著文章分類一起收納在對應資料夾底下的 `image/`，不要直接散落在根目錄。

## 每篇文章都應使用 frontmatter

所有正式文章，以及根目錄索引頁，都應使用類似 Obsidian 的 YAML frontmatter。

目前標準欄位如下：

```yaml
---
title: 標題
tags:
  - android
  - ...
desc: 一句描述這篇內容的摘要
author: icools
date: YYYY-MM-DD
source: local 或來源網址
status: active
---
```

### 欄位規範

- `title`
  文章標題，清楚描述內容主題。

- `tags`
  用於搜尋、分類與後續整理。

- `desc`
  一句摘要，應可快速理解這篇內容在寫什麼。

- `author`
  預設使用 `icools`。

- `date`
  使用 `YYYY-MM-DD` 格式。

- `source`
  如果是自行整理內容可填 `local`。
  如果內容明確來自官方文件、GitHub repo 或其他主要來源，應填來源網址。

- `status`
  預設使用 `active`。
  若未整理完成，也可以視情況使用 `draft`。

## 命名規則

檔名以英文、kebab-case 為主。

例如：

- `scrcpy.md`
- `jadx-ui.md`
- `google-ai-edge-gallery.md`
- `profile-gpu-rendering.md`

避免：

- 中文檔名
- 空白檔名
- 過度簡寫到看不出內容

## 圖片資產規則

如果文章需要圖片，應遵守以下規則：

- 圖片放在對應分類資料夾底下的 `image/` 子資料夾，例如 `tools/image/`、`kotlin/image/`、`projects/image/`
- 如果我附上的是 PNG 圖片，加入專案前一律先轉成 JPG
- 圖片檔名使用英文、kebab-case，且要直接反映內容主題
- 不使用像 `image1.png`、`final-final.png`、`screenshot-2.png` 這種沒有意義的命名
- 同一篇文章如果有多張圖片，檔名應維持同一主題前綴，方便搜尋與整理
- 文章引用圖片時，應優先使用對應分類自己的 `image/` 路徑

例如：

- `kotlin/image/typealias-illustrated-guide-home.jpg`
- `kotlin/image/typealias-null-safety-chapter.jpg`
- `tools/image/maestro-flow-example.jpg`

不需要為每個分類預先建立空的 `image/`，但只要該分類開始使用圖片，就應照這個規則收納。

## 內容撰寫原則

這個知識庫偏向「實戰型筆記」，不是純文件翻譯，也不是只貼功能清單。

每篇文章應盡量回答下面幾件事：

- 這是什麼
- 為什麼值得記
- 我為什麼推薦或不推薦
- 它適合解決什麼問題
- 我通常會在什麼情境下使用
- 有沒有什麼限制、代價或注意事項

## 工具介紹文章建議格式

工具型文章優先採用以下結構：

- 工具簡介
- 我為什麼推薦
- 主要用途
- 我覺得特別適合的情境
- 補充筆記
- GitHub 來源
- 來源說明

若有必要，可以增加：

- 和其他工具的比較
- 我實際怎麼用
- 我踩過的坑

## 專案介紹文章建議格式

專案型文章優先採用以下結構：

- 專案簡介
- 我覺得它值得看的原因
- 這個專案在做什麼
- 我會怎麼看這個專案
- 適合拿來學什麼
- 我的簡短評價
- GitHub 來源
- 來源說明

## 索引頁撰寫原則

像 `tools.md`、`projects.md` 這種索引頁，應該以條列整理為主。

格式建議如下：

```md
- [scrcpy](tools/scrcpy.md) - 一個可以用來控制 Android 裝置的實用工具，適合做畫面 mirror、真機操作與日常測試。
```

原則：

- 每項先放文章連結
- 後面接一句簡短介紹
- 不在索引頁展開完整長文
- 長文寫回對應資料夾中的文章

## 新增內容時的必要動作

未來新增任何正式文章時，必須同步完成以下事情：

1. 決定正確分類。
2. 建立對應資料夾內的新文章。
3. 補齊 frontmatter。
4. 如果文章有附圖，建立或使用該分類底下的 `image/`，並把圖片整理成有意義檔名的 JPG。
5. 依文章類型套用適合的內容結構。
6. 更新對應的根目錄索引頁。
7. 如果有需要，再視情況微調 `README.md` 的一句式介紹。

## 自動分類規則

新增文章時，應優先依內容性質自動分類：

- 工具介紹類 -> `tools/` 與 `tools.md`
- Android app / 開源專案介紹 -> `projects/` 與 `projects.md`
- Kotlin 語法 / 實務類 -> `kotlin/` 與 `kotlin.md`
- 除錯排查類 -> `debugging/` 與 `debugging.md`
- 效能觀察類 -> `performance/` 與 `performance.md`
- 工作流 / checklist / 流程類 -> `workflow/` 與 `workflow.md`
- 尚未消化完成、暫存類 -> `todo/` 與 `todo.md`

若內容同時符合多種類型，優先以「主要閱讀目的」決定分類，而不是什麼都拆成很多份。

## 來源與引用原則

如果文章提到工具、專案、文件或官方說明，應盡量附上主要來源。

優先順序：

1. 官方 GitHub repo
2. 官方文件
3. 官方 wiki / release notes
4. 其他補充來源

如果文章是個人觀察與心得，也可以寫出：

- 我的理解
- 我的評價
- 我目前的用法

但如果有明確提到功能、定位或官方描述，仍應盡量附上來源。

## AI 協作規則

未來透過 Codex、GitHub Copilot agent 或其他 AI 工具協助新增內容時，應遵守以下原則：

- 優先依照本文件決定文章放置位置
- 自動補上 frontmatter
- 自動更新對應的根目錄索引頁
- 如果我有附上 PNG 圖片，先轉成 JPG，再放進正確分類底下的 `image/`
- 不要把所有內容都塞進 `README.md`
- 工具與專案文章盡量附上官方來源
- 保持這個知識庫的語氣偏向個人實戰筆記，而不是制式百科整理

## 推薦的 agent 指令說法

之後如果要請 AI 幫你新增內容，可以直接這樣說：

```text
請依照 CONTENT_CHARTER.md，增加 ${某某某} 介紹，自動分類，自動更新對應的索引與內容。
```

或是：

```text
請參考 CONTENT_CHARTER.md，新增一篇關於 ${某某某} 的工具介紹，放到正確分類，補上 frontmatter，並更新對應索引頁。
```

再精準一點也可以說：

```text
請依照 CONTENT_CHARTER.md 的規範，把 ${某某某} 加入知識庫，包含：
1. 建立正式文章
2. 自動分類到正確資料夾
3. 更新根目錄索引頁
4. 補上來源與 frontmatter
```

## 這份文件的優先權

當未來新增內容時，若出現以下衝突：

- `README.md` 與索引頁寫法不一致
- 新文章不知道該放哪裡
- frontmatter 欄位格式不一致
- agent 不知道是否該更新索引

都應以 `CONTENT_CHARTER.md` 為最高準則。

## 結論

這份文件的目的，是讓這個專案長期維持一致的內容結構，而不會越寫越亂。

未來新增文章時，應以：

- 正確分類
- 固定格式
- 自動更新索引
- 保留個人觀察與實戰脈絡

作為主要原則。
