---
layout: page
title: Topics
---

<head>
  <meta charset="UTF-8">
  <title>Topics</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    h1 { text-align: center; }
    ul { list-style: none; padding: 0; }
    li { margin: 10px 0; }
    a.topic-link { cursor: pointer; color: blue; text-decoration: underline; }

    /* Modal */
    .modal { display: none; position: fixed; top:0; left:0; width:100%; height:100%;
             background: rgba(0,0,0,0.7); justify-content:center; align-items:center; z-index: 1000; }
    .modal-content { background:#fff; padding:20px; border-radius:8px;
                     max-width:700px; width:90%; max-height:80%; overflow:auto; position: relative; }
    #closeModal { position: absolute; right: 15px; top: 10px; cursor:pointer; font-size:20px; }
    .video-list-grid { display:grid; grid-template-columns: repeat(auto-fill, minmax(160px,1fr));
                       gap:15px; padding:10px 0; }
    .video-card { cursor:pointer; text-align:center; }
    .video-card img { width:100%; border-radius:6px; }
    .video-card p { margin:6px 0 0; font-size:0.9em; color:#333; }
    #autoPlayBtn { margin-top: 10px; padding:8px 16px; background:#007bff; color:#fff; border:none; border-radius:6px; cursor:pointer; }
    #autoPlayBtn:hover { background:#0056b3; }

    /* Add topic form */
    #addTopicForm { margin-top: 30px; border-top:1px solid #ccc; padding-top:20px; }
    #addTopicForm input, #addTopicForm textarea { width:100%; padding:8px; margin:6px 0; }
    #addTopicForm button { padding:8px 16px; background:green; color:#fff; border:none; border-radius:6px; cursor:pointer; }
    #addTopicForm button:hover { background:darkgreen; }
  </style>
</head>
<body>
  <h1>Topics</h1>
  <ul id="topicList"></ul>

  <!-- Modal -->
  <div id="videoModal" class="modal">
    <div class="modal-content">
      <span id="closeModal">&times;</span>
      <h2 id="modalTitle"></h2>
      <div id="videoList" class="video-list-grid"></div>
      <button id="autoPlayBtn">Autoplay All</button>
    </div>
  </div>

  <!-- Add Topic -->
  <div id="addTopicForm">
    <h2>Add New Topic</h2>
    <input type="text" id="topicName" placeholder="Topic Name">
    <textarea id="topicVideos" placeholder="Enter YouTube links separated by new lines"></textarea>
    <button onclick="addTopic()">Add Topic</button>
  </div>

  <script>
    // Load topics from localStorage
    let topics = JSON.parse(localStorage.getItem("topics")) || [
      {
        name: "Math Basics",
        videos: [
          "https://www.youtube.com/watch?v=dQw4w9WgXcQ",
          "https://www.youtube.com/watch?v=3JZ_D3ELwOQ"
        ]
      },
      {
        name: "Algorithms",
        videos: [
          "https://www.youtube.com/watch?v=8hly31xKli0",
          "https://www.youtube.com/watch?v=09_LlHjoEiY"
        ]
      }
    ];

    const topicList = document.getElementById("topicList");
    const modal = document.getElementById("videoModal");
    const closeModal = document.getElementById("closeModal");
    const modalTitle = document.getElementById("modalTitle");
    const videoList = document.getElementById("videoList");
    const autoPlayBtn = document.getElementById("autoPlayBtn");

    function saveTopics() {
      localStorage.setItem("topics", JSON.stringify(topics));
    }

    // Render topics
    function renderTopics() {
      topicList.innerHTML = '';
      topics.forEach((t, idx) => {
        const li = document.createElement("li");
        const link = document.createElement("a");
        link.textContent = t.name;
        link.className = "topic-link";
        link.onclick = () => openTopic(idx);
        li.appendChild(link);
        topicList.appendChild(li);
      });
    }

    // Fetch metadata & show modal
    async function openTopic(topicIndex) {
      const topic = topics[topicIndex];
      modalTitle.textContent = topic.name;
      videoList.innerHTML = '';

      for (let url of topic.videos) {
        const oembedUrl = `https://www.youtube.com/oembed?format=json&url=${encodeURIComponent(url)}`;
        try {
          const resp = await fetch(oembedUrl);
          const data = await resp.json();

          const card = document.createElement('div');
          card.className = 'video-card';
          card.innerHTML = `
            <img src="${data.thumbnail_url}" alt="${data.title}">
            <p>${data.title}</p>
          `;
          card.onclick = () => window.open(url, '_blank');
          videoList.appendChild(card);

        } catch (err) {
          console.error("Failed to fetch metadata for", url, err);
          const fallback = document.createElement('div');
          fallback.className = 'video-card';
          fallback.textContent = url;
          videoList.appendChild(fallback);
        }
      }

      autoPlayBtn.onclick = () => autoPlay(topicIndex);
      modal.style.display = "flex";
    }

    // Close modal
    closeModal.onclick = () => modal.style.display = "none";
    window.onclick = e => { if (e.target === modal) modal.style.display = "none"; };

    // Autoplay logic
    function autoPlay(topicIndex) {
      const delay = parseInt(prompt("Seconds per video:", "30"), 10);
      if (!delay || isNaN(delay) || delay < 5) {
        alert("Please enter at least 5 seconds.");
        return;
      }
      const urls = topics[topicIndex].videos;
      let i = 0;
      function step() {
        if (i >= urls.length) return;
        window.open(urls[i], "_blank");
        i++;
        setTimeout(step, delay * 1000);
      }
      step();
    }

    // Add new topic
    function addTopic() {
      const name = document.getElementById("topicName").value.trim();
      const videos = document.getElementById("topicVideos").value.trim().split("\n").map(v => v.trim()).filter(v => v);

      if (!name || videos.length === 0) {
        alert("Please enter topic name and at least one video URL.");
        return;
      }

      topics.push({ name, videos });
      saveTopics();
      renderTopics();

      document.getElementById("topicName").value = "";
      document.getElementById("topicVideos").value = "";
    }

    renderTopics();
  </script>
</body>
