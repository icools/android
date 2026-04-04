---
title: AI Markdown Workflow
tags:
  - ai
  - markdown
  - workflow
  - charter
  - obsidian
  - github-pages
desc: 提供給多種 AI 工具快速讀取的 Markdown 整理短版規格，定義指定範圍處理、隱私停止、frontmatter、分類、圖片與索引更新流程。
author: icools
date: 2026-04-04
source: local
status: active
---

# AI Markdown Workflow

這份文件是給 `Codex`、`GitHub Copilot`、`Gemini CLI`、`ChatGPT work with`、`Antigravity` 等工具快速讀取的短版規格。

完整準則以 [`CONTENT_CHARTER.md`](CONTENT_CHARTER.md) 為主；若兩者有衝突，應以 `CONTENT_CHARTER.md` 為準。

## 適用範圍

只在我明確指定的目標上執行：

- 單一新增 Markdown 檔案
- 單一既有 Markdown 檔案
- 指定資料夾

若指定的是資料夾，預設遞迴處理底下所有 `.md`。

除非我明確要求整個 repo 重新查證，否則不要自行對整個專案做全面處理。

## 規則優先序

1. 使用者在當次任務中的明示要求
2. 隱私停止規則
3. `CONTENT_CHARTER.md`
4. `AI_MARKDOWN_WORKFLOW.md`
5. 個別 skill 細節

## 預設互動模式

預設採兩階段：

1. 先檢查並提出整理建議
2. 等我確認後，再進行修改、搬移、補資料與索引更新

若我要直接整理，會另外明講。

## 固定處理順序

1. 確認本次指定的目標範圍。
2. 先做隱私掃描。
3. 若命中隱私規則，立即停止整批流程，只回報風險位置。
4. 檢查或補齊 frontmatter。
5. 從文內擷取主要來源網址，填入 `source`；若沒有明確網址，填 `local`。
6. 根據內容與路徑產生或補齊 `tags`。
7. 若文章原本位於 `draft/`，判斷是否移入最深且已存在的合適分類。
8. 若文章引用本地圖片，搬移、改名、轉檔並更新 Markdown 路徑。
9. 若內容不確定但文內已有網址，可補充可驗證資訊並稍微整理排版。
10. 需要時更新對應索引頁與 `draft.md`。

## Frontmatter 最小欄位

正式文章最少應有：

```yaml
---
title: 標題
tags:
  - ...
desc: 一句摘要
author: icools
date: YYYY-MM-DD
source: local 或主要來源網址
status: active
---
```

補充規則：

- `source` 只保留單一主要來源
- 其他重要參考連結放在正文
- 若文章來自 `draft/` 並已移入正式分類，先保留 `draft` tag，直到我人工確認
- 既有正式文章可補 frontmatter、`source`、`tags`，但不自動加 `draft` tag

## 分類規則

- 只有 `draft/` 來源文章預設允許自動重新分類與搬移
- 既有正式文章預設不重新搬移，除非我明確要求
- 分類時優先放到最深且已存在的合適分類
- 若判斷不明確，優先使用較穩定的既有父層分類，不任意新建過深結構

## 圖片規則

- 只處理 Markdown 內引用的本地圖片
- 圖片移到目標分類底下的 `image/`
- 檔名改成英文 kebab-case
- PNG 一律轉 JPG
- JPG 壓縮率固定 60%
- 更新文章中的相對路徑
- 遠端圖片網址不自動下載、不自動轉檔

## 隱私停止規則

只檢查以下四類：

- 私人 email
- API token
- API key
- 個人電話

若指定範圍中的任一檔案命中上述內容：

- 立即停止整批流程
- 不進行任何修改、搬移、外查或索引更新
- 只回報檔案與可疑位置

## 外查補強規則

只有在內容不確定且文內已有網址時，才進行外查補強。

優先來源：

1. 官方網站
2. 官方文件
3. 官方 GitHub repo
4. 可信技術文章

補充內容限於：

- 工具或主題介紹
- 功能定位
- 必要背景資訊
- 輕量排版整理

不要補無法驗證的推測，也不要重寫成百科文章。
