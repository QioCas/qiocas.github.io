---
layout: page
title: IDE
---

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Browser IDE</title>
    <style>
        textarea { width: 100%; height: 200px; font-family: monospace; }
        pre { background: #f4f4f4; padding: 10px; border: 1px solid #ddd; }
        .error { color: red; }
        select, button { margin: 10px 0; }
    </style>
    <!-- Load Pyodide for Python execution -->
    <script src="https://cdn.jsdelivr.net/pyodide/v0.23.4/full/pyodide.js"></script>
</head>
<body>
    <h2>Simple Browser IDE</h2>
    <label for="lang">Language:</label>
    <select id="lang">
        <option value="python">Python</option>
        <option value="cpp">C++</option>
    </select>
    <br>
    <textarea id="code" placeholder="Write your code here...">
print("Hello, World!")  # Python example
    </textarea>
    <br>
    <button onclick="runCode()">Run</button>
    <h3>Output:</h3>
    <pre id="output"></pre>
    <pre id="error" class="error"></pre>

    <script>
        let pyodide;

        // Load Pyodide asynchronously
        async function loadPyodideAndRun() {
            pyodide = await loadPyodide({
                indexURL: "https://cdn.jsdelivr.net/pyodide/v0.23.4/full/"
            });
            console.log("Pyodide loaded");
        }

        // Call the loading function when the page loads
        loadPyodideAndRun();

        async function runCode() {
            const code = document.getElementById("code").value;
            const lang = document.getElementById("lang").value;
            const outputEl = document.getElementById("output");
            const errorEl = document.getElementById("error");

            // Clear previous output
            outputEl.innerText = "";
            errorEl.innerText = "";

            if (lang === "python") {
                if (!pyodide) {
                    errorEl.innerText = "Pyodide is still loading, please wait...";
                    return;
                }
                try {
                    // Redirect Python stdout to capture output
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
            } else if (lang === "cpp") {
                errorEl.innerText = "C++ execution is not supported directly in the browser. " +
                    "You need a server-side solution (e.g., Flask with g++) or WebAssembly compilation.";
            }
        }
    </script>
</body>