<!-- ---
layout: page
title: Website
cover-img: "assets/img/background.jpg"
---

- Social:
    + [Facebook](https://www.facebook.com/)
    + [Telegram](https://web.telegram.org/)
    + [TikTok](https://www.tiktok.com/)
    + [Instagram](https://www.instagram.com/)
    + [Zalo](https://chat.zalo.me/)
    + [Discord](https://discord.com/channels/@me)

- Study:
    + [MS Team](https://teams.microsoft.com/v2/)
    + [Course](https://courses.uit.edu.vn/)
    + [Student](https://student.uit.edu.vn/)
    + [DRL](https://drl.uit.edu.vn/)

- Chatbot:
    + [ChatGPT](https://chatgpt.com/)
    + [Grok](https://grok.com/)

- Wibu:
    + [Mangadex](https://mangadex.org/)
    + [LN](https://ln.hako.vn/)
    + [Anime Vietsub](https://bit.ly/animevietsubtv)

 -->


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChatGPT Clone</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f4f4;
            text-align: center;
            margin: 0;
            padding: 0;
        }
        #chat-container {
            width: 50%;
            margin: 20px auto;
            background: white;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
        }
        .message {
            text-align: left;
            margin: 10px 0;
            padding: 10px;
            border-radius: 5px;
        }
        .user-message {
            background: #007bff;
            color: white;
        }
        .bot-message {
            background: #ddd;
        }
        input {
            width: 80%;
            padding: 10px;
            margin-top: 10px;
        }
        button {
            padding: 10px;
            background: #007bff;
            color: white;
            border: none;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div id="chat-container">
        <h2>ChatGPT Clone</h2>
        <div id="chat-box"></div>
        <input type="text" id="user-input" placeholder="Type a message..." />
        <button onclick="sendMessage()">Send</button>
    </div>

    <script>
        const OPENAI_API_KEY = "67ca908c-a600-8001-ab91-669e66523578"; // Replace with your OpenAI API key

        async function sendMessage() {
            let userInput = document.getElementById("user-input").value;
            if (!userInput.trim()) return;

            let chatBox = document.getElementById("chat-box");

            // Display user message
            let userMessage = document.createElement("div");
            userMessage.classList.add("message", "user-message");
            userMessage.textContent = "You: " + userInput;
            chatBox.appendChild(userMessage);

            // Call OpenAI API
            let response = await fetch("https://api.openai.com/v1/chat/completions", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Authorization": `Bearer ${OPENAI_API_KEY}`
                },
                body: JSON.stringify({
                    model: "gpt-4",
                    messages: [{ role: "user", content: userInput }]
                })
            });

            let data = await response.json();

            // Display bot response
            let botMessage = document.createElement("div");
            botMessage.classList.add("message", "bot-message");
            botMessage.textContent = "AI: " + data.choices[0].message.content;
            chatBox.appendChild(botMessage);

            document.getElementById("user-input").value = "";
        }
    </script>
</body>
</html>
