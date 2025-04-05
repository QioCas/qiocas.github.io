---
layout: page
title: IDE
---

<html lang="en">
<head>
  <!-- Add this in <head> -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/lib/codemirror.css">
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/lib/codemirror.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/mode/python/python.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/edit/closebrackets.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/show-hint.js"></script>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/show-hint.css">
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/python-hint.js"></script>

  <style>
    .CodeMirror {
      background-color: #1e1e1e; /* VS Code dark background */
      color: #d4d4d4;           /* VS Code light text */
      border: 1px solid #333;    /* Darker border */
      height: auto;
      min-height: 200px;
      font-family: monospace;
      font-size: 14px;
    }
  </style>

  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Simple Python Browser IDE</title>
  <style>
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
      theme: 'material',  // <<<=== add this
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
      // Load external libraries
      await pyodide.loadPackage(["numpy", "matplotlib"]);
      console.log("numpy and matplotlib loaded");
    }

    loadPyodideAndRun();

    async function runCode() {
      const code = editor.getValue();  // <<==== get code from CodeMirror
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
