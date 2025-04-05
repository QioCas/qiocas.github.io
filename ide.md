---
layout: page
title: IDE
---

<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Python Browser IDE</title>

  <!-- CodeMirror Styles & Scripts -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/lib/codemirror.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/show-hint.css">
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/lib/codemirror.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/mode/python/python.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/edit/closebrackets.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/show-hint.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/python-hint.js"></script>

  <!-- Pyodide -->
  <script src="https://cdn.jsdelivr.net/pyodide/v0.23.4/full/pyodide.js"></script>

  <style>
    body {
      font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
      margin: 20px;
      background: #fafafa;
    }
    h2, h3 {
      color: #333;
    }
    .CodeMirror {
      border: 1px solid #ccc;
      height: auto;
      min-height: 200px;
      font-family: monospace;
      font-size: 14px;
    }
    button {
      margin-top: 10px;
      padding: 8px 16px;
      font-size: 14px;
      cursor: pointer;
    }
    button:disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }
    pre {
      background: #f4f4f4;
      padding: 10px;
      border: 1px solid #ddd;
      white-space: pre-wrap;
      overflow-x: auto;
      max-height: 300px;
    }
    .error {
      color: #d33;
      font-weight: bold;
    }
    #status {
      margin-left: 10px;
      font-style: italic;
      color: #666;
    }
  </style>
</head>

<body>
  <h2>Python Browser IDE</h2>
  
  <textarea id="code">print("Hello, World!")</textarea>
  <br />
  <button id="runBtn">Run</button>
  <span id="status">Loading Pyodide...</span>

  <h3>Output:</h3>
  <pre id="output"></pre>

  <h3>Errors:</h3>
  <pre id="error" class="error"></pre>

  <script>
    let pyodide = null;
    let editor = null;

    document.addEventListener("DOMContentLoaded", async () => {
      editor = CodeMirror.fromTextArea(document.getElementById('code'), {
        mode: 'python',
        lineNumbers: true,
        indentUnit: 4,
        tabSize: 4,
        autoCloseBrackets: true,
        matchBrackets: true,
        extraKeys: { "Ctrl-Space": "autocomplete" }
      });

      await loadPyodideAndPackages();
      
      document.getElementById('runBtn').addEventListener('click', runCode);
    });

    async function loadPyodideAndPackages() {
      const statusEl = document.getElementById('status');
      try {
        statusEl.innerText = "Loading Pyodide...";
        pyodide = await loadPyodide({
          indexURL: "https://cdn.jsdelivr.net/pyodide/v0.23.4/full/"
        });
        await pyodide.loadPackage(["numpy", "matplotlib"]);
        statusEl.innerText = "Ready!";
      } catch (error) {
        statusEl.innerText = "Failed to load Pyodide.";
        console.error(error);
      }
    }

    async function runCode() {
      const code = editor.getValue();
      const outputEl = document.getElementById('output');
      const errorEl = document.getElementById('error');
      const runBtn = document.getElementById('runBtn');
      const statusEl = document.getElementById('status');

      outputEl.textContent = "";
      errorEl.textContent = "";
      runBtn.disabled = true;
      statusEl.innerText = "Running...";

      try {
        let result = await pyodide.runPythonAsync(code);
        outputEl.textContent = result ?? "Code executed with no result.";
      } catch (err) {
        console.error(err);
        errorEl.textContent = err.message || "Unknown Error.";
      } finally {
        runBtn.disabled = false;
        statusEl.innerText = "Done.";
      }
    }
  </script>
</body>
</html>
