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
journey
    title Android 知識庫閱讀路徑
    section 先快速找答案
      從 Tools 找到可用工具: 5: 讀者
      從 Debugging 排查當前問題: 5: 讀者
      從 Performance 找優化切入點: 4: 讀者
    section 再延伸做研究
      從 Projects 拆解案例: 4: 讀者
      從 AI 追工具與模型更新: 4: 讀者
      從 Workflow 複用工作流: 3: 讀者
    section 回到長線能力建設
      從 Kotlin 打底語言能力: 4: 讀者
      從 Rust 觀察跨語言整合: 3: 讀者
      從 Design 與 Books 建立方法論: 4: 讀者
</pre>

</section>

<section class="section-block">
  <div class="section-heading">
    <p class="panel-label">Positioning</p>
    <h2>各分類在知識庫中的角色定位</h2>
  </div>

<pre class="mermaid">
quadrantChart
    title 分類定位圖
    x-axis 即查即用 --> 長線研究
    y-axis 個別技巧 --> 系統化沉澱
    quadrant-1 立即解題且能延伸
    quadrant-2 值得深讀的主軸
    quadrant-3 快速入口與暫存
    quadrant-4 長線佈局題材
    Tools: [0.18, 0.74]
    Debugging: [0.22, 0.70]
    Performance: [0.38, 0.72]
    Kotlin: [0.42, 0.63]
    Workflow: [0.50, 0.58]
    Projects: [0.62, 0.56]
    Rust: [0.70, 0.60]
    AI: [0.76, 0.76]
    Draft: [0.72, 0.28]
    Todo: [0.88, 0.22]
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
