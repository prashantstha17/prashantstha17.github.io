+++
template = "homepage.html"
+++

<style>
  .minimal-home {
    max-width: 600px;
    margin: 3rem auto;
    padding: 0 1rem;
    line-height: 1.65;
  }
  .minimal-home h1 {
    font-size: 2rem;
    font-weight: 600;
    margin-bottom: 1.5rem;
    letter-spacing: -0.02em;
  }
  
  /* Avatar Styling */
  .chill-avatar {
    width: 140px;
    height: 140px;
    object-fit: cover;
    border-radius: 16px;
    margin-bottom: 1.5rem;
    display: block;
    transition: transform 0.3s ease;
  }
  .chill-avatar:hover {
    transform: scale(1.03) rotate(-2deg);
  }

  /* ===== THEME VARIABLES ===== */
  /* Default (Dark Mode) */
  :root {
    --code-bg: #1e1e2e;
    --code-header-bg: #313244;
    --code-border: #45475a;
    --code-text: #cdd6f4;
    --code-filename: #b4befe;
    
    /* Syntax Colors (Dark) */
    --c-kw: #cba6f7;   /* struct */
    --c-type: #f9e2af; /* YetUndefined */
    --c-field: #89b4fa;/* role, focus, mood */
    --c-str: #a6e3a1;  /* "..." */
    --c-punct: #6c7086;/* {, }, :, ; */
  }

  /* Light Mode Overrides (Catches all Apollo theme triggers) */
  html[data-theme="light"],
  body[data-theme="light"],
  html.light,
  body.light,
  .light-theme,
  [data-theme="light"] {
    --code-bg: #eff1f5;
    --code-header-bg: #e6e9ef;
    --code-border: #ccd0da;
    --code-text: #4c4f69;
    --code-filename: #1e66f5;
    
    /* Syntax Colors (Light) */
    --c-kw: #8839ef;
    --c-type: #df8e1d;
    --c-field: #1e66f5;
    --c-str: #40a02b;
    --c-punct: #9ca0b0;
  }

  /* ===== CODE EDITOR STYLES ===== */
  .code-window {
    background: var(--code-bg);
    border: 1px solid var(--code-border);
    border-radius: 10px;
    overflow: hidden;
    margin-top: 1rem;
    font-family: 'Courier New', Courier, monospace;
    font-size: 0.9em;
    transition: background 0.3s ease, border-color 0.3s ease;
  }
  .code-header {
    background: var(--code-header-bg);
    padding: 8px 12px;
    display: flex;
    align-items: center;
    gap: 6px;
    border-bottom: 1px solid var(--code-border);
    transition: background 0.3s ease, border-color 0.3s ease;
  }
  .dot { width: 10px; height: 10px; border-radius: 50%; }
  .dot.red { background: #ff5f56; }
  .dot.yellow { background: #ffbd2e; }
  .dot.green { background: #27c93f; }
  .filename { 
    margin-left: auto; 
    font-size: 0.85em; 
    color: var(--code-filename); 
  }
  .code-body {
    padding: 16px;
    line-height: 1.6;
    color: var(--code-text);
    margin: 0;
    white-space: pre;
  }

  /* Apply variables to syntax classes */
  .kw { color: var(--c-kw); }
  .type { color: var(--c-type); }
  .field { color: var(--c-field); }
  .str { color: var(--c-str); }
  .punct { color: var(--c-punct); }
</style>

<div class="minimal-home">
  <h1>YetUndefined</h1>

  <!-- Chill Guy Image -->
  <img 
    src="/chill.png" 
    alt="Chill Guy" 
    class="chill-avatar" 
  />

  <!-- Code Editor Bio -->
  <div class="code-window">
    <div class="code-header">
      <div class="dot red"></div>
      <div class="dot yellow"></div>
      <div class="dot green"></div>
      <span class="filename">yet_undefined.c</span>
    </div>
    <pre class="code-body"><code><span class="kw">struct</span> <span class="type">YetUndefined</span> <span class="punct">{</span>
    <span class="field">role</span><span class="punct">:</span> <span class="str">"CTF Player"</span><span class="punct">,</span>
    <span class="field">focus</span><span class="punct">:</span> <span class="str">"Low-level &amp; Security"</span><span class="punct">,</span>
    <span class="field">mood</span><span class="punct">:</span> <span class="str">"Chill"</span>
<span class="punct">};</span></code></pre>
  </div>
</div>