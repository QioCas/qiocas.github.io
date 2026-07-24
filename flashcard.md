---
layout: page
title: Flashcard
full-width: true
---

<style>
  .flashcard-app {
    --card-bg: #202124;
    --card-bg-soft: #24272c;
    --card-border: #3a3d44;
    --card-border-strong: #4f5664;
    --flash-accent: #139ee0;
    --flash-accent-2: #46c28d;
    --flash-danger: #ef6b73;
    --flash-muted: #a9b0bb;
    max-width: 1120px;
    margin: 0 auto 3rem;
    color: #e4e6eb;
  }

  .flashcard-shell {
    display: grid;
    grid-template-columns: minmax(280px, 360px) minmax(0, 1fr);
    gap: 1.25rem;
    align-items: start;
  }

  .flashcard-panel,
  .flashcard-stage {
    border: 1px solid var(--card-border);
    border-radius: 8px;
    background: var(--card-bg);
  }

  .flashcard-panel {
    padding: 1rem;
  }

  .flashcard-panel h2,
  .flashcard-stage h2 {
    margin: 0 0 0.85rem;
    font-size: 1.1rem;
    line-height: 1.3;
  }

  .flashcard-field {
    display: grid;
    gap: 0.4rem;
    margin-bottom: 0.9rem;
  }

  .flashcard-field label {
    margin: 0;
    color: var(--flash-muted);
    font-size: 0.92rem;
    font-weight: 600;
  }

  .flashcard-field input,
  .flashcard-field textarea {
    width: 100%;
    border: 1px solid var(--card-border);
    border-radius: 6px;
    background: #18191a;
    color: #e4e6eb;
    padding: 0.75rem 0.8rem;
    font: inherit;
  }

  .flashcard-field textarea {
    min-height: 112px;
    resize: vertical;
  }

  .flashcard-field input:focus,
  .flashcard-field textarea:focus {
    border-color: var(--flash-accent);
    box-shadow: 0 0 0 3px rgba(19, 158, 224, 0.16);
    outline: none;
  }

  .flashcard-actions,
  .flashcard-nav {
    display: flex;
    flex-wrap: wrap;
    gap: 0.6rem;
  }

  .flashcard-button {
    min-height: 42px;
    border: 1px solid var(--card-border-strong);
    border-radius: 6px;
    background: var(--card-bg-soft);
    color: #e4e6eb;
    padding: 0.65rem 0.9rem;
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

  .flashcard-button.success {
    border-color: var(--flash-accent-2);
    background: rgba(70, 194, 141, 0.14);
  }

  .flashcard-button.danger {
    border-color: rgba(239, 107, 115, 0.7);
    color: #ffd9dc;
  }

  .flashcard-button:disabled {
    opacity: 0.45;
    cursor: not-allowed;
  }

  .flashcard-stage {
    min-height: 520px;
    padding: 1rem;
  }

  .flashcard-toolbar {
    display: flex;
    gap: 1rem;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 1rem;
  }

  .flashcard-progress {
    color: var(--flash-muted);
    font-weight: 700;
  }

  .flashcard-topic {
    display: inline-flex;
    max-width: 100%;
    margin-bottom: 0.8rem;
    border: 1px solid rgba(70, 194, 141, 0.45);
    border-radius: 999px;
    color: #baf2d8;
    padding: 0.25rem 0.65rem;
    font-size: 0.85rem;
    font-weight: 700;
    overflow-wrap: anywhere;
  }

  .flashcard-card {
    display: grid;
    place-items: center;
    min-height: 300px;
    border: 1px solid var(--card-border);
    border-radius: 8px;
    background: #18191a;
    padding: clamp(1rem, 4vw, 2rem);
    text-align: center;
    cursor: pointer;
    user-select: none;
  }

  .flashcard-card:focus {
    border-color: var(--flash-accent);
    box-shadow: 0 0 0 3px rgba(19, 158, 224, 0.16);
    outline: none;
  }

  .flashcard-card-text {
    display: block;
    width: 100%;
    margin: 0;
    color: #ffffff;
    font-size: clamp(1.35rem, 2.4vw, 2rem);
    line-height: 1.45;
    white-space: pre-wrap;
    overflow-wrap: anywhere;
  }

  .flashcard-side-label {
    margin-top: 0.8rem;
    color: var(--flash-muted);
    font-size: 0.9rem;
  }

  .flashcard-nav {
    margin-top: 1rem;
  }

  .flashcard-empty {
    display: grid;
    place-items: center;
    min-height: 300px;
    border: 1px dashed var(--card-border-strong);
    border-radius: 8px;
    color: var(--flash-muted);
    padding: 2rem;
    text-align: center;
  }

  .flashcard-empty strong {
    display: block;
    margin-bottom: 0.4rem;
    color: #e4e6eb;
    font-size: 1.2rem;
  }

  .flashcard-status {
    min-height: 1.35rem;
    margin: 0.75rem 0 0;
    color: var(--flash-muted);
    font-size: 0.92rem;
  }

  @media (max-width: 820px) {
    .flashcard-shell {
      grid-template-columns: 1fr;
    }

    .flashcard-stage {
      min-height: auto;
    }

    .flashcard-toolbar {
      align-items: flex-start;
      flex-direction: column;
      gap: 0.75rem;
    }

    .flashcard-actions,
    .flashcard-nav {
      width: 100%;
    }

    .flashcard-button {
      flex: 1 1 140px;
    }
  }
</style>

<div class="flashcard-app" id="flashcard-app">
  <div class="flashcard-shell">
    <section class="flashcard-panel" aria-labelledby="flashcard-form-title">
      <h2 id="flashcard-form-title">Thêm thẻ mới</h2>
      <form id="flashcard-form">
        <div class="flashcard-field">
          <label for="flashcard-front">Mặt trước</label>
          <textarea id="flashcard-front" name="front" required placeholder="Ví dụ: Binary search là gì?"></textarea>
        </div>
        <div class="flashcard-field">
          <label for="flashcard-back">Mặt sau</label>
          <textarea id="flashcard-back" name="back" required placeholder="Ví dụ: Kỹ thuật tìm kiếm trên dãy đã sắp xếp bằng cách chia đôi khoảng tìm kiếm."></textarea>
        </div>
        <div class="flashcard-field">
          <label for="flashcard-topic">Topic</label>
          <input id="flashcard-topic-input" name="topic" type="text" placeholder="Không bắt buộc">
        </div>
        <div class="flashcard-actions">
          <button class="flashcard-button primary" type="submit">Thêm thẻ</button>
          <button class="flashcard-button" id="flashcard-clear-form" type="button">Xóa form</button>
        </div>
        <p class="flashcard-status" id="flashcard-status" role="status" aria-live="polite"></p>
      </form>
    </section>

    <section class="flashcard-stage" aria-labelledby="flashcard-study-title">
      <div class="flashcard-toolbar">
        <div>
          <h2 id="flashcard-study-title">Ôn tập</h2>
          <div class="flashcard-progress" id="flashcard-progress">0 / 0</div>
        </div>
        <div class="flashcard-actions">
          <button class="flashcard-button success" id="flashcard-shuffle" type="button">Trộn thẻ</button>
          <button class="flashcard-button danger" id="flashcard-delete" type="button">Xóa thẻ này</button>
        </div>
      </div>

      <div id="flashcard-topic" class="flashcard-topic" hidden></div>

      <div class="flashcard-empty" id="flashcard-empty">
        <div>
          <strong>Chưa có thẻ nào</strong>
          Hãy thêm thẻ đầu tiên ở form bên trái. Dữ liệu sẽ được lưu trong trình duyệt này.
        </div>
      </div>

      <button class="flashcard-card" id="flashcard-card" type="button" hidden>
        <span class="flashcard-card-text" id="flashcard-card-text"></span>
      </button>
      <div class="flashcard-side-label" id="flashcard-side-label" hidden></div>

      <div class="flashcard-nav">
        <button class="flashcard-button" id="flashcard-prev" type="button">Thẻ trước</button>
        <button class="flashcard-button primary" id="flashcard-flip" type="button">Lật thẻ</button>
        <button class="flashcard-button" id="flashcard-next" type="button">Thẻ sau</button>
      </div>
    </section>
  </div>
</div>

<script>
  (function () {
    var storageKey = "qiocas.flashcards.v1";
    var cards = [];
    var currentIndex = 0;
    var showingBack = false;

    var form = document.getElementById("flashcard-form");
    var frontInput = document.getElementById("flashcard-front");
    var backInput = document.getElementById("flashcard-back");
    var topicInput = document.getElementById("flashcard-topic-input");
    var clearFormButton = document.getElementById("flashcard-clear-form");
    var status = document.getElementById("flashcard-status");
    var progress = document.getElementById("flashcard-progress");
    var topic = document.getElementById("flashcard-topic");
    var empty = document.getElementById("flashcard-empty");
    var cardButton = document.getElementById("flashcard-card");
    var cardText = document.getElementById("flashcard-card-text");
    var sideLabel = document.getElementById("flashcard-side-label");
    var prevButton = document.getElementById("flashcard-prev");
    var nextButton = document.getElementById("flashcard-next");
    var flipButton = document.getElementById("flashcard-flip");
    var shuffleButton = document.getElementById("flashcard-shuffle");
    var deleteButton = document.getElementById("flashcard-delete");

    function createId() {
      if (window.crypto && typeof window.crypto.randomUUID === "function") {
        return window.crypto.randomUUID();
      }
      return String(Date.now()) + "-" + Math.random().toString(16).slice(2);
    }

    function normalizeCard(card) {
      if (!card || typeof card !== "object") return null;
      var front = typeof card.front === "string" ? card.front.trim() : "";
      var back = typeof card.back === "string" ? card.back.trim() : "";
      if (!front || !back) return null;
      return {
        id: typeof card.id === "string" && card.id ? card.id : createId(),
        front: front,
        back: back,
        topic: typeof card.topic === "string" ? card.topic.trim() : "",
        createdAt: typeof card.createdAt === "string" && card.createdAt ? card.createdAt : new Date().toISOString()
      };
    }

    function writeCardsToStorage(value) {
      try {
        window.localStorage.setItem(storageKey, JSON.stringify(value));
      } catch (error) {
        setStatus("Không thể lưu dữ liệu trong trình duyệt này.");
      }
    }

    function loadCards() {
      try {
        var raw = window.localStorage.getItem(storageKey);
        if (!raw) return [];
        var parsed = JSON.parse(raw);
        if (!Array.isArray(parsed)) throw new Error("Flashcard data is not an array.");
        return parsed.map(normalizeCard).filter(Boolean);
      } catch (error) {
        writeCardsToStorage([]);
        return [];
      }
    }

    function saveCards() {
      writeCardsToStorage(cards);
    }

    function setStatus(message) {
      status.textContent = message || "";
    }

    function clampIndex() {
      if (!cards.length) {
        currentIndex = 0;
        return;
      }
      if (currentIndex < 0) currentIndex = cards.length - 1;
      if (currentIndex >= cards.length) currentIndex = 0;
    }

    function render() {
      clampIndex();
      var hasCards = cards.length > 0;
      empty.hidden = hasCards;
      cardButton.hidden = !hasCards;
      sideLabel.hidden = !hasCards;
      topic.hidden = true;

      progress.textContent = hasCards ? String(currentIndex + 1) + " / " + String(cards.length) : "0 / 0";

      prevButton.disabled = !hasCards;
      nextButton.disabled = !hasCards;
      flipButton.disabled = !hasCards;
      shuffleButton.disabled = cards.length < 2;
      deleteButton.disabled = !hasCards;

      if (!hasCards) {
        cardText.textContent = "";
        sideLabel.textContent = "";
        return;
      }

      var currentCard = cards[currentIndex];
      cardText.textContent = showingBack ? currentCard.back : currentCard.front;
      sideLabel.textContent = showingBack ? "Mặt sau" : "Mặt trước";

      if (currentCard.topic) {
        topic.textContent = currentCard.topic;
        topic.hidden = false;
      }
    }

    function clearForm() {
      form.reset();
      frontInput.focus();
    }

    function addCard(event) {
      event.preventDefault();
      var card = normalizeCard({
        id: createId(),
        front: frontInput.value,
        back: backInput.value,
        topic: topicInput.value,
        createdAt: new Date().toISOString()
      });

      if (!card) {
        setStatus("Vui lòng nhập đủ mặt trước và mặt sau.");
        return;
      }

      cards.push(card);
      currentIndex = cards.length - 1;
      showingBack = false;
      saveCards();
      clearForm();
      setStatus("Đã thêm thẻ.");
      render();
    }

    function moveCard(step) {
      if (!cards.length) return;
      currentIndex += step;
      showingBack = false;
      render();
    }

    function flipCard() {
      if (!cards.length) return;
      showingBack = !showingBack;
      render();
    }

    function shuffleCards() {
      if (cards.length < 2) return;
      for (var i = cards.length - 1; i > 0; i -= 1) {
        var j = Math.floor(Math.random() * (i + 1));
        var temp = cards[i];
        cards[i] = cards[j];
        cards[j] = temp;
      }
      currentIndex = 0;
      showingBack = false;
      saveCards();
      setStatus("Đã trộn thẻ.");
      render();
    }

    function deleteCurrentCard() {
      if (!cards.length) return;
      var confirmed = window.confirm("Xóa thẻ hiện tại?");
      if (!confirmed) return;
      cards.splice(currentIndex, 1);
      showingBack = false;
      saveCards();
      setStatus("Đã xóa thẻ.");
      render();
    }

    form.addEventListener("submit", addCard);
    clearFormButton.addEventListener("click", clearForm);
    prevButton.addEventListener("click", function () {
      moveCard(-1);
    });
    nextButton.addEventListener("click", function () {
      moveCard(1);
    });
    flipButton.addEventListener("click", flipCard);
    cardButton.addEventListener("click", flipCard);
    shuffleButton.addEventListener("click", shuffleCards);
    deleteButton.addEventListener("click", deleteCurrentCard);

    cards = loadCards();
    saveCards();
    render();
  }());
</script>
