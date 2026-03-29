# Maestro

## 工具簡介

`Maestro` 是一套用 YAML 描述流程的 UI 自動化與 end-to-end 測試工具，可以拿來測 Android、iOS，甚至 web。

它的特色是上手快、語法可讀性高，而且對實務上常見的 UI 等待與不穩定狀況有不錯的容錯能力，所以很適合拿來寫 smoke test、核心流程測試、回歸測試。

## 我為什麼推薦

我會推薦 `Maestro`，因為它很適合想做 UI 自動化，但又不想一開始就投入太高成本的人。

- YAML flow 可讀性高，維護成本相對低
- 很適合描述登入、下單、搜尋、填表單這種操作流程
- 對 Android 開發者來說，建立第一批自動化測試的門檻不高
- 寫起來比不少傳統 UI 自動化方案更直覺
- 很適合當作 smoke test 或回歸測試工具

## 主要用途

- 建立 App 的 UI 自動化流程
- 驗證核心使用者路徑有沒有壞掉
- 做 smoke test
- 做版本更新後的基本回歸測試
- 搭配 CI 持續執行重要流程檢查

## 我覺得特別適合的情境

- 想快速替 Android App 補一批端到端流程測試
- 測試重點是「功能流程有沒有通」，不是深度單元測試
- 希望測試腳本讓工程師、QA 都容易理解
- 想要減少手動重複點擊驗證的時間

## 補充筆記

- 很適合先從最重要的 3 到 5 條流程開始寫
- 如果專案常改 UI，維護測試腳本時要注意 selector 穩定性
- 它很適合當成團隊早期建立自動化測試文化的切入點

## GitHub 來源

- 官方 repo: [mobile-dev-inc/maestro](https://github.com/mobile-dev-inc/maestro)

## 來源說明

官方 repo README 把 `Maestro` 描述為一個讓 Android、iOS 與 web UI / E2E 測試變得簡單又快速的開源框架，主打人類可讀的 YAML flow、跨平台與自動等待能力。
