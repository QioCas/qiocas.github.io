---
layout: page
title: Website
---

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        .search-container {
            margin-bottom: 20px;
        }
        #searchInput {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            background: #fff; /* Màu nền trắng */
            border: 1px solid #ccc; /* Viền xám nhạt */
            border-radius: 3px;
        }
        .link-list {
            list-style: none;
            padding: 0;
        }
        .link-item {
            padding: 10px;
            border-bottom: 1px solid #ddd;
            color: var(--link-col);
            cursor: pointer;
        }
        .link-item:hover {
            background-color: var(--hover-col);
        }
        .category {
            margin: 20px 0 10px;
            font-weight: bold;
            color: var(--text-col);
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

        // Hàm hiển thị danh sách link chính
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

        // Tìm kiếm từ input chính
        searchInput.addEventListener('input', (e) => {
            const searchTerm = e.target.value.toLowerCase();
            const filteredLinks = links.filter(link => 
                link.name.toLowerCase().includes(searchTerm)
            );
            displayLinks(filteredLinks);
        });

        // Nhấn Enter trong input chính
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

        // Lắng nghe gõ phím trên toàn trang
        document.addEventListener('keydown', (e) => {
            // Kiểm tra nếu phím là chữ cái, số hoặc ký tự hợp lệ, không trong input, và không có phím điều khiển
            if (
                e.key.length === 1 && 
                !e.target.tagName.match(/INPUT|TEXTAREA/) && 
                !e.ctrlKey && !e.altKey && !e.shiftKey && !e.metaKey && !(e.key === ' ')
            ) {
                e.preventDefault(); // Ngăn hành vi mặc định của trình duyệt
                searchInput.focus(); // Focus vào ô tìm kiếm
                searchInput.value += e.key; // Thêm ký tự vừa gõ vào ô tìm kiếm
                const searchTerm = searchInput.value.toLowerCase();
                const filteredLinks = links.filter(link => 
                    link.name.toLowerCase().includes(searchTerm)
                );
                displayLinks(filteredLinks);
            }
            // Nhấn ESC để xóa ô tìm kiếm
            if (e.key === 'Escape') {
                searchInput.value = '';
                displayLinks(links); // Reset danh sách link
            }
        });
    </script>
    <iframe src="https://www.google.com">
    </iframe>

</body>