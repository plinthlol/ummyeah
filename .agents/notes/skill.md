# Note template

Full source of `note-template.html` — copy this whole file to start a new note. Edit the `<h1>`, the Contents links, and the content inside `.page`. Leave the theme variables, the jump-return script, and the KaTeX setup as-is.

Quick reference while editing:
- Theme comes from `index.html` (forced into the note's iframe) — no toggle here.
- `\(...\)` inline math, `\[...\]` display math — rendered by KaTeX automatically.
- `href="#some-id"` links trigger the jump-and-return arrow; `href="../Folder/file.html"` links to another note and opens in a new tab.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Example Note</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:ital,wght@0,100..800;1,100..800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/katex.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/katex.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/contrib/auto-render.min.js"></script>
<script>
  // Applied before first paint to avoid a flash of dark mode.
  // Defaults to light mode until index.html has switched to dark.
  // Reads the SAME localStorage key as index.html, so a note opened
  // standalone (same origin) already matches whatever the app was set to.
  // When opened *inside* the app's iframe, index.html overrides this
  // live — this is only the fallback for opening the file directly.
  (function(){
    try{
      var saved = localStorage.getItem('academis-theme');
      if(saved === 'dark') document.documentElement.setAttribute('data-theme', 'dark');
    } catch(e){ /* localStorage unavailable — default to light */ }
  })();
</script>
<style>
  :root{
    --bg:#F6F8FA;
    --surface:#FFFFFF;
    --surface-hover:#EEF2F6;
    --border:#D9E0E7;
    --border-soft:#E7EBEF;
    --ink:#17212B;
    --ink-dim:#66737F;
    --ink-faint:#9AA5AF;
    --folder:#64748B;

    --note:#2563EB;        --note-soft:rgba(37,99,235,0.08);
    --good:#1F9D6B;         --good-soft:rgba(31,157,107,0.1);
    --info:#6366F1;         --info-soft:rgba(99,102,241,0.1);
    --warn:#D97706;         --warn-soft:rgba(217,119,6,0.1);
    --danger:#DC4C4C;       --danger-soft:rgba(220,76,76,0.08);
    --success:#0D9488;      --success-soft:rgba(13,148,136,0.1);

    /* code blocks stay dark-on-light in both themes, so these are separate
       from --ink/--surface rather than reusing them */
    --code-bg:#17212B;
    --code-fg:#E7EBEF;
  }

  :root[data-theme="dark"]{
    --bg:#10151B;
    --surface:#181F27;
    --surface-hover:#212A34;
    --border:#2C3844;
    --border-soft:#212B35;
    --ink:#E6EDF3;
    --ink-dim:#8B98A5;
    --ink-faint:#586674;
    --folder:#8593A3;

    --note:#4C8DFF;         --note-soft:rgba(76,141,255,0.14);
    --good:#3ECB8D;         --good-soft:rgba(62,203,141,0.14);
    --info:#8B93F8;         --info-soft:rgba(139,147,248,0.14);
    --warn:#E5A93F;         --warn-soft:rgba(229,169,63,0.14);
    --danger:#F0665F;       --danger-soft:rgba(240,102,95,0.14);
    --success:#2FBFAE;      --success-soft:rgba(47,191,174,0.14);

    /* a shade darker than --surface so code blocks still stand out */
    --code-bg:#0A0E13;
    --code-fg:#E6EDF3;
  }

  *{ box-sizing:border-box; }
  html{ scroll-behavior:smooth; }
  @media (prefers-reduced-motion: reduce){ html{ scroll-behavior:auto; } }
  body{
    margin:0;
    background:var(--bg);
    color:var(--ink);
    font-family:'JetBrains Mono', ui-monospace, monospace;
    font-size:15.5px;
    line-height:1.65;
    -webkit-font-smoothing:antialiased;
  }

  .back-to-top{
    position:fixed;
    bottom:18px;
    right:18px;
    width:34px;
    height:34px;
    border-radius:8px;
    background:var(--surface);
    border:1px solid var(--border);
    color:var(--ink-dim);
    display:flex;
    align-items:center;
    justify-content:center;
    cursor:pointer;
    box-shadow:0 2px 10px rgba(23,33,43,0.15);
    z-index:50;
    text-decoration:none;
  }
  .back-to-top:hover{ color:var(--ink); border-color:var(--note); }
  .back-to-top svg{ width:16px; height:16px; }

  /* small inline arrow that appears next to whichever heading you just
     jumped to — click it to return to exactly where you jumped from */
  .jump-return{
    display:inline-flex;
    align-items:center;
    justify-content:center;
    width:26px;
    height:26px;
    margin-left:5px;
    border-radius:7px;
    background:var(--note-soft);
    color:var(--note);
    border:none;
    cursor:pointer;
    vertical-align:top;
    position:relative;
    top:-3px;
    flex-shrink:0;
  }
  .jump-return:hover{ background:var(--note); color:var(--surface); }
  .jump-return svg{ width:17px; height:17px; }

  .page{
    max-width:840px;
    margin:0 auto;
    padding:44px 32px 64px;
  }

  /* ── headings ────────────────────────────────────────── */
  h1{
    font-size:29px;
    font-weight:700;
    letter-spacing:-0.01em;
    margin:0 0 6px;
  }
  .subtitle{
    color:var(--ink-dim);
    font-size:14px;
    margin:0 0 28px;
  }
  h2{
    font-size:20px;
    font-weight:700;
    margin:38px 0 14px;
    padding-bottom:8px;
    border-bottom:1px solid var(--border-soft);
    scroll-margin-top:24px;
  }
  h2:target{
    animation:jump-highlight 1.8s ease-out;
    border-radius:6px;
  }
  @keyframes jump-highlight{
    0%{ background:var(--note-soft); box-shadow:0 0 0 8px var(--note-soft); }
    100%{ background:transparent; box-shadow:0 0 0 8px transparent; }
  }
  h3{
    font-size:16px;
    font-weight:700;
    color:var(--note);
    margin:22px 0 9px;
  }

  p{ margin:0 0 13px; }
  a{ color:var(--note); text-decoration:none; border-bottom:1px solid var(--note-soft); }
  a:hover{ border-bottom-color:var(--note); }
  a.note-link{
    border-bottom-style:dashed;
  }
  a.note-link::before{
    content:'↳ ';
    color:var(--ink-faint);
  }

  strong{ font-weight:700; }
  em{ color:var(--ink-dim); }
  s{ color:var(--ink-faint); }
  u{ text-decoration-color:var(--note); text-underline-offset:3px; }
  mark{
    background:var(--note-soft);
    color:var(--ink);
    padding:1px 4px;
    border-radius:3px;
  }

  /* ── lists ───────────────────────────────────────────── */
  ul, ol{ margin:0 0 15px; padding-left:24px; }
  li{ margin-bottom:5px; }
  li::marker{ color:var(--note); }
  ul.plain{ list-style:none; padding-left:0; }
  ul.plain li{ padding-left:20px; position:relative; }
  ul.plain li::before{
    content:'›';
    position:absolute; left:0; color:var(--note); font-weight:700;
  }

  /* ── task list (visual only — notes are read-only) ─────*/
  .tasks{ list-style:none; padding-left:0; margin:0 0 15px; }
  .tasks li{
    display:flex; align-items:flex-start; gap:10px;
    margin-bottom:7px;
  }
  .box{
    flex-shrink:0;
    width:18px; height:18px;
    border:1.5px solid var(--border);
    border-radius:5px;
    margin-top:2px;
  }
  .box.done{
    background:var(--good);
    border-color:var(--good);
    position:relative;
  }
  .box.done::after{
    content:'';
    position:absolute; left:5px; top:2px;
    width:4px; height:8px;
    border:solid #FFFFFF;
    border-width:0 1.8px 1.8px 0;
    transform:rotate(45deg);
  }
  .tasks li.done span:last-child{ color:var(--ink-faint); text-decoration:line-through; }

  /* ── blockquote ──────────────────────────────────────── */
  blockquote{
    margin:0 0 15px;
    padding:11px 15px;
    border-left:3px solid var(--note);
    background:var(--note-soft);
    color:var(--ink-dim);
    border-radius:0 6px 6px 0;
  }
  cite{
    display:block;
    margin-top:8px;
    font-style:normal;
    font-size:13px;
    color:var(--ink-faint);
  }

  /* ── callouts ────────────────────────────────────────── */
  .callout{
    display:flex; gap:10px;
    padding:11px 14px;
    border-radius:8px;
    border:1px solid var(--border-soft);
    background:var(--surface);
    margin:0 0 9px;
    font-size:13.5px;
  }
  .callout .label{
    font-weight:700;
    font-size:11.5px;
    letter-spacing:0.06em;
    text-transform:uppercase;
    display:block;
    margin-bottom:2px;
  }
  .callout.note{ border-left:3px solid var(--note); }
  .callout.note .label{ color:var(--note); }
  .callout.tip{ border-left:3px solid var(--good); }
  .callout.tip .label{ color:var(--good); }
  .callout.info{ border-left:3px solid var(--info); }
  .callout.info .label{ color:var(--info); }
  .callout.warning{ border-left:3px solid var(--warn); }
  .callout.warning .label{ color:var(--warn); }
  .callout.danger{ border-left:3px solid var(--danger); }
  .callout.danger .label{ color:var(--danger); }
  .callout.success{ border-left:3px solid var(--success); }
  .callout.success .label{ color:var(--success); }

  /* ── code ────────────────────────────────────────────── */
  code{
    font-family:inherit;
    background:var(--surface-hover);
    border:1px solid var(--border-soft);
    border-radius:4px;
    padding:2px 6px;
    font-size:14.5px;
  }
  pre{
    background:var(--code-bg);
    color:var(--code-fg);
    border-radius:8px;
    padding:15px 17px;
    overflow-x:auto;
    margin:0 0 15px;
    font-size:13.5px;
    line-height:1.6;
  }
  pre code{ background:none; border:none; padding:0; color:inherit; }
  .tok-key{ color:#7FB0FF; }
  .tok-str{ color:#9FD6A0; }
  .tok-com{ color:#7A8794; }

  /* ── table ───────────────────────────────────────────── */
  table{
    width:100%;
    border-collapse:collapse;
    margin:0 0 15px;
    font-size:13.5px;
  }
  th, td{
    text-align:left;
    padding:8px 10px;
    border-bottom:1px solid var(--border-soft);
  }
  th{
    color:var(--ink-dim);
    font-weight:700;
    font-size:13px;
    text-transform:uppercase;
    letter-spacing:0.04em;
  }
  tr:last-child td{ border-bottom:none; }
  td.tag{
    display:inline-block;
    font-size:12.5px;
    padding:3px 9px;
    border-radius:20px;
  }
  .tag.ok{ background:var(--good-soft); color:var(--good); }
  .tag.risk{ background:var(--danger-soft); color:var(--danger); }
  .tag.wait{ background:var(--note-soft); color:var(--note); }

  /* ── table of contents ──────────────────────────────────*/
  .toc{
    display:flex;
    flex-wrap:wrap;
    align-items:center;
    gap:6px 4px;
    padding:12px 14px;
    background:var(--surface);
    border:1px solid var(--border-soft);
    border-radius:10px;
    margin:0 0 18px;
  }
  .toc-label{
    font-size:11px;
    font-weight:700;
    letter-spacing:0.06em;
    text-transform:uppercase;
    color:var(--ink-faint);
    margin-right:6px;
  }
  .toc a{
    font-size:13px;
    color:var(--ink-dim);
    background:var(--surface-hover);
    border:1px solid transparent;
    border-bottom:1px solid transparent;
    border-radius:20px;
    padding:4px 11px;
  }
  .toc a:hover{ color:var(--note); border-color:var(--note-soft); }

  /* ── divider ─────────────────────────────────────────── */
  hr{
    border:none;
    border-top:1px solid var(--border-soft);
    margin:28px 0;
  }

  /* ── figure / diagram / image card ──────────────────────*/
  figure{
    margin:0 0 15px;
    padding:16px;
    background:var(--surface);
    border:1px solid var(--border-soft);
    border-radius:10px;
  }
  figure img, figure svg{ display:block; width:100%; border-radius:6px; }
  figcaption{
    margin-top:12px;
    font-size:13.5px;
    color:var(--ink-dim);
    text-align:center;
  }

  .kbd{
    font-family:inherit;
    font-size:13px;
    padding:2px 7px;
    border:1px solid var(--border);
    border-bottom-width:2px;
    border-radius:4px;
    background:var(--surface);
  }

  /* ── math ────────────────────────────────────────────── */
  .math{
    background:var(--surface);
    border:1px solid var(--border-soft);
    border-radius:10px;
    padding:18px 15px;
    text-align:center;
    margin:0 0 15px;
  }
  .math .katex{ font-size:1.15em; color:var(--ink); }
  .math .katex .mfrac line{ border-color:var(--ink); }
  .math .label{
    display:block;
    font-size:12.5px;
    color:var(--ink-dim);
    margin-top:12px;
    font-style:italic;
    font-family:'JetBrains Mono', ui-monospace, monospace;
  }

  /* ── definition list ─────────────────────────────────── */
  dl{ margin:0 0 15px; }
  dt{
    font-weight:700;
    color:var(--note);
    margin-top:10px;
  }
  dt:first-child{ margin-top:0; }
  dd{ margin:4px 0 0; color:var(--ink-dim); }

  /* ── expandable / details ───────────────────────────────*/
  details{
    margin:0 0 9px;
    padding:11px 15px;
    background:var(--surface);
    border:1px solid var(--border-soft);
    border-radius:10px;
  }
  details[open]{ padding-bottom:14px; }
  summary{
    cursor:pointer;
    font-weight:700;
    list-style:none;
    display:flex;
    align-items:center;
    gap:8px;
  }
  summary::-webkit-details-marker{ display:none; }
  summary::before{
    content:'▸';
    color:var(--note);
    transition:transform 0.15s ease;
    display:inline-block;
  }
  details[open] summary::before{ transform:rotate(90deg); }
  details p{ margin:14px 0 0; }

  /* ── references ──────────────────────────────────────── */
  .refs{ list-style:none; padding-left:0; margin:0; }
  .refs li{
    display:flex; gap:9px;
    margin-bottom:7px;
    color:var(--ink-dim);
  }
  .refs .idx{ color:var(--ink-faint); flex-shrink:0; }

  /* ── heading-scale preview (illustrative only) ──────────*/
  .heading-demo{
    border:1px solid var(--border-soft);
    border-radius:10px;
    background:var(--surface);
    padding:15px 17px;
    margin:0 0 15px;
  }
  .heading-demo .row{ padding:10px 0; border-bottom:1px solid var(--border-soft); }
  .heading-demo .row:last-child{ border-bottom:none; padding-bottom:0; }
  .heading-demo .row:first-child{ padding-top:0; }
  .h-tag{ font-size:11px; color:var(--ink-faint); margin-right:10px; }
  .demo-h1{ font-size:30px; font-weight:700; letter-spacing:-0.01em; }
  .demo-h2{ font-size:20px; font-weight:700; }
  .demo-h3{ font-size:16px; font-weight:700; color:var(--note); }
</style>
</head>
<body>

<a class="back-to-top" href="#" title="back to top">
  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="12" y1="19" x2="12" y2="5"/><polyline points="5 12 12 5 19 12"/></svg>
</a>

<div class="page">

  <h1>Example Note</h1>
  <p class="subtitle">what you can put in a note, and what it will look like — notes are read-only, so nothing on this page is interactive beyond native expand/collapse</p>

  <nav class="toc" aria-label="table of contents">
    <span class="toc-label">Contents</span>
    <a href="#text">Text</a>
    <a href="#headings">Headings</a>
    <a href="#lists">Lists</a>
    <a href="#tasks">Tasks</a>
    <a href="#quotes">Quotes</a>
    <a href="#callouts">Callouts</a>
    <a href="#code">Code</a>
    <a href="#tables">Tables</a>
    <a href="#images">Images</a>
    <a href="#svg-diagrams">SVG / Diagrams</a>
    <a href="#charts">Charts</a>
    <a href="#math">Math</a>
    <a href="#definitions">Definitions</a>
    <a href="#expandable-sections">Expandable Sections</a>
    <a href="#keyboard-shortcuts">Keyboard Shortcuts</a>
    <a href="#references">References</a>
  </nav>

  <p>Same-page jumps work too — clicking a contents link above scrolls straight to that section, the way <a href="#references">a reference link near the bottom</a> can jump back up to a definition, or vice versa.</p>

  <h2 id="text">Text</h2>

  <p>A normal paragraph. Inline styles: <strong>bold</strong>, <em>italic</em>, <u>underline</u>, <s>strikethrough</s>, <mark>highlighted</mark>, and inline <code>code</code>. A horizontal rule separates topics:</p>

  <hr/>

  <p>Links can point <a href="https://example.com">outside the app</a>, or to another note in your Notes folder — which is the more useful case here:</p>

  <p><a class="note-link" href="../Biology/cells.html">Cell Structure</a> — a link like this jumps straight to another note, so notes can reference each other like a small knowledge base.</p>

  <h2 id="headings">Headings</h2>

  <p>Section titles use three weights of emphasis:</p>

  <div class="heading-demo">
    <div class="row"><span class="h-tag">H1</span><span class="demo-h1">Page title</span></div>
    <div class="row"><span class="h-tag">H2</span><span class="demo-h2">Section heading</span></div>
    <div class="row"><span class="h-tag">H3</span><span class="demo-h3">Sub-heading</span></div>
  </div>

  <h2 id="lists">Lists</h2>

  <h3>Bulleted</h3>
  <ul>
    <li>Plain unordered list item</li>
    <li>Second item with <code>inline code</code> inside it</li>
    <li>Third item, nested below:
      <ul>
        <li>A nested point</li>
        <li>Another nested point</li>
      </ul>
    </li>
  </ul>

  <h3>Numbered</h3>
  <ol>
    <li>First step in a process</li>
    <li>Second step</li>
    <li>Third step, the order actually matters here</li>
  </ol>

  <h3>Arrow-style</h3>
  <ul class="plain">
    <li>An alternate bullet style using a custom marker</li>
    <li>Good for a lighter, less "outline-y" feel</li>
  </ul>

  <h2 id="tasks">Tasks</h2>

  <p>A visual checklist for tracking progress on a topic — display-only, since notes aren't editable:</p>

  <ul class="tasks">
    <li class="done"><span class="box done"></span><span>Read chapter 4</span></li>
    <li class="done"><span class="box done"></span><span>Summarize key terms</span></li>
    <li><span class="box"></span><span>Practice problems</span></li>
    <li><span class="box"></span><span>Review before the test</span></li>
  </ul>

  <h2 id="quotes">Quotes</h2>

  <blockquote>
    "A blockquote pulls out a definition or key line from the surrounding text."
    <cite>— Source, Chapter 2</cite>
  </blockquote>

  <h2 id="callouts">Callouts</h2>

  <p>Six purely visual variants — pick whichever tone fits the note:</p>

  <div class="callout note"><div><span class="label">Note</span>General context worth keeping in mind while reading.</div></div>
  <div class="callout tip"><div><span class="label">Tip</span>A helpful shortcut or optional suggestion.</div></div>
  <div class="callout info"><div><span class="label">Info</span>Background detail that's useful but not critical.</div></div>
  <div class="callout warning"><div><span class="label">Warning</span>Something worth double-checking before moving on.</div></div>
  <div class="callout danger"><div><span class="label">Danger</span>A common mistake or a point that's easy to get wrong.</div></div>
  <div class="callout success"><div><span class="label">Success</span>A confirmation, milestone, or correct result.</div></div>

  <h2 id="code">Code</h2>

  <p>Inline code looks like <code>const x = 42</code>. A full block looks like this:</p>

  <pre><code><span class="tok-com">// fetch and render a folder's contents</span>
<span class="tok-key">async function</span> loadFolder(path) {
  <span class="tok-key">const</span> res = <span class="tok-key">await</span> fetch(path);
  <span class="tok-key">if</span> (!res.ok) <span class="tok-key">throw new</span> Error(<span class="tok-str">'failed to load'</span>);
  <span class="tok-key">return</span> res.json();
}</code></pre>

  <h2 id="tables">Tables</h2>

  <table>
    <thead>
      <tr><th>Topic</th><th>Chapter</th><th>Status</th></tr>
    </thead>
    <tbody>
      <tr><td>Cell structure</td><td>4</td><td><span class="tag ok">reviewed</span></td></tr>
      <tr><td>Photosynthesis</td><td>5</td><td><span class="tag ok">reviewed</span></td></tr>
      <tr><td>Cellular respiration</td><td>6</td><td><span class="tag wait">in progress</span></td></tr>
      <tr><td>Genetics intro</td><td>7</td><td><span class="tag risk">not started</span></td></tr>
    </tbody>
  </table>

  <h2 id="images">Images</h2>

  <figure>
    <img src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 800 320'%3E%3Crect width='800' height='320' fill='%23EEF2F6'/%3E%3Ccircle cx='140' cy='90' r='46' fill='%232563EB' opacity='0.18'/%3E%3Cpath d='M0 260 C 140 180, 260 220, 380 190 S 620 140, 800 210 L800 320 L0 320 Z' fill='%2364748B' opacity='0.25'/%3E%3Cpath d='M0 290 C 160 230, 300 260, 440 235 S 660 190, 800 250 L800 320 L0 320 Z' fill='%232563EB' opacity='0.35'/%3E%3Ccircle cx='640' cy='70' r='26' fill='%23F6F8FA' stroke='%23D9E0E7' stroke-width='2'/%3E%3C/svg%3E" alt="placeholder diagram of a cell membrane cross-section">
    <figcaption>Fig. 1 — a real photo or scan would go here, shown as a normal &lt;img&gt;</figcaption>
  </figure>

  <h2 id="svg-diagrams">SVG / Diagrams</h2>

  <figure>
    <svg viewBox="0 0 600 120">
      <defs>
        <marker id="arrow" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto">
          <path d="M0,0 L8,4 L0,8 Z" fill="#66737F"/>
        </marker>
      </defs>
      <g font-family="JetBrains Mono, monospace" font-size="12">
        <rect x="10" y="40" width="120" height="40" rx="8" fill="#FFFFFF" stroke="#2563EB" stroke-width="1.6"/>
        <text x="70" y="65" text-anchor="middle" fill="#17212B">Notes/</text>
        <line x1="130" y1="60" x2="230" y2="60" stroke="#66737F" stroke-width="1.6" marker-end="url(#arrow)"/>
        <rect x="235" y="40" width="140" height="40" rx="8" fill="#FFFFFF" stroke="#64748B" stroke-width="1.6"/>
        <text x="305" y="65" text-anchor="middle" fill="#17212B">Biology/</text>
        <line x1="375" y1="60" x2="475" y2="60" stroke="#66737F" stroke-width="1.6" marker-end="url(#arrow)"/>
        <rect x="480" y="40" width="110" height="40" rx="8" fill="#FFFFFF" stroke="#2563EB" stroke-width="1.6"/>
        <text x="535" y="65" text-anchor="middle" fill="#17212B">cells.html</text>
      </g>
    </svg>
    <figcaption>a simple flow diagram — plain SVG, no library needed</figcaption>
  </figure>

  <h2 id="charts">Charts</h2>

  <figure>
    <svg viewBox="0 0 600 220">
      <g font-family="JetBrains Mono, monospace" font-size="11" fill="#66737F">
        <line x1="40" y1="20" x2="40" y2="180" stroke="#E7EBEF" stroke-width="1"/>
        <line x1="40" y1="180" x2="580" y2="180" stroke="#D9E0E7" stroke-width="1.2"/>
        <text x="10" y="24">10</text>
        <text x="10" y="104">5</text>
        <text x="18" y="184">0</text>
        <rect x="70"  y="100" width="46" height="80" rx="4" fill="#2563EB" opacity="0.85"/>
        <rect x="150" y="60"  width="46" height="120" rx="4" fill="#2563EB" opacity="0.85"/>
        <rect x="230" y="130" width="46" height="50" rx="4" fill="#2563EB" opacity="0.55"/>
        <rect x="310" y="40"  width="46" height="140" rx="4" fill="#2563EB"/>
        <rect x="390" y="90"  width="46" height="90" rx="4" fill="#2563EB" opacity="0.85"/>
        <rect x="470" y="70"  width="46" height="110" rx="4" fill="#2563EB" opacity="0.85"/>
        <text x="80"  y="198" text-anchor="middle">Mon</text>
        <text x="160" y="198" text-anchor="middle">Tue</text>
        <text x="240" y="198" text-anchor="middle">Wed</text>
        <text x="320" y="198" text-anchor="middle">Thu</text>
        <text x="400" y="198" text-anchor="middle">Fri</text>
        <text x="480" y="198" text-anchor="middle">Sat</text>
      </g>
    </svg>
    <figcaption>study minutes per day — bars drawn directly as SVG rects</figcaption>
  </figure>

  <h2 id="math">Math</h2>

  <p>Inline math renders properly too, not just plain text with sup/sub: \(E = mc^2\), or \(H_2O\).</p>

  <div class="math">
    \[ x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a} \]
    <span class="label">the quadratic formula, typeset with KaTeX</span>
  </div>

  <h2 id="definitions">Definitions</h2>

  <dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language — the format every note in this app is written in</dd>
    <dt>Mitochondria</dt>
    <dd>the organelle responsible for producing ATP through cellular respiration</dd>
    <dt>Photosynthesis</dt>
    <dd>the process plants use to convert light energy into chemical energy</dd>
  </dl>

  <h2 id="expandable-sections">Expandable Sections</h2>

  <p>Fold away extra detail so the main point stays scannable:</p>

  <details>
    <summary>What's the difference between mitosis and meiosis?</summary>
    <p>Mitosis produces two identical daughter cells for growth and repair. Meiosis produces four genetically distinct cells with half the chromosome number, used in sexual reproduction.</p>
  </details>

  <details>
    <summary>Extra example — worked problem</summary>
    <p>If a cell has 46 chromosomes before meiosis, each resulting gamete will have 23 — half the original number, one from each pair.</p>
  </details>

  <h2 id="keyboard-shortcuts">Keyboard Shortcuts</h2>

  <p>Handy for a notes app itself, or for documenting shortcuts in software you're studying:</p>

  <p><span class="kbd">Ctrl</span> + <span class="kbd">K</span> to search, <span class="kbd">Esc</span> to close a note, <span class="kbd">⌘</span> + <span class="kbd">F</span> to find on the page.</p>

  <h2 id="references">References</h2>

  <ol class="refs">
    <li><span class="idx">[1]</span><span>Biology — Chapter 4, Cell Structure</span></li>
    <li><span class="idx">[2]</span><span>MDN Web Docs — HTML element reference</span></li>
    <li><span class="idx">[3]</span><span><a class="note-link" href="../Biology/cells.html">Cell Structure</a> — related note</span></li>
  </ol>

</div>

<script>
(function(){
  'use strict';
  // Clicking a "#anchor" link drops a small arrow right next to whichever
  // heading it lands on. Click that arrow and it takes you back to
  // exactly where you clicked from. Leave it alone and it just stays
  // there — only a new jump elsewhere replaces it.
  const ICON_ARROW_LEFT = `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 13l-4 -4l4 -4"/><path d="M5 9h7a4 4 0 1 1 0 8h-1"/></svg>`;
  let currentArrow = null;

  function placeReturnArrow(target, returnY){
    if(currentArrow) currentArrow.remove();
    const btn = document.createElement('button');
    btn.type = 'button';
    btn.className = 'jump-return';
    btn.title = 'back to where you jumped from';
    btn.innerHTML = ICON_ARROW_LEFT;
    btn.addEventListener('click', (e) => {
      e.preventDefault();
      window.scrollTo({ top: returnY, behavior: 'smooth' });
      btn.remove();
      if(currentArrow === btn) currentArrow = null;
    });
    target.appendChild(btn);
    currentArrow = btn;
  }

  document.querySelectorAll('a[href^="#"]').forEach(link => {
    link.addEventListener('click', () => {
      const id = link.getAttribute('href').slice(1);
      if(!id) return; // plain href="#" (the back-to-top button) — nothing to mark
      const target = document.getElementById(id);
      if(!target) return;
      placeReturnArrow(target, window.scrollY);
    });
  });
})();
</script>

<script>
(function(){
  'use strict';
  // Renders any \( \) inline math or \[ \] display math on the page
  // using KaTeX, once the library has finished loading.
  document.addEventListener('DOMContentLoaded', function(){
    if(typeof renderMathInElement === 'function'){
      renderMathInElement(document.body, {
        delimiters: [
          { left: '\\[', right: '\\]', display: true },
          { left: '\\(', right: '\\)', display: false }
        ]
      });
    }
  });
})();
</script>

</body>
</html>
```