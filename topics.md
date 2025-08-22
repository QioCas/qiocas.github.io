---
layout: page
title: Topics
---

<head>
  <meta charset="UTF-8">
  <title>Topics Dashboard</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #121212;
      color: #eee;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #ffcc00;
    }
    .topic {
      background: #1e1e1e;
      border-radius: 8px;
      padding: 15px;
      margin-bottom: 20px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.5);
    }
    .topic h2 {
      margin: 0 0 10px;
    }
    .topic button {
      margin-right: 5px;
      margin-bottom: 5px;
    }
    .video-list {
      list-style: none;
      padding-left: 20px;
    }
    .video-list li {
      margin: 4px 0;
    }
    .btn {
      background: #ffcc00;
      border: none;
      padding: 6px 12px;
      border-radius: 4px;
      cursor: pointer;
    }
    .btn:hover {
      background: #ffaa00;
    }
  </style>
</head>

<body>
  <h1>📂 My YouTube Topics</h1>
  <div id="topics"></div>

  <button class="btn" onclick="addTopic()">➕ Add Topic</button>

  <script>
    let topics = JSON.parse(localStorage.getItem('topics') || '[]');

    function saveTopics() {
      localStorage.setItem('topics', JSON.stringify(topics));
      renderTopics();
    }

    function addTopic() {
      const name = prompt("Enter topic name:");
      if (name) {
        topics.push({ name, videos: [] });
        saveTopics();
      }
    }

    function deleteTopic(index) {
      if (confirm("Delete this topic?")) {
        topics.splice(index, 1);
        saveTopics();
      }
    }

    function addVideo(topicIndex) {
      const url = prompt("Enter YouTube video URL:");
      if (url) {
        topics[topicIndex].videos.push(url);
        saveTopics();
      }
    }

    function deleteVideo(topicIndex, videoIndex) {
      topics[topicIndex].videos.splice(videoIndex, 1);
      saveTopics();
    }

    function autoPlay(topicIndex) {
      const delay = parseInt(prompt("Seconds per video:", "30"));
      if (!delay || delay < 5) {
        alert("Please enter at least 5 seconds.");
        return;
      }
      let i = 0;
      function openNext() {
        if (i >= topics[topicIndex].videos.length) return;
        let win = window.open(topics[topicIndex].videos[i], "_blank");
        setTimeout(() => {
          win.close();
          i++;
          openNext();
        }, delay * 1000);
      }
      openNext();
    }

    function renderTopics() {
      const container = document.getElementById('topics');
      container.innerHTML = "";
      topics.forEach((topic, tIndex) => {
        const div = document.createElement('div');
        div.className = "topic";
        div.innerHTML = `
          <h2>${topic.name}</h2>
          <button class="btn" onclick="addVideo(${tIndex})">➕ Add Video</button>
          <button class="btn" onclick="autoPlay(${tIndex})">▶ Auto Play</button>
          <button class="btn" onclick="deleteTopic(${tIndex})">🗑 Delete Topic</button>
          <ul class="video-list">
            ${topic.videos.map((v, vIndex) => `
              <li>
                <a href="${v}" target="_blank">${v}</a>
                <button class="btn" onclick="deleteVideo(${tIndex}, ${vIndex})">❌</button>
              </li>
            `).join('')}
          </ul>
        `;
        container.appendChild(div);
      });
    }

    renderTopics();
  </script>
</body>