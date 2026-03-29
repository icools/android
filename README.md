# Android 開發工具與心得筆記

這個專案是我自己的 Android 開發筆記倉庫。

我會把平常開發 Android 時用到的工具、除錯流程、效能觀察方式、學到的技巧、踩過的坑，以及一些還沒完全整理好的內容，慢慢累積在這裡。

之後我也會透過 Codex 協助我持續更新、整理、補充與改寫內容，所以這個專案會以「好維護、好查找、可持續擴充」為目標。

## 這個專案會放什麼

- Android 開發流程中的實戰心得
- 常用工具的介紹、使用情境與操作範例
- 除錯、效能分析、測試、自動化相關筆記
- Kotlin 語法與實務寫法整理
- 尚未完全消化、先暫存待整理的內容

## 預計目錄規劃

目前先規劃以下結構，之後可以再依內容量繼續拆分：

```text
.
├─ README.md
├─ todo/
├─ kotlin/
├─ tools/
├─ debugging/
├─ performance/
└─ workflow/
```

### `todo/`

放我目前還沒完全消化，但先值得記下來的內容，例如：

- 還沒整理完成的工具筆記
- 待驗證的技巧或操作流程
- 看文章、影片、文件時先抄下來的重點
- 之後想深入研究的主題

### `kotlin/`

放 Kotlin 語法與實務用法整理，例如：

- 基本語法速查
- null safety
- scope functions
- data class / sealed class / object
- coroutine 基礎與實戰
- flow / state flow / shared flow
- Android 專案中常見 Kotlin 寫法

### `tools/`

放 Android 工具介紹、使用心得與實戰範例。

目前想先整理這些：

- `adb`
- `scrcpy`
- `maestro`
- `jadx-ui`
- `Android Studio Profiler`
- `GPU Profile` / `Profile GPU Rendering`
- `Layout Inspector`
- `uiautomator`
- `logcat`
- `bundletool`
- `apktool`
- `aapt`

### `debugging/`

放除錯相關內容，例如：

- 真機除錯流程
- log 觀察方法
- crash 排查思路
- ANR 問題定位
- WebView 問題排查
- 權限、intent、activity 問題整理

### `performance/`

放效能分析與調校心得，例如：

- UI 卡頓觀察
- GPU rendering 分析
- 啟動速度優化
- 記憶體觀察
- 過度繪製
- RecyclerView 效能注意事項
- Compose / View 系統效能差異觀察

### `workflow/`

放平常開發與測試流程，例如：

- 新專案初始化 checklist
- 開發時常用 adb 指令流程
- 問題重現與錄影紀錄方式
- 自動化測試工具搭配方式
- 發版前自我檢查清單

## 我想優先整理的主題

下面這些很適合作為第一批內容：

### 1. 工具篇

- `adb` 常用指令整理
- `scrcpy` 在日常開發中的用途
- `maestro` 做 UI 自動化與 smoke test 的方式
- `jadx-ui` 用來看 APK / 反編譯程式碼時的心得
- `logcat` 過濾與關鍵字觀察技巧

### 2. 除錯篇

- App 打不開時第一步要看什麼
- Crash、ANR、卡頓的排查差異
- 模擬器與真機除錯的取捨
- 如何留下可回頭追查的 debug 資訊

### 3. 效能篇

- `Profile GPU Rendering` 怎麼看
- 畫面掉幀時可能優先檢查的方向
- Layout 過深、過度繪製、重複刷新 UI 的問題
- Android Studio Profiler 什麼時候該用

### 4. Kotlin 篇

- Kotlin 常用語法備忘錄
- 開發 Android 時常見 extension 寫法
- coroutine 與 lifecycle 搭配注意事項
- flow 與 UI state 管理心得

## 這份筆記的寫法方向

我希望這份筆記不是純理論文件，而是偏向「自己真的用過、踩過、值得回頭查」的實戰整理，因此內容可以盡量包含：

- 這個工具是拿來解決什麼問題
- 我通常在什麼情境下會打開它
- 最常用的指令或操作
- 我曾經踩過的坑
- 和其他工具相比的差異
- 哪些情況適合先用、哪些情況不一定要用

## 之後可以補的內容方向

除了上面的分類，未來也可以逐步加入這些主題：

- Android 測試工具整理
- CI/CD 與自動化流程筆記
- 常見 shell script 片段
- Jetpack Compose 開發心得
- 逆向分析相關基礎工具
- 發版、簽章、APK / AAB 相關知識
- 常見效能問題案例集
- 常見面試題與自己的理解版本

## 與 Codex 的協作方式

之後我可以直接請 Codex 幫我做這些事：

- 把零散筆記整理成一篇結構化 Markdown
- 幫我補上章節、標題與範例
- 幫我把口語內容改寫成可讀性更好的筆記
- 幫我整理工具比較表
- 幫我建立新的主題資料夾與索引頁
- 幫我把 `todo/` 裡的內容重新分類

## 備註

這個專案會以 Markdown 為主，重點是長期累積、持續整理，而不是一次寫完。

先把值得留下來的內容放進來，再慢慢打磨成真正有用的知識庫。
