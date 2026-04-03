---
title: Android 開發知識庫
desc: 把這個 Markdown 倉庫整理成可公開瀏覽的 GitHub Pages 文件站，集中展示 Android 工具、專案觀察、除錯與工作流筆記。
---

{% assign category_count = site.data.navigation.library | size %}
{% assign published_pages = site.pages | where_exp: "entry", "entry.path != 'index.md'" | where_exp: "entry", "entry.path != 'README.md'" | where_exp: "entry", "entry.path != '404.md'" | where_exp: "entry", "entry.path contains '.md'" %}
{% assign featured_paths = "tools/scrcpy.md|tools/maestro.md|tools/simvyn.md|projects/google-ai-edge-gallery.md" | split: "|" %}

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
