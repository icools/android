---
title: Scrcpy Virtual Display：讓 Android 擁有第二虛擬螢幕
tags:
  - tools
  - android
  - scrcpy
  - virtual-display
  - multi-tasking
desc: 介紹 Scrcpy 新增的 Virtual Display 功能，以及如何透過 Android DisplayManager 在程式碼層級將 Activity 投放到第二螢幕上。
author: icools
date: 2026-04-04
source: https://github.com/Genymobile/scrcpy
status: active
---

# Scrcpy Virtual Display：讓 Android 擁有第二虛擬螢幕

Scrcpy 在近期新增了 **Virtual Display (虛擬螢幕)** 功能，讓 Android 設備可以建立出第二個虛擬螢幕，並投影在電腦上顯示。這個功能極具潛力，讓使用者可以更靈活地運用 Android 裝置的多工能力。

例如：
- 你可以在手機上播放 YouTube 投放在電腦上，而手機主螢幕繼續滑動 Facebook 而不會被打斷，達到真正的多工體驗。
- 讓開發者將除錯資訊顯示在第二螢幕，主螢幕則維持 App 正常運行，提高開發效率。
- 讓手機變成行動 Dashboard，顯示不同的數據視覺化畫面（如即時分析結果或監控資料）。
- 作為額外的顯示設備，幫助用戶更方便地瀏覽或管理應用。

> 如果還不知道 `scrcpy` 專案，可以看這邊：[Genymobile/scrcpy (GitHub)](https://github.com/Genymobile/scrcpy)

---

## 啟動 Scrcpy 虛擬螢幕

使用 Scrcpy 創建虛擬螢幕，使得 Android 裝置可以將這塊全新領域鏡像輸出至 PC。

基本建立指令：
```bash
scrcpy --new-display=1920x1080
```

如果希望強制設定 DPI，可以使用 `/DPI` 語法：
```bash
scrcpy --new-display=1920x1080/420
```

### 在虛擬螢幕上啟動特定 App

如果只是開了新螢幕但沒有啟動 App，虛擬螢幕可能會顯示空白。我們可以使用 `--start-app` 參數來自動啟動特定應用程式：
```bash
scrcpy --new-display=1920x1080 --start-app=org.videolan.vlc
```

或者使用第三方 Launcher（例如 Fossify Launcher）來提供一個完整的桌面環境：
```bash
scrcpy --new-display=1920x1080 --no-vd-system-decorations --start-app=org.fossify.home
```

> **重點概念：**
> 透過先在電腦上啟動 Scrcpy，你會先看到一個虛擬桌面顯示在電腦上。這個畫面**不是單純的螢幕鏡像**，而是獨立的第二個手機桌面。因此，你需要先在電腦上透過上述指令啟動這個第二螢幕，接著才能在手機上控制顯示內容。

---

## 程式實作：利用 DisplayManager 偵測並投放到第二螢幕

在 Android 設備上，作為開發者，我們可以透過 `DisplayManager` 來偵測是否有連接第二螢幕，並透過程式自動將應用的 Activity 顯示到該螢幕上。

### 1. 偵測多螢幕並啟動 Activity

首先取得設備所有的 Display，當發現螢幕數量大於 1 時，就可以將我們的 Activity 投射過去：

```kotlin
val displayManager = getSystemService(Context.DISPLAY_SERVICE) as DisplayManager
val displays = displayManager.displays

if (displays.size > 1) {
    // 取得第二螢幕 (Index 1)
    val secondDisplay = displays[1]
    showActivityOnSecondScreen(secondDisplay)
} else {
    // 若沒有第二螢幕，走原本的預設行為
    createVirtualDisplayAndShowActivity()
}
```

### 2. 在第二螢幕顯示 Activity

當拿到第二螢幕物件後，可以透過 `ActivityOptions.setLaunchDisplayId()` 來精準指定這個 Activity 要在哪個 Display ID 上啟動。

```kotlin
private fun showActivityOnSecondScreen(display: Display) {
    val intent = Intent(this, SecondActivity::class.java).apply {
        addFlags(Intent.FLAG_ACTIVITY_NEW_TASK)
    }
    
    // 指定目標螢幕的 ID
    val options = ActivityOptions.makeBasic()
        .setLaunchDisplayId(display.displayId)
        .toBundle()
        
    startActivity(intent, options)
}
```

如此一來，`SecondActivity` 就會乖乖顯示在第二螢幕（即電腦上 Scrcpy 投影出來的視窗），而手機的實體主螢幕仍然可以保持原本的內容與操作。

---

## 應用場景盤點

這種作法讓 Android 設備能夠更靈活地運用實體資源，無論是開發測試還是生產環境，都能發揮更大的作用：

1. **開發者除錯資訊**：利用第二螢幕顯示 Logcat、效能監控儀表板或即時數據，主螢幕看 UI。
2. **資料分析與視覺化**：將即時數據或複雜的 Dashboard 顯示在 PC 螢幕，手機端保持輕便操作。
3. **投影與展示**：對客戶展示特定畫面，而手機端可以保留為簡報的「演講者備忘錄」或操作遙控器。
4. **多工處理優化**：在同一台手機上運行多個應用，例如用電腦大螢幕看教學影片，實體小螢幕做筆記。
5. **內網伺服器搭配 ADB Reverse**：透過 ADB Reverse 讓手機上的 Web Server 供應數據給 PC，再透過 Scrcpy 顯示特定的操作圖形介面。

透過 Scrcpy 與 Android `DisplayManager` 的結合，Android 設備的多工潛能將獲得史詩級的解鎖。