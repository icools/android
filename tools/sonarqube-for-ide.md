---
title: SonarQube for IDE 工具介紹
tags:
  - android
  - tool
  - sonarqube
  - sonarlint
  - static-analysis
  - code-quality
  - draft
desc: SonarQube for IDE 能在 Android Studio / IntelliJ 裡即時指出程式碼品質與安全性問題，適合把靜態分析前移到日常開發流程。
author: icools
date: 2026-04-04
source: https://docs.sonarsource.com/sonarqube-for-intellij/connect-your-ide/setup
status: active
---

# SonarQube for IDE

## 工具簡介

`SonarQube for IDE` 就是我現在想到「日常開發時提早抓問題」最直覺會推薦的一類工具。

它的價值不在於取代 code review，而是在你還在 Android Studio 裡寫 code 的時候，就先把一些 bug 風險、可讀性問題和部分安全性提醒標出來。很多本來會等到 CI、PR review，甚至進到 production 才被注意到的東西，可以更早在本地被看到。

這也是為什麼我自己一直把它當成偏「隱形開發夥伴」的存在。

## 我為什麼推薦

我會推薦 `SonarQube for IDE`，是因為它很適合把靜態分析往前拉。

- 邏輯小問題可以更早被看見
- 一些容易忽略的 code smell 會被即時提醒
- 對 Android / Kotlin 專案來說，比只靠肉眼 review 穩很多
- 如果團隊本來就有 SonarQube Server 或 Cloud，connected mode 會更完整

## 主要用途

- 即時靜態程式碼分析
- 提早發現 bug 風險與可讀性問題
- 輔助發現部分安全性問題
- 和團隊既有 SonarQube 流程接軌

## 我覺得特別適合的情境

- Android Studio 日常開發
- Kotlin / Java 混合專案
- 想把品質檢查前移，不想什麼都等 CI
- 團隊已經有 SonarQube 平台，希望 IDE 與伺服器結果更一致

## 補充筆記

- 它最有價值的地方不是「每條規則都要照單全收」，而是讓一些本來很容易被忽略的小問題提早浮上來。
- 如果團隊有自己的 rule set 或 quality gate，connected mode 會更有感。
- 我自己會把它理解成 IDE 內的第一道品質提醒，不是最終裁判。

## 官方來源

- 官方文件: [Connected mode setup for IntelliJ](https://docs.sonarsource.com/sonarqube-for-intellij/connect-your-ide/setup)

## 來源說明

Sonar 官方文件把 `SonarQube for IntelliJ` 定位成 IDE 端的即時程式碼檢查工具，可連接 SonarQube Server 或 Cloud，把自動化 code review 的能力前移到本地開發流程。這也正是我會推薦它的原因。
