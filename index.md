---
title: Android 開發知識庫
desc: 把這個 Markdown 倉庫整理成可公開瀏覽的 GitHub Pages 文件站，集中展示 Android 工具、專案觀察、除錯與工作流筆記。
---

{% assign category_count = site.data.navigation.library | size %}
{% assign published_pages = site.pages | where_exp: "entry", "entry.path != 'index.md'" | where_exp: "entry", "entry.path != 'README.md'" | where_exp: "entry", "entry.path != '404.md'" | where_exp: "entry", "entry.path contains '.md'" %}
{% assign featured_paths = "tools/scrcpy.md|tools/maestro.md|tools/simvyn.md|projects/google-ai-edge-gallery.md" | split: "|" %}

<style>
  body:has(.home-dark) {
    background:
      radial-gradient(circle at top left, rgba(17, 94, 89, 0.24), transparent 28%),
      radial-gradient(circle at top right, rgba(59, 130, 246, 0.16), transparent 24%),
      linear-gradient(180deg, #050d12 0%, #0a131a 45%, #0d1820 100%);
    color: #edf6f8;
  }

  body:has(.home-dark) .layout-grid {
    grid-template-columns: minmax(0, 1fr);
  }

  body:has(.home-dark) .sidebar {
    display: none;
  }

  body:has(.home-dark) .brand-copy {
    color: #9eb4bc;
  }

  body:has(.home-dark) .brand-mark,
  body:has(.home-dark) .top-nav a {
    background: rgba(11, 23, 30, 0.86);
    border-color: rgba(148, 163, 184, 0.16);
    color: #dce9ee;
  }

  body:has(.home-dark) .top-nav a[aria-current="page"],
  body:has(.home-dark) .top-nav a:hover {
    background: rgba(15, 33, 42, 0.98);
    border-color: rgba(94, 234, 212, 0.24);
    color: #ffffff;
  }

  body:has(.home-dark) .doc-card {
    padding: 0;
    background: transparent;
    border: 0;
    box-shadow: none;
  }

  .home-dark {
    color: #edf6f8;
  }

  .home-dark a {
    color: #94f7ea;
  }

  .home-dark .eyebrow,
  .home-dark .panel-label {
    color: #6ee7d8;
  }

  .home-dark .hero-block {
    padding: clamp(28px, 5vw, 48px);
    border-radius: 34px;
    border: 1px solid rgba(148, 163, 184, 0.14);
    background:
      radial-gradient(circle at 85% 15%, rgba(56, 189, 248, 0.18), transparent 22%),
      radial-gradient(circle at 8% 14%, rgba(94, 234, 212, 0.14), transparent 24%),
      linear-gradient(135deg, rgba(8, 19, 26, 0.96), rgba(14, 29, 38, 0.94));
    box-shadow: 0 32px 72px rgba(0, 0, 0, 0.28);
  }

  .home-dark .hero-block h1 {
    color: #f8fcfd;
    max-width: 12ch;
  }

  .home-dark .hero-copy {
    max-width: 860px;
    color: #a5bac2;
  }

  .home-priority-block,
  .home-dark .section-block,
  .home-dark .metric-row {
    margin-top: 32px;
  }

  .home-priority-block,
  .home-dark .section-block {
    padding: 28px;
    border-radius: 30px;
    border: 1px solid rgba(148, 163, 184, 0.12);
    background: linear-gradient(180deg, rgba(10, 23, 31, 0.94), rgba(14, 27, 36, 0.88));
    box-shadow: 0 22px 48px rgba(0, 0, 0, 0.22);
  }

  .home-priority-heading {
    margin-bottom: 18px;
  }

  .home-priority-heading h2,
  .home-dark .section-heading h2 {
    padding: 0;
    border: 0;
    color: #f4fafb;
  }

  .home-section-copy {
    margin: 0;
    max-width: 720px;
    color: #9db1b8;
    line-height: 1.8;
  }

  .home-priority-grid {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 18px;
  }

  .home-priority-card {
    position: relative;
    display: flex;
    flex-direction: column;
    gap: 14px;
    min-height: 300px;
    padding: 26px;
    border-radius: 28px;
    overflow: hidden;
    border: 1px solid rgba(148, 163, 184, 0.16);
    color: #f7fbfc;
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.04);
  }

  .home-priority-card::after {
    content: "";
    position: absolute;
    right: -6%;
    bottom: -18%;
    width: 220px;
    height: 220px;
    border-radius: 999px;
    filter: blur(10px);
    opacity: 0.36;
  }

  .home-priority-card.is-ai {
    background: linear-gradient(160deg, #0e2431 0%, #152f48 58%, #173645 100%);
  }

  .home-priority-card.is-ai::after {
    background: radial-gradient(circle, rgba(56, 189, 248, 0.58), transparent 70%);
  }

  .home-priority-card.is-android {
    background: linear-gradient(160deg, #0c261f 0%, #133427 56%, #1a4734 100%);
  }

  .home-priority-card.is-android::after {
    background: radial-gradient(circle, rgba(74, 222, 128, 0.52), transparent 70%);
  }

  .home-priority-card.is-kotlin {
    background: linear-gradient(160deg, #25163a 0%, #32214d 58%, #47275a 100%);
  }

  .home-priority-card.is-kotlin::after {
    background: radial-gradient(circle, rgba(192, 132, 252, 0.48), transparent 70%);
  }

  .home-priority-kicker {
    position: relative;
    z-index: 1;
    margin: 0;
    font-family: var(--mono);
    font-size: 0.8rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: rgba(231, 255, 251, 0.74);
  }

  .home-priority-card h2 {
    position: relative;
    z-index: 1;
    margin: 0;
    padding: 0;
    border: 0;
    font-size: clamp(2rem, 3vw, 2.8rem);
    line-height: 1;
    letter-spacing: -0.04em;
  }

  .home-priority-card p,
  .home-priority-card li {
    position: relative;
    z-index: 1;
    color: rgba(237, 246, 248, 0.82);
  }

  .home-priority-card p {
    margin: 0;
    line-height: 1.75;
  }

  .home-priority-card ul {
    margin: 0;
    padding-left: 1.15rem;
  }

  .home-priority-card li + li {
    margin-top: 0.42rem;
  }

  .home-priority-cta {
    position: relative;
    z-index: 1;
    margin-top: auto;
    font-weight: 700;
    color: #ffffff;
  }

  .home-dark .metric-card {
    background: linear-gradient(180deg, rgba(13, 27, 36, 0.94), rgba(17, 34, 44, 0.88));
    border: 1px solid rgba(148, 163, 184, 0.12);
  }

  .home-dark .metric-card strong {
    color: #f8fcfd;
  }

  .home-dark .metric-card span,
  .home-dark .feature-card span,
  .home-dark .mini-card span {
    color: #9eb1b9;
  }

  .home-dark .feature-card,
  .home-dark .mini-card {
    background: rgba(11, 23, 30, 0.88);
    border-color: rgba(148, 163, 184, 0.12);
  }

  .home-dark .feature-card:hover,
  .home-dark .mini-card:hover {
    border-color: rgba(94, 234, 212, 0.24);
    box-shadow: 0 16px 30px rgba(0, 0, 0, 0.24);
  }

  .home-dark .feature-card h3,
  .home-dark .mini-card strong {
    color: #f5fbfc;
  }

  .home-dark .prose pre.mermaid {
    display: grid;
    place-items: center;
    overflow: hidden;
    padding: 22px;
    border-radius: 24px;
    border: 1px solid rgba(148, 163, 184, 0.12);
    background: linear-gradient(180deg, #12212a 0%, #162832 100%);
  }

  .home-dark .prose pre.mermaid svg {
    display: block;
    width: 100% !important;
    height: auto !important;
    max-width: 100% !important;
  }

  @media (max-width: 900px) {
    .home-priority-grid {
      grid-template-columns: 1fr;
    }

    .home-priority-card {
      min-height: auto;
    }
  }
</style>

<div class="home-dark">

<section class="hero-block">
  <p class="eyebrow">GitHub Pages Edition</p>
  <h1>把 Android 開發筆記，整理成一個好逛、好查、可持續更新的公開網站。</h1>
  <p class="hero-copy">
    這個站點保留了原本 Markdown-first 的知識庫寫法，但補上了首頁、分類導覽、文章頁樣式與自動部署流程。
    之後只要在 repo 裡新增或更新筆記，GitHub Pages 就會自動重新發布。
  </p>
</section>

<section class="home-priority-block">
  <div class="home-priority-heading">
    <p class="panel-label">Primary Tracks</p>
    <h2>先看這三塊主幹</h2>
    <p class="home-section-copy">
      如果首頁只能先凸顯最重要的入口，我會先把重心放在 AI、Android 與 Kotlin。
      這三塊最能代表這個知識庫目前的主軸，也最適合當閱讀起點。
    </p>
  </div>

  <div class="home-priority-grid">
    <a class="home-priority-card is-ai" href="{{ '/ai/' | relative_url }}">
      <p class="home-priority-kicker">Priority 01</p>
      <h2>AI</h2>
      <p>模型、工具、agent workflow 與內容整理工作台，是目前最活躍也最常擴充的主題區。</p>
      <ul>
        <li>Codex、ChatGPT、Connect / Apps 生態</li>
        <li>Gemini、Antigravity、Android Studio Agent</li>
        <li>LM Studio、Ollama、Hugging Face</li>
      </ul>
      <span class="home-priority-cta">進入 AI 分類</span>
    </a>

    <a class="home-priority-card is-android" href="{{ '/android/' | relative_url }}">
      <p class="home-priority-kicker">Priority 02</p>
      <h2>Android</h2>
      <p>平台 API、架構分層、背景工作與自動化整合，是整個站點最核心的技術底盤。</p>
      <ul>
        <li>Accessibility、Hilt、Worker、MVVM</li>
        <li>Shizuku、自動化與系統級能力</li>
        <li>從 API 到實務工作流的整理</li>
      </ul>
      <span class="home-priority-cta">進入 Android 分類</span>
    </a>

    <a class="home-priority-card is-kotlin" href="{{ '/kotlin/' | relative_url }}">
      <p class="home-priority-kicker">Priority 03</p>
      <h2>Kotlin</h2>
      <p>語言能力、Flow / Result / sealed class 與錯誤處理，是連接 Android 架構與實作的基礎層。</p>
      <ul>
        <li>Learning Resources 與 Playground</li>
        <li>Error handling strategy 與樣本整理</li>
        <li>從語法回到專案內的實務用法</li>
      </ul>
      <span class="home-priority-cta">進入 Kotlin 分類</span>
    </a>
  </div>
</section>

<section class="section-block">
  <div class="section-heading">
    <p class="panel-label">Overview</p>
    <h2>知識庫全覽</h2>
  </div>

<pre class="mermaid">
mindmap
  root((Android Knowledge Base))
    快速解題
      Tools
        scrcpy
        Maestro
        JADX
        Simvyn
      Debugging
        Crash / Logcat
        ANR / WebView
      Performance
        Startup
        UI Rendering
        GPU / Memory
    AI 生態
      OpenAI
        Codex CLI
        ChatGPT
          Code Interpreter
          Canvas
          Connectors / Apps
          Desktop
          語音輸入
      Google
        Gemini CLI
        Gemini Chat
          Canvas / 簡報
          Deep Research
          Veo 3.1
          Nano Banana
          Gem
        Android Studio Skills
        Google Colab
      Mistral
      LLM / 模型
        Gemma
        Qwen
      AI Tools
        GitHub Copilot
        Hugging Face
        LM Studio
        Ollama
      Content Engineering
    語言與架構
      Android
        API
          Accessibility
          Hilt
          Worker / WorkManager
          MVVM
      Kotlin
        Learning Resources
        Kotlin Playground
      Rust
        Rust for Android
      Design
        BDD / TDD
        DDD
        UML / Mermaid
          Kotlin + UML
          LLM 整合產圖
          Gerador UML Plugin
      Books
        Clean Code
        Clean Architecture
    案例與流程
      Projects
        Google AI Edge Gallery
      Workflow
        GDG Taipei
      Draft
      Todo
</pre>

</section>

<section class="section-block">
  <div class="section-heading">
    <p class="panel-label">Structure</p>
    <h2>內容如何從筆記變成公開站點</h2>
  </div>

<pre class="mermaid">
flowchart LR
    A["原始 Markdown 筆記"] --> B["依主題歸類"]
    B --> C["分類索引頁"]
    B --> D["深度文章頁"]
    B --> E["首頁 Featured 入口"]
    C --> F["GitHub Pages 建置"]
    D --> F
    E --> F
    F --> G["公開閱讀 / 可搜尋"]
    G --> H["新觀察與補充"]
    H --> A
</pre>

</section>

<section class="section-block">
  <div class="section-heading">
    <p class="panel-label">Reader Flow</p>
    <h2>不同讀者會怎麼逛這個知識庫</h2>
  </div>

<pre class="mermaid">
flowchart LR
    Start(["讀者進站"])

    subgraph Quick["先快速找答案"]
      direction TB
      T["Tools<br/>可用工具"]
      D["Debugging<br/>排查問題"]
      P["Performance<br/>優化切入點"]
    end

    subgraph Explore["再延伸做研究"]
      direction TB
      PR["Projects<br/>拆解案例"]
      AI["AI<br/>工具與模型"]
      W["Workflow<br/>複用流程"]
    end

    subgraph Build["回到長線能力建設"]
      direction TB
      K["Kotlin<br/>語言能力"]
      R["Rust<br/>跨語言整合"]
      DS["Design / Books<br/>方法論"]
    end

    Start --> Quick --> Explore --> Build

    classDef hub fill:#0f766e,stroke:#99f6e4,color:#f8fafc,stroke-width:2px;
    classDef card fill:#f7f5ef,stroke:#94a3b8,color:#10212a,stroke-width:1.2px;
    class Start hub;
    class T,D,P,PR,AI,W,K,R,DS card;
    style Quick fill:#17303a,stroke:#5eead4,color:#f8fafc,stroke-width:1.4px
    style Explore fill:#17303a,stroke:#5eead4,color:#f8fafc,stroke-width:1.4px
    style Build fill:#17303a,stroke:#5eead4,color:#f8fafc,stroke-width:1.4px
</pre>

</section>

<section class="section-block">
  <div class="section-heading">
    <p class="panel-label">Positioning</p>
    <h2>各分類在知識庫中的角色定位</h2>
  </div>

<pre class="mermaid">
flowchart TB
    KB((知識庫角色))
    KB --> Solve["快速解題入口"]
    KB --> Research["延伸研究主軸"]
    KB --> Build["長線能力沉澱"]
    KB --> Staging["暫存與觀察區"]

    Solve --> T["Tools"]
    Solve --> D["Debugging"]
    Solve --> P["Performance"]

    Research --> A["AI"]
    Research --> PR["Projects"]
    Research --> AN["Android"]

    Build --> K["Kotlin"]
    Build --> R["Rust"]
    Build --> DS["Design / Books"]
    Build --> W["Workflow"]

    Staging --> DR["Draft"]
    Staging --> TD["Todo"]

    classDef core fill:#0f766e,stroke:#99f6e4,color:#f8fafc,stroke-width:2px;
    classDef group fill:#17303a,stroke:#cbd5e1,color:#f8fafc,stroke-width:1.5px;
    classDef leaf fill:#f7f5ef,stroke:#94a3b8,color:#10212a,stroke-width:1.2px;
    class KB core;
    class Solve,Research,Build,Staging group;
    class T,D,P,A,PR,AN,K,R,DS,W,DR,TD leaf;
</pre>

</section>

<section class="metric-row" aria-label="Site metrics">
  <div class="metric-card">
    <strong>{{ category_count }}</strong>
    <span>主要分類</span>
  </div>
  <div class="metric-card">
    <strong>{{ published_pages | size }}</strong>
    <span>目前頁面數</span>
  </div>
  <div class="metric-card">
    <strong>{{ site.time | date: "%Y-%m-%d" }}</strong>
    <span>站點建置日期</span>
  </div>
</section>

<section class="section-block">
  <div class="section-heading">
    <p class="panel-label">Browse</p>
    <h2>從分類開始</h2>
  </div>
  <div class="card-grid">
    {% for item in site.data.navigation.library %}
      <a class="feature-card" href="{{ item.url | relative_url }}">
        <span class="kicker">{{ item.kicker }}</span>
        <h3>{{ item.title }}</h3>
        <span>{{ item.description }}</span>
      </a>
    {% endfor %}
  </div>
</section>

<section class="section-block">
  <div class="section-heading">
    <p class="panel-label">Featured</p>
    <h2>先看這幾篇</h2>
  </div>
  <div class="card-grid">
    {% for path in featured_paths %}
      {% assign note = site.pages | where: "path", path | first %}
      {% if note %}
        <a class="feature-card" href="{{ note.url | relative_url }}">
          <span class="kicker">{{ note.tags | first | default: "note" }}</span>
          <h3>{{ note.title }}</h3>
          <span>{{ note.desc }}</span>
        </a>
      {% endif %}
    {% endfor %}
  </div>
</section>

<section class="section-block">
  <div class="section-heading">
    <p class="panel-label">Editorial</p>
    <h2>這個站怎麼維護</h2>
  </div>
  <div class="card-grid compact">
    <a class="mini-card" href="{{ '/CONTENT_CHARTER/' | relative_url }}">
      <strong>內容憲章</strong>
      <span>定義 frontmatter、命名、索引與文章撰寫原則，未來擴充內容時可直接遵循。</span>
    </a>
    <a class="mini-card" href="{{ '/tools/' | relative_url }}">
      <strong>工具索引</strong>
      <span>目前內容最完整的分類之一，適合拿來當這個知識庫的閱讀入口。</span>
    </a>
    <a class="mini-card" href="https://github.com/icools/android">
      <strong>原始 Repo</strong>
      <span>如果想直接看 Markdown 原文、版本差異或 issue/commit 歷史，可以回到 GitHub 倉庫。</span>
    </a>
  </div>
</section>

</div>
