---
title: 不要當 code monkey：我在 GDG Taipei 聽完 Antigravity 與 Spec-Driven Development 的心得
tags:
  - workflow
  - community
  - android
  - gdg
  - meetup
  - ai
  - antigravity
  - sdd
  - spec-driven-development
desc: 整理我在 GDG Taipei 聽完 Antigravity 與 SDD 之後的心得，重點放在 Spec-driven、Plan-driven、Verify-driven 的 AI 工作流。
author: icools
date: 2026-03-29
source: https://docs.google.com/document/d/16QTBks3xpf8IqvAMgBVejySRVL4hjQ7iZ4A1Evkv_Dc/edit?tab=t.0
status: active
---

# 不要當 code monkey：我在 GDG Taipei 聽完 Antigravity 與 Spec-Driven Development 的心得

今天去聽了一場跟 AI Coding、Antigravity、Spec-Driven Development 有關的分享。整場最有價值的地方，不是工具炫技，而是講者把 `SDD` 放進真實工程流程裡，示範怎麼先定義 spec、再讓 AI 提 plan，最後用驗收與測試把結果收斂回來。

我後來最大的收穫，是開始把 `Antigravity` 理解成一個圍繞 spec 運作的工程代理，而不是單純幫你生 code 的工具。真正重要的不是 prompt 多華麗，而是你能不能把需求、限制、驗收條件與任務切分寫清楚。

## 一、Antigravity 的核心不是生 code，而是 Plan → Act → Verify

現場有一張投影片我印象很深，上面用很直白的三個字描述 `Antigravity` 的流程：`Plan`、`Act`、`Verify`，而且中間還有一句話：`with your feedback in every step`。

這句話很重要，因為它代表 `Antigravity` 不是那種你講一句，它就一路埋頭做到完的黑盒子，而是一個可以讓你持續介入、持續修正、持續驗收的工作循環。

這樣的設計其實很像真正的工程流程。先根據 spec 做規劃，再開始實作，最後透過驗收或測試確認結果是否正確。如果結果不對，就再回去修 plan、修 spec、修實作。這整個過程不是一次性，而是反覆逼近你真正想要的結果。

## 二、AI Coding 的重點，已經不是多寫幾行 code，而是先把 spec 定義清楚

以前我們談寫程式，通常就是想功能、寫 code、修 bug、上線。現在的流程正在慢慢變成另一種形式：

`Spec → Plan → Task → Execute → Verify → Iterate`

也就是說，真正重要的事情，已經不是你親手打一行一行 code，而是你能不能把規格講清楚。你不能只跟 AI 說「幫我做一個相簿」，你要講清楚版面怎麼排、要不要 `Light / Dark mode`、點圖之後有沒有 `lightbox`、能不能左右切換、是否支援無限循環、色碼是什麼、最後要怎麼驗收。

更有意思的是，講者提到 AI 不只是執行者，有時候還會反過來幫你 revise spec。比方說你只說你要做 `Dark / Light mode`，它可能會進一步幫你補出兩套色碼建議，甚至連某些介面細節都會一起整理出來。這讓 AI 不只是施工者，也像是一個會幫你補齊規格的工程夥伴。

## 三、我覺得 SDD 特別適合的幾種使用情境

我覺得 `SDD` 第一個很適合的場景，是需求還在收斂中的新功能。這時候最怕的不是沒有 code，而是需求還沒講清楚就先做。先寫 spec，再讓 AI 幫你補 plan、補限制、補驗收條件，會比一句話直接開工穩很多。

第二個很適合的場景，是 UI 細節很多、互動條件很多的功能。像相簿、`lightbox`、`Dark / Light mode`、切換手勢、循環瀏覽這種需求，如果沒有先把行為寫進 spec，AI 很容易只做出一個看起來像、但其實不符合需求的版本。SDD 的價值就在於先把這些細節講清楚，再讓工具往下施工。

第三個很適合的場景，是多人協作、跨 session 交接，或是重構既有功能。因為 spec、plan、acceptance criteria 一旦寫清楚，下一個人接手時不需要重新猜需求，AI 也比較知道哪些行為不能壞掉。這讓 `SDD` 不只是需求文件，而是把實作、驗收、測試和後續維護串起來的工作基線。

## 四、比較好的工作流：先討論 spec，再讓 AI 提 plan

我很喜歡講者分享的一個習慣：不要一開始就叫 AI 直接開做，而是先讓它出 plan。

他的做法大概是這樣：先有自己的想法，再透過 `Gemini CLI` 之類的工具跟 AI 討論，把 high-level spec 先整理出來；接著再把這份 spec 丟給 `Antigravity`，讓它先提出一份 implementation plan。等你 review 覺得 OK 之後，再讓它真正開始實作。

這個差別非常大。因為從工程品質來看，「先做 plan 再執行」會比「一句話直接開幹」穩很多。當 plan 太大、太亂，還可以再要求 AI 補 checklist、補 summary、補限制條件，讓未來的自己或下一個 session 還能接得上。

如果要把這個思路再往前走一步，我現在會把 `Codex` 的 plan mode、`Antigravity` 的 implementation plan / task list、以及 `Android Studio Agent Mode` 這種先規劃再執行的設計，一起放回 `SDD` 與 `spec kit` 的脈絡來看。我另外把這條線整理成一篇獨立心得：[Plan Mode、SpecKit 與 SDD：先把需求問清楚，再讓 AI 開始做](plan-mode-spec-kit-and-sdd.md)。

## 五、驗收標準不是附屬品，而是 AI 的目標本身

這場分享我最有感的一點，是講者把「驗收」放到跟 spec 幾乎同等重要的位置。以前很多人以為 spec 只是寫需求、畫架構圖，但如果今天是要給 AI 寫，那你就必須把 acceptance criteria 講清楚，因為驗收標準本身，就是 AI 的目標。

例如做一個相簿，不是只說做一個 gallery，而是要明確寫出：可以正常開啟、可以瀏覽、有 `lightbox` 效果、可以左右切換、支援無限循環、`Dark / Light mode` 正常、介面符合指定規格。當這些條件清楚之後，AI 才知道什麼叫做做完。

更進一步，講者也提到，現在其實已經可以透過一些自動化 UI 測試去做驗收。也就是說，當你規格夠清楚時，AI 不只幫你做，還可以幫你驗證結果有沒有真的符合需求。這一點對工程落地非常重要，因為它把整件事從「幫我寫」提升成「幫我寫，而且幫我證明你寫對了」。

## 六、MCP 很強，但它真的很燒 token

現場另外一個我印象很深的重點，是對 `MCP` 的看法。`MCP` 當然很強，因為它可以接很多 tool、擴充很多能力，讓 agent 直接操作更多事情。但問題也很明顯：它很燒 token。

如果你一開始就把太多 tool 掛進來，每一次對話的基本成本都會先被拉高。你什麼事情都還沒做，可能就已經先固定送掉幾千甚至上萬 token。所以講者的觀點很務實：`MCP` 不應該預設全部打開，而是應該在真正需要的時候再開。

這個想法我非常認同。很多時候工具越多，不代表越有效率，反而只是讓上下文更肥、更貴、更混亂。我甚至覺得未來可以有一個更小型的 AI 管理員，先幫你判斷這次任務到底需不需要打開 `MCP`、要開哪些 tool、哪些暫時不要碰，這樣整體成本和穩定性都會更合理。

## 七、Flash 模型其實已經很夠寫程式了，Pro 更適合拿來做規劃與架構

現場還有人問一個很實際的問題：現在那些 `Pro` 等級的大模型真的有必要嗎？寫程式是不是一定要開到最強？

講者的回答很務實。他的意思大概是，多數 coding 場景其實不一定需要開到 `Pro`。現在的 `Flash` 類模型，在速度和 coding 能力之間已經取得不錯的平衡，實際上很多寫程式工作都能勝任。也就是說，我們不需要一直被 benchmark 或跑分牽著走，因為真實世界裡更重要的是它能不能穩定把事情做完。

`Pro` 模型不是沒價值，而是比較適合放在更重的規劃、架構設計、深度推理，或更複雜的數理問題上。換句話說，如果你在前期做 planning、要比較兩種架構方案、要處理很重的抽象推理，那 `Pro` 很有用；但當你進入大量撰寫程式碼、實作 task 的階段，很多時候 `Flash` 就已經很夠用了。

這個分工我覺得非常合理：規劃階段用更強的模型，實作階段用更快、更便宜、也夠好用的模型。重點不是每一步都用最頂規，而是在哪個階段用什麼才最划算。

## 八、AI 很像不同班次的工讀生，所以好的對話一定要存下來

講者還講到一個很真實、很有經驗值的現象：不要期待同一個模型每天都像同一個人。你昨天可能覺得某個模型表現超神，今天重新開一個新 chat，它可能就沒那麼穩。原因可能來自 session 不同、context 不同、隨機性不同，或當下理解角度不同。

他用了一個很好懂的比喻：AI 很像客服工讀生。昨天那位很會處理事情、很懂你，今天再打電話去，接的人可能已經不是昨天那一位了。這個比喻真的非常貼切。

所以他的建議也很實用：當你覺得某次對話很順、很有價值的時候，不要只是高興一下就關掉，而是要立刻請它 summarize，把這次的 plan、規則、流程、checklist、constraint 寫下來，最好存成 md 或 spec 文件。這樣下次你換一個新 session 時，至少可以把上一位很能幹的工讀生做事方式交接給下一位，不至於每次都從零開始。

## 九、不要當 code monkey，要當 architect

整場我最喜歡的，還是最後那句話：

> Don’t be a code monkey. Be the Architect. Let Antigravity be the contractor.

如果把它翻成我自己的理解，大概就是：不要再把自己定位成只會照需求打字的人。你真正該做的是定義方向、設計 spec、確認品質、管理流程，而 AI 可以當施工者、執行者、承包商。工程師的價值，正在從單純寫 code，慢慢往更高層次移動。

所以這場演講真正讓我有感的，不是哪一個功能多炫，而是整套思維方式：`Spec-driven`、`Plan-driven`、`Verify-driven`。AI coding 的未來，不是讓模型幫你多寫幾行 code，而是讓你從 code monkey 轉成 architect。

## 結語

如果只用一句話總結今天最大的收穫，我會寫成：AI 開發的本質，不是寫 code，而是建立一套可以被規劃、被驗證、被記錄、被延續的工作方式。

尤其在 `SDD` 這件事上，我覺得最值得記下來的，是它不是多一套文件流程，而是把 spec、plan、acceptance criteria 和驗證串成同一條工作鏈。當這條鏈建立起來之後，`Antigravity` 才真的有機會變成可靠的 contractor，而不是只會埋頭生 code 的工具。

今天我最大的感受是，AI 不是要把工程師變便宜，而是逼著工程師往更高層的角色移動。這不是壞事，反而是一種升級。
