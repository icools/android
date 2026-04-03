
# Nereid — 專案架構文件  
  
> **Nereid** 是一個 JetBrains IDE 外掛（Plugin），為 Mermaid 圖表語言提供完整的編輯、即時預覽、匯出及診斷功能。  > Plugin ID: `com.nereid.mermaid` | 目標平台: IntelliJ 233+ (IC/PC/WS)    
> 語言: Kotlin 1.9.21 | 構建: IntelliJ Platform Gradle Plugin 2.2.1  
  
---  
  
## 目錄  
  
- [1. 高層架構總覽](#1-高層架構總覽)  
- [2. 套件結構與職責](#2-套件結構與職責)  
- [3. 類別繼承圖](#3-類別繼承圖)  
- [4. 物件關聯圖（組合與依賴）](#4-物件關聯圖組合與依賴)  
- [5. 核心流程圖](#5-核心流程圖)  
  - [5.1 檔案開啟與編輯器初始化流程](#51-檔案開啟與編輯器初始化流程)  
  - [5.2 即時預覽渲染流程](#52-即時預覽渲染流程)  
  - [5.3 匯出流程（PNG / SVG / 剪貼簿）](#53-匯出流程png--svg--剪貼簿)  
  - [5.4 診斷與問題回報流程](#54-診斷與問題回報流程)  
  - [5.5 設定管理流程](#55-設定管理流程)  
  - [5.6 主題切換流程](#56-主題切換流程)  
  - [5.7 Markdown 整合流程](#57-markdown-整合流程)  
  - [5.8 Lexer 語法分析流程](#58-lexer-語法分析流程)  
- [6. 擴充點（Extension Points）](#6-擴充點extension-points)  
- [7. 各套件詳細類別清單](#7-各套件詳細類別清單)  
- [8. 資源檔案結構](#8-資源檔案結構)  
  
---  
  
## 1. 高層架構總覽  
  
```mermaid  
graph TB  
    subgraph IDE["JetBrains IDE Platform"]        EP[Extension Points]        FM[FileEditorManager]        LM[LafManager]        PSI[PSI Framework]    end  
    subgraph Nereid["Nereid Plugin"]        subgraph Language["language 套件"]  
            MFL[MermaidFileType]            ML[MermaidLexer]            MPD[MermaidParserDefinition]            MF[MermaidFile]        end  
        subgraph Editor["editor 套件"]  
            MSH[MermaidSyntaxHighlighter]            MA[MermaidAnnotator]            MCC[MermaidCompletionContributor]            MFB[MermaidFoldingBuilder]            MBM[MermaidBraceMatcher]            MCom[MermaidCommenter]            MSVF[MermaidStructureViewFactory]            MCS[MermaidColorSettingsPage]        end  
        subgraph SplitEditor["spliteditor 套件"]  
            MEP[MermaidEditorProvider]            MSE[MermaidSplitEditor]            MET[MermaidEditorToolbar]            TDA[ThemeDropdownAction]            BDA[BackgroundDropdownAction]        end  
        subgraph Preview["preview 套件"]  
            MPP[MermaidPreviewPanel]            TM[ThemeManager]            DDL[DebouncedDocumentListener]        end  
        subgraph Export["export 套件"]  
            PE[PngExporter]            SE[SvgExporter]            CE[ClipboardExporter]        end  
        subgraph Settings["settings 套件"]  
            MS[MermaidSettings]            MSC[MermaidSettingsConfigurable]            MSD[MermaidSettingsDialog]        end  
        subgraph Diagnostics["diagnostics 套件"]  
            DC[DiagnosticCollector]            DB[DiagnosticBundle]            DD[DiagnosticDialog]            DN[DiagnosticNotifier]            AL[ActionLogger]            GIB[GitHubIssueBuilder]        end  
        subgraph Actions["actions 套件"]  
            EPA[ExportToPngAction]            ESA[ExportToSvgAction]            CPA[CopyAsPngAction]            CDA[CollectDiagnosticsAction]        end  
        subgraph Markdown["markdown 套件"]  
            MLI[MermaidLanguageInjector]            MBP[MermaidBrowserPreviewExtension]            MMRP[MermaidMarkdownResourceProvider]        end    end  
    EP --> MEP    EP --> MSH    EP --> MA    EP --> MCC    FM --> MSE    LM --> TM    PSI --> ML  
    MSE --> MPP    MSE --> MET    MSE --> DDL    MSE --> DN    MPP --> TM    MET --> TDA    MET --> BDA    TM --> MS    MSE --> AL  
```  
  
---  
  
## 2. 套件結構與職責  
  
```mermaid  
graph LR  
    subgraph "com.nereid"        L["language<br/>語言定義、Lexer、Parser、PSI"]  
        E["editor<br/>語法高亮、自動完成、<br/>摺疊、標注、結構視圖"]  
        S["spliteditor<br/>分割編輯器、工具列、<br/>視圖模式切換"]  
        P["preview<br/>JCEF 預覽面板、<br/>主題管理、防抖監聽"]  
        X["export<br/>PNG/SVG/剪貼簿匯出"]  
        ST["settings<br/>持久化設定、<br/>設定 UI 頁面"]  
        D["diagnostics<br/>診斷收集、回報對話框、<br/>GitHub Issue 建構"]  
        A["actions<br/>IDE 選單動作"]  
        M["markdown<br/>Markdown 注入、<br/>預覽擴展"]  
    end  
    L --> E    L --> S    E --> L    S --> P    S --> D    S --> A    P --> ST    D --> ST    M --> L  
```  
  
| 套件 | 職責 | 核心類別數 |  
|------|------|-----------|  
| `language` | Mermaid 語言定義、檔��類型、Lexer、Parser、PSI 節點 | 7 |  
| `editor` | 語法高亮、自動完成、括號配對、註解器、摺疊、結構視圖 | 9 |  
| `spliteditor` | 分割編輯器提供者、編輯器主體、工具列、下拉選單動作 | 5 |  
| `preview` | JCEF 瀏覽器預覽、JavaScript 橋接、主題管理、文件變更防抖 | 3 |  
| `export` | PNG / SVG 匯出工具、剪貼簿複製（legacy，目前主路徑在 PreviewPanel） | 3 |  
| `settings` | 應用級持久化設定、IDE 設定頁面、模態設定對話框 | 3 |  
| `diagnostics` | 環境資訊收集、診斷報告建構、GitHub Issue URL 生成、通知列 | 6 |  
| `actions` | IDE 選單/快捷鍵動作（匯出 PNG/SVG、複製、收集診斷） | 4 |  
| `markdown` | Markdown 語言注入、瀏覽器預覽擴展、資源提供者 | 3 |  
  
---  
  
## 3. 類別繼承圖  
  
### 3.1 Language 套件繼承關係  
  
```mermaid  
classDiagram  
    class Language {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class LanguageFileType {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class IElementType {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class LexerBase {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class ParserDefinition {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class PsiFileBase {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }  
    Language <|-- MermaidLanguage    LanguageFileType <|-- MermaidFileType    IElementType <|-- MermaidElementType    LexerBase <|-- MermaidLexer    ParserDefinition <|.. MermaidParserDefinition    PsiFileBase <|-- MermaidFile  
    MermaidLanguage : +INSTANCE MermaidLanguage    MermaidFileType : +INSTANCE MermaidFileType    MermaidFileType : +getDefaultExtension() String    MermaidLexer : -buffer CharSequence    MermaidLexer : -bracketDepth Int    MermaidLexer : +advance()    MermaidParserDefinition : +createLexer() Lexer    MermaidParserDefinition : +createFile() PsiFile  
```  
  
### 3.2 Editor 套件繼承關係  
  
```mermaid  
classDiagram  
    class SyntaxHighlighterBase {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class SyntaxHighlighterFactory {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class ColorSettingsPage {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class PairedBraceMatcher {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class Commenter {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class FoldingBuilderEx {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class CompletionContributor {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class Annotator {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class PsiStructureViewFactory {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }  
    SyntaxHighlighterBase <|-- MermaidSyntaxHighlighter    SyntaxHighlighterFactory <|-- MermaidSyntaxHighlighterFactory    ColorSettingsPage <|.. MermaidColorSettingsPage    PairedBraceMatcher <|.. MermaidBraceMatcher    Commenter <|.. MermaidCommenter    FoldingBuilderEx <|-- MermaidFoldingBuilder    CompletionContributor <|-- MermaidCompletionContributor    Annotator <|.. MermaidAnnotator    PsiStructureViewFactory <|.. MermaidStructureViewFactory  
    MermaidSyntaxHighlighter : +DIAGRAM_TYPE$ TextAttributesKey    MermaidSyntaxHighlighter : +KEYWORD$ TextAttributesKey    MermaidSyntaxHighlighter : +getHighlightingLexer() Lexer    MermaidSyntaxHighlighter : +getTokenHighlights() Array    MermaidAnnotator : +annotate(PsiElement, AnnotationHolder)    MermaidCompletionContributor : -addDiagramTypeCompletions()    MermaidCompletionContributor : -addDirectionCompletions()    MermaidCompletionContributor : -addArrowCompletions()  
```  
  
### 3.3 SplitEditor 與 Actions 繼承關係  
  
```mermaid  
classDiagram  
    class FileEditorProvider {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class UserDataHolderBase {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class FileEditor {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class AnAction {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class ComboBoxAction {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class DumbAware {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class DialogWrapper {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }  
    FileEditorProvider <|.. MermaidEditorProvider    DumbAware <|.. MermaidEditorProvider    UserDataHolderBase <|-- MermaidSplitEditor    FileEditor <|.. MermaidSplitEditor  
    AnAction <|-- ExportToPngAction    AnAction <|-- ExportToSvgAction    AnAction <|-- CopyAsPngAction    AnAction <|-- CollectDiagnosticsAction    DumbAware <|.. ExportToPngAction    DumbAware <|.. ExportToSvgAction    DumbAware <|.. CopyAsPngAction    DumbAware <|.. CollectDiagnosticsAction  
    ComboBoxAction <|-- ThemeDropdownAction    ComboBoxAction <|-- BackgroundDropdownAction  
    DialogWrapper <|-- DiagnosticDialog    DialogWrapper <|-- MermaidSettingsDialog  
    MermaidSplitEditor : -textEditor TextEditor    MermaidSplitEditor : -previewPanel MermaidPreviewPanel    MermaidSplitEditor : -toolbar MermaidEditorToolbar    MermaidSplitEditor : -diagnosticNotifier DiagnosticNotifier    MermaidSplitEditor : +triggerExportPng()    MermaidSplitEditor : +triggerExportSvg()    MermaidSplitEditor : +triggerCopyPng()    MermaidSplitEditor : +setViewMode(ViewMode)  
```  
  
### 3.4 Settings 與 Diagnostics 繼承關係  
  
```mermaid  
classDiagram  
    class PersistentStateComponent {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class Configurable {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }    class Disposable {        &lt;&lt;IntelliJ_Platform&gt;&gt;    }  
    PersistentStateComponent <|.. MermaidSettings    Configurable <|.. MermaidSettingsConfigurable    Disposable <|.. MermaidPreviewPanel  
    MermaidSettings : +previewUpdateMode : PreviewUpdateMode    MermaidSettings : +themeMode : ThemeMode    MermaidSettings : +mermaidTheme : String    MermaidSettings : +previewBackground : PreviewBackground    MermaidSettings : +defaultExportFormat : ExportFormat    MermaidSettings : +securityMode : SecurityMode    MermaidSettings : +getInstance()$ MermaidSettings  
    MermaidPreviewPanel : -browser : JBCefBrowser    MermaidPreviewPanel : -jsQuery : JBCefJSQuery    MermaidPreviewPanel : -themeManager : ThemeManager    MermaidPreviewPanel : +renderDiagram(String, String?)    MermaidPreviewPanel : +exportAsPng(callback)    MermaidPreviewPanel : +exportAsSvg(callback)  
```  
  
---  
  
## 4. 物件關聯圖（組合與依賴）  
  
### 4.1 核心組件關聯  
  
```mermaid  
classDiagram  
    MermaidEditorProvider --> MermaidSplitEditor : creates    MermaidEditorProvider --> MermaidFileType : checks  
    MermaidSplitEditor *-- MermaidPreviewPanel : owns    MermaidSplitEditor *-- MermaidEditorToolbar : owns    MermaidSplitEditor *-- DebouncedDocumentListener : owns    MermaidSplitEditor *-- DiagnosticNotifier : owns    MermaidSplitEditor o-- TextEditor : wraps    MermaidSplitEditor --> ActionLogger : uses    MermaidSplitEditor --> DiagnosticCollector : creates on error    MermaidSplitEditor --> DiagnosticDialog : creates on error  
    MermaidPreviewPanel *-- JBCefBrowser : owns    MermaidPreviewPanel *-- JBCefJSQuery : owns (x3)    MermaidPreviewPanel *-- ThemeManager : owns    MermaidPreviewPanel --> MermaidSettings : reads via ThemeManager  
    ThemeManager --> MermaidSettings : reads    ThemeManager --> LafManager : subscribes  
    MermaidEditorToolbar *-- ThemeDropdownAction : contains    MermaidEditorToolbar *-- BackgroundDropdownAction : contains    MermaidEditorToolbar --> MermaidSettingsDialog : opens  
    ThemeDropdownAction --> MermaidSettings : writes    BackgroundDropdownAction --> MermaidSettings : writes  
    DiagnosticNotifier --> DiagnosticCollector : creates    DiagnosticNotifier --> DiagnosticDialog : opens    DiagnosticCollector --> MermaidSettings : reads    DiagnosticCollector --> ActionLogger : reads    DiagnosticCollector ..> DiagnosticBundle : produces  
    DiagnosticDialog --> GitHubIssueBuilder : uses    DiagnosticDialog o-- DiagnosticBundle : displays  
    MermaidSyntaxHighlighter --> MermaidLexer : creates    MermaidSyntaxHighlighter --> MermaidTokenTypes : maps  
    MermaidAnnotator --> MermaidFile : inspects  
    ExportToPngAction --> MermaidSplitEditor : finds & calls    ExportToSvgAction --> MermaidSplitEditor : finds & calls    CopyAsPngAction --> MermaidSplitEditor : finds & calls    CollectDiagnosticsAction --> DiagnosticCollector : creates    CollectDiagnosticsAction --> DiagnosticDialog : opens  
```  
  
### 4.2 Markdown 整合關聯  
  
```mermaid  
classDiagram  
    MermaidLanguageInjector --> MermaidLanguage : injects    MermaidLanguageInjector --> MarkdownCodeFenceImpl : targets (reflective)  
    MermaidBrowserPreviewExtension --> MermaidMarkdownResourceProvider : provides resources    MermaidBrowserPreviewExtension --> MarkdownHtmlPanel : extends  
    MermaidBrowserPreviewExtension_Provider --> MermaidBrowserPreviewExtension : creates  
    MermaidMarkdownResourceProvider --> ClasspathResources : serves /mermaid/*  
    class MermaidLanguageInjector {        +getLanguagesToInject()        +elementsToInjectIn()    }  
    class MermaidBrowserPreviewExtension {        +scripts : List~String~        +styles : List~String~        +resourceProvider : ResourceProvider    }  
    class MermaidBrowserPreviewExtension_Provider {        +createBrowserExtension()    }  
    class MermaidMarkdownResourceProvider {        +RESOURCE_PREFIX$        +canProvide(String) Boolean        +loadResource(String) ResourceProvider.Resource    }  
```  
  
---  
  
## 5. 核心流程圖  
  
### 5.1 檔案開啟與編輯器初始化流程  
  
```mermaid  
flowchart TD  
    A["使用者開啟 .mmd / .mermaid 檔案"] --> B["FileEditorManager 尋找 Provider"]    B --> C{"MermaidEditorProvider.accept()"}    C -->|"fileType == MermaidFileType"| D["MermaidEditorProvider.createEditor()"]    C -->|"不匹配"| Z["使用預設編輯器"]  
  
    D --> E["建立 TextEditor（標準文字編輯器）"]  
    E --> F["建立 MermaidSplitEditor(textEditor, project, file)"]  
    F --> G["init 區塊執行"]  
    G --> G1["建立 MermaidPreviewPanel"]    G --> G2["建立 JSplitPane（左: 文字, 右: 預覽）"]  
    G --> G3["建立 MermaidEditorToolbar"]    G --> G4["建立 DiagnosticNotifier 通知列"]  
    G --> G5["組裝 mainPanel（toolbar + notification + splitPane）"]  
  
    G1 --> H["MermaidPreviewPanel.init"]    H --> H1["建立 JBCefBrowser"]    H --> H2["建立 3 個 JBCefJSQuery（render, pngExport, svgExport）"]  
    H --> H3["setupJsBridge() — 註冊 Java ↔ JS 橋接"]  
    H --> H4["setupExportQueries() — 設定匯出回呼"]  
    H --> H5["setupContextMenu() — 自訂右鍵選單"]  
    H --> H6["loadPreviewHtml() — 載入內嵌 HTML + Mermaid.js v11 CDN"]    H --> H7["建立 ThemeManager — 訂閱 LafManagerListener"]  
    G5 --> I["setupDocumentListener()"]    I --> I1["建立 DebouncedDocumentListener(300ms)"]    I1 --> I2["附加到 TextEditor 的 Document"]  
    G5 --> J["setupExportCallbacks()"]    G5 --> K["setupRenderErrorCallback()"]    G5 --> L["updatePreview() — 首次渲染"]  
  
    L --> M{"isLoaded?"}    M -->|"否"| N["儲存為 pendingSource"]    M -->|"是"| O["executeJavaScript: renderDiagram()"]  
  
    style A fill:#e1f5fe    style F fill:#fff3e0    style H fill:#f3e5f5  
```  
  
### 5.2 即時預覽渲染流程  
  
```mermaid  
flowchart TD  
    A["使用者編輯 .mmd 檔案內容"] --> B["Document 變更事件觸發"]  
    B --> C["DebouncedDocumentListener 接收事件"]  
    C --> D["重置 Alarm 計時器（300ms）"]  
    D --> E{"300ms 內是否有新變更？"}  
    E -->|"是"| D  
    E -->|"否（穩定）"| F["觸發 onUpdate callback"]  
    F --> G["MermaidSplitEditor.updatePreview()"]    G --> H["取得 document.text"]    H --> I["MermaidPreviewPanel.renderDiagram(source)"]  
    I --> J["ThemeManager.getMermaidTheme()"]    J --> K{"isLoaded?"}    K -->|"否"| L["儲存 pendingSource + pendingTheme"]    K -->|"是"| M["轉義特殊字元（\\, `, $, \\n）"]  
  
    M --> N["browser.executeJavaScript:<br/>window.renderDiagram(source, theme)"]  
    N --> O["JCEF / Mermaid.js 渲染"]  
    O --> P{"渲染結果"}  
    P -->|"成功"| Q["更新 #diagram innerHTML"]    P -->|"失敗"| R["顯示 #error 錯誤區塊"]  
  
    Q --> S["JS: javaBridge.onRenderSuccess()"]    S --> T["JBCefJSQuery 回呼: 'success:'"]  
    T --> U["觸發 onRenderSuccess callback"]  
    R --> V["JS: javaBridge.onRenderError(msg)"]    V --> W["JBCefJSQuery 回呼: 'error:' + msg"]  
    W --> X["觸發 onRenderError callback"]    X --> Y["MermaidSplitEditor 記錄 lastRenderError"]  
    style A fill:#e8f5e9    style N fill:#fff3e0    style Q fill:#e8f5e9    style R fill:#ffebee  
```  
  
### 5.3 匯出流程（PNG / SVG / 剪貼簿）  
  
```mermaid  
flowchart TD  
    subgraph 觸發來源  
        T1["工具列選單: Tools → Nereid → Export as PNG"]  
        T2["右鍵選單: Export as PNG..."]  
        T3["快捷鍵: Ctrl+Shift+E"]  
        T4["工具列按鈕"]  
    end  
    T1 & T2 & T3 & T4 --> A["ExportToPngAction.actionPerformed()"]    A --> B["getActiveMermaidEditor()"]    B --> C["FileEditorManager.getEditors(file)<br/>.filterIsInstance&lt;MermaidSplitEditor&gt;()"]    C --> D["MermaidSplitEditor.triggerExportPng()"]  
    D --> E["invokeLater: 顯示 FileSaverDescriptor 對話框"]  
    E --> F{"使用者選擇儲存位置？"}  
    F -->|"取消"| Z["結束"]  
    F -->|"確認"| G["previewPanel.exportAsPng(callback)"]  
  
    G --> H["設定 pngExportCallback = callback"]    H --> I["executeJavaScript: window.exportAsPng()"]  
    I --> J["JS: 取得 #diagram SVG"]    J --> K["JS: clone SVG → 設定尺寸"]  
    K --> L["JS: XMLSerializer → Blob → ObjectURL"]    L --> M["JS: Image.onload → Canvas (2x scale)"]    M --> N["JS: canvas.toDataURL('image/png')"]    N --> O["JS: javaBridge.onPngExport(dataUrl)"]  
    O --> P["pngExportQuery handler"]    P --> Q["觸發 pngExportCallback(dataUrl)"]    Q --> R["Base64 decode → imageBytes"]    R --> S["wrapper.file.writeBytes(imageBytes)"]    S --> T["ActionLogger.log('Exported PNG')"]  
    Q --> ERR{"例外？"}  
    ERR -->|"是"| E1["ActionLogger.log('PNG export failed')"]  
    E1 --> E2["DiagnosticNotifier.showError()"]  
    style D fill:#e3f2fd    style I fill:#fff3e0    style S fill:#e8f5e9    style E2 fill:#ffebee  
```  
  
#### SVG 匯出流程  
  
```mermaid  
flowchart LR  
    A["triggerExportSvg()"] --> B["FileSaverDescriptor 對話框"]  
    B --> C["previewPanel.exportAsSvg(callback)"]    C --> D["JS: XMLSerializer.serializeToString(svg)"]    D --> E["javaBridge.onSvgExport(svgData)"]    E --> F["svgExportQuery handler"]    F --> G["callback(svgContent)"]    G --> H["wrapper.file.writeText(svgContent)"]  
```  
  
#### 剪貼簿複製流程  
  
```mermaid  
flowchart LR  
    A["triggerCopyPng()"] --> B["exportAsPng(callback)"]    B --> C["Base64 decode → bytes"]    C --> D["ImageIO.read(ByteArrayInputStream)"]    D --> E["ImageTransferable(BufferedImage)"]    E --> F["SystemClipboard.setContents()"]  
```  
  
### 5.4 診斷與問題回報流程  
  
```mermaid  
flowchart TD  
    subgraph 觸發來源  
        T1["Help 選單 → Collect Mermaid Diagnostic Info"]        T2["通知列 → Report Issue 連結"]  
        T3["預覽面板 → Report this issue 連結"]  
    end  
    T1 --> A1["CollectDiagnosticsAction.actionPerformed()"]    T2 --> A2["DiagnosticNotifier.openDiagnosticDialog()"]    T3 --> A3["MermaidSplitEditor.onReportIssue callback"]  
    A1 & A2 & A3 --> B["建立 DiagnosticCollector"]  
    B --> C["DiagnosticCollector.collect()"]    C --> C1["collectEnvironmentInfo()<br/>IDE 名稱/版本, Plugin 版本,<br/>OS, Java, JCEF 狀態"]  
    C --> C2["collectPluginSettings()<br/>MermaidSettings 快照"]  
    C --> C3["RenderingStatus<br/>lastError, mermaidVersion, console"]    C --> C4["ActionLogger.getRecentActions()<br/>最近 20 筆操作記錄"]  
    C --> C5["collectIdeLogs()<br/>idea.log 中 nereid/mermaid 關鍵字<br/>最後 50 行"]  
  
    C1 & C2 & C3 & C4 & C5 --> D["DiagnosticBundle"]  
    D --> E["DiagnosticDialog(project, bundle)"]    E --> F["顯示可摺疊 Sections<br/>每個 Section 有 CheckBox 控制"]  
  
    F --> G{"使用者操作"}  
    G -->|"Copy to Clipboard"| H["GitHubIssueBuilder.buildReportBody()"]    H --> H1["生成 Markdown 格式報告"]  
    H1 --> H2["複製到系統剪貼簿"]  
  
    G -->|"Open GitHub Issue"| I["GitHubIssueBuilder.buildGitHubUrl()"]    I --> I1{"URL > 8000 字元？"}  
    I1 -->|"是"| I2["truncateSections()"]  
    I2 --> I3["附加截斷提示"]  
    I1 -->|"否"| I4["BrowserUtil.browse(url)"]  
    I3 --> I4  
    G -->|"Close"| J["關閉對話框"]  
  
    style B fill:#e3f2fd    style D fill:#fff3e0    style E fill:#f3e5f5  
```  
  
### 5.5 設定管理流程  
  
```mermaid  
flowchart TD  
    subgraph 設定存取方式  
        A1["IDE Settings → Tools → Nereid<br/>(MermaidSettingsConfigurable)"]        A2["工具列齒輪按鈕<br/>(MermaidSettingsDialog)"]  
        A3["主題下拉選單<br/>(ThemeDropdownAction)"]  
        A4["背景下拉選單<br/>(BackgroundDropdownAction)"]  
    end  
    A1 --> B1["Kotlin UI DSL Panel<br/>直接綁定 MermaidSettings 屬性"]  
    A2 --> B2["Tabbed Dialog<br/>複製到 local vars → OK 時 apply"]    A3 --> B3["直接寫入 MermaidSettings.mermaidTheme<br/>+ themeMode = MERMAID_THEME"]    A4 --> B4["直接寫入 MermaidSettings.previewBackground"]  
    B1 & B2 & B3 & B4 --> C["MermaidSettings"]  
    C --> D["PersistentStateComponent<br/>持久化到 mermaid.xml"]  
    C --> E["各元件讀取設定"]  
    E --> E1["ThemeManager: themeMode, mermaidTheme, background"]    E --> E2["DiagnosticCollector: 設定快照"]  
    E --> E3["MermaidPreviewPanel: via ThemeManager"]  
    B3 & B4 --> F["onSettingsChanged callback"]    F --> G["MermaidPreviewPanel.applySettings()"]    G --> H["ThemeManager.applyCurrentSettings()"]    H --> I["notifyThemeChanged()"]    I --> J["re-render diagram"]  
    style C fill:#fff9c4    style D fill:#e8eaf6  
```  
  
### 5.6 主題切換流程  
  
```mermaid  
flowchart TD  
    subgraph 觸發來源  
        T1["IDE Look & Feel 變更"]  
        T2["ThemeDropdownAction 選擇"]  
        T3["BackgroundDropdownAction 選擇"]  
        T4["Settings 修改後 apply"]    end  
    T1 --> A["LafManagerListener.TOPIC 事件"]  
    A --> B["ThemeManager.notifyThemeChanged()"]  
    T2 --> C["MermaidSettings.mermaidTheme = selected"]    T3 --> D["MermaidSettings.previewBackground = selected"]    C & D --> E["onSettingsChanged()"]    E --> F["MermaidPreviewPanel.applySettings()"]    F --> B  
    T4 --> F  
    B --> G["計算 isDark, mermaidTheme, backgroundColor"]  
    G --> G1["isDarkTheme()"]    G1 --> G1a{"themeMode?"}    G1a -->|"FOLLOW_IDE"| G1b["檢查 LAF 名稱含 dark/darcula<br/>或 UIManager 'ui.dark'"]    G1a -->|"MERMAID_THEME"| G1c["mermaidTheme == 'dark'"]    G1a -->|"CUSTOM"| G1d["return false"]  
    G --> G2["getMermaidTheme()"]    G --> G3["getBackgroundColor()"]  
    G1 & G2 & G3 --> H["onThemeChanged callback"]    H --> I["MermaidPreviewPanel"]    I --> I1["setDarkMode(isDark)<br/>JS: body.classList.add/remove('dark')"]    I --> I2["setBackground(color)<br/>JS: container.style.background = color"]    I --> I3["renderDiagram(source, newTheme)<br/>mermaid.initialize({theme: newTheme})"]  
    style G fill:#fff3e0    style I fill:#e3f2fd  
```  
  
### 5.7 Markdown 整合流程  
  
```mermaid  
flowchart TD  
    subgraph 前置條件  
        P["org.intellij.plugins.markdown 外掛已安裝"]  
        P --> Q["plugin.xml optional depends<br/>載入 markdown-support.xml"]    end  
    subgraph 語法注入  
        A["使用者在 .md 檔案中撰寫<br/>```mermaid<br/>...code...<br/>```"]  
        A --> B["PSI 解析產生 MarkdownCodeFenceImpl"]        B --> C["MermaidLanguageInjector.elementsToInjectIn()"]        C --> D["Class.forName('...MarkdownCodeFenceImpl')<br/>反射載入"]  
        D --> E["getLanguagesToInject()"]        E --> F{"isMermaidFencedCodeBlock?<br/>text.startsWith('```mermaid')"}        F -->|"是"| G["registrar.startInjecting(MermaidLanguage)"]  
        G --> H["addPlace(TextRange: 第一個\\n 到最後```之前)"]  
        H --> I["MermaidLanguage 功能生效<br/>語法高亮 + 自動完成 + 標注"]  
    end  
    subgraph 預覽擴展  
        J["Markdown Preview Panel 初始化"]  
        J --> K["Provider.createBrowserExtension(panel)"]        K --> L["MermaidBrowserPreviewExtension"]        L --> L1["注入 scripts:<br/>mermaid.min.js + markdown-init.js"]        L --> L2["注入 styles:<br/>markdown-preview.css"]        L --> L3["resourceProvider:<br/>MermaidMarkdownResourceProvider"]        L3 --> M["從 classpath /mermaid/ 載入資源<br/>正確 MIME type 回應"]  
    end  
    style A fill:#e8f5e9    style I fill:#e3f2fd    style L fill:#f3e5f5  
```  
  
### 5.8 Lexer 語法分析流程  
  
```mermaid  
flowchart TD  
    A["MermaidLexer.start(buffer, startOffset, endOffset)"] --> B["初始化 bracketDepth/parenDepth/braceDepth = 0"]    B --> C["advance()"]  
    C --> D{"tokenStart >= bufferEnd?"}    D -->|"是"| E["tokenType = null (結束)"]  
    D -->|"否"| F["讀取字元 c = buffer[tokenStart]"]  
    F --> G{"bracketDepth > 0 且 c ≠ ']'?"}    G -->|"是"| H["lexBracketString()<br/>讀取到 ']' 為止<br/>回傳 STRING"]    G -->|"否"| I{"字元判斷"}  
  
    I -->|"%%"| J["lexCommentOrDirective()"]    J --> J1{"第3字元 == '{'?"}    J1 -->|"是"| J2["讀取到 }%% → DIRECTIVE"]    J1 -->|"否"| J3["讀取到行尾 → COMMENT"]  
    I -->|"\\n \\r"| K["lexNewline() → NEWLINE"]    I -->|"空白"| L["lexWhitespace() → WHITE_SPACE"]  
    I -->|"引號字元"| M["lexQuotedString() → STRING"]  
    I -->|"["| N["lexBracketContent() → LBRACKET<br/>bracketDepth++"]    I -->|"("| O["lexParenContent() → LPAREN"]    I -->|"{"| P["lexBraceContent() → LBRACE"]    I -->|"]"| Q["RBRACKET, bracketDepth--"]    I -->|") }"| R["RPAREN / RBRACE"]    I -->|"箭頭起始字元<br/>- = . < o x"| S["lexArrow()"]  
    I -->|"字母 / _"| T["lexIdentifier()"]    I -->|"數字"| U["lexNumber() → NUMBER"]  
    I -->|"其他"| V["BAD_CHARACTER"]  
  
    S --> S1["依長度降序嘗試匹配<br/>16 種箭頭模式"]  
    S1 -->|"匹配"| S2["ARROW"]  
    S1 -->|"不匹配"| S3{"是字母？"}  
    S3 -->|"是"| T  
    S3 -->|"否"| V  
  
    T --> T1["讀取 letterOrDigit / _ / -"]    T1 --> T2{"文字比對"}  
    T2 -->|"DIAGRAM_KEYWORDS"| T3["對應 Diagram Token<br/>如 FLOWCHART, SEQUENCE_DIAGRAM"]    T2 -->|"DIRECTIONS"| T4["DIRECTION"]    T2 -->|"KEYWORDS"| T5["KEYWORD / SUBGRAPH / END"]    T2 -->|"其他"| T6["IDENTIFIER"]  
  
    style A fill:#e8eaf6    style E fill:#ffcdd2    style S2 fill:#c8e6c9    style T3 fill:#c8e6c9  
```  
  
---  
  
## 6. 擴充點（Extension Points）  
  
以下為在 `plugin.xml` 中註冊的所有擴充點：  
  
```mermaid  
graph LR  
    subgraph "Core Extensions (com.intellij)"        FT["fileType<br/>MermaidFileType"]        SHF["lang.syntaxHighlighterFactory<br/>MermaidSyntaxHighlighterFactory"]        CSP["colorSettingsPage<br/>MermaidColorSettingsPage"]        FEP["fileEditorProvider<br/>MermaidEditorProvider"]        BM["lang.braceMatcher<br/>MermaidBraceMatcher"]        COM["lang.commenter<br/>MermaidCommenter"]        FB["lang.foldingBuilder<br/>MermaidFoldingBuilder"]        CC["completion.contributor<br/>MermaidCompletionContributor"]        PD["lang.parserDefinition<br/>MermaidParserDefinition"]        ANN["annotator<br/>MermaidAnnotator"]        AS["applicationService<br/>MermaidSettings"]        AC["applicationConfigurable<br/>MermaidSettingsConfigurable"]        SVF["lang.psiStructureViewFactory<br/>MermaidStructureViewFactory"]    end  
    subgraph "Markdown Extensions (optional)"        MHI["multiHostInjector<br/>MermaidLanguageInjector"]        BPE["browserPreviewExtensionProvider<br/>MermaidBrowserPreviewExtension.Provider"]    end  
    subgraph "Actions"        EG["Mermaid.ExportGroup → ToolsMenu"]        EP2["Mermaid.ExportPng (Ctrl+Shift+E)"]        ES["Mermaid.ExportSvg"]        CP["Mermaid.CopyPng"]        CD["Mermaid.CollectDiagnostics → HelpMenu"]    end  
```  
  
### 擴充點詳細表  
  
| 擴充點 | 實作類別 | 說明 |  
|--------|---------|------|  
| `fileType` | `MermaidFileType` | 註冊 `.mmd`, `.mermaid` 檔案類型 |  
| `lang.syntaxHighlighterFactory` | `MermaidSyntaxHighlighterFactory` | 語法高亮工廠 |  
| `colorSettingsPage` | `MermaidColorSettingsPage` | Settings → Editor → Color Scheme → Mermaid |  
| `fileEditorProvider` | `MermaidEditorProvider` | 分割編輯器提供者，策略 `HIDE_DEFAULT_EDITOR` |  
| `lang.braceMatcher` | `MermaidBraceMatcher` | `[]`, `()`, `{}` 括號配對 |  
| `lang.commenter` | `MermaidCommenter` | 行註解前綴 `%% ` |  
| `lang.foldingBuilder` | `MermaidFoldingBuilder` | `subgraph...end`, `%%{...}%%` 摺疊 |  
| `completion.contributor` | `MermaidCompletionContributor` | 圖表類型、方向、關鍵字、箭頭、形狀補全 |  
| `lang.parserDefinition` | `MermaidParserDefinition` | 建立 Lexer 和扁平 Parser |  
| `annotator` | `MermaidAnnotator` | 圖表類型驗證、方向檢查、括號匹配 |  
| `applicationService` | `MermaidSettings` | 應用級持久化設定服務 |  
| `applicationConfigurable` | `MermaidSettingsConfigurable` | Settings → Tools → Nereid |  
| `lang.psiStructureViewFactory` | `MermaidStructureViewFactory` | Structure 視窗中顯示 subgraph/nodes |  
| `multiHostInjector` | `MermaidLanguageInjector` | Markdown code fence 語言注入 |  
| `browserPreviewExtensionProvider` | `MermaidBrowserPreviewExtension.Provider` | Markdown 預覽 Mermaid.js 注入 |  
  
---  
  
## 7. 各套件詳細類別清單  
  
### 7.1 `com.nereid.language`  
  
| 類別 | 類型 | 父類/介面 | 說明 |  
|------|------|----------|------|  
| `MermaidLanguage` | `object` | `Language` | 語言單例，name = "Mermaid" |  
| `MermaidFileType` | `object` | `LanguageFileType` | 檔案類型，extensions: `mmd`, `mermaid` |  
| `MermaidElementType` | `class` | `IElementType` | 自訂元素類型，綁定 MermaidLanguage |  
| `MermaidTokenTypes` | `object` | — | Token 註冊中心，22 種圖表類型 + 方向/關鍵字/箭頭等 |  
| `MermaidLexer` | `class` | `LexerBase` | 手寫 Lexer，追蹤括號深度 |  
| `MermaidParserDefinition` | `class` | `ParserDefinition` | 建立扁平 AST（無結構解析） |  
| `MermaidFile` | `class` | `PsiFileBase` | PSI 檔案節點 |  
  
### 7.2 `com.nereid.editor`  
  
| 類別 | 父類/介面 | 說明 |  
|------|----------|------|  
| `MermaidSyntaxHighlighter` | `SyntaxHighlighterBase` | 11 種 TextAttributesKey 映射 |  
| `MermaidSyntaxHighlighterFactory` | `SyntaxHighlighterFactory` | 工廠模式 |  
| `MermaidColorSettingsPage` | `ColorSettingsPage` | 顏色設定 UI |  
| `MermaidBraceMatcher` | `PairedBraceMatcher` | `[]` `()` `{}` 配對 |  
| `MermaidCommenter` | `Commenter` | 行註解 `%% ` |  
| `MermaidFoldingBuilder` | `FoldingBuilderEx`, `DumbAware` | Regex 折疊 |  
| `MermaidCompletionContributor` | `CompletionContributor` | 上下文感知補全 |  
| `MermaidAnnotator` | `Annotator` | 檔案級標注檢查 |  
| `MermaidStructureViewFactory` | `PsiStructureViewFactory` | 結構視圖 |  
  
### 7.3 `com.nereid.spliteditor`  
  
| 類別 | 父類/介面 | 說明 |  
|------|----------|------|  
| `MermaidEditorProvider` | `FileEditorProvider`, `DumbAware` | 接受 MermaidFileType，建立 SplitEditor |  
| `MermaidSplitEditor` | `UserDataHolderBase`, `FileEditor` | 核心編輯器，組合 TextEditor + PreviewPanel + Toolbar |  
| `MermaidEditorState` | `FileEditorState` (data class) | ViewMode 狀態持久化 |  
| `MermaidEditorToolbar` | — | 工具列組裝：主題/背景下拉、縮放、設定 |  
| `ThemeDropdownAction` | `ComboBoxAction` | 主題選擇下拉 |  
| `BackgroundDropdownAction` | `ComboBoxAction` | 背景選擇下拉 |  
  
### 7.4 `com.nereid.preview`  
  
| 類別 | 父類/介面 | 說明 |  
|------|----------|------|  
| `MermaidPreviewPanel` | `Disposable` | JCEF 預覽核心，JS 橋接，匯出 API |  
| `ThemeManager` | — | 讀取設定，監聽 LAF 變更，通知主題變化 |  
| `DebouncedDocumentListener` | `DocumentListener` | 300ms Alarm 防抖 |  
  
### 7.5 `com.nereid.export`  
  
| 類別 | 說明 | 備註 |  
|------|------|------|  
| `PngExporter` | SVG → Canvas → PNG data URL 的 JS 生成器 | Legacy，主路徑在 PreviewPanel |  
| `SvgExporter` | SVG clone + 序列化的 JS 生成器 | Legacy |  
| `ClipboardExporter` | 系統剪貼簿操作（Image/SVG/String） | Legacy |  
  
### 7.6 `com.nereid.settings`  
  
| 類別 | 父類/介面 | 說明 |  
|------|----------|------|  
| `MermaidSettings` | `PersistentStateComponent<MermaidSettings>` | 持久化到 `mermaid.xml`，包含 8 種 enum |  
| `MermaidSettingsConfigurable` | `Configurable` | IDE 設定頁（Kotlin UI DSL） |  
| `MermaidSettingsDialog` | `DialogWrapper` | 模態設定對話框（3 個 Tab） |  
  
#### MermaidSettings 設定分類  
  
```mermaid  
graph TD  
    MS["MermaidSettings"]    MS --> ED["Editor 設定"]  
    MS --> AP["Appearance 設定"]  
    MS --> EX["Export 設定"]  
    MS --> ZM["Zoom 設定"]  
    MS --> AD["Advanced 設定"]  
  
    ED --> ED1["previewUpdateMode: LIVE | ON_SAVE | MANUAL"]    ED --> ED2["debounceDelayMs: 300"]    ED --> ED3["defaultViewMode: CODE_ONLY | SPLIT | PREVIEW_ONLY"]    ED --> ED4["showLineNumbersInErrors: true"]  
    AP --> AP1["themeMode: FOLLOW_IDE | MERMAID_THEME | CUSTOM"]    AP --> AP2["mermaidTheme: default/dark/forest/neutral"]    AP --> AP3["customThemeJson: String"]    AP --> AP4["previewBackground: TRANSPARENT | MATCH_IDE | WHITE | DARK"]  
    EX --> EX1["defaultExportFormat: PNG | SVG"]    EX --> EX2["pngScaleFactor: 2"]    EX --> EX3["pngTransparentBackground: true"]    EX --> EX4["svgEmbedFonts: true"]  
    ZM --> ZM1["mouseWheelZoomEnabled: true"]    ZM --> ZM2["zoomModifierKey: CTRL | CMD | NONE"]    ZM --> ZM3["zoomSensitivity: 5"]    ZM --> ZM4["defaultZoomLevel: FIT_ALL | ACTUAL_SIZE | LAST_USED"]  
    AD --> AD1["useCustomMermaidJs: false"]    AD --> AD2["customMermaidJsUrl: String"]    AD --> AD3["securityMode: STRICT | LOOSE"]    AD --> AD4["experimentalFeaturesEnabled: false"]  
```  
  
### 7.7 `com.nereid.diagnostics`  
  
| 類別 | 類型 | 說明 |  
|------|------|------|  
| `ActionLogger` | `object` | 執行緒安全環形緩衝區（max 20） |  
| `ActionLogEntry` | `data class` | timestamp + action，格式化為 `[HH:mm:ss.SSS] action` |  
| `EnvironmentInfo` | `data class` | IDE/Plugin/OS/Java/JCEF 資訊 |  
| `RenderingStatus` | `data class` | 最後錯誤、Mermaid 版本、console 訊息 |  
| `DiagnosticSection` | `data class` | title + content 報告區段 |  
| `DiagnosticBundle` | `data class` | 聚合所有診斷資料 |  
| `DiagnosticCollector` | `class` | 收集環境/設定/日誌/操作記錄 |  
| `DiagnosticDialog` | `class` (extends `DialogWrapper`) | 互動式報告審閱對話框 |  
| `DiagnosticNotifier` | `class` | 編輯器內黃色通知列 |  
| `GitHubIssueBuilder` | `object` | Markdown 報告生成 + GitHub URL 建構 |  
  
### 7.8 `com.nereid.actions`  
  
| 類別 | 父類 | 說明 |  
|------|------|------|  
| `ExportToPngAction` | `AnAction`, `DumbAware` | 匯出 PNG |  
| `ExportToSvgAction` | `AnAction`, `DumbAware` | 匯出 SVG |  
| `CopyAsPngAction` | `AnAction`, `DumbAware` | 複製 PNG 到剪貼簿 |  
| `CollectDiagnosticsAction` | `AnAction`, `DumbAware` | Help 選單，收集診斷資訊 |  
  
### 7.9 `com.nereid.markdown`  
  
| 類別 | 父類/介面 | 說明 |  
|------|----------|------|  
| `MermaidLanguageInjector` | `MultiHostInjector` | 注入 MermaidLanguage 到 Markdown code fence |  
| `MermaidBrowserPreviewExtension` | `MarkdownBrowserPreviewExtension` | 注入 Mermaid.js 到 Markdown 預覽 |  
| `MermaidBrowserPreviewExtension.Provider` | `MarkdownBrowserPreviewExtension.Provider` | 工廠 |  
| `MermaidMarkdownResourceProvider` | `ResourceProvider` | 從 classpath `/mermaid/` 提供資源 |  
  
---  
  
## 8. 資源檔案結構  
  
```  
src/main/resources/  
├── icons/  
│   ├── mermaid.svg              # 淺色主題圖示  
│   └── mermaid_dark.svg         # 深色主題圖示  
├── mermaid/  
│   ├── mermaid.min.js           # Mermaid.js 本地副本（Markdown 預覽用）  
│   ├── markdown-init.js         # Markdown 預覽初始化腳本  
│   ├── markdown-preview.css     # Markdown 預覽樣式  
│   ├── preview.html             # 預覽 HTML 模板（未使用，內嵌在 Kotlin 中）  
│   └── preview.css              # 預覽樣式（未使用）  
└── META-INF/  
    ├── plugin.xml               # 主外掛描述檔  
    ├── markdown-support.xml     # Markdown 可選依賴擴展  
    ├── pluginIcon.svg           # 外掛市場圖示  
    └── pluginIcon_dark.svg      # 外掛市場深色圖示  
```  
  
---  
  
## 附錄：技術架構決策說明  
  
### Java ↔ JavaScript 橋接機制  
  
`MermaidPreviewPanel` 使用 **3 個 `JBCefJSQuery`** 實例來實現 Java 與 JavaScript 的雙向通信：  
  
| Query 實例 | 用途 | JS → Java 傳遞內容 |  
|-----------|------|-------------------|  
| `jsQuery` | 渲染結果 + 縮放 + 回報 | `"success:"`, `"error:msg"`, `"zoom:1.5"`, `"report"` |  
| `pngExportQuery` | PNG 匯出 | `"data:image/png;base64,..."` (data URL) |  
| `svgExportQuery` | SVG 匯出 | SVG XML 字串 |  
  
### 為何有兩套設定 UI  
  
| UI | 入口 | 綁定方式 |  
|----|------|---------|  
| `MermaidSettingsConfigurable` | IDE Settings → Tools → Nereid | Kotlin UI DSL 直接綁定屬性 |  
| `MermaidSettingsDialog` | 工具列齒輪按鈕 | 複製到 local vars，OK 時 apply |  
  
兩者的存在讓使用者可以從不同的入口快速修改設定。  
  
### 扁平 Parser 設計  
  
`MermaidParserDefinition` 的 Parser 不建構有意義的 AST，而是將所有 Token 放在單一根節點下。所有的「智慧」功能（錯誤檢查、結構識別）都依賴 Lexer token 和 `MermaidAnnotator` 的文字分析。這簡化了實作，但限制了 PSI 級別的導航和重構能力。