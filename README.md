<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>MD-Studio | Rich Markdown Creator & Live Editor</title>
  
  <!-- Tailwind CSS -->
  <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>

  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;500;600&family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600;700&family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Share+Tech+Mono&family=Lora:ital,wght@0,400;0,600;1,400&display=swap" rel="stylesheet">

  <!-- Prism.js Syntax Highlighting Style -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/prism-tomorrow.min.css">

  <style>
    /* Custom Scrollbars */
    ::-webkit-scrollbar {
      width: 8px;
      height: 8px;
    }
    ::-webkit-scrollbar-track {
      background: rgba(15, 23, 42, 0.05);
    }
    .dark ::-webkit-scrollbar-track {
      background: rgba(30, 41, 59, 0.5);
    }
    ::-webkit-scrollbar-thumb {
      background: rgba(148, 163, 184, 0.5);
      border-radius: 4px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: rgba(148, 163, 184, 0.8);
    }

    /* Core Styles */
    body {
      font-family: 'Inter', sans-serif;
    }
    .font-mono-editor {
      font-family: 'JetBrains Mono', 'Fira Code', monospace;
    }

    /* Markdown Render Theme Styles */
    .md-preview {
      line-height: 1.625;
      font-size: 15px;
    }
    
    /* Theme 1: GitHub Clean Style */
    .theme-github {
      --preview-bg: #ffffff;
      --preview-text: #24292f;
      --preview-heading: #1f2328;
      --preview-border: #d0d7de;
      --preview-code-bg: #afb8c133;
      --preview-blockquote-bg: #f6f8fa;
      --preview-blockquote-border: #fd8c73; /* custom accent */
      --preview-accent: #0969da;
      font-family: 'Inter', -apple-system, sans-serif;
    }
    .dark .theme-github {
      --preview-bg: #0d1117;
      --preview-text: #dbe6ec;
      --preview-heading: #f0f6fc;
      --preview-border: #30363d;
      --preview-code-bg: #6e768166;
      --preview-blockquote-bg: #161b22;
      --preview-accent: #58a6ff;
    }

    /* Theme 2: Minimalist Serif (Elegant Editorial) */
    .theme-editorial {
      --preview-bg: #faf8f5;
      --preview-text: #2c2a29;
      --preview-heading: #1a1918;
      --preview-border: #e6e1da;
      --preview-code-bg: #f0ede8;
      --preview-blockquote-bg: #f4efeb;
      --preview-blockquote-border: #8b5a2b;
      --preview-accent: #8b5a2b;
      font-family: 'Lora', 'Playfair Display', serif;
    }
    .dark .theme-editorial {
      --preview-bg: #141412;
      --preview-text: #dfdedb;
      --preview-heading: #fcfcfc;
      --preview-border: #2a2927;
      --preview-code-bg: #1c1b19;
      --preview-blockquote-bg: #181715;
      --preview-blockquote-border: #b89876;
      --preview-accent: #d2b48c;
    }

    /* Theme 3: Retro Terminal */
    .theme-terminal {
      --preview-bg: #030712;
      --preview-text: #10b981;
      --preview-heading: #34d399;
      --preview-border: #065f46;
      --preview-code-bg: #111827;
      --preview-blockquote-bg: #070f1a;
      --preview-blockquote-border: #10b981;
      --preview-accent: #10b981;
      font-family: 'Share Tech Mono', monospace;
    }

    /* Theme 4: Modern Neo-Indigo */
    .theme-indigo {
      --preview-bg: #f8fafc;
      --preview-text: #334155;
      --preview-heading: #0f172a;
      --preview-border: #e2e8f0;
      --preview-code-bg: #f1f5f9;
      --preview-blockquote-bg: #eef2ff;
      --preview-blockquote-border: #6366f1;
      --preview-accent: #4f46e5;
      font-family: 'Inter', sans-serif;
    }
    .dark .theme-indigo {
      --preview-bg: #0f172a;
      --preview-text: #cbd5e1;
      --preview-heading: #f8fafc;
      --preview-border: #334155;
      --preview-code-bg: #1e293b;
      --preview-blockquote-bg: #1e1b4b;
      --preview-blockquote-border: #818cf8;
      --preview-accent: #818cf8;
    }

    /* Apply styles dynamically inside preview pane */
    .rendered-markdown {
      background-color: var(--preview-bg);
      color: var(--preview-text);
      transition: background-color 0.2s, color 0.2s;
    }
    .rendered-markdown h1, 
    .rendered-markdown h2, 
    .rendered-markdown h3, 
    .rendered-markdown h4, 
    .rendered-markdown h5, 
    .rendered-markdown h6 {
      color: var(--preview-heading);
      font-weight: 700;
      margin-top: 1.5em;
      margin-bottom: 0.5em;
      border-bottom: 1px solid var(--preview-border);
      padding-bottom: 0.3em;
    }
    .rendered-markdown h1 { font-size: 2.1em; }
    .rendered-markdown h2 { font-size: 1.6em; }
    .rendered-markdown h3 { font-size: 1.3em; }
    .rendered-markdown h4 { font-size: 1.15em; }
    
    .rendered-markdown p {
      margin-bottom: 1em;
    }
    .rendered-markdown a {
      color: var(--preview-accent);
      text-decoration: underline;
      font-weight: 500;
    }
    .rendered-markdown blockquote {
      background-color: var(--preview-blockquote-bg);
      border-left: 4px solid var(--preview-blockquote-border);
      padding: 0.75rem 1.25rem;
      margin: 1.25rem 0;
      border-radius: 0 4px 4px 0;
      font-style: italic;
    }
    .rendered-markdown code {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.9em;
      background-color: var(--preview-code-bg);
      padding: 0.2em 0.4em;
      border-radius: 4px;
    }
    .rendered-markdown pre {
      background-color: #1e293b;
      padding: 1rem;
      border-radius: 8px;
      overflow-x: auto;
      margin: 1.25rem 0;
    }
    .rendered-markdown pre code {
      background-color: transparent;
      padding: 0;
      color: #f8fafc;
      border-radius: 0;
    }
    .rendered-markdown ul {
      list-style-type: disc;
      padding-left: 2rem;
      margin-bottom: 1rem;
    }
    .rendered-markdown ol {
      list-style-type: decimal;
      padding-left: 2rem;
      margin-bottom: 1rem;
    }
    .rendered-markdown li {
      margin-bottom: 0.35rem;
    }
    .rendered-markdown table {
      width: 100%;
      border-collapse: collapse;
      margin: 1.5rem 0;
    }
    .rendered-markdown th {
      background-color: var(--preview-code-bg);
      border: 1px solid var(--preview-border);
      padding: 8px 12px;
      font-weight: 600;
      text-align: left;
    }
    .rendered-markdown td {
      border: 1px solid var(--preview-border);
      padding: 8px 12px;
    }
    .rendered-markdown tr:nth-child(even) {
      background-color: rgba(148, 163, 184, 0.05);
    }
    .rendered-markdown hr {
      border: 0;
      height: 1px;
      background: var(--preview-border);
      margin: 2rem 0;
    }
    .rendered-markdown img {
      max-width: 100%;
      height: auto;
      border-radius: 6px;
      margin: 1.5rem auto;
      display: block;
    }
    
    /* Styled tasks list */
    .rendered-markdown input[type="checkbox"] {
      margin-right: 0.5rem;
      border-radius: 4px;
      accent-color: var(--preview-accent);
    }

    /* Alerts within markdown custom styling */
    .markdown-alert {
      padding: 1rem;
      border-radius: 6px;
      margin: 1rem 0;
      border-left: 4px solid;
    }
    .markdown-alert-note {
      background-color: rgba(59, 130, 246, 0.08);
      border-color: #3b82f6;
      color: #1e3a8a;
    }
    .dark .markdown-alert-note {
      color: #93c5fd;
    }
    .markdown-alert-warning {
      background-color: rgba(245, 158, 11, 0.08);
      border-color: #f59e0b;
      color: #78350f;
    }
    .dark .markdown-alert-warning {
      color: #fde047;
    }
    .markdown-alert-tip {
      background-color: rgba(16, 185, 129, 0.08);
      border-color: #10b981;
      color: #064e3b;
    }
    .dark .markdown-alert-tip {
      color: #6ee7b7;
    }

    /* Scroll synchronization utility class */
    .editor-pane, .preview-pane {
      scroll-behavior: auto;
    }

    /* Toast animation */
    @keyframes slideIn {
      from { transform: translateY(100%) translateX(-50%); opacity: 0; }
      to { transform: translateY(0) translateX(-50%); opacity: 1; }
    }
    .toast-animate {
      animation: slideIn 0.2s ease-out forwards;
    }

    /* Print Styles */
    @media print {
      body, html {
        background: #ffffff !important;
        color: #000000 !important;
        height: auto !important;
        overflow: visible !important;
      }
      /* Hide navigation header, files sidebar, toolbars, editor, outlines, and floating widgets */
      header,
      aside,
      #findPanel,
      .bg-white.dark\:bg-slate-900.border-b,
      .border-b.border-slate-100.dark\:border-slate-800.select-none,
      #editorPane,
      #outlineWidget,
      button,
      select {
        display: none !important;
      }
      /* Expand the preview pane to full width */
      #previewPane {
        display: block !important;
        width: 100% !important;
        height: auto !important;
        overflow: visible !important;
        position: static !important;
        border: none !important;
        padding: 0 !important;
        margin: 0 !important;
      }
      .rendered-markdown {
        background: #ffffff !important;
        color: #000000 !important;
        padding: 0 !important;
        margin: 0 !important;
        overflow: visible !important;
        max-height: none !important;
        height: auto !important;
      }
      /* Styled print-ready links */
      .rendered-markdown a {
        text-decoration: underline !important;
        color: #0969da !important;
      }
    }
  </style>
</head>
<body class="bg-slate-50 dark:bg-slate-950 text-slate-800 dark:text-slate-100 min-h-screen flex flex-col transition-colors duration-200">

  <!-- TOP HEADER PANEL -->
  <header class="border-b border-slate-200 dark:border-slate-800 bg-white dark:bg-slate-900 shadow-sm px-4 py-3 sticky top-0 z-40 transition-colors duration-200">
    <div class="max-w-[1800px] mx-auto flex flex-col md:flex-row items-center justify-between gap-4">
      
      <!-- Brand & File Info -->
      <div class="flex items-center gap-3 w-full md:w-auto">
        <div class="bg-gradient-to-tr from-indigo-600 to-pink-600 text-white p-2 rounded-lg flex items-center justify-center shadow-md">
          <i class="fa-solid fa-file-pen text-lg"></i>
        </div>
        <div>
          <div class="flex items-center gap-2">
            <span class="font-bold text-slate-800 dark:text-white tracking-tight text-lg">MD-Studio</span>
            <span class="text-xs bg-indigo-50 text-indigo-600 dark:bg-indigo-950 dark:text-indigo-400 font-medium px-2 py-0.5 rounded-full border border-indigo-100 dark:border-indigo-900/50">v2.1</span>
          </div>
          <div class="flex items-center gap-1.5 mt-0.5 text-xs text-slate-400 dark:text-slate-500">
            <span>Markdown Creation Suite</span>
            <span>•</span>
            <span class="text-indigo-500 font-medium">Autosaved Draft</span>
          </div>
        </div>

        <!-- File Name Input -->
        <div class="ml-4 flex items-center gap-2 bg-slate-100 dark:bg-slate-800 px-3 py-1.5 rounded-lg border border-slate-200 dark:border-slate-700 w-44 md:w-60 focus-within:ring-2 focus-within:ring-indigo-500/50 transition-all">
          <i class="fa-regular fa-file-lines text-slate-400"></i>
          <input type="text" id="fileNameInput" value="README.md" class="bg-transparent font-medium text-sm text-slate-700 dark:text-slate-300 outline-none w-full border-none p-0 focus:ring-0" placeholder="filename.md" title="Rename active draft file">
        </div>
      </div>

      <!-- Live File Stats Summary (Mobile hidden / MD visible) -->
      <div class="hidden lg:flex items-center gap-4 text-xs font-medium text-slate-500 dark:text-slate-400 bg-slate-50 dark:bg-slate-900/50 border border-slate-200/60 dark:border-slate-800/60 rounded-lg px-4 py-2">
        <div class="flex items-center gap-1.5">
          <i class="fa-solid fa-calculator text-indigo-500"></i>
          <span id="statsWords">0</span> words
        </div>
        <div class="h-3 w-px bg-slate-300 dark:bg-slate-700"></div>
        <div class="flex items-center gap-1.5">
          <i class="fa-solid fa-font text-pink-500"></i>
          <span id="statsChars">0</span> chars
        </div>
        <div class="h-3 w-px bg-slate-300 dark:bg-slate-700"></div>
        <div class="flex items-center gap-1.5">
          <i class="fa-regular fa-clock text-blue-500"></i>
          <span id="statsReadingTime">0 min</span> read
        </div>
      </div>

      <!-- Action Buttons -->
      <div class="flex items-center flex-wrap gap-2 w-full md:w-auto justify-end">
        
        <!-- Import MD File button -->
        <label for="importFile" class="flex items-center gap-2 cursor-pointer bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 px-3 py-2 rounded-lg text-sm font-medium border border-slate-200 dark:border-slate-700 transition-colors">
          <i class="fa-solid fa-file-import text-slate-500"></i>
          <span class="hidden sm:inline">Import</span>
          <input type="file" id="importFile" accept=".md,.txt" class="hidden">
        </label>

        <!-- Template Selector -->
        <div class="relative group" id="templateDropdown">
          <button id="templateBtn" class="flex items-center gap-2 bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 px-3 py-2 rounded-lg text-sm font-medium border border-slate-200 dark:border-slate-700 transition-colors">
            <i class="fa-solid fa-wand-magic-sparkles text-indigo-500"></i>
            <span>Templates</span>
            <i class="fa-solid fa-chevron-down text-[10px] ml-1"></i>
          </button>
          <div id="templateMenu" class="hidden absolute right-0 top-full mt-2 w-52 bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-xl shadow-xl z-50 overflow-hidden">
            <div class="p-2 border-b border-slate-100 dark:border-slate-800 bg-slate-50 dark:bg-slate-900/50">
              <span class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Choose File Preset</span>
            </div>
            <button onclick="loadTemplate('default')" class="w-full text-left px-3 py-2 hover:bg-indigo-50 dark:hover:bg-indigo-950/40 text-xs font-semibold text-indigo-600 dark:text-indigo-400 flex items-center gap-2">
              <i class="fa-solid fa-graduation-cap"></i> Markdown Crash Course
            </button>
            <button onclick="loadTemplate('readme')" class="w-full text-left px-3 py-2 hover:bg-slate-100 dark:hover:bg-slate-800/60 text-xs text-slate-600 dark:text-slate-300 flex items-center gap-2">
              <i class="fa-solid fa-cube text-slate-400"></i> Professional README.md
            </button>
            <button onclick="loadTemplate('api')" class="w-full text-left px-3 py-2 hover:bg-slate-100 dark:hover:bg-slate-800/60 text-xs text-slate-600 dark:text-slate-300 flex items-center gap-2">
              <i class="fa-solid fa-code text-slate-400"></i> REST API Documentation
            </button>
            <button onclick="loadTemplate('notes')" class="w-full text-left px-3 py-2 hover:bg-slate-100 dark:hover:bg-slate-800/60 text-xs text-slate-600 dark:text-slate-300 flex items-center gap-2">
              <i class="fa-regular fa-clipboard text-slate-400"></i> Project Status & Notes
            </button>
            <button onclick="loadTemplate('resume')" class="w-full text-left px-3 py-2 hover:bg-slate-100 dark:hover:bg-slate-800/60 text-xs text-slate-600 dark:text-slate-300 flex items-center gap-2">
              <i class="fa-regular fa-id-card text-slate-400"></i> Professional Resume
            </button>
            <button onclick="loadTemplate('blank')" class="w-full text-left px-3 py-2 hover:bg-slate-100 dark:hover:bg-slate-800/60 text-xs text-rose-500 flex items-center gap-2 border-t border-slate-100 dark:border-slate-800">
              <i class="fa-regular fa-file"></i> Clean Slate (Blank)
            </button>
          </div>
        </div>

        <!-- Download / Export Suite Dropdown -->
        <div class="relative group" id="exportDropdown">
          <button id="exportBtn" class="flex items-center gap-2 bg-gradient-to-r from-indigo-600 to-indigo-700 hover:from-indigo-700 hover:to-indigo-800 text-white px-4 py-2 rounded-lg text-sm font-semibold shadow-md shadow-indigo-500/20 transition-all">
            <i class="fa-solid fa-download"></i>
            <span>Export File</span>
            <i class="fa-solid fa-chevron-down text-[10px] ml-1"></i>
          </button>
          <div id="exportMenu" class="hidden absolute right-0 top-full mt-2 w-64 bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-xl shadow-xl z-50 overflow-hidden">
            <div class="p-2 border-b border-slate-100 dark:border-slate-800 bg-slate-50 dark:bg-slate-900/50">
              <span class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Download Formats</span>
            </div>
            
            <button onclick="downloadAsMarkdown()" class="w-full text-left px-4 py-2.5 hover:bg-slate-50 dark:hover:bg-slate-800/50 flex items-start gap-3">
              <span class="p-1.5 bg-green-50 dark:bg-green-950/40 text-green-600 rounded-md mt-0.5"><i class="fa-brands fa-markdown text-sm"></i></span>
              <div>
                <div class="text-xs font-bold text-slate-700 dark:text-slate-200">Download .md File</div>
                <div class="text-[10px] text-slate-400">Standard Markdown Document</div>
              </div>
            </button>

            <button onclick="downloadAsHTML()" class="w-full text-left px-4 py-2.5 hover:bg-slate-50 dark:hover:bg-slate-800/50 flex items-start gap-3">
              <span class="p-1.5 bg-orange-50 dark:bg-orange-950/40 text-orange-600 rounded-md mt-0.5"><i class="fa-solid fa-code text-sm"></i></span>
              <div>
                <div class="text-xs font-bold text-slate-700 dark:text-slate-200">Download .html File</div>
                <div class="text-[10px] text-slate-400">Fully Rendered Static HTML Page</div>
              </div>
            </button>

            <button onclick="printPreview()" class="w-full text-left px-4 py-2.5 hover:bg-slate-50 dark:hover:bg-slate-800/50 flex items-start gap-3 border-t border-slate-100 dark:border-slate-800">
              <span class="p-1.5 bg-purple-50 dark:bg-purple-950/40 text-purple-600 rounded-md mt-0.5"><i class="fa-solid fa-print text-sm"></i></span>
              <div>
                <div class="text-xs font-bold text-slate-700 dark:text-slate-200">Print / Save to PDF</div>
                <div class="text-[10px] text-slate-400">Format using Print CSS Rules</div>
              </div>
            </button>

            <div class="p-2 border-t border-slate-100 dark:border-slate-800 bg-slate-50 dark:bg-slate-900/50">
              <span class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Clipboard Helpers</span>
            </div>

            <button onclick="copyMarkdownText()" class="w-full text-left px-4 py-2 hover:bg-slate-50 dark:hover:bg-slate-800/50 flex items-center gap-2 text-xs text-slate-700 dark:text-slate-300">
              <i class="fa-regular fa-copy text-slate-400"></i> Copy raw Markdown code
            </button>
            <button onclick="copyHTMLText()" class="w-full text-left px-4 py-2 hover:bg-slate-50 dark:hover:bg-slate-800/50 flex items-center gap-2 text-xs text-slate-700 dark:text-slate-300">
              <i class="fa-regular fa-file-code text-slate-400"></i> Copy rendered HTML source
            </button>
          </div>
        </div>

        <div class="h-6 w-px bg-slate-200 dark:bg-slate-800"></div>

        <!-- Light/Dark Mode Switcher -->
        <button id="themeToggleBtn" onclick="toggleTheme()" class="p-2 text-slate-500 hover:text-indigo-600 dark:text-slate-400 dark:hover:text-indigo-400 rounded-lg bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 transition-colors" title="Toggle App Dark/Light Theme">
          <i id="themeToggleIcon" class="fa-solid fa-moon text-sm"></i>
        </button>

        <!-- Help Info button -->
        <button onclick="openHelpModal()" class="p-2 text-slate-500 hover:text-indigo-600 dark:text-slate-400 dark:hover:text-indigo-400 rounded-lg bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 transition-colors" title="View Keyboard Shortcuts & Cheat Sheet">
          <i class="fa-solid fa-circle-question text-sm"></i>
        </button>
      </div>

    </div>
  </header>

  <!-- WORKSPACE GRID CONTAINER -->
  <main class="flex-1 flex flex-col md:flex-row overflow-hidden relative">
    
    <!-- SUB-SIDEBAR: Saved Files Manager (Can toggle hide) -->
    <aside id="fileSidebar" class="w-full md:w-64 border-r border-slate-200 dark:border-slate-800 bg-white dark:bg-slate-900 flex flex-col shrink-0 transition-all duration-300">
      <div class="p-4 border-b border-slate-100 dark:border-slate-800 flex items-center justify-between">
        <h2 class="text-xs font-bold text-slate-400 dark:text-slate-500 uppercase tracking-widest flex items-center gap-2">
          <i class="fa-solid fa-folder-open"></i> Local Documents
        </h2>
        <button onclick="createNewFile()" class="p-1 text-indigo-600 hover:text-indigo-800 dark:text-indigo-400 dark:hover:text-indigo-300 text-xs font-bold flex items-center gap-1 bg-indigo-50 dark:bg-indigo-950/50 px-2 py-0.5 rounded transition-all">
          <i class="fa-solid fa-plus"></i> New File
        </button>
      </div>
      
      <!-- List of Files -->
      <div id="fileListContainer" class="flex-1 overflow-y-auto p-2 space-y-1">
        <!-- Dynamic loading of saved markdown keys -->
      </div>

      <!-- Quick Action / Tips -->
      <div class="p-3 bg-slate-50 dark:bg-slate-900/50 border-t border-slate-100 dark:border-slate-800">
        <div class="text-[10px] text-slate-400 leading-relaxed font-medium">
          <i class="fa-solid fa-circle-info text-indigo-500"></i> Changes are autosaved to local storage immediately.
        </div>
      </div>
    </aside>

    <!-- CENTER & RIGHT: Splits for Editor & Preview -->
    <div class="flex-1 flex flex-col overflow-hidden">
      
      <!-- CONTROL BAR: View Modifiers & Text Styling Shortcuts -->
      <div class="bg-white dark:bg-slate-900 border-b border-slate-200 dark:border-slate-800 px-4 py-2 flex flex-wrap items-center justify-between gap-3 shadow-sm select-none z-10">
        
        <!-- Markdown Formatting Helpers -->
        <div class="flex items-center flex-wrap gap-1">
          <!-- Text Styling -->
          <button onclick="insertAtCursor('**', '**')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Bold Text (Ctrl+B)">
            <i class="fa-solid fa-bold text-sm"></i>
          </button>
          <button onclick="insertAtCursor('*', '*')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Italic Text (Ctrl+I)">
            <i class="fa-solid fa-italic text-sm"></i>
          </button>
          <button onclick="insertAtCursor('~~', '~~')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Strikethrough text">
            <i class="fa-solid fa-strikethrough text-sm"></i>
          </button>
          <button onclick="insertAtCursor('`', '`')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Inline Code">
            <i class="fa-solid fa-code text-sm"></i>
          </button>

          <div class="h-4 w-px bg-slate-200 dark:bg-slate-800 mx-1"></div>

          <!-- Headings -->
          <button onclick="insertHeading('# ')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors font-bold text-xs" title="Main Heading H1">
            H1
          </button>
          <button onclick="insertHeading('## ')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors font-bold text-xs" title="Heading H2">
            H2
          </button>
          <button onclick="insertHeading('### ')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors font-bold text-xs" title="Heading H3">
            H3
          </button>

          <div class="h-4 w-px bg-slate-200 dark:bg-slate-800 mx-1"></div>

          <!-- Structures & Lists -->
          <button onclick="insertAtCursor('\n- ', '')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Unordered Bullet List">
            <i class="fa-solid fa-list text-sm"></i>
          </button>
          <button onclick="insertAtCursor('\n1. ', '')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Ordered Numeric List">
            <i class="fa-solid fa-list-ol text-sm"></i>
          </button>
          <button onclick="insertAtCursor('\n- [ ] ', '')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Checklist Task Item">
            <i class="fa-regular fa-square-check text-sm"></i>
          </button>
          <button onclick="insertAtCursor('\n> ', '')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Block Quote">
            <i class="fa-solid fa-quote-left text-sm"></i>
          </button>
          <button onclick="insertAtCursor('\n\n---\n\n', '')" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Horizontal Rule">
            <i class="fa-solid fa-minus text-sm"></i>
          </button>

          <div class="h-4 w-px bg-slate-200 dark:bg-slate-800 mx-1"></div>

          <!-- Modals & Complex insertion -->
          <button onclick="openLinkModal()" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Hyperlink (Ctrl+K)">
            <i class="fa-solid fa-link text-sm"></i>
          </button>
          <button onclick="openImageModal()" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Insert Image Link">
            <i class="fa-regular fa-image text-sm"></i>
          </button>
          <button onclick="openTableModal()" class="p-1.5 text-slate-500 hover:text-indigo-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:text-indigo-400 dark:hover:bg-slate-800 rounded transition-colors" title="Insert Multi-column Grid Table">
            <i class="fa-solid fa-table text-sm"></i>
          </button>
          
          <div class="h-4 w-px bg-slate-200 dark:bg-slate-800 mx-1"></div>

          <!-- Styled Alert Boxes -->
          <button onclick="insertAlert('note')" class="p-1.5 text-xs font-semibold text-blue-500 hover:bg-blue-50 dark:hover:bg-blue-950/30 rounded border border-blue-200/50 dark:border-blue-900/30 px-2" title="Blue Information Banner">
            + Note Alert
          </button>
          <button onclick="insertAlert('warning')" class="p-1.5 text-xs font-semibold text-amber-500 hover:bg-amber-50 dark:hover:bg-amber-950/30 rounded border border-amber-200/50 dark:border-amber-900/30 px-2" title="Yellow Warning Banner">
            + Warning
          </button>
          <button onclick="insertAlert('tip')" class="p-1.5 text-xs font-semibold text-emerald-500 hover:bg-emerald-50 dark:hover:bg-emerald-950/30 rounded border border-emerald-200/50 dark:border-emerald-900/30 px-2" title="Green Tip Banner">
            + Tip
          </button>
        </div>

        <!-- View Controls & Visual Themes -->
        <div class="flex items-center gap-3">
          
          <!-- Search & Replace Mini-Panel toggle -->
          <button onclick="toggleFindPanel()" class="p-1.5 text-slate-400 hover:text-indigo-500 rounded bg-slate-50 dark:bg-slate-800 hover:bg-slate-100 transition-colors text-xs font-medium flex items-center gap-1.5" title="Search text (Ctrl+F)">
            <i class="fa-solid fa-magnifying-glass"></i>
            <span class="hidden sm:inline">Find</span>
          </button>

          <div class="h-5 w-px bg-slate-200 dark:bg-slate-800"></div>

          <!-- Document Themes Selector -->
          <div class="flex items-center gap-1.5">
            <span class="text-xs text-slate-400 font-semibold hidden sm:inline">Theme:</span>
            <select id="previewThemeSelector" onchange="changePreviewTheme(this.value)" class="text-xs bg-slate-100 dark:bg-slate-800 border-none rounded-md py-1 px-2.5 font-medium text-slate-700 dark:text-slate-300 focus:ring-1 focus:ring-indigo-500">
              <option value="github">GitHub Classic</option>
              <option value="indigo">Neo Indigo</option>
              <option value="editorial">Editorial Lora</option>
              <option value="terminal">Retro Green Terminal</option>
            </select>
          </div>

          <div class="h-5 w-px bg-slate-200 dark:bg-slate-800"></div>

          <!-- Split View Controls -->
          <div class="bg-slate-100 dark:bg-slate-800 p-0.5 rounded-lg flex items-center">
            <button onclick="setViewMode('editor')" id="viewBtnEditor" class="px-2.5 py-1 text-xs font-semibold rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition-all flex items-center gap-1.5" title="Zen Mode - Editor Only">
              <i class="fa-solid fa-pen"></i>
              <span class="hidden lg:inline">Zen</span>
            </button>
            <button onclick="setViewMode('split')" id="viewBtnSplit" class="px-2.5 py-1 text-xs font-semibold rounded-md text-slate-800 dark:text-white bg-white dark:bg-slate-700 shadow-sm transition-all flex items-center gap-1.5" title="Split Screen Mode">
              <i class="fa-solid fa-columns"></i>
              <span class="hidden lg:inline">Split</span>
            </button>
            <button onclick="setViewMode('preview')" id="viewBtnPreview" class="px-2.5 py-1 text-xs font-semibold rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition-all flex items-center gap-1.5" title="Read Mode - Preview Only">
              <i class="fa-solid fa-eye"></i>
              <span class="hidden lg:inline">Preview</span>
            </button>
          </div>

          <!-- Sidebar toggle button -->
          <button id="sidebarToggleBtn" onclick="toggleSidebar()" class="p-1.5 bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 rounded-lg text-slate-500 dark:text-slate-400 transition-colors" title="Toggle File Explorer Sidebar">
            <i class="fa-solid fa-sidebar"></i>
          </button>
        </div>

      </div>

      <!-- SEARCH AND REPLACE DRAWER (HIDDEN BY DEFAULT) -->
      <div id="findPanel" class="hidden bg-slate-50 dark:bg-slate-900/90 border-b border-slate-200 dark:border-slate-800 px-4 py-2.5 flex flex-wrap items-center gap-3 animate-slideIn">
        <div class="flex items-center gap-1 bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded px-2.5 py-1 text-xs">
          <span class="text-slate-400 font-bold uppercase tracking-wide text-[9px] mr-1">Find</span>
          <input type="text" id="findInput" class="bg-transparent outline-none border-none p-0 w-32 focus:ring-0 text-slate-700 dark:text-slate-200 placeholder-slate-400" placeholder="Text to search...">
        </div>
        <div class="flex items-center gap-1 bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded px-2.5 py-1 text-xs">
          <span class="text-slate-400 font-bold uppercase tracking-wide text-[9px] mr-1">Replace</span>
          <input type="text" id="replaceInput" class="bg-transparent outline-none border-none p-0 w-32 focus:ring-0 text-slate-700 dark:text-slate-200 placeholder-slate-400" placeholder="Replacement text...">
        </div>
        <div class="flex items-center gap-1.5">
          <button onclick="findNextOccurrence()" class="px-2.5 py-1 bg-indigo-50 hover:bg-indigo-100 dark:bg-indigo-950 dark:hover:bg-indigo-900 text-indigo-600 dark:text-indigo-400 font-bold text-xs rounded transition-colors">
            Find Next
          </button>
          <button onclick="replaceSingle()" class="px-2.5 py-1 bg-slate-200 hover:bg-slate-300 dark:bg-slate-700 dark:hover:bg-slate-600 text-slate-700 dark:text-slate-200 font-semibold text-xs rounded transition-colors">
            Replace
          </button>
          <button onclick="replaceAllOccurrences()" class="px-2.5 py-1 bg-slate-200 hover:bg-slate-300 dark:bg-slate-700 dark:hover:bg-slate-600 text-slate-700 dark:text-slate-200 font-semibold text-xs rounded transition-colors">
            Replace All
          </button>
          <span id="findStatus" class="text-xs text-slate-400 ml-2 font-medium"></span>
        </div>
        <button onclick="toggleFindPanel()" class="ml-auto text-slate-400 hover:text-rose-500 text-sm">
          <i class="fa-solid fa-xmark"></i>
        </button>
      </div>

      <!-- MAIN EDITOR AND PREVIEW WORKSPACE -->
      <div class="flex-1 flex overflow-hidden">
        
        <!-- EDITOR PANEL -->
        <div id="editorPane" class="flex-1 flex flex-col min-w-0 bg-white dark:bg-slate-900 overflow-hidden relative border-r border-slate-200 dark:border-slate-800 transition-colors duration-200">
          <div class="flex items-center justify-between px-4 py-1.5 bg-slate-50 dark:bg-slate-900 border-b border-slate-100 dark:border-slate-800 select-none">
            <span class="text-[10px] font-bold text-indigo-600 dark:text-indigo-400 tracking-wider uppercase flex items-center gap-1.5">
              <i class="fa-solid fa-terminal text-[9px]"></i> WYSIWYM MarkDown Input
            </span>
            <span class="text-[10px] text-slate-400 font-medium">
              Press <kbd class="px-1 bg-slate-200 dark:bg-slate-800 rounded">Tab</kbd> to indent
            </span>
          </div>
          
          <textarea id="markdownEditor" 
            class="flex-1 p-6 font-mono-editor text-sm leading-relaxed bg-white dark:bg-slate-900 text-slate-800 dark:text-slate-200 outline-none resize-none focus:ring-0 overflow-y-auto"
            placeholder="Type your markdown here..."
            oninput="handleInput()"
            spellcheck="true"
          ></textarea>
        </div>

        <!-- LIVE PREVIEW PANEL -->
        <div id="previewPane" class="flex-1 flex flex-col min-w-0 bg-white dark:bg-slate-900 overflow-hidden relative transition-colors duration-200">
          <div class="flex items-center justify-between px-4 py-1.5 bg-slate-50 dark:bg-slate-900 border-b border-slate-100 dark:border-slate-800 select-none">
            <span class="text-[10px] font-bold text-slate-400 tracking-wider uppercase flex items-center gap-1.5">
              <i class="fa-regular fa-eye"></i> Rendered Output Preview
            </span>
            <span class="text-[10px] text-emerald-600 dark:text-emerald-400 font-semibold flex items-center gap-1">
              <span class="h-1.5 w-1.5 bg-emerald-500 rounded-full animate-ping"></span> Live Synced
            </span>
          </div>

          <div class="flex-1 overflow-y-auto rendered-markdown p-6 md:p-8 theme-github" id="renderedPreview">
            <!-- Rendered HTML injects here -->
          </div>

          <!-- FLOATING TABLE OF CONTENTS OUTLINE -->
          <div id="outlineWidget" class="absolute right-4 bottom-4 w-60 max-h-56 bg-white/95 dark:bg-slate-900/95 shadow-lg border border-slate-200 dark:border-slate-800 rounded-xl overflow-hidden hidden flex-col">
            <div class="p-2.5 bg-slate-100 dark:bg-slate-800 flex items-center justify-between border-b border-slate-200 dark:border-slate-700">
              <span class="text-[10px] font-bold uppercase tracking-wider text-slate-500 dark:text-slate-400">
                <i class="fa-solid fa-list-ul mr-1"></i> Outline (TOC)
              </span>
              <button onclick="toggleOutlineWidget()" class="text-slate-400 hover:text-slate-600 text-xs">
                <i class="fa-solid fa-chevron-down"></i>
              </button>
            </div>
            <div id="outlineList" class="p-2 space-y-1 overflow-y-auto text-xs max-h-44">
              <!-- Outline list elements -->
            </div>
          </div>

          <!-- Open Outline Button -->
          <button onclick="toggleOutlineWidget()" class="absolute right-6 bottom-6 bg-slate-900 dark:bg-white text-white dark:text-slate-900 p-3 rounded-full shadow-lg hover:scale-105 active:scale-95 transition-all flex items-center justify-center" title="Toggle Outline Table of Contents">
            <i class="fa-solid fa-list-ul text-sm"></i>
          </button>
        </div>

      </div>

    </div>

  </main>

  <!-- HELP & KEYBOARD SHORTCUTS DIALOG / MODAL -->
  <dialog id="helpModal" class="bg-transparent backdrop:bg-slate-950/70 p-4 max-w-2xl w-full">
    <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl shadow-2xl p-6 text-slate-800 dark:text-slate-200 overflow-hidden flex flex-col max-h-[85vh]">
      
      <div class="flex items-center justify-between border-b border-slate-100 dark:border-slate-800 pb-4">
        <div class="flex items-center gap-2">
          <i class="fa-solid fa-keyboard text-indigo-500 text-lg"></i>
          <h3 class="font-bold text-lg">Keyboard Shortcuts & Markdown Syntax</h3>
        </div>
        <button onclick="closeHelpModal()" class="text-slate-400 hover:text-rose-500 text-lg transition-colors">
          <i class="fa-solid fa-xmark"></i>
        </button>
      </div>

      <div class="flex-1 overflow-y-auto py-4 space-y-6 text-sm">
        
        <!-- Shortcuts -->
        <div>
          <h4 class="font-bold text-indigo-500 text-xs uppercase tracking-wider mb-2">IDE Keyboard Shortcuts</h4>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-2 text-xs">
            <div class="flex items-center justify-between bg-slate-50 dark:bg-slate-800 p-2 rounded">
              <span>Bold Text</span>
              <kbd class="px-1.5 py-0.5 bg-slate-200 dark:bg-slate-700 rounded font-mono font-bold">Ctrl + B</kbd>
            </div>
            <div class="flex items-center justify-between bg-slate-50 dark:bg-slate-800 p-2 rounded">
              <span>Italic Text</span>
              <kbd class="px-1.5 py-0.5 bg-slate-200 dark:bg-slate-700 rounded font-mono font-bold">Ctrl + I</kbd>
            </div>
            <div class="flex items-center justify-between bg-slate-50 dark:bg-slate-800 p-2 rounded">
              <span>Insert Hyperlink</span>
              <kbd class="px-1.5 py-0.5 bg-slate-200 dark:bg-slate-700 rounded font-mono font-bold">Ctrl + K</kbd>
            </div>
            <div class="flex items-center justify-between bg-slate-50 dark:bg-slate-800 p-2 rounded">
              <span>Open Find / Replace</span>
              <kbd class="px-1.5 py-0.5 bg-slate-200 dark:bg-slate-700 rounded font-mono font-bold">Ctrl + F</kbd>
            </div>
            <div class="flex items-center justify-between bg-slate-50 dark:bg-slate-800 p-2 rounded">
              <span>Switch Split Mode</span>
              <kbd class="px-1.5 py-0.5 bg-slate-200 dark:bg-slate-700 rounded font-mono font-bold">Alt + S</kbd>
            </div>
            <div class="flex items-center justify-between bg-slate-50 dark:bg-slate-800 p-2 rounded">
              <span>Indent text</span>
              <kbd class="px-1.5 py-0.5 bg-slate-200 dark:bg-slate-700 rounded font-mono font-bold">Tab</kbd>
            </div>
          </div>
        </div>

        <!-- Markdown Basics -->
        <div>
          <h4 class="font-bold text-pink-500 text-xs uppercase tracking-wider mb-2">Markdown Quick Reference</h4>
          <table class="w-full text-xs border border-slate-200 dark:border-slate-800 rounded-lg overflow-hidden">
            <thead>
              <tr class="bg-slate-50 dark:bg-slate-800">
                <th class="p-2 border-b border-slate-200 dark:border-slate-700">Style</th>
                <th class="p-2 border-b border-slate-200 dark:border-slate-700">Syntax Example</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td class="p-2 border-b border-slate-200 dark:border-slate-800 font-bold">Headings</td>
                <td class="p-2 border-b border-slate-200 dark:border-slate-800 font-mono"># H1, ## H2, ### H3</td>
              </tr>
              <tr>
                <td class="p-2 border-b border-slate-200 dark:border-slate-800 font-bold">Lists</td>
                <td class="p-2 border-b border-slate-200 dark:border-slate-800 font-mono">- bullet, 1. numbered, - [ ] checklist</td>
              </tr>
              <tr>
                <td class="p-2 border-b border-slate-200 dark:border-slate-800 font-bold">Blockquote</td>
                <td class="p-2 border-b border-slate-200 dark:border-slate-800 font-mono">> indented quote block</td>
              </tr>
              <tr>
                <td class="p-2 border-b border-slate-200 dark:border-slate-800 font-bold">Code Block</td>
                <td class="p-2 border-b border-slate-200 dark:border-slate-800 font-mono">```javascript \n console.log("Hi"); \n ```</td>
              </tr>
              <tr>
                <td class="p-2 border-b border-slate-200 dark:border-slate-800 font-bold">Custom Alerts</td>
                <td class="p-2 border-b border-slate-200 dark:border-slate-800 font-mono">[!NOTE] Note text or [!WARNING] Warning text</td>
              </tr>
            </tbody>
          </table>
        </div>

      </div>

      <div class="border-t border-slate-100 dark:border-slate-800 pt-4 flex justify-end">
        <button onclick="closeHelpModal()" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-700 text-white font-bold text-xs rounded-lg transition-colors">
          Got it!
        </button>
      </div>

    </div>
  </dialog>

  <!-- DIALOG: LINK INSERTER MODAL -->
  <dialog id="linkModal" class="bg-transparent backdrop:bg-slate-950/70 p-4 max-w-sm w-full">
    <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl shadow-2xl p-5 text-slate-800 dark:text-slate-200">
      <div class="flex items-center justify-between pb-3 border-b border-slate-100 dark:border-slate-800">
        <span class="font-bold flex items-center gap-1.5 text-indigo-500 text-sm">
          <i class="fa-solid fa-link"></i> Insert Link URL
        </span>
        <button onclick="closeModal('linkModal')" class="text-slate-400 hover:text-slate-600"><i class="fa-solid fa-xmark"></i></button>
      </div>
      <div class="py-3 space-y-3 text-xs">
        <div>
          <label class="block font-semibold text-slate-500 mb-1">Display Text</label>
          <input type="text" id="linkText" class="w-full bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-lg p-2 outline-none focus:ring-1 focus:ring-indigo-500" placeholder="e.g., GitHub Homepage">
        </div>
        <div>
          <label class="block font-semibold text-slate-500 mb-1">Target Web URL</label>
          <input type="text" id="linkUrl" class="w-full bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-lg p-2 outline-none focus:ring-1 focus:ring-indigo-500" placeholder="https://github.com">
        </div>
      </div>
      <div class="flex justify-end gap-2 pt-3 border-t border-slate-100 dark:border-slate-800">
        <button onclick="closeModal('linkModal')" class="px-3 py-1.5 bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 rounded-md font-semibold text-xs">Cancel</button>
        <button onclick="executeLinkInsert()" class="px-3 py-1.5 bg-indigo-600 hover:bg-indigo-700 text-white font-bold text-xs rounded-md">Insert</button>
      </div>
    </div>
  </dialog>

  <!-- DIALOG: IMAGE INSERTER MODAL -->
  <dialog id="imageModal" class="bg-transparent backdrop:bg-slate-950/70 p-4 max-w-sm w-full">
    <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl shadow-2xl p-5 text-slate-800 dark:text-slate-200">
      <div class="flex items-center justify-between pb-3 border-b border-slate-100 dark:border-slate-800">
        <span class="font-bold flex items-center gap-1.5 text-pink-500 text-sm">
          <i class="fa-regular fa-image"></i> Insert Image URL
        </span>
        <button onclick="closeModal('imageModal')" class="text-slate-400 hover:text-slate-600"><i class="fa-solid fa-xmark"></i></button>
      </div>
      <div class="py-3 space-y-3 text-xs">
        <div>
          <label class="block font-semibold text-slate-500 mb-1">Alt / Description Text</label>
          <input type="text" id="imageAlt" class="w-full bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-lg p-2 outline-none focus:ring-1 focus:ring-indigo-500" placeholder="e.g., Code Workspace Image">
        </div>
        <div>
          <label class="block font-semibold text-slate-500 mb-1">Image Source URL</label>
          <input type="text" id="imageUrl" class="w-full bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-lg p-2 outline-none focus:ring-1 focus:ring-indigo-500" placeholder="https://images.unsplash.com/photo-...">
        </div>
      </div>
      <div class="flex justify-end gap-2 pt-3 border-t border-slate-100 dark:border-slate-800">
        <button onclick="closeModal('imageModal')" class="px-3 py-1.5 bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 rounded-md font-semibold text-xs">Cancel</button>
        <button onclick="executeImageInsert()" class="px-3 py-1.5 bg-indigo-600 hover:bg-indigo-700 text-white font-bold text-xs rounded-md">Insert Image</button>
      </div>
    </div>
  </dialog>

  <!-- DIALOG: INTERACTIVE TABLE BUILDER MODAL -->
  <dialog id="tableModal" class="bg-transparent backdrop:bg-slate-950/70 p-4 max-w-sm w-full">
    <div class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl shadow-2xl p-5 text-slate-800 dark:text-slate-200">
      <div class="flex items-center justify-between pb-3 border-b border-slate-100 dark:border-slate-800">
        <span class="font-bold flex items-center gap-1.5 text-emerald-500 text-sm">
          <i class="fa-solid fa-table"></i> Build Markdown Table
        </span>
        <button onclick="closeModal('tableModal')" class="text-slate-400 hover:text-slate-600"><i class="fa-solid fa-xmark"></i></button>
      </div>
      <div class="py-3 space-y-4 text-xs">
        <div class="grid grid-cols-2 gap-3">
          <div>
            <label class="block font-semibold text-slate-500 mb-1">Rows Count</label>
            <input type="number" id="tableRows" class="w-full bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-lg p-2 outline-none focus:ring-1 focus:ring-indigo-500" value="3" min="1" max="15">
          </div>
          <div>
            <label class="block font-semibold text-slate-500 mb-1">Columns Count</label>
            <input type="number" id="tableCols" class="w-full bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-lg p-2 outline-none focus:ring-1 focus:ring-indigo-500" value="3" min="1" max="10">
          </div>
        </div>
        <div>
          <label class="block font-semibold text-slate-500 mb-1">Alignment Preference</label>
          <select id="tableAlign" class="w-full bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-lg p-2 outline-none focus:ring-1 focus:ring-indigo-500">
            <option value="left">Left-aligned (default)</option>
            <option value="center">Centered</option>
            <option value="right">Right-aligned</option>
          </select>
        </div>
      </div>
      <div class="flex justify-end gap-2 pt-3 border-t border-slate-100 dark:border-slate-800">
        <button onclick="closeModal('tableModal')" class="px-3 py-1.5 bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 rounded-md font-semibold text-xs">Cancel</button>
        <button onclick="executeTableInsert()" class="px-3 py-1.5 bg-indigo-600 hover:bg-indigo-700 text-white font-bold text-xs rounded-md">Generate Table</button>
      </div>
    </div>
  </dialog>

  <!-- FLOATING TOAST NOTIFICATION POPUP -->
  <div id="toast" class="fixed bottom-6 left-1/2 -translate-x-1/2 z-50 bg-slate-900/95 dark:bg-white/95 text-white dark:text-slate-950 px-5 py-3 rounded-xl shadow-2xl flex items-center gap-3 text-sm font-semibold border border-slate-800/20 dark:border-white/20 hidden max-w-md w-max">
    <i id="toastIcon" class="fa-solid fa-circle-check text-indigo-500 text-base"></i>
    <span id="toastMsg">Action completed successfully</span>
  </div>

  <!-- MARKED.JS MARKDOWN PARSER CDN -->
  <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>

  <!-- DOMPURIFY SANITIZER CDN -->
  <script src="https://cdn.jsdelivr.net/npm/dompurify@3.0.6/dist/purify.min.js"></script>

  <!-- PRISM SYNTAX HIGHLIGHTER -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js"></script>
  <!-- Prism Autoloader: will automatically fetch styling for languages like javascript, html, python, css, rust -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/plugins/autoloader/prism-autoloader.min.js"></script>

  <!-- MAIN SCRIPT IMPLEMENTATION -->
  <script>
    // Config details & State variables
    let currentFileName = "README.md";
    let isDark = true;
    let viewMode = 'split'; // split, editor, preview
    let findIndex = 0;
    let findMatches = [];

    // Markdown presets & Templates
    const TEMPLATES = {
      default: `# 🎓 Markdown Crash Course & Demo Document

Welcome to **MD-Studio**! This demo text highlights modern markdown syntax and custom components supported in our reader. 

Click **Export File** to download this content instantly as a \`.md\` file or edit this text directly!

---

## 🛠️ Essential Interactive Styling Shortcuts

1. **Bold Text**: Wrap elements inside double asterisks like **this bold term**.
2. *Italics*: Use single asterisks to show *emphasis text*.
3. ~~Strikethrough~~: Wrap text in double tildes for ~~completed tasks~~.
4. Inline Code: Single backticks help showcase \`const message = "Hello World"\` statements.

### Checklists & Bullet Lists
- [x] Create a gorgeous single-page markdown application
- [x] Configure live side-by-side split screen preview
- [ ] Share this amazing Markdown editor with development teammates
- [ ] Write documentation for the new API interface

---

## 💡 Rich Alert Callout Banners
Our parser converts bracket alert annotations into high-quality visual message blocks:

[!NOTE]
This is an information message block designed to guide users on secondary helpful content.

[!WARNING]
Be careful when executing direct terminal scripts in root workspace directories.

[!TIP]
You can double-click anywhere inside the live HTML preview to trigger instant print/PDF mode.

---

## 💻 Code Block Syntax Highlight Examples
Below is fully colored syntax for a custom Javascript class, auto-highlighted via PrismJS.

\`\`\`javascript
// High performance Fibonacci sequence algorithm
class FibonacciCalculator {
  constructor(limit = 10) {
    this.limit = limit;
  }

  generate() {
    let sequence = [0, 1];
    for (let i = 2; i < this.limit; i++) {
      sequence.push(sequence[i - 1] + sequence[i - 2]);
    }
    return sequence;
  }
}

const calculator = new FibonacciCalculator(8);
console.log("Fibonacci:", calculator.generate());
\`\`\`

---

## 📊 Beautiful Styled Data Tables

| Component Name | Feature Description | Developer Productivity Rank |
| :--- | :---: | :---: |
| Marked.js Renderer | Blazing fast live rendering engine | #1 |
| DOMPurify | Dynamic client-side HTML sanitation | #2 |
| Tailwind CSS | Utility-first beautiful styled shell | #3 |

---

## 🔗 Integrated Media Links

Here is an image reference loaded via standard markdown format:
![Workspace Landscape](https://images.unsplash.com/photo-1498050108023-c5249f4df085?auto=format&fit=crop&w=800&q=80)

Visit the [Official Markdown Guide website](https://www.markdownguide.org) to read advanced documentation.
`,

      readme: `# 📦 Project Title

A brief, high-impact description of what this repository or project achieves. Write 1-2 powerful introductory sentences here.

## 🚀 Key Highlights & Capabilities
* **Speedy Rendering**: Engineered on high-frequency asynchronous compilation.
* **Custom Themes**: Toggle dark, minimalist elegant, and cyberterminal colors instantly.
* **Highly Modular**: Plug-and-play architecture supports seamless schema extensions.

## ⚙️ Installation and Execution

Clone the workspace repository and trigger deployment commands:

\`\`\`bash
# Install node packages
npm install --save-dev markdown-studio-core

# Trigger developer preview server
npm run dev
\`\`\`

## 🛠️ Usage Example

Configure custom options directly inside the configuration startup file:

\`\`\`javascript
const studio = require('markdown-studio-core');

studio.init({
  hotReload: true,
  theme: 'dark-neon',
  autoSaveIntervalMs: 5000
});
\`\`\`

## 📄 License
Licensed under the [MIT License](LICENSE). Feel free to modify and scale.
`,

      api: `# 📡 REST API Reference

Endpoint index documentation for internal workspace communications. Base URL: \`https://api.v2.studio.io/v1\`

## 🔐 Authentication Scheme
All requests must pass standard Bearer JWT authorization keys inside HTTP headers:

\`\`\`http
Authorization: Bearer <YOUR_ACCESS_TOKEN_HERE>
\`\`\`

---

## 👤 Fetch User Account Information
Retrieves metadata context for a registered user account profile.

* **HTTP Method**: \`GET\`
* **Route Path**: \`/users/:userId\`

### Route Parameters
| Parameter | Type | Required? | Details |
| :--- | :---: | :---: | :--- |
| \`userId\` | String | **Yes** | Unique account hash code value |

### Response Body Format (\`application/json\`)
\`\`\`json
{
  "status": "success",
  "data": {
    "userId": "usr_9982x7",
    "name": "Jane Developer",
    "email": "jane@studio.io",
    "role": "Lead Architect",
    "active": true
  }
}
\`\`\`
`,

      notes: `# 📝 Status Meeting & Project Roadmap
**Date:** March 31, 2026  
**Host / Lead:** Product Management Office

---

## 🎯 High-Priority Agenda Issues
1. **Infrastructure Upgrade**: Move legacy microservices to unified cluster clusters.
2. **Payment Integrations**: Implement alternative developer billing schemes.
3. **Accessibility Audit**: Achieve comprehensive WCAG 2.2 AA compliant status.

## 👥 Attendees & Responsibility Matrix
* [x] **Sarah Watson** (Engineering Lead) - Microservice deployment architecture setup.
* [x] **David K.** (UX Design) - Color accessibility updates in core components.
* [ ] **Leila Moore** (QA Testing) - Automation test suite matrix compilation.

## 🚀 Key Roadmap Milestones
- **Q1 Finish Line**: Finalize backend service deprecations.
- **Q2 Kickoff**: Push new unified payment portal to beta environment.
`,

      resume: `# Jane Developer
**Senior Full-Stack Engineer** | New York, NY | jane.dev@gmail.com | +1 (555) 234-5678 | [GitHub](https://github.com)

---

## 🌟 Executive Summary
Passionate Full-Stack Developer with over 7 years of industrial experience designing, shipping, and maintaining distributed web applications. Expert in React.js, Node.js microservices, and database query optimization.

## 🛠️ Tech Stack & Skills
* **Languages**: JavaScript (ES6+), TypeScript, Rust, Go, SQL, Python
* **Frameworks**: Next.js, Express, TailwindCSS, Fastify, Spring Boot
* **Tools & Databases**: Docker, PostgreSQL, Redis, Kubernetes, AWS, Git

## 📈 Professional Experience

### **Senior Software Engineer** | Acme Systems Inc. (2023 - Present)
* Led a high-velocity agile squad of 6 developers in designing a new real-time content management editor.
* Redesigned backend cache strategies using Redis, improving dashboard response speeds by **35%**.
* Mentored 3 junior developers and established test coverage guidelines (+90%).

### **Software Developer** | Delta Labs (2020 - 2023)
* Designed secure user registration modules supporting OAuth2 and 2FA features.
* Built and documented 20+ robust REST API endpoints used daily by over 100k active users.
`,

      blank: `# New Document
Start drafting your markdown document here...
`
    };

    // System Startup Execution
    window.addEventListener('DOMContentLoaded', () => {
      initTheme();
      initMarkdownParser();
      loadSavedFilesList();
      
      // Load current or default document
      const lastActiveFile = localStorage.getItem('md_studio_last_active') || 'README.md';
      currentFileName = lastActiveFile;
      document.getElementById('fileNameInput').value = currentFileName;
      
      const savedContent = localStorage.getItem('md_file_' + currentFileName);
      if (savedContent !== null) {
        document.getElementById('markdownEditor').value = savedContent;
      } else {
        document.getElementById('markdownEditor').value = TEMPLATES.default;
      }

      handleInput();
      setupDropdowns();
      setupTabIndentation();
      setupScrollSync();
      setupAutoBracketClosing();

      // Keyboard shortcuts setup
      window.addEventListener('keydown', handleGlobalShortcuts);
    });

    // Toggle Sidebar visibility
    function toggleSidebar() {
      const sidebar = document.getElementById('fileSidebar');
      const toggleBtn = document.getElementById('sidebarToggleBtn');
      if (sidebar.classList.contains('md:w-64')) {
        sidebar.classList.remove('md:w-64', 'w-full');
        sidebar.classList.add('w-0', 'overflow-hidden', 'border-r-0');
        toggleBtn.classList.add('bg-indigo-50', 'text-indigo-600', 'dark:bg-slate-800');
      } else {
        sidebar.classList.remove('w-0', 'overflow-hidden', 'border-r-0');
        sidebar.classList.add('md:w-64', 'w-full');
        toggleBtn.classList.remove('bg-indigo-50', 'text-indigo-600', 'dark:bg-slate-800');
      }
    }

    // Toggle Find/Search Panel drawer
    function toggleFindPanel() {
      const panel = document.getElementById('findPanel');
      panel.classList.toggle('hidden');
      if (!panel.classList.contains('hidden')) {
        document.getElementById('findInput').focus();
      }
    }

    // Manage dropdown state clicks
    function setupDropdowns() {
      const dropDowns = [
        { btn: 'templateBtn', menu: 'templateMenu' },
        { btn: 'exportBtn', menu: 'exportMenu' }
      ];

      dropDowns.forEach(drop => {
        const btn = document.getElementById(drop.btn);
        const menu = document.getElementById(drop.menu);
        
        btn.addEventListener('click', (e) => {
          e.stopPropagation();
          // Hide others
          dropDowns.forEach(other => {
            if (other.menu !== drop.menu) {
              document.getElementById(other.menu).classList.add('hidden');
            }
          });
          menu.classList.toggle('hidden');
        });
      });

      // Close dropdowns on clicking outside
      document.addEventListener('click', () => {
        dropDowns.forEach(drop => {
          document.getElementById(drop.menu).classList.add('hidden');
        });
      });
    }

    // Setup Indentation behavior (Tab and Shift+Tab support inside editor)
    function setupTabIndentation() {
      const editor = document.getElementById('markdownEditor');
      editor.addEventListener('keydown', function(e) {
        if (e.key === 'Tab') {
          e.preventDefault();
          const start = this.selectionStart;
          const end = this.selectionEnd;
          const value = this.value;

          if (e.shiftKey) {
            // Dedent: remove 2 spaces if possible
            if (value.substring(start - 2, start) === "  ") {
              this.value = value.substring(0, start - 2) + value.substring(start);
              this.selectionStart = this.selectionEnd = start - 2;
            }
          } else {
            // Indent: insert 2 spaces
            this.value = value.substring(0, start) + "  " + value.substring(end);
            this.selectionStart = this.selectionEnd = start + 2;
          }
          handleInput();
        }
      });
    }

    // Auto-bracket pair generator matching
    function setupAutoBracketClosing() {
      const editor = document.getElementById('markdownEditor');
      const bracketsMap = {
        '(': ')',
        '[': ']',
        '{': '}',
        '"': '"',
        "'": "'",
        '`': '`',
        '*': '*'
      };

      editor.addEventListener('keydown', function(e) {
        if (bracketsMap[e.key] !== undefined) {
          const start = this.selectionStart;
          const end = this.selectionEnd;
          const text = this.value;
          const selectedText = text.substring(start, end);

          // If text is selected, wrap it
          if (start !== end) {
            e.preventDefault();
            const closing = bracketsMap[e.key];
            this.value = text.substring(0, start) + e.key + selectedText + closing + text.substring(end);
            this.selectionStart = start + 1;
            this.selectionEnd = end + 1;
            handleInput();
          } else {
            // Auto close brackets at cursor position, unless character ahead is already the same
            // (e.g. typing closing quote over preview quote)
            if (e.key === '`') {
              // Custom logic if backtick
              e.preventDefault();
              this.value = text.substring(0, start) + '``' + text.substring(start);
              this.selectionStart = this.selectionEnd = start + 1;
              handleInput();
            }
          }
        }
      });
    }

    // Proportional scrolling between editor and preview
    function setupScrollSync() {
      const editorPane = document.getElementById('markdownEditor');
      const previewPane = document.getElementById('renderedPreview');
      
      let isEditorScrolling = false;
      let isPreviewScrolling = false;

      editorPane.addEventListener('scroll', () => {
        if (isPreviewScrolling) return;
        isEditorScrolling = true;
        
        const editorScrollPct = editorPane.scrollTop / (editorPane.scrollHeight - editorPane.clientHeight);
        previewPane.scrollTop = editorScrollPct * (previewPane.scrollHeight - previewPane.clientHeight);
        
        setTimeout(() => { isEditorScrolling = false; }, 50);
      });

      previewPane.addEventListener('scroll', () => {
        if (isEditorScrolling) return;
        isPreviewScrolling = true;
        
        const previewScrollPct = previewPane.scrollTop / (previewPane.scrollHeight - previewPane.clientHeight);
        editorPane.scrollTop = previewScrollPct * (editorPane.scrollHeight - editorPane.clientHeight);
        
        setTimeout(() => { isPreviewScrolling = false; }, 50);
      });
    }

    // Parse and render raw text into beautiful elements
    function initMarkdownParser() {
      // Configure Marked Renderer with custom visual extension
      const renderer = new marked.Renderer();
      
      // Support GFM alerts inside blockquotes
      renderer.blockquote = function(quote) {
        // Safe check for string parameter or newer AST token structure
        let text = typeof quote === 'object' ? (quote.text || quote.raw || '') : quote;
        
        // Clean out paragraph tags if injected by standard parsing
        const cleanText = text.replace(/<\/?p>/gi, '').trim();

        if (cleanText.includes('[!NOTE]') || cleanText.includes('[!WARNING]') || cleanText.includes('[!TIP]')) {
          let alertClass = "markdown-alert-note";
          let alertTitle = "NOTE";
          let alertIcon = "fa-solid fa-circle-info";
          let content = cleanText;

          if (cleanText.includes('[!NOTE]')) {
            alertClass = "markdown-alert-note";
            alertTitle = "NOTE";
            alertIcon = "fa-solid fa-circle-info";
            content = cleanText.replace(/\[!NOTE\]/g, '').trim();
          } else if (cleanText.includes('[!WARNING]')) {
            alertClass = "markdown-alert-warning";
            alertTitle = "WARNING";
            alertIcon = "fa-solid fa-triangle-exclamation";
            content = cleanText.replace(/\[!WARNING\]/g, '').trim();
          } else if (cleanText.includes('[!TIP]')) {
            alertClass = "markdown-alert-tip";
            alertTitle = "TIP";
            alertIcon = "fa-solid fa-lightbulb";
            content = cleanText.replace(/\[!TIP\]/g, '').trim();
          }

          // Strip any leading HTML linebreaks remaining inside the content
          content = content.replace(/^<br\s*\/?>/i, '').trim();

          return `<div class="markdown-alert ${alertClass} my-4">
            <div class="flex items-center gap-1.5 font-bold text-xs uppercase tracking-wide mb-1">
              <i class="${alertIcon}"></i> ${alertTitle}
            </div>
            <div class="text-sm leading-relaxed">${content}</div>
          </div>`;
        }
        
        // Wrap normal blockquote output
        return `<blockquote class="italic text-slate-500 border-l-4 border-slate-300 dark:border-slate-700 pl-4 py-1 my-3 bg-slate-50 dark:bg-slate-900/50">${text}</blockquote>`;
      };

      marked.setOptions({
        renderer: renderer,
        gfm: true,
        breaks: true,
        headerIds: true,
        mangle: false,
        sanitize: false // handled afterward via safe DOMPurify
      });
    }

    // Realtime input updates & Calculations
    function handleInput() {
      const editorValue = document.getElementById('markdownEditor').value;
      const previewDiv = document.getElementById('renderedPreview');
      
      // Synchronize input file name changes
      const nameInput = document.getElementById('fileNameInput');
      currentFileName = nameInput.value.trim() || "untitled.md";

      // Parse markdown asynchronously to preserve UX speeds
      const rawHTML = marked.parse(editorValue);
      const cleanHTML = DOMPurify.sanitize(rawHTML);
      previewDiv.innerHTML = cleanHTML;

      // Force Code block highlight refresh via Prism
      Prism.highlightAllUnder(previewDiv);

      // Perform word counts
      updateStatistics(editorValue);

      // Save file state immediately in localStorage
      localStorage.setItem('md_file_' + currentFileName, editorValue);
      localStorage.setItem('md_studio_last_active', currentFileName);

      // Dynamic table of contents builder
      generateTableOfContents();
    }

    // Auto-save tracker status bar update
    function updateStatistics(text) {
      const wordCountSpan = document.getElementById('statsWords');
      const charCountSpan = document.getElementById('statsChars');
      const readingTimeSpan = document.getElementById('statsReadingTime');

      const trimmed = text.trim();
      const words = trimmed === "" ? 0 : trimmed.split(/\s+/).length;
      const chars = text.length;
      
      // Standard reading benchmark: ~200 Words per Minute
      const readingTimeMinutes = Math.max(1, Math.ceil(words / 200));

      wordCountSpan.innerText = words.toLocaleString();
      charCountSpan.innerText = chars.toLocaleString();
      readingTimeSpan.innerText = `${readingTimeMinutes} min`;
    }

    // Generate Outline Widget structure dynamically
    function generateTableOfContents() {
      const outlineList = document.getElementById('outlineList');
      const previewDiv = document.getElementById('renderedPreview');
      const headings = previewDiv.querySelectorAll('h1, h2, h3');
      
      outlineList.innerHTML = '';
      
      if (headings.length === 0) {
        outlineList.innerHTML = `<span class="text-[10px] text-slate-400 font-medium italic block text-center py-4">No headings found</span>`;
        return;
      }

      headings.forEach((heading, idx) => {
        // Create matching ID anchor tags if not exist
        const id = 'heading_' + idx;
        heading.setAttribute('id', id);

        const a = document.createElement('button');
        a.className = "w-full text-left truncate text-slate-600 dark:text-slate-400 hover:text-indigo-500 font-medium text-xs block py-1 hover:underline transition-all";
        
        // Indentation depending on hierarchy
        let indent = "";
        if (heading.tagName === 'H2') indent = "• ";
        if (heading.tagName === 'H3') indent = "  - ";
        
        a.innerText = indent + heading.textContent;
        a.onclick = () => {
          heading.scrollIntoView({ behavior: 'smooth' });
        };

        outlineList.appendChild(a);
      });
    }

    function toggleOutlineWidget() {
      const widget = document.getElementById('outlineWidget');
      widget.classList.toggle('hidden');
      widget.classList.toggle('flex');
    }

    // Modify selected layout mode display structures
    function setViewMode(mode) {
      viewMode = mode;
      const editorPane = document.getElementById('editorPane');
      const previewPane = document.getElementById('previewPane');
      
      const btnEditor = document.getElementById('viewBtnEditor');
      const btnSplit = document.getElementById('viewBtnSplit');
      const btnPreview = document.getElementById('viewBtnPreview');

      // Reset styles
      [btnEditor, btnSplit, btnPreview].forEach(btn => {
        btn.className = "px-2.5 py-1 text-xs font-semibold rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition-all flex items-center gap-1.5";
      });

      const activeStyle = "px-2.5 py-1 text-xs font-semibold rounded-md text-slate-800 dark:text-white bg-white dark:bg-slate-700 shadow-sm transition-all flex items-center gap-1.5";

      if (mode === 'editor') {
        editorPane.classList.remove('hidden');
        previewPane.classList.add('hidden');
        btnEditor.className = activeStyle;
      } else if (mode === 'preview') {
        editorPane.classList.add('hidden');
        previewPane.classList.remove('hidden');
        btnPreview.className = activeStyle;
      } else {
        editorPane.classList.remove('hidden');
        previewPane.classList.remove('hidden');
        btnSplit.className = activeStyle;
      }
    }

    // Document Preview Visual CSS Themes
    function changePreviewTheme(themeName) {
      const previewDiv = document.getElementById('renderedPreview');
      // Clear theme classes
      previewDiv.classList.remove('theme-github', 'theme-editorial', 'theme-terminal', 'theme-indigo');
      
      // Inject selected
      previewDiv.classList.add('theme-' + themeName);
      showToast(`Switched preview styling to ${themeName}`, "fa-solid fa-paintbrush");
    }

    // Setup dark theme application variables
    function initTheme() {
      isDark = localStorage.getItem('md_studio_dark') !== 'false';
      applyThemeStyles();
    }

    function toggleTheme() {
      isDark = !isDark;
      localStorage.setItem('md_studio_dark', isDark);
      applyThemeStyles();
    }

    function applyThemeStyles() {
      const icon = document.getElementById('themeToggleIcon');
      if (isDark) {
        document.documentElement.classList.add('dark');
        icon.className = "fa-solid fa-sun text-sm text-amber-400";
      } else {
        document.documentElement.classList.remove('dark');
        icon.className = "fa-solid fa-moon text-sm text-indigo-500";
      }
    }

    // Modal view handlers
    function openHelpModal() {
      document.getElementById('helpModal').showModal();
    }
    function closeHelpModal() {
      document.getElementById('helpModal').close();
    }

    function openLinkModal() {
      document.getElementById('linkText').value = getSelectedText() || '';
      document.getElementById('linkModal').showModal();
    }
    function openImageModal() {
      document.getElementById('imageAlt').value = getSelectedText() || '';
      document.getElementById('imageModal').showModal();
    }
    function openTableModal() {
      document.getElementById('tableModal').showModal();
    }
    function closeModal(id) {
      document.getElementById(id).close();
    }

    // Helper text selection fetcher
    function getSelectedText() {
      const editor = document.getElementById('markdownEditor');
      return editor.value.substring(editor.selectionStart, editor.selectionEnd);
    }

    // Core insertion utility functions
    function insertAtCursor(before, after) {
      const editor = document.getElementById('markdownEditor');
      const start = editor.selectionStart;
      const end = editor.selectionEnd;
      const text = editor.value;

      const selected = text.substring(start, end);
      const replacement = before + selected + after;

      editor.value = text.substring(0, start) + replacement + text.substring(end);
      editor.focus();
      
      // Reposition cursor
      editor.selectionStart = start + before.length;
      editor.selectionEnd = start + before.length + selected.length;
      
      handleInput();
    }

    function insertHeading(prefix) {
      const editor = document.getElementById('markdownEditor');
      const start = editor.selectionStart;
      const text = editor.value;

      // Find beginning of active line
      let lineStart = text.lastIndexOf('\n', start - 1) + 1;
      
      editor.value = text.substring(0, lineStart) + prefix + text.substring(lineStart);
      editor.focus();
      handleInput();
    }

    function insertAlert(type) {
      const header = `[!${type.toUpperCase()}]\n`;
      insertAtCursor(`\n${header}`, '\n');
    }

    // Modal form processors
    function executeLinkInsert() {
      const text = document.getElementById('linkText').value || 'Link text';
      const url = document.getElementById('linkUrl').value || 'https://';
      
      insertAtCursor(`[${text}](${url})`, '');
      closeModal('linkModal');
      showToast("Link address inserted");
    }

    function executeImageInsert() {
      const alt = document.getElementById('imageAlt').value || 'Workspace Landscape';
      const url = document.getElementById('imageUrl').value || 'https://images.unsplash.com/photo-1498050108023-c5249f4df085';
      
      insertAtCursor(`![${alt}](${url})`, '');
      closeModal('imageModal');
      showToast("Image asset source link inserted");
    }

    function executeTableInsert() {
      const rows = parseInt(document.getElementById('tableRows').value) || 3;
      const cols = parseInt(document.getElementById('tableCols').value) || 3;
      const align = document.getElementById('tableAlign').value;

      let tableMd = "\n";
      
      // Header row
      for (let c = 1; c <= cols; c++) {
        tableMd += `| Header ${c} `;
      }
      tableMd += "|\n";

      // Alignment separator row
      for (let c = 1; c <= cols; c++) {
        if (align === 'center') {
          tableMd += "| :---: ";
        } else if (align === 'right') {
          tableMd += "| ---: ";
        } else {
          tableMd += "| :--- ";
        }
      }
      tableMd += "|\n";

      // Data Rows
      for (let r = 1; r <= rows; r++) {
        for (let c = 1; c <= cols; c++) {
          tableMd += `| Cell ${r}-${c} `;
        }
        tableMd += "|\n";
      }

      insertAtCursor(tableMd, "");
      closeModal('tableModal');
      showToast(`Generated custom ${rows}x${cols} data table structure`);
    }

    // FILE SYSTEM & SAVED HISTORY MANAGER
    function loadSavedFilesList() {
      const container = document.getElementById('fileListContainer');
      container.innerHTML = '';
      
      // Look up files in localStorage
      let hasFiles = false;
      for (let i = 0; i < localStorage.length; i++) {
        const key = localStorage.key(i);
        if (key.startsWith('md_file_')) {
          hasFiles = true;
          const fileName = key.replace('md_file_', '');
          
          const div = document.createElement('div');
          div.className = `group flex items-center justify-between p-2 rounded-lg text-xs font-semibold cursor-pointer transition-all ${fileName === currentFileName ? 'bg-indigo-50 text-indigo-700 dark:bg-indigo-950/40 dark:text-indigo-300' : 'text-slate-600 dark:text-slate-400 hover:bg-slate-100 dark:hover:bg-slate-800/50'}`;
          
          // Switch click handler
          div.onclick = (e) => {
            // Prevent trigger if clicking delete
            if (e.target.closest('.delete-btn')) return;
            switchActiveFile(fileName);
          };

          div.innerHTML = `
            <div class="flex items-center gap-2 truncate">
              <i class="fa-regular fa-file-lines text-slate-400"></i>
              <span class="truncate" title="${fileName}">${fileName}</span>
            </div>
            <button class="delete-btn opacity-0 group-hover:opacity-100 p-1 text-slate-400 hover:text-rose-500 rounded transition-all" onclick="deleteSavedFile('${fileName}')" title="Delete Saved File Draft">
              <i class="fa-regular fa-trash-can text-[10px]"></i>
            </button>
          `;
          container.appendChild(div);
        }
      }

      if (!hasFiles) {
        container.innerHTML = `
          <div class="text-center py-6 text-slate-400 dark:text-slate-500 text-xs italic">
            No custom file templates saved.
          </div>
        `;
      }
    }

    function switchActiveFile(fileName) {
      // Save current file text first
      const curVal = document.getElementById('markdownEditor').value;
      localStorage.setItem('md_file_' + currentFileName, curVal);

      currentFileName = fileName;
      document.getElementById('fileNameInput').value = fileName;

      const loadedContent = localStorage.getItem('md_file_' + fileName) || '';
      document.getElementById('markdownEditor').value = loadedContent;
      
      handleInput();
      loadSavedFilesList();
      showToast(`Opened active file ${fileName}`, "fa-regular fa-folder-open");
    }

    function createNewFile() {
      // Prompt user or autogenerate unique name
      const defaultName = `document_${Date.now().toString().slice(-4)}.md`;
      const fileName = prompt("Enter a filename for your new Markdown file:", defaultName);
      
      if (!fileName) return;

      const sanitizedName = fileName.endsWith('.md') ? fileName : fileName + '.md';
      
      // Save empty baseline draft
      localStorage.setItem('md_file_' + sanitizedName, `# ${sanitizedName.replace('.md','')}\n\nStart your markdown drafts...`);
      switchActiveFile(sanitizedName);
    }

    function deleteSavedFile(fileName) {
      if (fileName === "README.md") {
        alert("The primary template README.md file is protected and cannot be deleted.");
        return;
      }
      
      if (confirm(`Are you sure you want to permanently delete draft file "${fileName}"?`)) {
        localStorage.removeItem('md_file_' + fileName);
        if (currentFileName === fileName) {
          switchActiveFile('README.md');
        } else {
          loadSavedFilesList();
        }
        showToast(`Deleted "${fileName}" draft`, "fa-solid fa-trash-can");
      }
    }

    // Dynamic Template selector loader
    function loadTemplate(key) {
      if (TEMPLATES[key] !== undefined) {
        if (confirm("This action will overwrite your active editor field. Do you wish to continue?")) {
          document.getElementById('markdownEditor').value = TEMPLATES[key];
          
          // Generate customized file names
          if (key !== 'default' && key !== 'blank') {
            document.getElementById('fileNameInput').value = `${key.toUpperCase()}-Sample.md`;
          } else if (key === 'default') {
            document.getElementById('fileNameInput').value = "README.md";
          }
          
          handleInput();
          showToast(`Injected fresh "${key}" preset template layout`);
        }
      }
    }

    // EXPORT DOWNLOAD SUITE HANDLERS
    function downloadAsMarkdown() {
      const text = document.getElementById('markdownEditor').value;
      const blob = new Blob([text], { type: 'text/markdown;charset=utf-8' });
      triggerFileDownload(blob, currentFileName);
      showToast(`Exported "${currentFileName}" successfully`, "fa-solid fa-file-arrow-down");
    }

    function downloadAsHTML() {
      const markdownHTML = document.getElementById('renderedPreview').innerHTML;
      const previewTheme = document.getElementById('previewThemeSelector').value;
      
      // Formulate complete standalone static beautiful responsive HTML file page
      const fullStaticHTML = `<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>${currentFileName} | MD-Studio Export Output</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/prism-tomorrow.min.css">
  <style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif; background-color: #f8fafc; }
    .export-container { max-width: 900px; margin: 3rem auto; padding: 2.5rem; background: #ffffff; border-radius: 12px; box-shadow: 0 4px 12px rgba(15,23,42,0.05); border: 1px solid #e2e8f0; }
  </style>
</head>
<body>
  <div class="export-container">
    <div style="margin-bottom: 2rem; border-bottom: 2px solid #e2e8f0; padding-bottom: 1rem;">
      <span style="font-size: 11px; font-weight: bold; color: #4f46e5; text-transform: uppercase;">Exported Markdown Document</span>
      <h1 style="font-size: 20px; font-weight: 800; color: #0f172a; margin-top: 2px;">${currentFileName}</h1>
    </div>
    <div class="rendered-html">
      ${markdownHTML}
    </div>
  </div>
</body>
</html>`;

      const blob = new Blob([fullStaticHTML], { type: 'text/html;charset=utf-8' });
      const htmlFilename = currentFileName.replace('.md', '') + '.html';
      triggerFileDownload(blob, htmlFilename);
      showToast(`Exported converted index page "${htmlFilename}" successfully`);
    }

    function triggerFileDownload(blob, filename) {
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = filename;
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(a.href);
    }

    // Dynamic browser trigger print helper
    function printPreview() {
      window.print();
    }

    // Clipboard handlers
    function copyMarkdownText() {
      const val = document.getElementById('markdownEditor').value;
      navigator.clipboard.writeText(val).then(() => {
        showToast("Markdown source code copied to clipboard");
      });
    }

    function copyHTMLText() {
      const htmlVal = document.getElementById('renderedPreview').innerHTML;
      navigator.clipboard.writeText(htmlVal).then(() => {
        showToast("Rendered HTML copied to clipboard", "fa-regular fa-file-code");
      });
    }

    // Import and parse local files
    const fileInput = document.getElementById('importFile');
    fileInput.addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(evt) {
        const text = evt.target.result;
        
        // Switch context
        document.getElementById('fileNameInput').value = file.name;
        document.getElementById('markdownEditor').value = text;
        
        handleInput();
        loadSavedFilesList();
        showToast(`Imported external file "${file.name}"`, "fa-solid fa-file-import");
      };
      reader.readAsText(file);
    });

    // Custom Interactive Toast alerts
    function showToast(msg, iconClass = "fa-solid fa-circle-check") {
      const toast = document.getElementById('toast');
      const toastIcon = document.getElementById('toastIcon');
      const toastMsg = document.getElementById('toastMsg');

      toastIcon.className = `${iconClass} text-indigo-500`;
      toastMsg.innerText = msg;

      toast.classList.remove('hidden');
      toast.classList.add('flex', 'toast-animate');

      // Clear after 3.2 seconds
      setTimeout(() => {
        toast.classList.add('hidden');
        toast.classList.remove('flex', 'toast-animate');
      }, 3200);
    }

    // SEARCH & REPLACE ENGINE
    function findNextOccurrence() {
      const editor = document.getElementById('markdownEditor');
      const query = document.getElementById('findInput').value;
      const statusSpan = document.getElementById('findStatus');
      
      if (!query) {
        statusSpan.innerText = "No query string";
        return;
      }

      const text = editor.value;
      
      // Rebuild matching index structures if query changes or matches are empty
      if (findMatches.length === 0 || findMatches[0].query !== query) {
        findMatches = [];
        let index = 0;
        while ((index = text.toLowerCase().indexOf(query.toLowerCase(), index)) !== -1) {
          findMatches.push({ index, query });
          index += query.length;
        }
        findIndex = 0;
      }

      if (findMatches.length === 0) {
        statusSpan.innerText = "0 matches found";
        return;
      }

      // Highlight active match inside textarea editor selection
      const activeMatch = findMatches[findIndex];
      editor.focus();
      editor.selectionStart = activeMatch.index;
      editor.selectionEnd = activeMatch.index + query.length;

      // Scroll into view if needed
      const scrollHeight = editor.scrollHeight;
      const progress = activeMatch.index / text.length;
      editor.scrollTop = scrollHeight * progress - 100;

      statusSpan.innerText = `${findIndex + 1} of ${findMatches.length}`;
      
      // Step loop index forward
      findIndex = (findIndex + 1) % findMatches.length;
    }

    function replaceSingle() {
      const editor = document.getElementById('markdownEditor');
      const query = document.getElementById('findInput').value;
      const replacer = document.getElementById('replaceInput').value;

      if (!query) return;

      const start = editor.selectionStart;
      const end = editor.selectionEnd;
      const text = editor.value;

      // Check if current selection matches query
      const selected = text.substring(start, end);
      if (selected.toLowerCase() === query.toLowerCase()) {
        editor.value = text.substring(0, start) + replacer + text.substring(end);
        editor.selectionStart = editor.selectionEnd = start + replacer.length;
        
        // Force state reset
        findMatches = [];
        handleInput();
        showToast("Replaced single occurrence");
      } else {
        // Move to next search to highlight first
        findNextOccurrence();
      }
    }

    function replaceAllOccurrences() {
      const editor = document.getElementById('markdownEditor');
      const query = document.getElementById('findInput').value;
      const replacer = document.getElementById('replaceInput').value;

      if (!query) return;

      const text = editor.value;
      const regex = new RegExp(query.replace(/[-\/\\^$*+?.()|[\]{}]/g, '\\$&'), 'gi');
      const count = (text.match(regex) || []).length;

      if (count > 0) {
        editor.value = text.replace(regex, replacer);
        findMatches = [];
        handleInput();
        showToast(`Successfully replaced all ${count} occurrences!`);
      } else {
        showToast("No matching entries found to replace", "fa-solid fa-circle-exclamation");
      }
    }

    // Keyboard global listener mappings
    function handleGlobalShortcuts(e) {
      // Ctrl+B: Bold
      if (e.ctrlKey && e.key === 'b') {
        e.preventDefault();
        insertAtCursor('**', '**');
      }
      // Ctrl+I: Italic
      if (e.ctrlKey && e.key === 'i') {
        e.preventDefault();
        insertAtCursor('*', '*');
      }
      // Ctrl+K: Hyperlink
      if (e.ctrlKey && e.key === 'k') {
        e.preventDefault();
        openLinkModal();
      }
      // Ctrl+F: Open search
      if (e.ctrlKey && e.key === 'f') {
        e.preventDefault();
        toggleFindPanel();
      }
      // Alt+S: Cycle layout Split screen modes
      if (e.altKey && e.key === 's') {
        e.preventDefault();
        if (viewMode === 'split') {
          setViewMode('editor');
        } else if (viewMode === 'editor') {
          setViewMode('preview');
        } else {
          setViewMode('split');
        }
      }
    }
  </script>
</body>
</html>
