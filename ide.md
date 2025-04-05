---
layout: page
title: IDE
---

<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Python Browser IDE</title>

  <!-- CodeMirror CSS and JS -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/lib/codemirror.css">
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/lib/codemirror.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/mode/python/python.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/edit/closebrackets.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/show-hint.js"></script>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/show-hint.css">
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/hint/python-hint.js"></script>

  <!-- Pyodide JS -->
  <script src="https://cdn.jsdelivr.net/pyodide/v0.23.4/full/pyodide.js"></script>

  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
    }
    h2 {
      margin-bottom: 10px;
    }
    .CodeMirror {
      border: 1px solid #ddd;
      min-height: 200px;
      font-size: 14px;
    }
    button {
      margin: 10px 0;
      padding: 8px 16px;
      font-size: 14px;
    }
    pre {
      background: #f4f4f4;
      padding: 10px;
      border: 1px solid #ddd;
      overflow-x: auto;
    }
    .error {
      color: red;
    }
    #status {
      font-style: italic;
      color: #555;
      margin-left: 10px;
    }
    button:disabled {
      opacity: 0.5;
    }
  </style>
</head>

<body>
  <h2>Python Browser IDE</h2>
  
  <textarea id="code">print("Hello, World!")</textarea>
  <br />
  <button id="runBtn" onclick="runCode()">Run</button>
  <span id="status"></span>

  <h3>Output:</h3>
  <pre id="output"></pre>

  <h3>Errors:</h3>
  <pre id="error" class="error"></pre>

  <script>
    // Initialize CodeMirror
    const editor = CodeMirror.fromTextArea(document.getElementById('code'), {
      mode: 'python',
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

    // Load Pyodide
    let pyodide;

    async function loadPyodideAndPackages() {
      pyodide = await loadPyodide({
        indexURL: "https://cdn.jsdelivr.net/pyodide/v0.23.4/full/"
      });
      console.log("Pyodide loaded");
      await pyodide.loadPackage(["numpy", "matplotlib"]);
      console.log("numpy and matplotlib loaded");
    }

    loadPyodideAndPackages();

    // Run Python Code
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
        const result = await pyodide.runPythonAsync(code);
        outputEl.innerText = result !== undefined ? result.toString() : "Code executed successfully.";
        statusEl.innerText = "Done.";
      } catch (err) {
        errorEl.innerText = err;
        statusEl.innerText = "Error.";
      } finally {
        runBtn.disabled = false;
      }
    }
  </script>
</body>
</html>
