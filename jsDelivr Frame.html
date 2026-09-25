<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>jsDelivr Frame</title>
<style>
  :root{
    --bg: #0a0c1a; --panel: #14162c; --panel-2: #1b1e3c; --border: #2c3060;
    --text: #e4e7fb; --text-dim: #8d94c4; --accent: #8b7cf6; --error: #ff6b8a; --ok: #8be9a0;
  }
  * { box-sizing: border-box; }
  html, body { height: 100%; margin: 0; }
  body {
    background: var(--bg); color: var(--text); font-family: -apple-system, "Segoe UI", Helvetica, Arial, sans-serif;
    display: flex; flex-direction: column;
  }
  #bar {
    display: flex; align-items: center; gap: 8px; padding: 10px 12px; background: var(--panel);
    border-bottom: 1px solid var(--border); flex-wrap: wrap;
  }
  #bar h1 { font-size: 14px; margin: 0 12px 0 0; white-space: nowrap; }
  #urlInput {
    flex: 1; min-width: 220px; background: var(--panel-2); border: 1px solid var(--border); color: var(--text);
    padding: 8px 10px; border-radius: 7px; font: 12.5px "SF Mono", Menlo, Consolas, monospace;
  }
  #urlInput:focus { outline: none; border-color: var(--accent); }
  button {
    border: 1px solid var(--border); background: var(--panel-2); color: var(--text); padding: 8px 14px;
    border-radius: 7px; font-size: 12.5px; cursor: pointer; transition: border-color .15s ease, color .15s ease, transform .12s ease;
    white-space: nowrap;
  }
  button:hover { border-color: var(--accent); color: var(--accent); }
  button:active { transform: scale(0.96); }
  button:disabled { opacity: 0.5; cursor: not-allowed; }
  button.primary { background: var(--accent); color: #14132b; border-color: var(--accent); font-weight: 600; }
  button.primary:hover { filter: brightness(1.08); color: #14132b; }
  #status { font-size: 11.5px; color: var(--text-dim); padding: 0 12px 8px; }
  #status.err { color: var(--error); }
  #status.ok { color: var(--ok); }
  #badge {
    font-size: 10.5px; font-weight: 600; letter-spacing: .03em; text-transform: uppercase;
    padding: 2px 7px; border-radius: 5px; border: 1px solid var(--border); color: var(--text-dim); display: none;
  }
  #badge.html { display: inline-block; color: #ffb86b; border-color: #ffb86b44; }
  #badge.python { display: inline-block; color: #8be9a0; border-color: #8be9a044; }
  #stage { flex: 1; position: relative; background: #000; display: flex; flex-direction: column; }
  #frame { position: absolute; inset: 0; width: 100%; height: 100%; border: none; background: #fff; }
  #placeholder {
    position: absolute; inset: 0; display: flex; align-items: center; justify-content: center;
    color: var(--text-dim); font-size: 13px; text-align: center; padding: 20px;
  }
  /* ---- Python run view: code + console, shown instead of the iframe ---- */
  #pyStage { position: absolute; inset: 0; display: none; flex-direction: column; }
  #pyStage.active { display: flex; }
  #pyToolbar {
    display: flex; align-items: center; gap: 8px; padding: 8px 12px; background: var(--panel);
    border-bottom: 1px solid var(--border); font-size: 12px; color: var(--text-dim);
  }
  #pyPanes { flex: 1; display: flex; min-height: 0; }
  #pyCode, #pyConsole {
    flex: 1; overflow: auto; padding: 12px; font: 12.5px/1.55 "SF Mono", Menlo, Consolas, monospace;
    white-space: pre-wrap; word-break: break-word;
  }
  #pyCode { background: #0d0f1e; color: #cfd3f5; border-right: 1px solid var(--border); }
  #pyConsole { background: #050611; color: var(--text); }
  #pyConsole .line { margin-bottom: 2px; }
  #pyConsole .line.error { color: var(--error); }
  #pyConsole .line.muted { color: var(--text-dim); }
  #pyLoadBar { height: 3px; background: var(--panel-2); overflow: hidden; }
  #pyLoadFill { height: 100%; width: 0%; background: var(--accent); transition: width .2s ease; }
</style>
</head>
<body>
<div id="bar">
  <h1>🌐 jsDelivr Frame</h1>
  <input type="text" id="urlInput" placeholder="cdn.jsdelivr.net/gh/user/repo@branch/file  (or paste the full https:// URL)" spellcheck="false">
  <span id="badge"></span>
  <button type="button" id="loadBtn" class="primary">▶ Fetch &amp; Run</button>
  <button type="button" id="newTabBtn">↗ Open in new tab</button>
</div>
<div id="status">Paste a link to a raw HTML or Python file — it's fetched, checked, and run as whichever kind it is.</div>
<div id="stage">
  <div id="placeholder">Nothing loaded yet.</div>
  <iframe id="frame" style="display:none;" title="Fetched HTML content"></iframe>
  <div id="pyStage">
    <div id="pyLoadBar"><div id="pyLoadFill"></div></div>
    <div id="pyToolbar">Detected Python — running with an in-page Python runtime</div>
    <div id="pyPanes">
      <div id="pyCode"></div>
      <div id="pyConsole"></div>
    </div>
  </div>
</div>

<script>
(function(){
  "use strict";
  const urlInput = document.getElementById('urlInput');
  const statusEl = document.getElementById('status');
  const badgeEl = document.getElementById('badge');
  const frame = document.getElementById('frame');
  const placeholder = document.getElementById('placeholder');
  const loadBtn = document.getElementById('loadBtn');
  const pyStage = document.getElementById('pyStage');
  const pyCode = document.getElementById('pyCode');
  const pyConsole = document.getElementById('pyConsole');
  const pyLoadBar = document.getElementById('pyLoadBar');
  const pyLoadFill = document.getElementById('pyLoadFill');
  const STORAGE_KEY = 'jsdelivr_frame_last_url_v1';
  const PYODIDE_VERSION = 'v0.26.4';

  let pyodideInstance = null;
  let pyodideLoadPromise = null;

  function setStatus(msg, kind){
    statusEl.textContent = msg;
    statusEl.className = kind ? kind : '';
  }
  function setBadge(kind){
    badgeEl.className = kind || '';
    badgeEl.textContent = kind || '';
  }
  function showFrameStage(){ pyStage.classList.remove('active'); placeholder.style.display = 'none'; frame.style.display = 'block'; }
  function showPyStage(){ frame.style.display = 'none'; placeholder.style.display = 'none'; pyStage.classList.add('active'); }

  // Accepts a bare "cdn.jsdelivr.net/…" path, a plain "user/repo@branch/file"
  // jsDelivr shorthand, or a full URL.
  function normalizeUrl(raw){
    let u = raw.trim();
    if (!u) return null;
    if (!/^https?:\/\//i.test(u)) {
      if (/^cdn\.jsdelivr\.net\//i.test(u)) u = 'https://' + u;
      else if (/^gh\//i.test(u)) u = 'https://cdn.jsdelivr.net/' + u;
      else if (/^[\w.-]+\/[\w.-]+@/.test(u)) u = 'https://cdn.jsdelivr.net/gh/' + u; // e.g. "user/repo@main/file.html"
      else u = 'https://' + u;
    }
    return u;
  }

  // Extension is the primary signal; falls back to sniffing the fetched text
  // when the URL doesn't end in something recognizable.
  function detectKind(url, text){
    const path = url.split(/[?#]/)[0].toLowerCase();
    if (/\.(py|pyw)$/.test(path)) return 'python';
    if (/\.(html?|xhtml)$/.test(path)) return 'html';
    const head = text.slice(0, 1000).toLowerCase();
    if (/<!doctype html|<html[\s>]/.test(head)) return 'html';
    return 'python';
  }

  function pyLog(type, text){
    const div = document.createElement('div');
    div.className = 'line' + (type ? ' ' + type : '');
    div.textContent = text;
    pyConsole.appendChild(div);
    pyConsole.scrollTop = pyConsole.scrollHeight;
  }

  function ensurePyodide(){
    if (pyodideInstance) return Promise.resolve(pyodideInstance);
    if (pyodideLoadPromise) return pyodideLoadPromise;
    pyLoadBar.style.display = 'block';
    pyLoadFill.style.width = '8%';
    pyLog('muted', 'Loading Python runtime (first run only, needs an internet connection)…');
    let simulated = 8;
    const ticker = setInterval(() => { simulated += (85 - simulated) * 0.08; pyLoadFill.style.width = simulated + '%'; }, 200);
    pyodideLoadPromise = (async () => {
      try {
        await new Promise((resolve, reject) => {
          const script = document.createElement('script');
          script.src = 'https://cdn.jsdelivr.net/pyodide/' + PYODIDE_VERSION + '/full/pyodide.js';
          script.onload = resolve;
          script.onerror = () => reject(new Error('Could not load the Python runtime — this needs an internet connection the first time.'));
          document.head.appendChild(script);
        });
        const instance = await loadPyodide({
          indexURL: 'https://cdn.jsdelivr.net/pyodide/' + PYODIDE_VERSION + '/full/',
          stdout: (s) => pyLog('', s),
          stderr: (s) => pyLog('error', s)
        });
        clearInterval(ticker);
        pyLoadFill.style.width = '100%';
        setTimeout(() => { pyLoadBar.style.display = 'none'; pyLoadFill.style.width = '0%'; }, 700);
        pyodideInstance = instance;
        return instance;
      } catch(err){
        clearInterval(ticker);
        pyLoadBar.style.display = 'none';
        pyodideLoadPromise = null;
        throw err;
      }
    })();
    return pyodideLoadPromise;
  }

  async function runPython(code){
    showPyStage();
    pyCode.textContent = code;
    pyConsole.innerHTML = '';
    try {
      const instance = await ensurePyodide();
      pyLog('muted', 'Running…');
      await instance.runPythonAsync(code);
      pyLog('muted', 'Finished.');
    } catch(err){
      pyLog('error', String(err && err.message ? err.message : err));
    }
  }

  function runHtml(url, text){
    showFrameStage();
    // srcdoc keeps this fully self-contained (no extra request, works for
    // any fetched HTML regardless of what the source server allows), and
    // still runs the page's own scripts and relative same-origin fetches.
    frame.removeAttribute('src');
    frame.srcdoc = text;
  }

  async function run(){
    const raw = urlInput.value;
    const url = normalizeUrl(raw);
    if (!url) { setStatus('Paste a URL first.', 'err'); return; }
    try { localStorage.setItem(STORAGE_KEY, raw.trim()); } catch(e){}

    setBadge(null);
    setStatus('Fetching ' + url + ' …');
    loadBtn.disabled = true;
    try {
      const res = await fetch(url);
      if (!res.ok) throw new Error('Server responded ' + res.status + ' ' + res.statusText);
      const text = await res.text();
      const kind = detectKind(url, text);
      setBadge(kind);
      if (kind === 'html'){
        runHtml(url, text);
        setStatus('Detected HTML — running it in the frame below.', 'ok');
      } else {
        setStatus('Detected Python — running it below with an in-page Python runtime.', 'ok');
        await runPython(text);
      }
    } catch(err){
      setStatus('Could not run that: ' + (err && err.message ? err.message : err), 'err');
    }
    loadBtn.disabled = false;
  }

  loadBtn.addEventListener('click', run);
  urlInput.addEventListener('keydown', (e) => { if (e.key === 'Enter') run(); });
  document.getElementById('newTabBtn').addEventListener('click', () => {
    const url = normalizeUrl(urlInput.value);
    if (!url) { setStatus('Paste a URL first.', 'err'); return; }
    window.open(url, '_blank');
  });

  // Restore the last URL used in this browser, for convenience — doesn't
  // auto-run it, so nothing loads until you press Fetch & Run.
  try {
    const saved = localStorage.getItem(STORAGE_KEY);
    if (saved) urlInput.value = saved;
  } catch(e){}
})();
</script>
</body>
</html>
