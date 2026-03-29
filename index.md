---
title: Android 開發知識庫
desc: 把這個 Markdown 倉庫整理成可公開瀏覽的 GitHub Pages 文件站，集中展示 Android 工具、專案觀察、除錯與工作流筆記。
---

{% assign category_count = site.data.navigation.library | size %}
{% assign published_pages = site.pages | where_exp: "entry", "entry.path != 'index.md' and entry.path != 'README.md' and entry.path != '404.md' and entry.path contains '.md'" %}
{% assign featured_paths = "tools/scrcpy.md|tools/maestro.md|tools/simvyn.md|projects/google-ai-edge-gallery.md" | split: "|" %}

<section class="hero-block">
  <p class="eyebrow">GitHub Pages Edition</p>
  <h1>把 Android 開發筆記，整理成一個好逛、好查、可持續更新的公開網站。</h1>
  <p class="hero-copy">
    這個站點保留了原本 Markdown-first 的知識庫寫法，但補上了首頁、分類導覽、文章頁樣式與自動部署流程。
    之後只要在 repo 裡新增或更新筆記，GitHub Pages 就會自動重新發布。
  </p>
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
