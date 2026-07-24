---
layout: page
title: Flashcard
full-width: true
---

<style>
  .header-section,
  body > footer {
    display: none;
  }

  main.container-fluid {
    padding-top: 0;
  }

  .flashcard-app {
    --flash-bg: #18191a;
    --flash-surface: #202124;
    --flash-surface-2: #242526;
    --flash-border: #393a3b;
    --flash-border-strong: #555b66;
    --flash-text: #e4e6eb;
    --flash-muted: #a9b0bb;
    --flash-accent: #139ee0;
    --flash-danger: #ef6b73;
    --flash-success: #46c28d;
    min-height: calc(100vh - 98px);
    display: grid;
    place-items: center;
    color: var(--flash-text);
  }

  .flashcard-study {
    width: min(920px, calc(100vw - 2rem));
  }

  .flashcard-card-meta {
    position: fixed;
    left: clamp(1rem, 4vw, 3rem);
    right: clamp(1rem, 4vw, 3rem);
    bottom: clamp(1rem, 3vw, 2rem);
    display: flex;
    justify-content: space-between;
    gap: 1rem;
    color: var(--flash-muted);
    font-size: 0.82rem;
    font-weight: 600;
    opacity: 0.72;
    pointer-events: none;
  }

  .flashcard-topic {
    max-width: 70%;
    border: 0;
    background: transparent;
    color: inherit;
    padding: 0;
    font: inherit;
    text-align: left;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    cursor: pointer;
    pointer-events: auto;
  }

  .flashcard-topic:hover,
  .flashcard-topic:focus {
    color: var(--flash-text);
    outline: none;
    text-decoration: underline;
    text-underline-offset: 0.2em;
  }

  .flashcard-answer-form {
    width: 100%;
    min-height: calc(100vh - 180px);
    display: grid;
    align-content: center;
    gap: 1.25rem;
    border: 0;
    background: transparent;
    color: #ffffff;
    padding: clamp(1rem, 5vw, 3rem);
    text-align: center;
    cursor: default;
  }

  .flashcard-prompt {
    display: block;
    width: 100%;
    font-size: clamp(1.45rem, 4vw, 3rem);
    font-weight: 400;
    line-height: 1.5;
    overflow-wrap: anywhere;
    white-space: pre-wrap;
  }

  .flashcard-answer-input {
    width: min(680px, 100%);
    margin: 0 auto;
    border: 0;
    border-bottom: 1px solid rgba(228, 230, 235, 0.42);
    background: transparent;
    color: var(--flash-text);
    padding: 0.55rem 0;
    font: inherit;
    font-size: clamp(1.25rem, 3vw, 2rem);
    text-align: center;
  }

  .flashcard-answer-input:focus {
    border-bottom-color: var(--flash-accent);
    outline: none;
  }

  .flashcard-feedback {
    min-height: 4rem;
    color: var(--flash-muted);
    font-size: clamp(1rem, 2vw, 1.35rem);
    line-height: 1.5;
    overflow-wrap: anywhere;
    white-space: pre-wrap;
  }

  .flashcard-feedback strong {
    display: block;
    font-size: 0.9rem;
    margin-bottom: 0.25rem;
  }

  .flashcard-feedback.correct strong {
    color: var(--flash-success);
  }

  .flashcard-feedback.incorrect strong {
    color: var(--flash-danger);
  }

  .flashcard-empty {
    display: grid;
    gap: 0.6rem;
    color: var(--flash-muted);
    font-size: clamp(1.05rem, 3vw, 1.35rem);
  }

  .flashcard-empty strong {
    color: var(--flash-text);
    font-size: clamp(1.45rem, 4vw, 2.2rem);
  }

  .flashcard-modal {
    position: fixed;
    inset: 0;
    z-index: 2000;
    display: none;
    place-items: center;
    background: rgba(0, 0, 0, 0.58);
    padding: 1rem;
  }

  .flashcard-modal.is-open {
    display: grid;
  }

  .flashcard-settings {
    width: min(720px, 100%);
    max-height: min(760px, calc(100vh - 2rem));
    overflow: auto;
    border: 1px solid var(--flash-border);
    border-radius: 8px;
    background: var(--flash-surface);
    box-shadow: 0 24px 80px rgba(0, 0, 0, 0.45);
  }

  .flashcard-settings-header,
  .flashcard-settings-footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    padding: 1rem;
    border-bottom: 1px solid var(--flash-border);
  }

  .flashcard-settings-footer {
    border-top: 1px solid var(--flash-border);
    border-bottom: 0;
  }

  .flashcard-settings-title {
    margin: 0;
    font-size: 1.15rem;
    line-height: 1.3;
  }

  .flashcard-settings-body {
    display: grid;
    gap: 1.25rem;
    padding: 1rem;
  }

  .flashcard-section {
    display: grid;
    gap: 0.85rem;
  }

  .flashcard-section h3 {
    margin: 0;
    font-size: 1rem;
    line-height: 1.3;
  }

  .flashcard-grid {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 0.85rem;
  }

  .flashcard-field {
    display: grid;
    gap: 0.4rem;
  }

  .flashcard-field.full {
    grid-column: 1 / -1;
  }

  .flashcard-field label {
    margin: 0;
    color: var(--flash-muted);
    font-size: 0.9rem;
    font-weight: 700;
  }

  .flashcard-field input,
  .flashcard-field select,
  .flashcard-field textarea {
    width: 100%;
    border: 1px solid var(--flash-border);
    border-radius: 6px;
    background: var(--flash-bg);
    color: var(--flash-text);
    padding: 0.72rem 0.78rem;
    font: inherit;
  }

  .flashcard-field textarea {
    min-height: 112px;
    resize: vertical;
  }

  .flashcard-field input:focus,
  .flashcard-field select:focus,
  .flashcard-field textarea:focus {
    border-color: var(--flash-accent);
    box-shadow: 0 0 0 3px rgba(19, 158, 224, 0.16);
    outline: none;
  }

  .flashcard-actions {
    display: flex;
    flex-wrap: wrap;
    gap: 0.6rem;
  }

  .flashcard-button {
    min-height: 40px;
    border: 1px solid var(--flash-border-strong);
    border-radius: 6px;
    background: var(--flash-surface-2);
    color: var(--flash-text);
    padding: 0.58rem 0.85rem;
    font-weight: 700;
    line-height: 1.1;
    cursor: pointer;
  }

  .flashcard-button:hover,
  .flashcard-button:focus {
    border-color: var(--flash-accent);
    color: #ffffff;
    outline: none;
  }

  .flashcard-button.primary {
    border-color: var(--flash-accent);
    background: var(--flash-accent);
    color: #ffffff;
  }

  .flashcard-button.danger {
    border-color: rgba(239, 107, 115, 0.75);
    color: #ffd8dc;
  }

  .flashcard-button.icon {
    width: 40px;
    padding: 0;
    font-size: 1.25rem;
  }

  .flashcard-button:disabled {
    opacity: 0.45;
    cursor: not-allowed;
  }

  .flashcard-status {
    min-height: 1.25rem;
    margin: 0;
    color: var(--flash-muted);
    font-size: 0.9rem;
  }

  @media (max-width: 640px) {
    .flashcard-app {
      min-height: calc(100vh - 86px);
    }

    .flashcard-answer-form {
      min-height: calc(100vh - 150px);
    }

    .flashcard-prompt {
      font-size: clamp(1.35rem, 8vw, 2.4rem);
    }

    .flashcard-grid {
      grid-template-columns: 1fr;
    }

    .flashcard-settings-header,
    .flashcard-settings-footer {
      align-items: flex-start;
      flex-direction: column;
    }

    .flashcard-button {
      flex: 1 1 130px;
    }

    .flashcard-button.icon {
      flex: 0 0 40px;
    }
  }
</style>

<div class="flashcard-app" id="flashcard-app">
  <main class="flashcard-study" aria-label="Flashcard">
    <div class="flashcard-card-meta">
      <button class="flashcard-topic" id="flashcard-active-topic" type="button" title="Đổi topic">English</button>
      <span id="flashcard-progress">0 / 0</span>
    </div>

    <form class="flashcard-answer-form" id="flashcard-answer-form">
      <div class="flashcard-prompt" id="flashcard-prompt">
        <span class="flashcard-empty">
          <strong>Chưa có flashcard</strong>
          <span>Dùng Ctrl + Enter để tạo thẻ đầu tiên.</span>
        </span>
      </div>
      <input class="flashcard-answer-input" id="flashcard-answer-input" type="text" autocomplete="off" placeholder="Điền đáp án" hidden>
      <div class="flashcard-feedback" id="flashcard-feedback" aria-live="polite"></div>
    </form>
  </main>

  <div class="flashcard-modal" id="flashcard-settings-modal" role="dialog" aria-modal="true" aria-labelledby="flashcard-settings-title">
    <section class="flashcard-settings">
      <header class="flashcard-settings-header">
        <h2 class="flashcard-settings-title" id="flashcard-settings-title">Settings</h2>
        <button class="flashcard-button icon" id="flashcard-close-settings" type="button" aria-label="Đóng Settings">&times;</button>
      </header>

      <div class="flashcard-settings-body">
        <section class="flashcard-section" aria-labelledby="flashcard-study-settings-title">
          <h3 id="flashcard-study-settings-title">Topic đang học</h3>
          <div class="flashcard-grid">
            <div class="flashcard-field">
              <label for="flashcard-study-topic">Topic</label>
              <select id="flashcard-study-topic"></select>
            </div>
          </div>
        </section>

        <section class="flashcard-section" aria-labelledby="flashcard-topic-settings-title">
          <h3 id="flashcard-topic-settings-title">Quản lý topic</h3>
          <div class="flashcard-grid">
            <div class="flashcard-field">
              <label for="flashcard-new-topic">Topic mới</label>
              <input id="flashcard-new-topic" type="text" placeholder="Ví dụ: Algorithms">
            </div>
            <div class="flashcard-field">
              <label for="flashcard-delete-topic">Topic cần xóa</label>
              <select id="flashcard-delete-topic"></select>
            </div>
          </div>
          <div class="flashcard-actions">
            <button class="flashcard-button" id="flashcard-add-topic" type="button">Thêm topic</button>
            <button class="flashcard-button danger" id="flashcard-delete-topic-button" type="button">Xóa topic</button>
          </div>
        </section>
      </div>

      <footer class="flashcard-settings-footer">
        <p class="flashcard-status" id="flashcard-status" role="status" aria-live="polite"></p>
      </footer>
    </section>
  </div>

  <div class="flashcard-modal" id="flashcard-create-modal" role="dialog" aria-modal="true" aria-labelledby="flashcard-create-title">
    <section class="flashcard-settings">
      <header class="flashcard-settings-header">
        <h2 class="flashcard-settings-title" id="flashcard-create-title">Tạo flashcard</h2>
        <button class="flashcard-button icon" id="flashcard-close-create" type="button" aria-label="Đóng tạo flashcard">&times;</button>
      </header>

      <form id="flashcard-form">
        <div class="flashcard-settings-body">
          <div class="flashcard-grid">
            <div class="flashcard-field full">
              <label for="flashcard-prompt-input">Câu gợi ý</label>
              <textarea id="flashcard-prompt-input" name="prompt" required placeholder="Ví dụ: Một từ tiếng Anh có nghĩa là từ bỏ, bỏ rơi"></textarea>
            </div>
            <div class="flashcard-field full">
              <label for="flashcard-answer-input-create">Đáp án</label>
              <textarea id="flashcard-answer-input-create" name="answer" required placeholder="Ví dụ: abandon"></textarea>
            </div>
            <div class="flashcard-field">
              <label for="flashcard-card-topic">Topic</label>
              <select id="flashcard-card-topic" name="topic" required></select>
            </div>
          </div>
        </div>

        <footer class="flashcard-settings-footer">
          <p class="flashcard-status" id="flashcard-create-status" role="status" aria-live="polite"></p>
          <div class="flashcard-actions">
            <button class="flashcard-button primary" type="submit">Thêm thẻ</button>
            <button class="flashcard-button" id="flashcard-clear-form" type="button">Xóa form</button>
          </div>
        </footer>
      </form>
    </section>
  </div>
</div>

<script>
  (function () {
    var cardsKey = "qiocas.flashcards.v1";
    var settingsKey = "qiocas.flashcards.settings.v1";
    var defaultTopic = "English";
    var cards = [];
    var settings = {
      topics: [defaultTopic],
      activeTopic: defaultTopic,
      lastTopic: defaultTopic
    };
    var currentCardId = "";
    var answerRevealed = false;
    var nextCardTimer = null;
    var lastFocusedElement = null;

    var answerForm = document.getElementById("flashcard-answer-form");
    var promptText = document.getElementById("flashcard-prompt");
    var answerInput = document.getElementById("flashcard-answer-input");
    var feedback = document.getElementById("flashcard-feedback");
    var progress = document.getElementById("flashcard-progress");
    var activeTopicLabel = document.getElementById("flashcard-active-topic");
    var settingsModal = document.getElementById("flashcard-settings-modal");
    var createModal = document.getElementById("flashcard-create-modal");
    var closeSettingsButton = document.getElementById("flashcard-close-settings");
    var closeCreateButton = document.getElementById("flashcard-close-create");
    var studyTopicSelect = document.getElementById("flashcard-study-topic");
    var form = document.getElementById("flashcard-form");
    var promptInput = document.getElementById("flashcard-prompt-input");
    var answerCreateInput = document.getElementById("flashcard-answer-input-create");
    var cardTopicSelect = document.getElementById("flashcard-card-topic");
    var clearFormButton = document.getElementById("flashcard-clear-form");
    var newTopicInput = document.getElementById("flashcard-new-topic");
    var addTopicButton = document.getElementById("flashcard-add-topic");
    var deleteTopicSelect = document.getElementById("flashcard-delete-topic");
    var deleteTopicButton = document.getElementById("flashcard-delete-topic-button");
    var status = document.getElementById("flashcard-status");
    var createStatus = document.getElementById("flashcard-create-status");

    function createId() {
      if (window.crypto && typeof window.crypto.randomUUID === "function") {
        return window.crypto.randomUUID();
      }
      return String(Date.now()) + "-" + Math.random().toString(16).slice(2);
    }

    function uniqTopics(topics) {
      var seen = {};
      var cleanTopics = [];
      (topics || []).forEach(function (topic) {
        var value = typeof topic === "string" ? topic.trim() : "";
        var key = value.toLowerCase();
        if (!value || seen[key]) return;
        seen[key] = true;
        cleanTopics.push(value);
      });
      if (!seen[defaultTopic.toLowerCase()]) cleanTopics.unshift(defaultTopic);
      return cleanTopics;
    }

    function normalizeTopic(topic) {
      var value = typeof topic === "string" ? topic.trim() : "";
      if (!value) return defaultTopic;
      var found = settings.topics.find(function (item) {
        return item.toLowerCase() === value.toLowerCase();
      });
      return found || value;
    }

    function normalizeCard(card) {
      if (!card || typeof card !== "object") return null;
      var prompt = typeof card.prompt === "string" ? card.prompt.trim() : "";
      var answer = typeof card.answer === "string" ? card.answer.trim() : "";
      if (!prompt && typeof card.front === "string") prompt = card.front.trim();
      if (!answer && typeof card.back === "string") answer = card.back.trim();
      if (!prompt || !answer) return null;
      return {
        id: typeof card.id === "string" && card.id ? card.id : createId(),
        prompt: prompt,
        answer: answer,
        topic: normalizeTopic(card.topic),
        createdAt: typeof card.createdAt === "string" && card.createdAt ? card.createdAt : new Date().toISOString()
      };
    }

    function setStatus(message) {
      status.textContent = message || "";
    }

    function setCreateStatus(message) {
      createStatus.textContent = message || "";
    }

    function setFeedback(state, message, answer) {
      feedback.classList.remove("correct", "incorrect");
      if (!message) {
        feedback.textContent = "";
        return;
      }
      feedback.classList.add(state);
      feedback.innerHTML = answer
        ? "<strong>" + message + "</strong>Đáp án đúng là: " + answer
        : "<strong>" + message + "</strong>";
    }

    function clearNextCardTimer() {
      if (!nextCardTimer) return;
      window.clearTimeout(nextCardTimer);
      nextCardTimer = null;
    }

    function writeStorage(key, value) {
      try {
        window.localStorage.setItem(key, JSON.stringify(value));
      } catch (error) {
        setStatus("Không thể lưu dữ liệu trong trình duyệt này.");
      }
    }

    function readStorage(key, fallback) {
      try {
        var raw = window.localStorage.getItem(key);
        return raw ? JSON.parse(raw) : fallback;
      } catch (error) {
        writeStorage(key, fallback);
        return fallback;
      }
    }

    function loadSettings() {
      var stored = readStorage(settingsKey, settings);
      settings.topics = uniqTopics(stored.topics);
      settings.activeTopic = normalizeTopic(stored.activeTopic);
      settings.lastTopic = normalizeTopic(stored.lastTopic);
      if (settings.topics.indexOf(settings.activeTopic) === -1) settings.activeTopic = defaultTopic;
      if (settings.topics.indexOf(settings.lastTopic) === -1) settings.lastTopic = settings.activeTopic;
    }

    function loadCards() {
      var stored = readStorage(cardsKey, []);
      if (!Array.isArray(stored)) {
        writeStorage(cardsKey, []);
        return [];
      }
      return stored.map(normalizeCard).filter(Boolean);
    }

    function saveAll() {
      writeStorage(cardsKey, cards);
      writeStorage(settingsKey, settings);
    }

    function topicCards() {
      return cards.filter(function (card) {
        return card.topic === settings.activeTopic;
      });
    }

    function pickRandomCardId(list, previousId) {
      if (!list.length) {
        return "";
      }
      if (list.length === 1) return list[0].id;
      var nextCard = list[Math.floor(Math.random() * list.length)];
      while (nextCard.id === previousId) {
        nextCard = list[Math.floor(Math.random() * list.length)];
      }
      return nextCard.id;
    }

    function normalizeAnswer(value) {
      return String(value || "").trim().toLowerCase().replace(/\s+/g, " ");
    }

    function currentCard() {
      var list = topicCards();
      if (!list.length) return null;
      var foundCard = list.find(function (card) {
        return card.id === currentCardId;
      });
      if (foundCard) return foundCard;
      currentCardId = pickRandomCardId(list, currentCardId);
      return list.find(function (card) {
        return card.id === currentCardId;
      }) || null;
    }

    function fillSelect(select, selectedValue) {
      select.innerHTML = "";
      settings.topics.forEach(function (topic) {
        var option = document.createElement("option");
        option.value = topic;
        option.textContent = topic;
        if (topic === selectedValue) option.selected = true;
        select.appendChild(option);
      });
    }

    function renderSettingsControls() {
      fillSelect(studyTopicSelect, settings.activeTopic);
      fillSelect(cardTopicSelect, settings.lastTopic);
      fillSelect(deleteTopicSelect, settings.activeTopic);
      updateDeleteTopicState();
    }

    function updateDeleteTopicState() {
      deleteTopicButton.disabled = settings.topics.length <= 1 || deleteTopicSelect.value === defaultTopic;
    }

    function renderCard() {
      var list = topicCards();
      activeTopicLabel.textContent = settings.activeTopic;
      progress.textContent = list.length ? String(list.length) + " thẻ" : "0 thẻ";

      if (!list.length) {
        currentCardId = "";
        promptText.innerHTML = '<span class="flashcard-empty"><strong>Chưa có flashcard</strong><span>Dùng Ctrl + Enter để tạo thẻ cho topic này.</span></span>';
        setFeedback("", "");
        answerInput.value = "";
        answerInput.hidden = true;
        return;
      }

      var card = currentCard();
      promptText.textContent = card.prompt;
      answerInput.hidden = false;
      if (answerRevealed) {
        setFeedback("incorrect", "Đáp án không chính xác", card.answer);
      } else {
        setFeedback("", "");
      }
    }

    function render() {
      renderSettingsControls();
      renderCard();
    }

    function clearForm() {
      form.reset();
      cardTopicSelect.value = settings.lastTopic;
      promptInput.focus();
    }

    function addCard(event) {
      event.preventDefault();
      clearNextCardTimer();
      var selectedTopic = normalizeTopic(cardTopicSelect.value);
      var card = normalizeCard({
        id: createId(),
        prompt: promptInput.value,
        answer: answerCreateInput.value,
        topic: selectedTopic,
        createdAt: new Date().toISOString()
      });

      if (!card) {
        setCreateStatus("Vui lòng nhập đủ câu gợi ý và đáp án.");
        return;
      }

      cards.push(card);
      settings.lastTopic = selectedTopic;
      settings.activeTopic = selectedTopic;
      currentCardId = card.id;
      answerRevealed = false;
      saveAll();
      clearForm();
      setCreateStatus("Đã thêm flashcard.");
      render();
    }

    function addTopic() {
      clearNextCardTimer();
      var topic = newTopicInput.value.trim();
      if (!topic) {
        setStatus("Vui lòng nhập tên topic.");
        return;
      }
      var exists = settings.topics.some(function (item) {
        return item.toLowerCase() === topic.toLowerCase();
      });
      if (exists) {
        setStatus("Topic này đã tồn tại.");
        return;
      }
      settings.topics.push(topic);
      settings.activeTopic = topic;
      settings.lastTopic = topic;
      newTopicInput.value = "";
      currentCardId = "";
      answerRevealed = false;
      saveAll();
      setStatus("Đã thêm topic.");
      render();
    }

    function deleteTopic() {
      clearNextCardTimer();
      var topic = deleteTopicSelect.value;
      if (settings.topics.length <= 1 || !topic) return;
      if (topic === defaultTopic) {
        setStatus("Không thể xóa topic mặc định English.");
        return;
      }
      var cardCount = cards.filter(function (card) {
        return card.topic === topic;
      }).length;
      var message = cardCount
        ? 'Xóa topic "' + topic + '" và ' + cardCount + " flashcard trong topic này?"
        : 'Xóa topic "' + topic + '"?';
      if (!window.confirm(message)) return;

      settings.topics = settings.topics.filter(function (item) {
        return item !== topic;
      });
      cards = cards.filter(function (card) {
        return card.topic !== topic;
      });
      settings.activeTopic = settings.topics[0] || defaultTopic;
      settings.lastTopic = settings.activeTopic;
      currentCardId = "";
      answerRevealed = false;
      saveAll();
      setStatus("Đã xóa topic.");
      render();
    }

    function changeStudyTopic() {
      clearNextCardTimer();
      settings.activeTopic = normalizeTopic(studyTopicSelect.value);
      currentCardId = "";
      answerRevealed = false;
      saveAll();
      render();
    }

    function switchToNextTopic() {
      if (!settings.topics.length) return;
      clearNextCardTimer();
      var currentTopicIndex = settings.topics.indexOf(settings.activeTopic);
      var nextTopicIndex = currentTopicIndex === -1 ? 0 : (currentTopicIndex + 1) % settings.topics.length;
      settings.activeTopic = settings.topics[nextTopicIndex];
      currentCardId = "";
      answerRevealed = false;
      saveAll();
      render();
    }

    function checkAnswer(event) {
      event.preventDefault();
      clearNextCardTimer();
      var card = currentCard();
      if (!card) return;
      if (normalizeAnswer(answerInput.value) === normalizeAnswer(card.answer)) {
        answerRevealed = false;
        setFeedback("correct", "Đáp án chính xác");
        nextCardTimer = window.setTimeout(function () {
          showRandomCard();
          nextCardTimer = null;
        }, 700);
        return;
      }
      answerRevealed = true;
      renderCard();
      answerInput.select();
    }

    function showRandomCard() {
      var list = topicCards();
      if (!list.length) return;
      clearNextCardTimer();
      currentCardId = pickRandomCardId(list, currentCardId);
      answerRevealed = false;
      answerInput.value = "";
      renderCard();
      answerInput.focus();
    }

    function openSettings() {
      lastFocusedElement = document.activeElement;
      settingsModal.classList.add("is-open");
      settingsModal.removeAttribute("aria-hidden");
      setStatus("");
      studyTopicSelect.focus();
    }

    function closeSettings() {
      settingsModal.classList.remove("is-open");
      settingsModal.setAttribute("aria-hidden", "true");
      restoreFocus();
    }

    function openCreate() {
      lastFocusedElement = document.activeElement;
      createModal.classList.add("is-open");
      createModal.removeAttribute("aria-hidden");
      setCreateStatus("");
      cardTopicSelect.value = settings.lastTopic;
      promptInput.focus();
    }

    function closeCreate() {
      createModal.classList.remove("is-open");
      createModal.setAttribute("aria-hidden", "true");
      restoreFocus();
    }

    function restoreFocus() {
      if (lastFocusedElement && typeof lastFocusedElement.focus === "function") {
        lastFocusedElement.focus();
      } else {
        answerInput.focus();
      }
    }

    form.addEventListener("submit", addCard);
    answerForm.addEventListener("submit", checkAnswer);
    clearFormButton.addEventListener("click", clearForm);
    addTopicButton.addEventListener("click", addTopic);
    deleteTopicButton.addEventListener("click", deleteTopic);
    deleteTopicSelect.addEventListener("change", updateDeleteTopicState);
    studyTopicSelect.addEventListener("change", changeStudyTopic);
    activeTopicLabel.addEventListener("click", switchToNextTopic);
    closeSettingsButton.addEventListener("click", closeSettings);
    closeCreateButton.addEventListener("click", closeCreate);
    settingsModal.addEventListener("click", function (event) {
      if (event.target === settingsModal) closeSettings();
    });
    createModal.addEventListener("click", function (event) {
      if (event.target === createModal) closeCreate();
    });

    document.addEventListener("keydown", function (event) {
      if (event.ctrlKey && event.key === ",") {
        event.preventDefault();
        if (settingsModal.classList.contains("is-open")) {
          closeSettings();
        } else {
          closeCreate();
          openSettings();
        }
        return;
      }
      if (event.ctrlKey && event.key === "Enter") {
        event.preventDefault();
        if (createModal.classList.contains("is-open")) {
          if (typeof form.requestSubmit === "function") {
            form.requestSubmit();
          } else {
            form.dispatchEvent(new Event("submit", { cancelable: true }));
          }
        } else {
          closeSettings();
          openCreate();
        }
        return;
      }
      if (event.key === "Escape" && settingsModal.classList.contains("is-open")) {
        closeSettings();
        return;
      }
      if (event.key === "Escape" && createModal.classList.contains("is-open")) {
        closeCreate();
        return;
      }
      if (settingsModal.classList.contains("is-open") || createModal.classList.contains("is-open")) return;
      if (event.key === "ArrowLeft") showRandomCard();
      if (event.key === "ArrowRight") showRandomCard();
    });

    settingsModal.setAttribute("aria-hidden", "true");
    createModal.setAttribute("aria-hidden", "true");
    loadSettings();
    cards = loadCards();
    settings.topics = uniqTopics(settings.topics.concat(cards.map(function (card) {
      return card.topic;
    })));
    if (settings.topics.indexOf(settings.activeTopic) === -1) settings.activeTopic = defaultTopic;
    if (settings.topics.indexOf(settings.lastTopic) === -1) settings.lastTopic = settings.activeTopic;
    saveAll();
    render();
  }());
</script>
