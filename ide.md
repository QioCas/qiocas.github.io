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
        #status { font-style: italic; color: #555; }
        button:disabled { opacity: 0.5; }
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
    <button id="runBtn" onclick="runCode()">Run</button>
    <span id="status"></span>
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

        loadPyodideAndRun();

        async function runCode() {
            const code = document.getElementById("code").value;
            const lang = document.getElementById("lang").value;
            const outputEl = document.getElementById("output");
            const errorEl = document.getElementById("error");
            const runBtn = document.getElementById("runBtn");
            const statusEl = document.getElementById("status");

            // Clear previous output and set loading state
            outputEl.innerText = "";
            errorEl.innerText = "";
            statusEl.innerText = "Running...";
            runBtn.disabled = true;

            if (lang === "python") {
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
            } else if (lang === "cpp") {
                const requestBody = {
                    "cmd": "g++ -o a.out -x c++ - && ./a.out", // Compile and run C++
                    "src": code
                };

                try {
                    const response = await fetch("http://coliru.stacked-crooked.com/compile", {
                        method: "POST",
                        headers: { "Content-Type": "application/json" },
                        body: JSON.stringify(requestBody)
                    });

                    if (!response.ok) {
                        const text = await response.text();
                        throw new Error(`Coliru returned status ${response.status}: ${text}`);
                    }

                    const output = await response.text();
                    outputEl.innerText = output || "No output";
                } catch (err) {
                    errorEl.innerText = `Error with Coliru: ${err.message}`;
                }
            }

            // Reset status and button
            statusEl.innerText = "";
            runBtn.disabled = false;
        }
    </script>
</body>