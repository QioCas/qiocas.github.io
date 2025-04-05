---
layout: page
title: IDE
---

<html lang="en">
<head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/lib/codemirror.css">
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/lib/codemirror.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/mode/python/python.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/edit/closebrackets.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/show-hint.js"></script>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/show-hint.css">
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/python-hint.js"></script>

  <style>
    .CodeMirror {
      border: 1px solid #ddd;
      height: auto;
      min-height: 200px;
      font-family: monospace;
      font-size: 14px;
      line-height: 1.5;
    }

    .CodeMirror-scroll {
      overflow: auto !important;
      max-height: 400px;
    }

    .CodeMirror::-webkit-scrollbar {
      width: 10px;
      height: 10px;
    }

    .CodeMirror::-webkit-scrollbar-track {
      background: #f1f1f1;
      border-radius: 5px;
    }

    .CodeMirror::-webkit-scrollbar-thumb {
      background: #888;
      border-radius: 5px;
    }

    .CodeMirror::-webkit-scrollbar-thumb:hover {
      background: #555;
    }

    .CodeMirror {
      scrollbar-width: thin;
      scrollbar-color: #888 #f1f1f1;
    }

    textarea { width: 100%; height: 200px; font-family: monospace; }
    pre { background: #f4f4f4; padding: 10px; border: 1px solid #ddd; }
    .error { color: red; }
    button { margin: 10px 0; }
    #status { font-style: italic; color: #555; }
    button:disabled { opacity: 0.5; }
  </style>
  <script src="https://cdn.jsdelivr.net/pyodide/v0.23.4/full/pyodide.js"></script>
</head>
<body>
  <h2>Python Browser IDE</h2>
  <textarea id="code">print("Hello, World!")</textarea>
  <script>
    const editor = CodeMirror.fromTextArea(document.getElementById('code'), {
      mode: 'python',
      theme: 'material',
      lineNumbers: true,
      indentUnit: 4,
      tabSize: 4,
      indentWithTabs: true,
      autoCloseBrackets: true,
      matchBrackets: true,
      extraKeys: {
        "Ctrl-Space": "autocomplete"
      }
    });
  </script>
  <br />
  <button id="runBtn" onclick="runCode()">Run</button>
  <span id="status"></span>
  <h3>Output:</h3>
  <pre id="output"></pre>
  <pre id="error" class="error"></pre>

  <script>
    let pyodide;

    async function loadPyodideAndRun() {
      pyodide = await loadPyodide({
        indexURL: "https://cdn.jsdelivr.net/pyodide/v0.23.4/full/"
      });
      console.log("Pyodide loaded");
      await pyodide.loadPackage(["numpy", "matplotlib"]);
      console.log("numpy and matplotlib loaded");
    }

    loadPyodideAndRun();

    async function runCode() {
      const code = editor.getValue();
      const outputEl = document.getElementById("output");
      const errorEl = document.getElementById("error");
      const runBtn = document.getElementById("runBtn");
      const statusEl = document.getElementById("status");

      outputEl.innerText = "";
      errorEl.innerText = "";
      statusEl.innerText = "Running...";
      runBtn.disabled = true;

      if (!pyodide) {
        errorEl.innerText = "Pyodide is still loading, please wait...";
        statusEl.innerText = "";
        runBtn.disabled = false;
        return;
      }

      try {
        pyodide.runPython(`
          import sys
          import io
          sys.stdout = io.StringIO()
        `);
        pyodide.runPython(code);
        const output = pyodide.runPython("sys.stdout.getvalue()");
        outputEl.innerText = output;
      } catch (err) {
        errorEl.innerText = err.message;
      }

      statusEl.innerText = "";
      runBtn.disabled = false;
    }
  </script>
</body>
</html>