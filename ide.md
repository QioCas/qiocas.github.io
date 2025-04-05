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
  <script src="https://cdn.jsdelivr.net/npm/codemirror@5.65.16/addon/edit/matchbrackets.js"></script>

  <style>
    .CodeMirror {
      border: 1px solid #ddd;
      height: 400px; /* Fixed height */
      width: 800px;  /* Fixed width */
      max-width: 100%; /* Ensures it doesn't exceed container */
      font-family: monospace;
      font-size: 14px;
      line-height: 1.5;
    }

    .CodeMirror-scroll {
      overflow: auto !important;
      height: 100%; /* Takes full height of container */
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

    textarea { width: 100%; height: 200px; font-family: monospace; display: none; } /* Hide original textarea */
    pre { background: #f4f4f4; padding: 10px; border: 1px solid #ddd; width: 800px; max-width: 100%; }
    .error { color: red; }
    button { margin: 10px 0; }
    #status { font-style: italic; color: #555; }
    button:disabled { opacity: 0.5; }
    
    /* Optional: Center the editor */
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
    }
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
        "Ctrl-Space": "autocomplete",
        "Ctrl-Enter": function(cm) { runCode(); },
        "Alt-Up": "swapLineUp",
        "Alt-Down": "swapLineDown"
      }
    });

    editor.on("inputRead", function(cm, change) {
      if (change.origin !== "paste") {
        const cursor = cm.getCursor();
        const token = cm.getTokenAt(cursor);
        if (/[a-zA-Z.]/.test(change.text[0]) && !/\d/.test(token.string)) {
          CodeMirror.commands.autocomplete(cm, null, {
            completeSingle: false
          });
        }
      }
    });

    editor.on("change", function(cm, change) {
      if (change.origin === "+input" && !cm.state.completionActive) {
        setTimeout(function() {
          cm.execCommand("autocomplete");
        }, 100);
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