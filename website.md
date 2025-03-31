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
        /* CSS cho pop-up */
        .search-popup {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: none; /* Không có background */
            padding: 0; /* Bỏ padding */
            border: none; /* Bỏ viền */
            box-shadow: none; /* Bỏ bóng */
            z-index: 1000;
            width: 300px; /* Kích thước giống trong hình */
            max-height: 400px;
            overflow-y: auto;
        }
        .search-popup input {
            width: 100%;
            padding: 8px;
            box-sizing: border-box;
            background: #f0f0f0; /* Màu nền ô tìm kiếm giống trong hình */
            border: 1px solid #ccc;
            border-radius: 3px;
        }
        .popup-results {
            list-style: none;
            padding: 0;
            margin-top: 5px;
        }
        .popup-result-item {
            padding: 8px;
            cursor: pointer;
            color: #333; /* Màu chữ giống trong hình */
        }
        .popup-result-item:hover {
            background-color: #e0e0e0; /* Hiệu ứng hover giống trong hình */
        }
        .overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: transparent; /* Overlay trong suốt */
            z-index: 999;
        }
    </style>
</head>
<body>
    <div class="search-container">
        <input type="text" id="searchInput" placeholder="Tìm kiếm đường dẫn...">
    </div>
    <ul class="link-list" id="linkList">
    </ul>

    <!-- Pop-up tìm kiếm -->
    <div class="overlay" id="overlay"></div>
    <div class="search-popup" id="searchPopup">
        <input type="text" id="popupSearchInput" placeholder="Gõ để tìm kiếm...">
        <ul class="popup-results" id="popupResults"></ul>
    </div>

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
        const searchPopup = document.getElementById('searchPopup');
        const popupSearchInput = document.getElementById('popupSearchInput');
        const popupResults = document.getElementById('popupResults');
        const overlay = document.getElementById('overlay');

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

        // Hàm hiển thị kết quả trong pop-up
        function displayPopupResults(linkArray) {
            popupResults.innerHTML = '';
            linkArray.forEach(link => {
                const li = document.createElement('li');
                li.className = 'popup-result-item';
                li.textContent = link.name;
                li.onclick = () => {
                    window.open(link.url, '_blank');
                    closePopup();
                };
                popupResults.appendChild(li);
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

        // Tìm kiếm từ pop-up
        popupSearchInput.addEventListener('input', (e) => {
            const searchTerm = e.target.value.toLowerCase();
            const filteredLinks = links.filter(link => 
                link.name.toLowerCase().includes(searchTerm)
            );
            displayPopupResults(filteredLinks);
            displayLinks(filteredLinks); // Đồng bộ với danh sách chính
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

        // Nhấn Enter trong pop-up
        popupSearchInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter' && popupSearchInput.value) {
                const searchTerm = popupSearchInput.value.toLowerCase();
                const filteredLinks = links.filter(link => 
                    link.name.toLowerCase().includes(searchTerm)
                );
                if (filteredLinks.length > 0) {
                    window.open(filteredLinks[0].url, '_blank');
                    closePopup();
                }
            }
        });

        // Lắng nghe gõ phím trên toàn trang
        document.addEventListener('keydown', (e) => {
            if (e.key.length === 1 && !e.target.tagName.match(/INPUT|TEXTAREA/)) {
                searchPopup.style.display = 'block';
                overlay.style.display = 'block';
                popupSearchInput.focus();
                popupSearchInput.value = e.key;
                const filteredLinks = links.filter(link => 
                    link.name.toLowerCase().includes(e.key.toLowerCase())
                );
                displayPopupResults(filteredLinks);
                displayLinks(filteredLinks);
            }
            if (e.key === 'Escape') {
                closePopup();
            }
        });

        // Đóng pop-up khi nhấp vào overlay
        overlay.addEventListener('click', closePopup);

        // Hàm đóng pop-up
        function closePopup() {
            searchPopup.style.display = 'none';
            overlay.style.display = 'none';
            popupSearchInput.value = '';
            displayPopupResults([]); // Xóa kết quả trong pop-up
            displayLinks(links); // Reset danh sách link chính
        }
    </script>
</body>