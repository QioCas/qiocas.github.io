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
    text-align: left;
    cursor: default;
  }

  .flashcard-prompt {
    display: block;
    width: 100%;
    font-size: clamp(1.1rem, 2.2vw, 1.85rem);
    font-weight: 400;
    line-height: 1.5;
    overflow-wrap: anywhere;
    white-space: pre-wrap;
  }

  .flashcard-answer-input {
    width: min(680px, 100%);
    margin: 0;
    border: 0;
    border-bottom: 1px solid rgba(228, 230, 235, 0.42);
    background: transparent;
    color: var(--flash-text);
    padding: 0.55rem 0;
    font: inherit;
    font-size: clamp(1.05rem, 2vw, 1.45rem);
    text-align: left;
  }

  .flashcard-answer-hint {
    color: var(--flash-muted);
    font-size: clamp(0.95rem, 1.8vw, 1.2rem);
    line-height: 1.5;
    overflow-wrap: anywhere;
    white-space: pre-wrap;
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

  .flashcard-debug {
    border-left: 2px solid var(--flash-accent);
    color: var(--flash-muted);
    font-size: 0.82rem;
    line-height: 1.5;
    margin: 0;
    max-width: min(760px, 100%);
    overflow: auto;
    padding-left: 0.8rem;
    white-space: pre-wrap;
  }

  .flashcard-due-drop {
    color: var(--flash-muted);
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    max-width: min(760px, 100%);
  }

  .flashcard-due-drop span {
    flex: 1 1 100%;
    font-size: 0.92rem;
  }

  .flashcard-due-drop button {
    min-height: 34px;
    border: 1px solid var(--flash-border-strong);
    border-radius: 6px;
    background: transparent;
    color: var(--flash-text);
    padding: 0.35rem 0.65rem;
    font: inherit;
    font-size: 0.9rem;
    cursor: pointer;
  }

  .flashcard-due-drop button:hover,
  .flashcard-due-drop button:focus {
    border-color: var(--flash-accent);
    outline: none;
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

  .flashcard-formula {
    margin: 0;
    border: 1px solid var(--flash-border);
    border-radius: 6px;
    background: var(--flash-bg);
    color: var(--flash-muted);
    padding: 0.8rem;
    font-size: 0.82rem;
    line-height: 1.5;
    overflow: auto;
    white-space: pre;
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
      font-size: clamp(1.05rem, 5.5vw, 1.55rem);
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
      <div class="flashcard-answer-hint" id="flashcard-answer-hint"></div>
      <input class="flashcard-answer-input" id="flashcard-answer-input" type="text" autocomplete="off" placeholder="Điền đáp án" hidden>
      <div class="flashcard-feedback" id="flashcard-feedback" aria-live="polite"></div>
      <div class="flashcard-due-drop" id="flashcard-due-drop" hidden>
        <span>Drop due ngẫu nhiên theo số card hiện tại:</span>
        <button type="button" data-ratio="0.25">25%</button>
        <button type="button" data-ratio="0.5">50%</button>
        <button type="button" data-ratio="0.75">75%</button>
        <button type="button" data-ratio="1">100%</button>
      </div>
      <pre class="flashcard-debug" id="flashcard-debug" hidden></pre>
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

        <section class="flashcard-section" aria-labelledby="flashcard-card-settings-title">
          <h3 id="flashcard-card-settings-title">Quản lý thẻ</h3>
          <div class="flashcard-grid">
            <div class="flashcard-field full">
              <label for="flashcard-delete-card">Thẻ cần xóa</label>
              <select id="flashcard-delete-card"></select>
            </div>
          </div>
          <div class="flashcard-actions">
            <button class="flashcard-button danger" id="flashcard-delete-card-button" type="button">Xóa thẻ</button>
          </div>
        </section>

        <section class="flashcard-section" aria-labelledby="flashcard-srs-settings-title">
          <h3 id="flashcard-srs-settings-title">SRS</h3>
          <div class="flashcard-field full">
            <label>Công thức</label>
            <pre class="flashcard-formula">new card: dueAt = now + newDelay
wrong answer: dueAt = now + wrongDelay
correct answer: dueAt = now + correctDelay × correctGrowth^(correctStreak - 1)

weight = baseWeight
       + wrongCount × wrongWeight
       + recentWrong × recentWrongWeight
       - correctStreak × streakPenalty
       + overdueBonus

weight = clamp(weight, minWeight, maxWeight)</pre>
          </div>
          <div class="flashcard-grid">
            <div class="flashcard-field">
              <label for="flashcard-new-delay">newDelay</label>
              <input id="flashcard-new-delay" type="text" inputmode="text" placeholder="10m">
            </div>
            <div class="flashcard-field">
              <label for="flashcard-wrong-delay">wrongDelay</label>
              <input id="flashcard-wrong-delay" type="text" inputmode="text" placeholder="2m">
            </div>
            <div class="flashcard-field">
              <label for="flashcard-correct-delay">correctDelay</label>
              <input id="flashcard-correct-delay" type="text" inputmode="text" placeholder="30m">
            </div>
            <div class="flashcard-field">
              <label for="flashcard-correct-growth">correctGrowth</label>
              <input id="flashcard-correct-growth" type="number" step="0.1" min="1">
            </div>
            <div class="flashcard-field">
              <label for="flashcard-base-weight">baseWeight</label>
              <input id="flashcard-base-weight" type="number" step="0.1" min="0">
            </div>
            <div class="flashcard-field">
              <label for="flashcard-wrong-weight">wrongWeight</label>
              <input id="flashcard-wrong-weight" type="number" step="0.1" min="0">
            </div>
            <div class="flashcard-field">
              <label for="flashcard-recent-wrong-weight">recentWrongWeight</label>
              <input id="flashcard-recent-wrong-weight" type="number" step="0.1" min="0">
            </div>
            <div class="flashcard-field">
              <label for="flashcard-streak-penalty">streakPenalty</label>
              <input id="flashcard-streak-penalty" type="number" step="0.1" min="0">
            </div>
            <div class="flashcard-field">
              <label for="flashcard-overdue-weight">overdueWeight</label>
              <input id="flashcard-overdue-weight" type="number" step="0.1" min="0">
            </div>
            <div class="flashcard-field">
              <label for="flashcard-min-weight">minWeight</label>
              <input id="flashcard-min-weight" type="number" step="0.1" min="0">
            </div>
            <div class="flashcard-field">
              <label for="flashcard-max-weight">maxWeight</label>
              <input id="flashcard-max-weight" type="number" step="0.1" min="0.1">
            </div>
          </div>
          <div class="flashcard-actions">
            <button class="flashcard-button primary" id="flashcard-save-srs" type="button">Lưu SRS</button>
            <button class="flashcard-button" id="flashcard-reset-srs" type="button">Reset default</button>
          </div>
        </section>
      </div>

      <footer class="flashcard-settings-footer">
        <p class="flashcard-status" id="flashcard-status" role="status" aria-live="polite"></p>
        <button class="flashcard-button danger" id="flashcard-reset-all" type="button">Reset toàn bộ</button>
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
    var defaultSrs = {
      newDelay: "10m",
      wrongDelay: "2m",
      correctDelay: "30m",
      correctGrowth: 2,
      baseWeight: 1,
      wrongWeight: 2,
      recentWrongWeight: 4,
      streakPenalty: 0.8,
      overdueWeight: 1,
      minWeight: 0.2,
      maxWeight: 10
    };
    var cards = [];
    var settings = {
      topics: [defaultTopic],
      activeTopic: defaultTopic,
      lastTopic: defaultTopic,
      srs: Object.assign({}, defaultSrs)
    };
    var currentCardId = "";
    var lastSeenCardId = "";
    var answerRevealed = false;
    var currentAttemptFailed = false;
    var debugMode = false;
    var nextCardTimer = null;
    var lastFocusedElement = null;

    var answerForm = document.getElementById("flashcard-answer-form");
    var promptText = document.getElementById("flashcard-prompt");
    var answerHint = document.getElementById("flashcard-answer-hint");
    var answerInput = document.getElementById("flashcard-answer-input");
    var feedback = document.getElementById("flashcard-feedback");
    var dueDrop = document.getElementById("flashcard-due-drop");
    var debugPanel = document.getElementById("flashcard-debug");
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
    var deleteCardSelect = document.getElementById("flashcard-delete-card");
    var deleteCardButton = document.getElementById("flashcard-delete-card-button");
    var newDelayInput = document.getElementById("flashcard-new-delay");
    var wrongDelayInput = document.getElementById("flashcard-wrong-delay");
    var correctDelayInput = document.getElementById("flashcard-correct-delay");
    var correctGrowthInput = document.getElementById("flashcard-correct-growth");
    var baseWeightInput = document.getElementById("flashcard-base-weight");
    var wrongWeightInput = document.getElementById("flashcard-wrong-weight");
    var recentWrongWeightInput = document.getElementById("flashcard-recent-wrong-weight");
    var streakPenaltyInput = document.getElementById("flashcard-streak-penalty");
    var overdueWeightInput = document.getElementById("flashcard-overdue-weight");
    var minWeightInput = document.getElementById("flashcard-min-weight");
    var maxWeightInput = document.getElementById("flashcard-max-weight");
    var saveSrsButton = document.getElementById("flashcard-save-srs");
    var resetSrsButton = document.getElementById("flashcard-reset-srs");
    var resetAllButton = document.getElementById("flashcard-reset-all");
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

    function isDurationString(value) {
      return /^(\d+(?:\.\d+)?)\s*([mhd])?$/.test(String(value || "").trim().toLowerCase());
    }

    function normalizeDurationString(value, fallbackValue) {
      var source = String(value || "").trim().toLowerCase();
      return isDurationString(source) ? source : fallbackValue;
    }

    function parseDurationMs(value, fallbackValue) {
      var source = String(value || "").trim().toLowerCase();
      var fallbackSource = String(fallbackValue || "").trim().toLowerCase();
      if (!isDurationString(source)) source = isDurationString(fallbackSource) ? fallbackSource : "0m";
      var match = source.match(/^(\d+(?:\.\d+)?)\s*([mhd])?$/);
      if (!match) return 0;
      var amount = Number(match[1]);
      if (!Number.isFinite(amount) || amount < 0) return 0;
      var unit = match[2] || "m";
      if (unit === "d") return amount * 24 * 60 * 60 * 1000;
      if (unit === "h") return amount * 60 * 60 * 1000;
      return amount * 60 * 1000;
    }

    function parseNumber(value, fallbackValue, minValue) {
      var number = Number(value);
      if (!Number.isFinite(number)) return fallbackValue;
      if (typeof minValue === "number" && number < minValue) return fallbackValue;
      return number;
    }

    function clamp(value, minValue, maxValue) {
      return Math.min(Math.max(value, minValue), maxValue);
    }

    function normalizeSrsConfig(srs) {
      var source = srs && typeof srs === "object" ? srs : {};
      var nextSrs = {
        newDelay: normalizeDurationString(source.newDelay, defaultSrs.newDelay),
        wrongDelay: normalizeDurationString(source.wrongDelay, defaultSrs.wrongDelay),
        correctDelay: normalizeDurationString(source.correctDelay, defaultSrs.correctDelay),
        correctGrowth: parseNumber(source.correctGrowth, defaultSrs.correctGrowth, 1),
        baseWeight: parseNumber(source.baseWeight, defaultSrs.baseWeight, 0),
        wrongWeight: parseNumber(source.wrongWeight, defaultSrs.wrongWeight, 0),
        recentWrongWeight: parseNumber(source.recentWrongWeight, defaultSrs.recentWrongWeight, 0),
        streakPenalty: parseNumber(source.streakPenalty, defaultSrs.streakPenalty, 0),
        overdueWeight: parseNumber(source.overdueWeight, defaultSrs.overdueWeight, 0),
        minWeight: parseNumber(source.minWeight, defaultSrs.minWeight, 0),
        maxWeight: parseNumber(source.maxWeight, defaultSrs.maxWeight, 0.1)
      };
      if (nextSrs.maxWeight < nextSrs.minWeight) nextSrs.maxWeight = nextSrs.minWeight;
      return nextSrs;
    }

    function normalizeTimestamp(value, fallbackValue) {
      var time = Date.parse(value);
      if (Number.isFinite(time)) return new Date(time).toISOString();
      return fallbackValue;
    }

    function defaultStats(nowMs, dueDelayMs) {
      return {
        seenCount: 0,
        correctCount: 0,
        wrongCount: 0,
        recentWrong: 0,
        correctStreak: 0,
        lastSeenAt: null,
        lastAnsweredAt: null,
        dueAt: new Date(nowMs + dueDelayMs).toISOString()
      };
    }

    function normalizeStats(stats, nowMs) {
      var source = stats && typeof stats === "object" ? stats : {};
      var defaults = defaultStats(nowMs, 0);
      return {
        seenCount: Math.max(0, Math.floor(parseNumber(source.seenCount, defaults.seenCount, 0))),
        correctCount: Math.max(0, Math.floor(parseNumber(source.correctCount, defaults.correctCount, 0))),
        wrongCount: Math.max(0, Math.floor(parseNumber(source.wrongCount, defaults.wrongCount, 0))),
        recentWrong: Math.max(0, Math.floor(parseNumber(source.recentWrong, defaults.recentWrong, 0))),
        correctStreak: Math.max(0, Math.floor(parseNumber(source.correctStreak, defaults.correctStreak, 0))),
        lastSeenAt: source.lastSeenAt ? normalizeTimestamp(source.lastSeenAt, null) : null,
        lastAnsweredAt: source.lastAnsweredAt ? normalizeTimestamp(source.lastAnsweredAt, null) : null,
        dueAt: normalizeTimestamp(source.dueAt, defaults.dueAt)
      };
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
        stats: normalizeStats(card.stats, Date.now()),
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

    function resetAttemptState() {
      answerRevealed = false;
      currentAttemptFailed = false;
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
      settings.srs = normalizeSrsConfig(stored.srs);
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

    function dueTime(card) {
      var time = Date.parse(card.stats && card.stats.dueAt);
      return Number.isFinite(time) ? time : 0;
    }

    function lastSeenTime(card) {
      var time = Date.parse(card.stats && card.stats.lastSeenAt);
      return Number.isFinite(time) ? time : 0;
    }

    function overdueBonus(card, nowMs) {
      var overdueMs = Math.max(0, nowMs - dueTime(card));
      var correctDelayMs = parseDurationMs(settings.srs.correctDelay, defaultSrs.correctDelay);
      if (!correctDelayMs) return 0;
      return Math.min(1, overdueMs / correctDelayMs) * settings.srs.overdueWeight;
    }

    function cardWeight(card, nowMs) {
      var stats = card.stats || normalizeStats(null, nowMs);
      var weight = settings.srs.baseWeight
        + stats.wrongCount * settings.srs.wrongWeight
        + stats.recentWrong * settings.srs.recentWrongWeight
        - stats.correctStreak * settings.srs.streakPenalty
        + overdueBonus(card, nowMs);
      return clamp(weight, settings.srs.minWeight, settings.srs.maxWeight);
    }

    function formatLocalDateTime(value) {
      if (!value) return "null";
      var date = value instanceof Date ? value : new Date(value);
      var time = date.getTime();
      if (!Number.isFinite(time)) return "invalid";
      var offsetMinutes = -date.getTimezoneOffset();
      var offsetSign = offsetMinutes >= 0 ? "+" : "-";
      var absOffset = Math.abs(offsetMinutes);
      var offsetHours = String(Math.floor(absOffset / 60)).padStart(2, "0");
      var offsetMins = String(absOffset % 60).padStart(2, "0");
      return [
        date.getFullYear(),
        String(date.getMonth() + 1).padStart(2, "0"),
        String(date.getDate()).padStart(2, "0")
      ].join("-") + " " + [
        String(date.getHours()).padStart(2, "0"),
        String(date.getMinutes()).padStart(2, "0"),
        String(date.getSeconds()).padStart(2, "0")
      ].join(":") + " UTC" + offsetSign + offsetHours + ":" + offsetMins;
    }

    function formatMs(ms) {
      if (!Number.isFinite(ms)) return "n/a";
      var sign = ms < 0 ? "-" : "";
      var value = Math.abs(ms);
      var minutes = Math.round(value / 60000);
      if (minutes < 60) return sign + minutes + "m";
      var hours = Math.round(minutes / 60);
      if (hours < 48) return sign + hours + "h";
      return sign + Math.round(hours / 24) + "d";
    }

    function debugBreakdown(card) {
      if (!card) return "";
      var nowMs = Date.now();
      var stats = card.stats || normalizeStats(null, nowMs);
      var basePart = settings.srs.baseWeight;
      var wrongPart = stats.wrongCount * settings.srs.wrongWeight;
      var recentWrongPart = stats.recentWrong * settings.srs.recentWrongWeight;
      var streakPart = stats.correctStreak * settings.srs.streakPenalty;
      var overduePart = overdueBonus(card, nowMs);
      var rawWeight = basePart + wrongPart + recentWrongPart - streakPart + overduePart;
      var finalWeight = cardWeight(card, nowMs);
      var dueMs = dueTime(card) - nowMs;
      var seenCooldownMs = parseDurationMs(settings.srs.wrongDelay, defaultSrs.wrongDelay);
      var seenCooldownLeftMs = Math.max(0, seenCooldownMs - (nowMs - lastSeenTime(card)));

      return [
        "DEBUG MODE (Ctrl + Shift + D)",
        "id = " + card.id,
        "now = " + formatLocalDateTime(new Date(nowMs)),
        "dueAt = " + formatLocalDateTime(stats.dueAt) + " (" + (dueMs <= 0 ? "due " + formatMs(-dueMs) + " ago" : "due in " + formatMs(dueMs)) + ")",
        "lastSeenAt = " + formatLocalDateTime(stats.lastSeenAt),
        "lastAnsweredAt = " + formatLocalDateTime(stats.lastAnsweredAt),
        "seenCooldownLeft = " + formatMs(seenCooldownLeftMs),
        "",
        "seenCount = " + stats.seenCount,
        "correctCount = " + stats.correctCount,
        "wrongCount = " + stats.wrongCount,
        "recentWrong = " + stats.recentWrong,
        "correctStreak = " + stats.correctStreak,
        "currentAttemptFailed = " + currentAttemptFailed,
        "",
        "weight = baseWeight",
        "       + wrongCount × wrongWeight",
        "       + recentWrong × recentWrongWeight",
        "       - correctStreak × streakPenalty",
        "       + overdueBonus",
        "",
        "baseWeight = " + basePart,
        "wrongCount × wrongWeight = " + stats.wrongCount + " × " + settings.srs.wrongWeight + " = " + wrongPart,
        "recentWrong × recentWrongWeight = " + stats.recentWrong + " × " + settings.srs.recentWrongWeight + " = " + recentWrongPart,
        "correctStreak × streakPenalty = " + stats.correctStreak + " × " + settings.srs.streakPenalty + " = " + streakPart,
        "overdueBonus = " + overduePart.toFixed(3),
        "rawWeight = " + rawWeight.toFixed(3),
        "finalWeight = clamp(rawWeight, " + settings.srs.minWeight + ", " + settings.srs.maxWeight + ") = " + finalWeight.toFixed(3)
      ].join("\n");
    }

    function renderDebug(card) {
      debugPanel.hidden = !debugMode;
      if (!debugMode) {
        debugPanel.textContent = "";
        return;
      }
      debugPanel.textContent = card ? debugBreakdown(card) : "DEBUG MODE (Ctrl + Shift + D)\nKhông có thẻ trong topic hiện tại.";
    }

    function weightedRandomCard(candidates, nowMs) {
      var totalWeight = candidates.reduce(function (sum, card) {
        return sum + cardWeight(card, nowMs);
      }, 0);
      if (totalWeight <= 0) return candidates[Math.floor(Math.random() * candidates.length)];
      var point = Math.random() * totalWeight;
      for (var i = 0; i < candidates.length; i += 1) {
        point -= cardWeight(candidates[i], nowMs);
        if (point <= 0) return candidates[i];
      }
      return candidates[candidates.length - 1];
    }

    function pickNextCard(list, previousId) {
      if (!list.length) return null;
      var nowMs = Date.now();
      var pool = list.filter(function (card) {
        return dueTime(card) <= nowMs;
      });
      if (!pool.length) {
        return null;
      }
      if (pool.length > 1) {
        pool = pool.filter(function (card) {
          return card.id !== previousId;
        });
      }
      if (pool.length === 1 && pool[0].id === previousId && list.length > 1) {
        pool = list.filter(function (card) {
          return card.id !== previousId;
        }).sort(function (a, b) {
          return dueTime(a) - dueTime(b);
        }).slice(0, 1);
      }
      if (pool.length > 1) {
        var seenCooldownMs = parseDurationMs(settings.srs.wrongDelay, defaultSrs.wrongDelay);
        var cooledPool = pool.filter(function (card) {
          return nowMs - lastSeenTime(card) >= seenCooldownMs;
        });
        if (cooledPool.length) pool = cooledPool;
      }
      return weightedRandomCard(pool, nowMs);
    }

    function pickRandomCardId(list, previousId) {
      var nextCard = pickNextCard(list, previousId);
      return nextCard ? nextCard.id : "";
    }

    function nextDueCard(list) {
      if (!list.length) return null;
      return list.slice().sort(function (a, b) {
        return dueTime(a) - dueTime(b);
      })[0];
    }

    function markCardSeen(card) {
      if (!card) return;
      card.stats = normalizeStats(card.stats, Date.now());
      card.stats.seenCount += 1;
      card.stats.lastSeenAt = new Date().toISOString();
    }

    function syncCardSeen(card) {
      if (!card || card.id === lastSeenCardId) return;
      markCardSeen(card);
      lastSeenCardId = card.id;
      saveAll();
    }

    function normalizeAnswer(value) {
      return String(value || "").trim().toLowerCase().replace(/\s+/g, " ");
    }

    function maskAnswer(answer) {
      return String(answer || "").replace(/\S+/g, function (word) {
        if (word.length <= 1) return word;
        return word.charAt(0) + "_".repeat(word.length - 1);
      });
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

    function shortenText(text) {
      var value = String(text || "").replace(/\s+/g, " ").trim();
      return value.length > 80 ? value.slice(0, 77) + "..." : value;
    }

    function renderDeleteCardSelect() {
      var list = topicCards();
      deleteCardSelect.innerHTML = "";
      list.forEach(function (card) {
        var option = document.createElement("option");
        option.value = card.id;
        option.textContent = shortenText(card.prompt);
        if (card.id === currentCardId) option.selected = true;
        deleteCardSelect.appendChild(option);
      });
      deleteCardButton.disabled = !list.length;
    }

    function renderSrsControls() {
      newDelayInput.value = settings.srs.newDelay;
      wrongDelayInput.value = settings.srs.wrongDelay;
      correctDelayInput.value = settings.srs.correctDelay;
      correctGrowthInput.value = settings.srs.correctGrowth;
      baseWeightInput.value = settings.srs.baseWeight;
      wrongWeightInput.value = settings.srs.wrongWeight;
      recentWrongWeightInput.value = settings.srs.recentWrongWeight;
      streakPenaltyInput.value = settings.srs.streakPenalty;
      overdueWeightInput.value = settings.srs.overdueWeight;
      minWeightInput.value = settings.srs.minWeight;
      maxWeightInput.value = settings.srs.maxWeight;
    }

    function renderSettingsControls() {
      fillSelect(studyTopicSelect, settings.activeTopic);
      fillSelect(cardTopicSelect, settings.lastTopic);
      fillSelect(deleteTopicSelect, settings.activeTopic);
      renderDeleteCardSelect();
      renderSrsControls();
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
        lastSeenCardId = "";
        promptText.innerHTML = '<span class="flashcard-empty"><strong>Chưa có flashcard</strong><span>Dùng Ctrl + Enter để tạo thẻ cho topic này.</span></span>';
        answerHint.textContent = "";
        answerHint.hidden = true;
        setFeedback("", "");
        answerInput.value = "";
        answerInput.hidden = true;
        dueDrop.hidden = true;
        renderDebug(null);
        return;
      }

      var card = currentCard();
      if (!card) {
        var nextCard = nextDueCard(list);
        var nextDueMs = nextCard ? dueTime(nextCard) - Date.now() : 0;
        currentCardId = "";
        lastSeenCardId = "";
        promptText.innerHTML = '<span class="flashcard-empty"><strong>Chưa có thẻ tới hạn</strong><span>Thẻ tiếp theo sẽ due sau ' + formatMs(nextDueMs) + '.</span></span>';
        answerHint.textContent = "";
        answerHint.hidden = true;
        setFeedback("", "");
        answerInput.value = "";
        answerInput.hidden = true;
        dueDrop.hidden = false;
        renderDebug(null);
        return;
      }
      syncCardSeen(card);
      promptText.textContent = card.prompt;
      answerHint.textContent = "Hint: " + maskAnswer(card.answer);
      answerHint.hidden = false;
      answerInput.hidden = false;
      dueDrop.hidden = true;
      if (answerRevealed) {
        setFeedback("incorrect", "Đáp án không chính xác", card.answer);
      } else {
        setFeedback("", "");
      }
      renderDebug(card);
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
      var nowMs = Date.now();
      var card = normalizeCard({
        id: createId(),
        prompt: promptInput.value,
        answer: answerCreateInput.value,
        topic: selectedTopic,
        stats: defaultStats(nowMs, parseDurationMs(settings.srs.newDelay, defaultSrs.newDelay)),
        createdAt: new Date().toISOString()
      });

      if (!card) {
        setCreateStatus("Vui lòng nhập đủ câu gợi ý và đáp án.");
        return;
      }

      cards.push(card);
      settings.lastTopic = selectedTopic;
      settings.activeTopic = selectedTopic;
      currentCardId = "";
      lastSeenCardId = "";
      resetAttemptState();
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
      lastSeenCardId = "";
      resetAttemptState();
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
      lastSeenCardId = "";
      resetAttemptState();
      saveAll();
      setStatus("Đã xóa topic.");
      render();
    }

    function deleteCard() {
      clearNextCardTimer();
      var cardId = deleteCardSelect.value;
      if (!cardId) return;
      var card = cards.find(function (item) {
        return item.id === cardId;
      });
      if (!card) return;
      var message = 'Xóa thẻ "' + shortenText(card.prompt) + '"?';
      if (!window.confirm(message)) return;

      cards = cards.filter(function (item) {
        return item.id !== cardId;
      });
      if (currentCardId === cardId) {
        currentCardId = "";
        lastSeenCardId = "";
      }
      resetAttemptState();
      answerInput.value = "";
      saveAll();
      setStatus("Đã xóa thẻ.");
      render();
    }

    function collectSrsControls() {
      return normalizeSrsConfig({
        newDelay: newDelayInput.value,
        wrongDelay: wrongDelayInput.value,
        correctDelay: correctDelayInput.value,
        correctGrowth: correctGrowthInput.value,
        baseWeight: baseWeightInput.value,
        wrongWeight: wrongWeightInput.value,
        recentWrongWeight: recentWrongWeightInput.value,
        streakPenalty: streakPenaltyInput.value,
        overdueWeight: overdueWeightInput.value,
        minWeight: minWeightInput.value,
        maxWeight: maxWeightInput.value
      });
    }

    function saveSrsSettings() {
      settings.srs = collectSrsControls();
      saveAll();
      renderSrsControls();
      setStatus("Đã lưu SRS.");
    }

    function resetSrsSettings() {
      settings.srs = Object.assign({}, defaultSrs);
      saveAll();
      renderSrsControls();
      setStatus("Đã reset SRS về default.");
    }

    function resetAllData() {
      var confirmed = window.confirm("Reset toàn bộ flashcard, topic, stats và SRS settings?");
      if (!confirmed) return;
      clearNextCardTimer();
      window.localStorage.removeItem(cardsKey);
      window.localStorage.removeItem(settingsKey);
      cards = [];
      settings = {
        topics: [defaultTopic],
        activeTopic: defaultTopic,
        lastTopic: defaultTopic,
        srs: Object.assign({}, defaultSrs)
      };
      currentCardId = "";
      lastSeenCardId = "";
      resetAttemptState();
      answerInput.value = "";
      setFeedback("", "");
      setCreateStatus("");
      setStatus("Đã reset toàn bộ dữ liệu.");
      saveAll();
      render();
    }

    function changeStudyTopic() {
      clearNextCardTimer();
      settings.activeTopic = normalizeTopic(studyTopicSelect.value);
      currentCardId = "";
      lastSeenCardId = "";
      resetAttemptState();
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
      lastSeenCardId = "";
      resetAttemptState();
      saveAll();
      render();
    }

    function checkAnswer(event) {
      event.preventDefault();
      clearNextCardTimer();
      var card = currentCard();
      if (!card) return;
      var nowMs = Date.now();
      card.stats = normalizeStats(card.stats, nowMs);
      if (normalizeAnswer(answerInput.value) === normalizeAnswer(card.answer)) {
        if (!currentAttemptFailed) {
          card.stats.correctCount += 1;
          card.stats.correctStreak += 1;
          card.stats.recentWrong = Math.max(0, card.stats.recentWrong - 1);
          card.stats.lastAnsweredAt = new Date(nowMs).toISOString();
          card.stats.dueAt = new Date(nowMs + parseDurationMs(settings.srs.correctDelay, defaultSrs.correctDelay) * Math.pow(settings.srs.correctGrowth, card.stats.correctStreak - 1)).toISOString();
          saveAll();
        }
        answerRevealed = false;
        setFeedback("correct", "Đáp án chính xác");
        nextCardTimer = window.setTimeout(function () {
          showRandomCard();
          nextCardTimer = null;
        }, 700);
        return;
      }
      if (!currentAttemptFailed) {
        card.stats.wrongCount += 1;
        card.stats.recentWrong += 1;
        card.stats.correctStreak = 0;
        card.stats.lastAnsweredAt = new Date(nowMs).toISOString();
        card.stats.dueAt = new Date(nowMs + parseDurationMs(settings.srs.wrongDelay, defaultSrs.wrongDelay)).toISOString();
        currentAttemptFailed = true;
        saveAll();
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
      lastSeenCardId = "";
      resetAttemptState();
      answerInput.value = "";
      renderCard();
      if (!answerInput.hidden) answerInput.focus();
    }

    function dropDueByRatio(ratio) {
      var list = topicCards();
      if (!list.length) return;
      var count = Math.max(1, Math.ceil(list.length * ratio));
      var shuffled = list.slice();
      for (var i = shuffled.length - 1; i > 0; i -= 1) {
        var j = Math.floor(Math.random() * (i + 1));
        var temp = shuffled[i];
        shuffled[i] = shuffled[j];
        shuffled[j] = temp;
      }
      var nowIso = new Date().toISOString();
      shuffled.slice(0, count).forEach(function (card) {
        card.stats = normalizeStats(card.stats, Date.now());
        card.stats.dueAt = nowIso;
      });
      currentCardId = "";
      lastSeenCardId = "";
      resetAttemptState();
      answerInput.value = "";
      saveAll();
      render();
      if (!answerInput.hidden) answerInput.focus();
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
    deleteCardButton.addEventListener("click", deleteCard);
    saveSrsButton.addEventListener("click", saveSrsSettings);
    resetSrsButton.addEventListener("click", resetSrsSettings);
    resetAllButton.addEventListener("click", resetAllData);
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
    dueDrop.addEventListener("click", function (event) {
      var button = event.target.closest("button[data-ratio]");
      if (!button) return;
      dropDueByRatio(Number(button.dataset.ratio));
    });

    document.addEventListener("keydown", function (event) {
      if (event.ctrlKey && event.shiftKey && event.key.toLowerCase() === "d") {
        event.preventDefault();
        debugMode = !debugMode;
        renderCard();
        return;
      }
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
