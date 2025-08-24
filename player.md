---
layout: page
title: Website
---

<head>
  <meta charset="UTF-8">
  <title>Video Player</title>
  <style>
    body {
      margin: 0;
      display: flex;
      flex-direction: column;
      height: 100vh;
      background: #111;
      color: white;
      font-family: sans-serif;
    }
    iframe {
      flex: 1;
      width: 100%;
      border: none;
    }
    #controls {
      padding: 10px;
      background: #222;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    button {
      background: #444;
      color: white;
      border: none;
      padding: 8px 12px;
      cursor: pointer;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <iframe id="player" src=""></iframe>
  <div id="controls">
    <span id="status">Loading...</span>
    <button onclick="stop()">Stop</button>
  </div>

  <script>
    let videos = [];
    let delay = 30; // seconds
    let i = 0;
    let timer = null;

    function playNext() {
      if (i >= videos.length) {
        document.getElementById("status").innerText = "Done!";
        return;
      }
      let url = videos[i];
      document.getElementById("player").src = url.replace("watch?v=", "embed/") + "?autoplay=1";
      document.getElementById("status").innerText = `Playing ${i+1}/${videos.length}`;
      
      timer = setTimeout(() => {
        i++;
        playNext();
      }, delay * 1000);
    }

    function stop() {
      clearTimeout(timer);
      document.getElementById("player").src = "";
      document.getElementById("status").innerText = "Stopped";
    }

    // Load params from URL (topics + delay)
    const params = new URLSearchParams(window.location.search);
    if (params.get("videos")) {
      videos = JSON.parse(decodeURIComponent(params.get("videos")));
    }
    if (params.get("delay")) {
      delay = parseInt(params.get("delay"));
    }

    if (videos.length > 0) playNext();
  </script>
</body>
