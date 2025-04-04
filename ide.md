<html lang="en">
<head>
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
  <textarea id="code" placeholder="Write your Python code here...">print("Hello, World!")</textarea>
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
    }

    loadPyodideAndRun();

    async function runCode() {
      const code = document.getElementById("code").value;
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
