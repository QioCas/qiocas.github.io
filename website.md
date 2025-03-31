---
layout: page
title: Website
cover-img: "assets/img/background.jpg"
---

<!-- <!DOCTYPE html>
<html lang="vi"> -->
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Danh sách đường dẫn</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        .search-container {
            margin-bottom: 20px;
        }
        #searchInput {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
        }
        .link-list {
            list-style: none;
            padding: 0;
        }
        .link-item {
            padding: 10px;
            border-bottom: 1px solid #ddd;
            cursor: pointer;
        }
        .link-item:hover {
            background-color: #f5f5f5;
        }
    </style>
</head>
<body>
    <div class="search-container">
        <input type="text" id="searchInput" placeholder="Tìm kiếm đường dẫn...">
    </div>
    <ul class="link-list" id="linkList">
        <!-- Danh sách link sẽ được thêm bằng JavaScript -->
    </ul>

    <script>
        // Danh sách link mẫu (bạn có thể thêm link của mình vào đây)
        const links = [
            { name: "Google", url: "https://www.google.com" },
            { name: "Facebook", url: "https://www.facebook.com" },
            { name: "YouTube", url: "https://www.youtube.com" },
            { name: "Twitter", url: "https://www.twitter.com" }
        ];

        const linkList = document.getElementById('linkList');
        const searchInput = document.getElementById('searchInput');

        // Hàm hiển thị danh sách link
        function displayLinks(linkArray) {
            linkList.innerHTML = '';
            linkArray.forEach(link => {
                const li = document.createElement('li');
                li.className = 'link-item';
                li.textContent = link.name;
                li.onclick = () => {
                    window.open(link.url, '_blank', 'width=800,height=600');
                };
                linkList.appendChild(li);
            });
        }

        // Hiển thị tất cả link khi tải trang
        displayLinks(links);

        // Tìm kiếm link
        searchInput.addEventListener('input', (e) => {
            const searchTerm = e.target.value.toLowerCase();
            const filteredLinks = links.filter(link => 
                link.name.toLowerCase().includes(searchTerm)
            );
            displayLinks(filteredLinks);
        });
    </script>
</body>
<!-- </html> -->

<!-- - Social:
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
    + [Anime Vietsub](https://bit.ly/animevietsubtv) -->