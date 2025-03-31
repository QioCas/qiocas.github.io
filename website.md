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
    <style>
        /* body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        } */
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
            background-color: var(--text-col);
        }
        .category {
            margin: 20px 0 10px;
            font-weight: bold;
            color: var(--link-col);
        }
    </style>
</head>
<body>
    <div class="search-container">
        <input type="text" id="searchInput" placeholder="Tìm kiếm đường dẫn...">
    </div>
    <ul class="link-list" id="linkList">
    </ul>

    <script>
        // Danh sách link của bạn
        const links = [
            // Social
            { name: "Facebook", url: "https://www.facebook.com/", category: "Social" },
            { name: "Telegram", url: "https://web.telegram.org/", category: "Social" },
            { name: "TikTok", url: "https://www.tiktok.com/", category: "Social" },
            { name: "Instagram", url: "https://www.instagram.com/", category: "Social" },
            { name: "Zalo", url: "https://chat.zalo.me/", category: "Social" },
            { name: "Discord", url: "https://discord.com/channels/@me", category: "Social" },
            
            // Study
            { name: "MS Team", url: "https://teams.microsoft.com/v2/", category: "Study" },
            { name: "Course", url: "https://courses.uit.edu.vn/", category: "Study" },
            { name: "Student", url: "https://student.uit.edu.vn/", category: "Study" },
            { name: "DRL", url: "https://drl.uit.edu.vn/", category: "Study" },
            
            // Chatbot
            { name: "ChatGPT", url: "https://chatgpt.com/", category: "Chatbot" },
            { name: "Grok", url: "https://grok.com/", category: "Chatbot" },
            
            // Wibu
            { name: "Mangadex", url: "https://mangadex.org/", category: "Wibu" },
            { name: "LN", url: "https://ln.hako.vn/", category: "Wibu" },
            { name: "Anime Vietsub", url: "https://bit.ly/animevietsubtv", category: "Wibu" }
        ];

        const linkList = document.getElementById('linkList');
        const searchInput = document.getElementById('searchInput');

        // Hàm hiển thị danh sách link
        function displayLinks(linkArray) {
            linkList.innerHTML = '';
            const categories = [...new Set(linkArray.map(link => link.category))];
            
            categories.forEach(category => {
                const categoryDiv = document.createElement('div');
                categoryDiv.className = 'category';
                categoryDiv.textContent = category;
                linkList.appendChild(categoryDiv);

                const categoryLinks = linkArray.filter(link => link.category === category);
                categoryLinks.forEach(link => {
                    const li = document.createElement('li');
                    li.className = 'link-item';
                    li.textContent = link.name;
                    li.onclick = () => {
                        window.open(link.url, '_blank');
                    };
                    linkList.appendChild(li);
                });
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

        // Nhấn Enter để mở link đầu tiên
        searchInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter' && searchInput.value) {
                const searchTerm = searchInput.value.toLowerCase();
                const filteredLinks = links.filter(link => 
                    link.name.toLowerCase().includes(searchTerm)
                );
                if (filteredLinks.length > 0) {
                    window.open(filteredLinks[0].url, '_blank');
                }
            }
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