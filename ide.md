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
                // Use Wandbox API for C++ compilation
                const requestBody = {
                    code: code,
                    compiler: "gcc-14.1.0", // Latest GCC version on Wandbox as of now
                    options: "", // Optional compiler flags
                    "compiler-option-raw": "", // Raw compiler options
                    "runtime-option-raw": "" // Runtime options
                };

                try {
                    const response = await fetch("https://wandbox.org/api/compile.json", {
                        method: "POST",
                        headers: { "Content-Type": "application/json" },
                        body: JSON.stringify(requestBody)
                    });
                    const data = await response.json();

                    if (data.status === "0") {
                        outputEl.innerText = data.program_message || "No output";
                    } else {
                        errorEl.innerText = data.compiler_error || data.program_error || "Compilation failed";
                    }
                } catch (err) {
                    errorEl.innerText = "Error connecting to Wandbox: " + err.message;
                }
            }
        }
    </script>
</body>